# Microservices Hands-On Labs Solutions

This document contains the complete Java and configuration source code for the **Spring Boot 3 / Spring Cloud Microservices** exercises.

---

## Table of Contents
1. [HOL 1: Account and Loan Microservices](#hol-1-account-and-loan-microservices)
2. [HOL 2: Eureka Discovery Server Registration](#hol-2-eureka-discovery-server-registration)
3. [HOL 3: Spring Cloud API Gateway & Logging Filter](#hol-3-spring-cloud-api-gateway--logging-filter)
4. [HOL 4: Load Balancing, Resilience4j, OAuth2 & JWT Security](#hol-4-load-balancing-resilience4j-oauth2--jwt-security)

---

## HOL 1: Account and Loan Microservices

### 1. Account Microservice (Port 8080)

#### `src/main/resources/application.properties`
```properties
server.port=8080
spring.application.name=account-service
```

#### `src/main/java/com/cognizant/account/model/Account.java`
```java
package com.cognizant.account.model;

public class Account {
    private String number;
    private String type;
    private double balance;

    public Account(String number, String type, double balance) {
        this.number = number;
        this.type = type;
        this.balance = balance;
    }

    // Getters and Setters
    public String getNumber() { return number; }
    public String getType() { return type; }
    public double getBalance() { return balance; }
}
```

#### `src/main/java/com/cognizant/account/controller/AccountController.java`
```java
package com.cognizant.account.controller;

import com.cognizant.account.model.Account;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class AccountController {

    @GetMapping("/accounts/{number}")
    public Account getAccount(@PathVariable String number) {
        // Return dummy response
        return new Account(number, "savings", 234343.0);
    }
}
```

### 2. Loan Microservice (Port 8081)

#### `src/main/resources/application.properties`
```properties
server.port=8081
spring.application.name=loan-service
```

#### `src/main/java/com/cognizant/loan/model/Loan.java`
```java
package com.cognizant.loan.model;

public class Loan {
    private String number;
    private String type;
    private double loan;
    private double emi;
    private int tenure;

    public Loan(String number, String type, double loan, double emi, int tenure) {
        this.number = number;
        this.type = type;
        this.loan = loan;
        this.emi = emi;
        this.tenure = tenure;
    }

    // Getters and Setters
    public String getNumber() { return number; }
    public String getType() { return type; }
    public double getLoan() { return loan; }
    public double getEmi() { return emi; }
    public int getTenure() { return tenure; }
}
```

#### `src/main/java/com/cognizant/loan/controller/LoanController.java`
```java
package com.cognizant.loan.controller;

import com.cognizant.loan.model.Loan;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class LoanController {

    @GetMapping("/loans/{number}")
    public Loan getLoan(@PathVariable String number) {
        // Return dummy response
        return new Loan(number, "car", 400000.0, 3258.0, 18);
    }
}
```

---

## HOL 2: Eureka Discovery Server Registration

### 1. Eureka Server Configuration (Port 8761)

#### `pom.xml` dependency
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

#### `src/main/resources/application.properties`
```properties
server.port=8761
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
logging.level.com.netflix.eureka=OFF
logging.level.com.netflix.discovery=OFF
```

#### `src/main/java/com/cognizant/eurekaserver/EurekaServerApplication.java`
```java
package com.cognizant.eurekaserver;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer // Enables Discovery server registry capabilities
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

### 2. Registering Client Microservices (Account & Loan)

#### Add Eureka Client to client pom files:
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

#### Annotate AccountApplication/LoanApplication:
```java
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient // Auto registers client with the running Eureka registry
public class AccountApplication { ... }
```

---

## HOL 3: Spring Cloud API Gateway & Logging Filter

### 1. Gateway Server Configuration (Port 9090)

#### `pom.xml` dependencies
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

#### `src/main/resources/application.properties`
```properties
server.port=9090
spring.application.name=api-gateway

# Enable discovery locator configurations
spring.cloud.gateway.discovery.locator.enabled=true
spring.cloud.gateway.discovery.locator.lower-case-service-id=true
```

### 2. Global Logging Filter

#### `src/main/java/com/cts/gateway/filters/LogFilter.java`
```java
package com.cts.gateway.filters;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

@Component
public class LogFilter implements GlobalFilter {
    private static final Logger logger = LoggerFactory.getLogger(LogFilter.class);

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // Logs each incoming API Gateway request URI
        logger.info("====> Request URL {}", exchange.getRequest().getURI());
        return chain.filter(exchange);
    }
}
```

---

## HOL 4: Load Balancing, Resilience4j, OAuth2 & JWT Security

### 1. Load Balancing and Resilience4j Configuration

#### `application.properties`
```properties
# Route load balancing using lb:// prefix
spring.cloud.gateway.routes[0].id=load_balanced_route
spring.cloud.gateway.routes[0].uri=lb://account-service
spring.cloud.gateway.routes[0].predicates[0]=Path=/loadbalanced/**

# Resilience4j Circuit Breaker threshold settings
resilience4j.circuitbreaker.instances.exampleCircuitBreaker.registerHealthIndicator=true
resilience4j.circuitbreaker.instances.exampleCircuitBreaker.slidingWindowSize=10
resilience4j.circuitbreaker.instances.exampleCircuitBreaker.failureRateThreshold=50
```

#### `src/main/java/com/cts/gateway/config/ResilienceConfiguration.java`
```java
package com.cts.gateway.config;

import io.github.resilience4j.circuitbreaker.CircuitBreakerConfig;
import io.github.resilience4j.timelimiter.TimeLimiterConfig;
import org.springframework.cloud.circuitbreaker.resilience4j.ReactiveResilience4JCircuitBreakerFactory;
import org.springframework.cloud.circuitbreaker.resilience4j.Resilience4JConfigBuilder;
import org.springframework.cloud.client.circuitbreaker.Customizer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ResilienceConfiguration {
    @Bean
    public Customizer<ReactiveResilience4JCircuitBreakerFactory> defaultCustomizer() {
        return factory -> factory.configureDefault(id -> new Resilience4JConfigBuilder(id)
                .circuitBreakerConfig(CircuitBreakerConfig.ofDefaults())
                .timeLimiterConfig(TimeLimiterConfig.ofDefaults())
                .build());
    }
}
```

### 2. Centralized Authentication (OAuth2 Client)

#### `src/main/resources/application.yml`
```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          my-client:
            client-id: YOUR_CLIENT_ID
            client-secret: YOUR_CLIENT_SECRET
            scope: openid, profile, email
            authorization-grant-type: authorization_code
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
        provider:
          my-provider:
            authorization-uri: https://accounts.google.com/o/oauth2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://openidconnect.googleapis.com/v1/userinfo
            user-name-attribute: sub
```

### 3. JSON Web Token (JWT) Verification

#### `pom.xml` dependency
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
```

#### `src/main/java/com/cts/security/JwtTokenProvider.java`
```java
package com.cts.security;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import java.util.Date;

@Component
public class JwtTokenProvider {
    
    @Value("${spring.security.jwt.secret:defaultSecret}")
    private String secretKey;

    public String createToken(String username) {
        Claims claims = Jwts.claims().setSubject(username);
        Date now = new Date();
        Date validity = new Date(now.getTime() + 3600000); // 1 hour validity

        return Jwts.builder()
                .setClaims(claims)
                .setIssuedAt(now)
                .setExpiration(validity)
                .signWith(SignatureAlgorithm.HS256, secretKey)
                .compact();
    }
}
```

#### `src/main/java/com/cts/security/JwtTokenFilter.java`
```java
package com.cts.security;

import org.springframework.web.filter.OncePerRequestFilter;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class JwtTokenFilter extends OncePerRequestFilter {

    private JwtTokenProvider jwtTokenProvider;
    
    public JwtTokenFilter(JwtTokenProvider provider) {
        this.jwtTokenProvider = provider;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        String token = resolveToken(request);
        if (token != null) {
            // In a production app, validate the token here and set authentication in SecurityContext
            System.out.println("Validating extracted JWT: " + token);
        }
        filterChain.doFilter(request, response);
    }

    private String resolveToken(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        if (bearerToken != null && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }
}
```
