# Spring Core and Maven Module - Answers

This document provides the complete, step-by-step answers and fully functional code implementations for all exercises in the **Spring Core and Maven** module.

---

## Table of Contents
1. [Exercise 1: Configuring a Basic Spring Application](#exercise-1-configuring-a-basic-spring-application)
2. [Exercise 2: Implementing Dependency Injection](#exercise-2-implementing-dependency-injection)
3. [Exercise 3: Implementing Logging with Spring AOP](#exercise-3-implementing-logging-with-spring-aop)
4. [Exercise 4: Creating and Configuring a Maven Project](#exercise-4-creating-and-configuring-a-maven-project)
5. [Exercise 5: Configuring the Spring IoC Container](#exercise-5-configuring-the-spring-ioc-container)
6. [Exercise 6: Configuring Beans with Annotations](#exercise-6-configuring-beans-with-annotations)
7. [Exercise 7: Implementing Constructor and Setter Injection](#exercise-7-implementing-constructor-and-setter-injection)
8. [Exercise 8: Implementing Basic AOP with Spring](#exercise-8-implementing-basic-aop-with-spring)
9. [Exercise 9: Creating a Spring Boot Application](#exercise-9-creating-a-spring-boot-application)

---

## Exercise 1: Configuring a Basic Spring Application

### Scenario
You are developing a backend operations system for a library. You need to configure a basic Spring application containing a service and a repository.

### Implementation Details

#### 1. Maven Dependency Configuration (`pom.xml`)
Add the Spring Core and Context dependencies:
```xml
<dependencies>
    <!-- Spring Core -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>5.3.29</version>
    </dependency>
    <!-- Spring Context -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.29</version>
    </dependency>
</dependencies>
```

#### 2. Class Definitions
##### Repository: `com.library.repository.BookRepository`
```java
package com.library.repository;

public class BookRepository {
    public void saveData() {
        System.out.println("Saving book data into the database repository...");
    }
}
```

##### Service: `com.library.service.BookService`
```java
package com.library.service;

import com.library.repository.BookRepository;

public class BookService {
    private BookRepository bookRepository;

    // Setter for XML wiring
    public void setBookRepository(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public void registerBook() {
        System.out.println("Registering a new book in BookService...");
        if (bookRepository != null) {
            bookRepository.saveData();
        } else {
            System.out.println("Error: BookRepository is null!");
        }
    }
}
```

#### 3. XML Configuration (`src/main/resources/applicationContext.xml`)
Define the beans in XML format:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Define BookRepository Bean -->
    <bean id="bookRepository" class="com.library.repository.BookRepository" />

    <!-- Define BookService Bean -->
    <bean id="bookService" class="com.library.service.BookService">
        <!-- Manual wiring setter injection -->
        <property name="bookRepository" ref="bookRepository" />
    </bean>

</beans>
```

#### 4. Testing Execution: `LibraryManagementApplication.java`
```java
package com.library;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.library.service.BookService;

public class LibraryManagementApplication {
    public static void main(String[] args) {
        // Load the Spring container XML configuration
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        
        // Retrieve BookService bean
        BookService bookService = context.getBean("bookService", BookService.class);
        
        // Execute service method
        bookService.registerBook();
    }
}
```

---

## Exercise 2: Implementing Dependency Injection

### Scenario
Wire the dependencies between `BookService` and `BookRepository` using Spring's IoC container setter injection.

### Implementation Details
The XML configuration uses the `<property>` tag, linking the dependency dynamically via setter methods.

#### Setter Method in `BookService.java`
```java
package com.library.service;

import com.library.repository.BookRepository;

public class BookService {
    private BookRepository bookRepository;

    // Setter Injection Method
    public void setBookRepository(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public void run() {
        bookRepository.saveData();
    }
}
```

#### XML Definition (`applicationContext.xml`)
```xml
<bean id="bookRepository" class="com.library.repository.BookRepository" />

<bean id="bookService" class="com.library.service.BookService">
    <!-- Wiring the dependency dynamically using setter injection -->
    <property name="bookRepository" ref="bookRepository" />
</bean>
```

---

## Exercise 3: Implementing Logging with Spring AOP

### Scenario
Implement logging capabilities using Spring AOP to record the execution time of method calls in the service classes.

### Implementation Details

#### 1. AOP Dependencies in `pom.xml`
```xml
<!-- Spring AOP -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.3.29</version>
</dependency>
<!-- AspectJ Weaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.7</version>
</dependency>
```

#### 2. Create the Logging Aspect Class: `com.library.aspect.LoggingAspect`
```java
package com.library.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;

@Aspect
public class LoggingAspect {

    @Around("execution(* com.library.service.BookService.*(..))")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long startTime = System.currentTimeMillis();
        
        System.out.println("[AOP-LOG] Before executing: " + joinPoint.getSignature().getName());
        
        // Execute the target method
        Object proceed = joinPoint.proceed();
        
        long executionTime = System.currentTimeMillis() - startTime;
        System.out.println("[AOP-LOG] After executing: " + joinPoint.getSignature().getName() + " (Executed in: " + executionTime + "ms)");
        
        return proceed;
    }
}
```

#### 3. Update XML Configuration (`applicationContext.xml`)
Enable AOP auto-proxying and define the aspect:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
           http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- Auto-Proxy Configuration -->
    <aop:aspectj-autoproxy />

    <!-- Beans -->
    <bean id="bookRepository" class="com.library.repository.BookRepository" />
    
    <bean id="bookService" class="com.library.service.BookService">
        <property name="bookRepository" ref="bookRepository" />
    </bean>

    <!-- Aspect Bean -->
    <bean id="loggingAspect" class="com.library.aspect.LoggingAspect" />

</beans>
```

---

## Exercise 4: Creating and Configuring a Maven Project

### Scenario
Configure the base structure and standard elements of a Maven compilation workspace with multi-dependency configurations.

### Configuration (`pom.xml`)
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.library</groupId>
    <artifactId>LibraryManagement</artifactId>
    <version>1.0.0</version>
    <name>LibraryManagement</name>
    <description>Library management application using Spring Framework and Maven.</description>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <spring.version>5.3.29</spring.version>
    </properties>

    <dependencies>
        <!-- Spring Context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- Spring AOP -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- Spring WebMVC -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- AspectJ Weaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.7</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Maven Compiler Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

---

## Exercise 5: Configuring the Spring IoC Container

### Scenario
Provide central bean definition configuration and validation methods inside the Spring container context.

### Configuration (`applicationContext.xml`)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Repository Bean -->
    <bean id="bookRepository" class="com.library.repository.BookRepository" />

    <!-- Service Bean with Setter injection -->
    <bean id="bookService" class="com.library.service.BookService">
        <property name="bookRepository" ref="bookRepository" />
    </bean>

</beans>
```

---

## Exercise 6: Configuring Beans with Annotations

### Scenario
Configure components and auto-scan packages to simplify dependency management and remove boiler-plate XML configurations.

### Implementation Details

#### 1. Annotations on Repository Class
```java
package com.library.repository;

import org.springframework.stereotype.Repository;

@Repository
public class BookRepository {
    public void saveData() {
        System.out.println("Saving book data (Annotation configuration)...");
    }
}
```

#### 2. Annotations on Service Class
```java
package com.library.service;

import com.library.repository.BookRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BookService {

    @Autowired
    private BookRepository bookRepository;

    public void registerBook() {
        System.out.println("Registering a new book via Service...");
        bookRepository.saveData();
    }
}
```

#### 3. XML Package Scanner Definition (`applicationContext.xml`)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
           http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- Instructs Spring to auto-discover classes annotated with @Component, @Service, @Repository, etc. -->
    <context:component-scan base-package="com.library" />

</beans>
```

---

## Exercise 7: Implementing Constructor and Setter Injection

### Scenario
Understand and implement both setter and constructor injection strategies for service initialization.

### Implementation Details

#### 1. Class Definitions: `BookService.java`
```java
package com.library.service;

import com.library.repository.BookRepository;

public class BookService {
    private BookRepository bookRepository;
    private String serviceType;

    // Default constructor
    public BookService() {}

    // Constructor Injection
    public BookService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
        this.serviceType = "Constructor-Injected";
    }

    // Setter Injection
    public void setBookRepository(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public void setServiceType(String serviceType) {
        this.serviceType = serviceType;
    }

    public void printDetails() {
        System.out.println("Service Type: " + serviceType);
        if (bookRepository != null) {
            bookRepository.saveData();
        }
    }
}
```

#### 2. XML Configuration (`applicationContext.xml`)
```xml
<!-- Repository -->
<bean id="bookRepository" class="com.library.repository.BookRepository" />

<!-- Scenario A: Using Constructor Injection -->
<bean id="bookServiceConstructor" class="com.library.service.BookService">
    <constructor-arg ref="bookRepository" />
</bean>

<!-- Scenario B: Using Setter Injection -->
<bean id="bookServiceSetter" class="com.library.service.BookService">
    <property name="bookRepository" ref="bookRepository" />
    <property name="serviceType" value="Setter-Injected-Service" />
</bean>
```

---

## Exercise 8: Implementing Basic AOP with Spring

### Scenario
Implement aspect rules (Advice method execution timings) at execution join points of the library application service.

### Implementation Details

#### 1. Define logging aspect class: `com.library.aspect.LoggingAspect`
```java
package com.library.aspect;

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.After;

@Aspect
public class LoggingAspect {

    @Before("execution(* com.library.service.BookService.*(..))")
    public void logBefore() {
        System.out.println("[AOP INFO] A method in BookService is about to begin execution.");
    }

    @After("execution(* com.library.service.BookService.*(..))")
    public void logAfter() {
        System.out.println("[AOP INFO] A method in BookService has finished execution.");
    }
}
```

#### 2. XML Configuration Enablement
```xml
<!-- Register proxy compiler -->
<aop:aspectj-autoproxy />

<!-- Declaring Beans -->
<bean id="bookRepository" class="com.library.repository.BookRepository" />
<bean id="bookService" class="com.library.service.BookService">
    <property name="bookRepository" ref="bookRepository"/>
</bean>

<!-- Registering Aspect Bean -->
<bean id="loggingAspect" class="com.library.aspect.LoggingAspect" />
```

---

## Exercise 9: Creating a Spring Boot Application

### Scenario
Refactor the library backend application to run inside a modern Spring Boot web server configuration environment.

### Implementation Details

#### 1. Database Configuration (`src/main/resources/application.properties`)
```properties
# H2 Database Configurations
spring.datasource.url=jdbc:h2:mem:librarydb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password

# Hibernate configurations
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
```

#### 2. Entity: `com.library.model.Book`
```java
package com.library.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "books")
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String author;

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getTitle() { return title; }
    public void setTitle(String title) { this.title = title; }
    public String getAuthor() { return author; }
    public void setAuthor(String author) { this.author = author; }
}
```

#### 3. Repository: `com.library.repository.BookRepository`
```java
package com.library.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.library.model.Book;

@Repository
public interface BookRepository extends JpaRepository<Book, Long> {
}
```

#### 4. REST Controller: `com.library.controller.BookController`
```java
package com.library.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import com.library.model.Book;
import com.library.repository.BookRepository;
import java.util.List;

@RestController
@RequestMapping("/books")
public class BookController {

    @Autowired
    private BookRepository bookRepository;

    @GetMapping
    public List<Book> getAllBooks() {
        return bookRepository.findAll();
    }

    @PostMapping
    public Book createBook(@RequestBody Book book) {
        return bookRepository.save(book);
    }

    @GetMapping("/{id}")
    public Book getBookById(@PathVariable Long id) {
        return bookRepository.findById(id)
                .orElseThrow(() -> new RuntimeException("Book not found with id: " + id));
    }

    @PutMapping("/{id}")
    public Book updateBook(@PathVariable Long id, @RequestBody Book updatedBook) {
        return bookRepository.findById(id).map(book -> {
            book.setTitle(updatedBook.getTitle());
            book.setAuthor(updatedBook.getAuthor());
            return bookRepository.save(book);
        }).orElseThrow(() -> new RuntimeException("Book not found with id: " + id));
    }

    @DeleteMapping("/{id}")
    public String deleteBook(@PathVariable Long id) {
        bookRepository.deleteById(id);
        return "Book with ID " + id + " has been successfully deleted.";
    }
}
```

#### 5. Application Main: `com.library.LibraryApplication`
```java
package com.library;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class LibraryApplication {
    public static void main(String[] args) {
        SpringApplication.run(LibraryApplication.class, args);
    }
}
```
