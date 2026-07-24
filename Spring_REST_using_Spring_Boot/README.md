# Spring REST using Spring Boot Module - Answers

This document provides the complete, step-by-step answers and production-grade implementations for all hands-on exercises in the **Spring REST using Spring Boot** module.

---

## Table of Contents
1. [Part 1: Spring Web Project Setup & XML Configuration Beans](#part-1-spring-web-project-setup--xml-configuration-beans)
   - Hands-on 1: Project Setup
   - Hands-on 2: Loading `SimpleDateFormat` Bean from XML Context
   - Hands-on 3: Logging Implementations
   - Hands-on 4: Loading Country Beans List from XML
2. [Part 2: HTTP Request-Response & REST Controller Mappings](#part-2-http-request-response--rest-controller-mappings)
   - HTTP Headers Analysis
   - REST Hello World Web Service
   - Country Controller (Get India, Get All, Get by Case-Insensitive Code)
   - Exceptional Scenarios: `CountryNotFoundException`
   - MockMVC Service Testing Suites
3. [Part 3: Spring REST Integration & CRUD Operations](#part-3-spring-rest-integration--crud-operations)
   - Employee & Department List Feeds (XML Data Sources)
   - RESTful API Naming Guidelines Table
   - POST Request Processing & Jackson Parsing Details
   - Model Annotations Validation & Global Exception Handling
   - Employee PUT and DELETE Operations (Format Exception Filters)
4. [Part 4: Spring Security & JWT Token Authentication](#part-4-spring-security--jwt-token-authentication)
   - Spring Security Enablement & Basic Authentication
   - In-Memory User and Role Management (BCrypt Security)
   - JWT Concepts & Flow Structure
   - `/authenticate` Generation Service
   - `JwtAuthorizationFilter` Request Interceptors

---

## Part 1: Spring Web Project Setup & XML Configuration Beans

### Hands-on 1: Project Setup
Initialize a Spring Web project `spring-learn` with dependencies: `Spring Web`, `Spring Boot DevTools`.

---

### Hands-on 2: Loading `SimpleDateFormat` Bean from XML Context

#### 1. XML Definition: `src/main/resources/date-format.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dateFormat" class="java.text.SimpleDateFormat">
        <constructor-arg value="dd/MM/yyyy" />
    </bean>

</beans>
```

#### 2. Display Date Code in `SpringLearnApplication.java`
```java
package com.cognizant.springlearn;

import java.text.SimpleDateFormat;
import java.util.Date;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

@SpringBootApplication
public class SpringLearnApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringLearnApplication.class, args);
        displayDate();
    }

    public static void displayDate() {
        ApplicationContext context = new ClassPathXmlApplicationContext("date-format.xml");
        SimpleDateFormat format = context.getBean("dateFormat", SimpleDateFormat.class);
        try {
            Date date = format.parse("31/12/2018");
            System.out.println("Parsed Date: " + date);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

### Hands-on 3: Logging Implementations

#### 1. Custom Format Configuration (`application.properties`)
```properties
logging.level.org.springframework=info
logging.level.com.cognizant.springlearn=debug
logging.pattern.console=%d{yyMMdd}|%d{HH:mm:ss.SSS}|%-20.20thread|%5p|%-25.25logger{25}|%25M|%m%n
```

#### 2. Use LoggerFactory inside `SpringLearnApplication.java`
```java
package com.cognizant.springlearn;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.text.SimpleDateFormat;
import java.util.Date;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringLearnApplication {
    private static final Logger LOGGER = LoggerFactory.getLogger(SpringLearnApplication.class);

    public static void displayDate() {
        LOGGER.info("START displayDate");
        ApplicationContext context = new ClassPathXmlApplicationContext("date-format.xml");
        SimpleDateFormat format = context.getBean("dateFormat", SimpleDateFormat.class);
        try {
            Date date = format.parse("31/12/2018");
            LOGGER.debug("Parsed Date Value: {}", date);
        } catch (Exception e) {
            LOGGER.error("Parsing error occurred: ", e);
        }
        LOGGER.info("END displayDate");
    }
}
```

---

### Hands-on 4: Loading Country Beans List from XML

#### 1. XML Definition: `src/main/resources/country.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="in" class="com.cognizant.springlearn.Country">
        <property name="code" value="IN" />
        <property name="name" value="India" />
    </bean>

    <bean id="us" class="com.cognizant.springlearn.Country">
        <property name="code" value="US" />
        <property name="name" value="United States" />
    </bean>

    <bean id="de" class="com.cognizant.springlearn.Country">
        <property name="code" value="DE" />
        <property name="name" value="Germany" />
    </bean>

    <bean id="jp" class="com.cognizant.springlearn.Country">
        <property name="code" value="JP" />
        <property name="name" value="Japan" />
    </bean>

    <bean id="countryList" class="java.util.ArrayList">
        <constructor-arg>
            <list>
                <ref bean="in" />
                <ref bean="us" />
                <ref bean="de" />
                <ref bean="jp" />
            </list>
        </constructor-arg>
    </bean>

</beans>
```

#### 2. Class Model: `com.cognizant.springlearn.Country`
```java
package com.cognizant.springlearn;

public class Country {
    private String code;
    private String name;

    // Getters and Setters...
}
```

---

## Part 2: HTTP Request-Response & REST Controller Mappings

### HTTP Headers Analysis
- **Request Headers**: Defines metadata parameters supplied by user agent (e.g. `GET /hello.txt HTTP/1.1`, `User-Agent: curl`, `Host: example.com`).
- **Response Headers**: Defines properties of server payload (e.g. `HTTP/1.1 200 OK`, `Content-Type: application/json`).

---

### REST Hello World Web Service
```java
package com.cognizant.springlearn.controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    private static final Logger LOGGER = LoggerFactory.getLogger(HelloController.class);

    @GetMapping("/hello")
    public String sayHello() {
        LOGGER.info("START sayHello");
        LOGGER.info("END sayHello");
        return "Hello World!!";
    }
}
```

---

### Country Controller (Get India, Get All, Get by Case-Insensitive Code)

#### 1. Custom Exception: `CountryNotFoundException`
```java
package com.cognizant.springlearn.service.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(value = HttpStatus.NOT_FOUND, reason = "Country not found")
public class CountryNotFoundException extends Exception {
    public CountryNotFoundException(String msg) {
        super(msg);
    }
}
```

#### 2. CountryService: `com.cognizant.springlearn.service.CountryService`
```java
package com.cognizant.springlearn.service;

import java.util.List;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Service;
import com.cognizant.springlearn.Country;
import com.cognizant.springlearn.service.exception.CountryNotFoundException;

@Service
public class CountryService {
    private List<Country> countries;

    public CountryService() {
        ApplicationContext context = new ClassPathXmlApplicationContext("country.xml");
        this.countries = context.getBean("countryList", List.class);
    }

    public List<Country> getAllCountries() {
        return countries;
    }

    public Country getCountry(String code) throws CountryNotFoundException {
        return countries.stream()
            .filter(c -> c.getCode().equalsIgnoreCase(code))
            .findFirst()
            .orElseThrow(() -> new CountryNotFoundException("Country not found"));
    }
}
```

#### 3. CountryController: `com.cognizant.springlearn.controller.CountryController`
```java
package com.cognizant.springlearn.controller;

import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import com.cognizant.springlearn.Country;
import com.cognizant.springlearn.service.CountryService;
import com.cognizant.springlearn.service.exception.CountryNotFoundException;

@RestController
@RequestMapping("/countries")
public class CountryController {

    @Autowired
    private CountryService countryService;

    @GetMapping("/india")
    public Country getCountryIndia() throws CountryNotFoundException {
        return countryService.getCountry("IN");
    }

    @GetMapping
    public List<Country> getAllCountries() {
        return countryService.getAllCountries();
    }

    @GetMapping("/{code}")
    public Country getCountry(@PathVariable String code) throws CountryNotFoundException {
        return countryService.getCountry(code);
    }
}
```

---

### MockMVC Service Testing Suites
Create JUnit test validation blocks inside `SpringLearnApplicationTests.java`:
```java
package com.cognizant.springlearn;

import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;
import com.cognizant.springlearn.controller.CountryController;

@SpringBootTest
@AutoConfigureMockMvc
public class SpringLearnApplicationTests {

    @Autowired
    private CountryController countryController;

    @Autowired
    private MockMvc mvc;

    @Test
    public void contextLoads() {
        assertNotNull(countryController);
    }

    @Test
    public void testGetCountry() throws Exception {
        mvc.perform(get("/countries/IN"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.code").exists())
            .andExpect(jsonPath("$.code").value("IN"))
            .andExpect(jsonPath("$.name").value("India"));
    }

    @Test
    public void testGetCountryException() throws Exception {
        mvc.perform(get("/countries/ZZ"))
            .andExpect(status().isNotFound());
    }
}
```

---

## Part 3: Spring REST Integration & CRUD Operations

### Employee & Department List Feeds (XML Data Sources)
Create `src/main/resources/employee.xml` defining employee beans lists, read by `EmployeeDao` using class constructor context loading.

---

### RESTful API Naming Guidelines Table
| Action | HTTP Method | Endpoint URL | Request Payload | Response Status |
|---|---|---|---|---|
| Get All Countries | `GET` | `/countries` | None | `200 OK` |
| Get Country | `GET` | `/countries/{code}` | None | `200 OK` / `404 NOT FOUND` |
| Create Country | `POST` | `/countries` | `Country` (JSON) | `201 CREATED` / `400 BAD REQUEST` |
| Update Employee | `PUT` | `/employees` | `Employee` (JSON) | `200 OK` / `400 / 404` |
| Delete Employee | `DELETE` | `/employees/{id}` | None | `200 OK` / `404 NOT FOUND` |

---

### POST Request Processing & Jackson Parsing Details
When receiving JSON payload:
1. Spring parses payload using **Jackson Parser**.
2. Attributes from JSON map to Java bean setters via reflection.
3. Controller triggers operations with target bean populated.

---

### Model Annotations Validation & Global Exception Handling

#### 1. Annotations on Model Fields
```java
public class Country {
    @NotNull(message="Country code cannot be null")
    @Size(min=2, max=2, message="Country code should be 2 characters")
    private String code;
    
    @NotBlank(message="Country name cannot be blank")
    private String name;
}
```

#### 2. Create `com.cognizant.springlearn.GlobalExceptionHandler`
```java
package com.cognizant.springlearn;

import java.util.*;
import java.util.stream.Collectors;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.http.converter.HttpMessageNotReadableException;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;
import com.fasterxml.jackson.databind.exc.InvalidFormatException;

@ControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {
    private static final Logger LOGGER = LoggerFactory.getLogger(GlobalExceptionHandler.class);

    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex,
            HttpHeaders headers, HttpStatus status, WebRequest request) {
        LOGGER.info("START handleMethodArgumentNotValid");
        Map<String, Object> body = new LinkedHashMap<>();
        body.put("timestamp", new Date());
        body.put("status", status.value());

        List<String> errors = ex.getBindingResult().getFieldErrors().stream()
                .map(x -> x.getDefaultMessage())
                .collect(Collectors.toList());

        body.put("errors", errors);
        LOGGER.info("END handleMethodArgumentNotValid");
        return new ResponseEntity<>(body, headers, status);
    }

    @Override
    protected ResponseEntity<Object> handleHttpMessageNotReadable(
            HttpMessageNotReadableException ex, HttpHeaders headers, HttpStatus status, WebRequest request) {
        Map<String, Object> body = new LinkedHashMap<>();
        body.put("timestamp", new Date());
        body.put("status", status.value());
        body.put("error", "Bad Request");

        if (ex.getCause() instanceof InvalidFormatException) {
            final Throwable cause = ex.getCause();
            for (InvalidFormatException.Reference reference : ((InvalidFormatException) cause).getPath()) {
                body.put("message", "Incorrect format for field '" + reference.getFieldName() + "'");
            }
        }
        return new ResponseEntity<>(body, headers, status);
    }
}
```

---

### Employee PUT and DELETE Operations (Format Exception Filters)

#### 1. Validations on `Employee` Model
```java
public class Employee {
    @NotNull(message="ID cannot be null")
    private Integer id;

    @NotNull(message="Name cannot be null")
    @Size(min=1, max=30, message="Name must be between 1 and 30 characters")
    private String name;

    @Min(value=0, message="Salary should be zero or above")
    private double salary;

    @com.fasterxml.jackson.annotation.JsonFormat(shape=com.fasterxml.jackson.annotation.JsonFormat.Shape.STRING, pattern="dd/MM/yyyy")
    private Date dateOfBirth;

    // Getters and Setters...
}
```

#### 2. REST update/delete endpoints in `EmployeeController.java`
```java
@PutMapping("/employees")
public void updateEmployee(@RequestBody @Valid Employee employee) throws EmployeeNotFoundException {
    employeeService.updateEmployee(employee);
}

@DeleteMapping("/employees/{id}")
public void deleteEmployee(@PathVariable int id) throws EmployeeNotFoundException {
    employeeService.deleteEmployee(id);
}
```

---

## Part 4: Spring Security & JWT Token Authentication

### Spring Security Enablement & Basic Authentication
Add dependency: `spring-boot-starter-security`.
restrict `/countries` GET service:
```java
package com.cognizant.springlearn.security;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity httpSecurity) throws Exception {
        httpSecurity.csrf().disable()
            .httpBasic().and()
            .authorizeRequests()
            .antMatchers("/countries").hasRole("USER")
            .anyRequest().authenticated();
    }
}
```

---

### In-Memory User and Role Management (BCrypt Security)
Add Authentication Builder settings inside `SecurityConfig.java`:
```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    auth.inMemoryAuthentication()
        .withUser("admin").password(passwordEncoder().encode("pwd")).roles("ADMIN")
        .and()
        .withUser("user").password(passwordEncoder().encode("pwd")).roles("USER");
}

@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

---

### JWT Concepts & Flow Structure
1. User provides authentication details.
2. Server validates details, signs token containing claims, returns it.
3. User adds `Authorization: Bearer <token>` to sub-headers.
4. Server filter validates signature for each REST query.

---

### `/authenticate` Generation Service

#### 1. Add Dependency (`pom.xml`)
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.0</version>
</dependency>
```

#### 2. AuthenticationController: `com.cognizant.springlearn.controller.AuthenticationController`
```java
package com.cognizant.springlearn.controller;

import java.util.*;
import org.springframework.web.bind.annotation.*;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

@RestController
public class AuthenticationController {

    @GetMapping("/authenticate")
    public Map<String, String> authenticate(@RequestHeader("Authorization") String authHeader) {
        String base64Credentials = authHeader.substring("Basic ".length()).trim();
        String credentials = new String(Base64.getDecoder().decode(base64Credentials));
        String username = credentials.split(":", 2)[0];

        String token = Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + 1200000)) // 20 min
                .signWith(SignatureAlgorithm.HS256, "secretkey")
                .compact();

        Map<String, String> result = new HashMap<>();
        result.put("token", token);
        return result;
    }
}
```

---

### `JwtAuthorizationFilter` Request Interceptors

#### 1. Filter: `com.cognizant.springlearn.security.JwtAuthorizationFilter`
```java
package com.cognizant.springlearn.security;

import java.io.IOException;
import java.util.ArrayList;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.www.BasicAuthenticationFilter;
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;

public class JwtAuthorizationFilter extends BasicAuthenticationFilter {

    public JwtAuthorizationFilter(AuthenticationManager authManager) {
        super(authManager);
    }

    @Override
    protected void doFilterInternal(HttpServletRequest req, HttpServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        String header = req.getHeader("Authorization");

        if (header == null || !header.startsWith("Bearer ")) {
            chain.doFilter(req, res);
            return;
        }

        UsernamePasswordAuthenticationToken authentication = getAuthentication(req);
        SecurityContextHolder.getContext().setAuthentication(authentication);
        chain.doFilter(req, res);
    }

    private UsernamePasswordAuthenticationToken getAuthentication(HttpServletRequest request) {
        String token = request.getHeader("Authorization");
        if (token != null) {
            try {
                String user = Jwts.parser()
                        .setSigningKey("secretkey")
                        .parseClaimsJws(token.replace("Bearer ", ""))
                        .getBody()
                        .getSubject();

                if (user != null) {
                    return new UsernamePasswordAuthenticationToken(user, null, new ArrayList<>());
                }
            } catch (Exception e) {
                return null;
            }
        }
        return null;
    }
}
```

#### 2. Update `SecurityConfig.java` to apply filter
```java
@Override
protected void configure(HttpSecurity httpSecurity) throws Exception {
    httpSecurity.csrf().disable()
        .httpBasic().and()
        .authorizeRequests()
        .antMatchers("/authenticate").hasAnyRole("USER", "ADMIN")
        .anyRequest().authenticated().and()
        .addFilter(new JwtAuthorizationFilter(authenticationManager()));
}
```
