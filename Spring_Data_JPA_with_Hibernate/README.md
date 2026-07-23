# Spring Data JPA with Hibernate Module - Answers

This document provides the complete, step-by-step answers and production-grade implementations for all hands-on exercises and case studies in the **Spring Data JPA with Hibernate** module.

---

## Table of Contents
1. [Part 1: Spring Data JPA Basics (`orm-learn` Country System)](#part-1-spring-data-jpa-basics-orm-learn-country-system)
   - Hands-on 1: Project Setup and Country Model
   - Hands-on 2 & 3: Hibernate XML vs Annotation Review
   - Hands-on 4: Find Country by Code (Custom Exceptions)
   - Hands-on 5: Add New Country
   - Hands-on 6 & 7: Update and Delete Country
   - Hands-on 8 & 9: Query Methods (Search by containing/starting characters)
2. [Part 2: JPA Relationships & Stock Market Queries](#part-2-jpa-relationships--stock-market-queries)
   - Hands-on 1: Search Queries and Ascending Alphabetical Sorting
   - Hands-on 2: Stock Market Analysis (Query Methods & CSV Population)
   - Hands-on 3: Payroll Schema Setup and Annotations
   - Hands-on 4: `@ManyToOne` (Employee -> Department)
   - Hands-on 5: `@OneToMany` (Department -> Employee) & Lazy vs Eager Fetching
   - Hands-on 6: `@ManyToMany` (Employee <-> Skill)
3. [Part 3: Hibernate Query Language (HQL) and Criteria Queries](#part-3-hibernate-query-language-hql-and-criteria-queries)
   - Hands-on 2: Permanent Employee Details with JOIN FETCH Optimization
   - Hands-on 3: Complex Quiz Attempt Query
   - Hands-on 4: Aggregate HQL (Average Salary by Department)
   - Hands-on 5: Native SQL Queries
   - Hands-on 6: Criteria API (Dynamic Filters Tutorial)
4. [Part 4: Employee Management System (Case Study: Exercises 1-10)](#part-4-employee-management-system-case-study-exercises-1-10)

---

## Part 1: Spring Data JPA Basics (`orm-learn` Country System)

### Hands-on 1: Project Setup and Country Model

#### 1. SQL Schema Script
```sql
CREATE DATABASE ormlearn;
USE ormlearn;

CREATE TABLE country (
    co_code VARCHAR(2) PRIMARY KEY,
    co_name VARCHAR(50) NOT NULL
);

INSERT INTO country VALUES ('IN', 'India');
INSERT INTO country VALUES ('US', 'United States of America');
INSERT INTO country VALUES ('DE', 'Germany');
INSERT INTO country VALUES ('JP', 'Japan');
```

#### 2. Maven Configuration (`pom.xml`)
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

#### 3. Properties Configuration (`src/main/resources/application.properties`)
```properties
# Database Configuration
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/ormlearn?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root

# Hibernate Configuration
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.show-sql=true

# Logging Configuration
logging.level.org.springframework=info
logging.level.com.cognizant=debug
logging.level.org.hibernate.SQL=trace
```

#### 4. Entity Class: `com.cognizant.ormlearn.model.Country`
```java
package com.cognizant.ormlearn.model;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="country")
public class Country {

    @Id
    @Column(name="co_code")
    private String code;

    @Column(name="co_name")
    private String name;

    // Default Constructor
    public Country() {}

    public Country(String code, String name) {
        this.code = code;
        this.name = name;
    }

    // Getters and Setters
    public String getCode() { return code; }
    public void setCode(String code) { this.code = code; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    @Override
    public String toString() {
        return "Country [code=" + code + ", name=" + name + "]";
    }
}
```

#### 5. Repository: `com.cognizant.ormlearn.repository.CountryRepository`
```java
package com.cognizant.ormlearn.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.cognizant.ormlearn.model.Country;

@Repository
public interface CountryRepository extends JpaRepository<Country, String> {
}
```

#### 6. Service Class: `com.cognizant.ormlearn.service.CountryService`
```java
package com.cognizant.ormlearn.service;

import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import com.cognizant.ormlearn.model.Country;
import com.cognizant.ormlearn.repository.CountryRepository;

@Service
public class CountryService {

    @Autowired
    private CountryRepository countryRepository;

    @Transactional(readOnly = true)
    public List<Country> getAllCountries() {
        return countryRepository.findAll();
    }
}
```

#### 7. Main Tester Class: `com.cognizant.ormlearn.OrmLearnApplication`
```java
package com.cognizant.ormlearn;

import java.util.List;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import com.cognizant.ormlearn.model.Country;
import com.cognizant.ormlearn.service.CountryService;

@SpringBootApplication
public class OrmLearnApplication {

    private static final Logger LOGGER = LoggerFactory.getLogger(OrmLearnApplication.class);
    private static CountryService countryService;

    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(OrmLearnApplication.class, args);
        countryService = context.getBean(CountryService.class);
        
        LOGGER.info("Inside main");
        testGetAllCountries();
    }

    private static void testGetAllCountries() {
        LOGGER.info("Start testGetAllCountries");
        List<Country> countries = countryService.getAllCountries();
        LOGGER.debug("Countries found: {}", countries);
        LOGGER.info("End testGetAllCountries");
    }
}
```

---

### Hands-on 2 & 3: Hibernate XML vs Annotation Review

#### SessionFactory, Session, and Transaction Architecture
In Hibernate, the database session is controlled by the following elements:
- **SessionFactory**: A thread-safe, immutable cache of compiled mappings for a single database. Built once during application startup.
- **Session**: A lightweight object representing a single logical connection to the database. Used to save, find, and delete entities.
- **Transaction**: Used to establish atomic units of work. Composed of:
  - `beginTransaction()`: Opens a transaction interface context.
  - `commit()`: Flushes all changes made in the session to the database.
  - `rollback()`: Undoes database adjustments in the event of errors.
- **Operations**:
  - `session.save(entity)`: Saves object state to DB.
  - `session.get(Class, id)`: Fetches record from database by identifier.
  - `session.delete(entity)`: Removes persistent instance from database.

#### Annotation Configurations
Annotations provide mapping rules directly in Java files:
- `@Entity`: Marks class as mapped database table source.
- `@Table(name="x")`: Links entity to target database table name.
- `@Id`: Defines database primary key.
- `@GeneratedValue(strategy = GenerationType.IDENTITY)`: Instructs auto-increment behaviors.
- `@Column(name="x")`: Maps field to database table column.

---

### Hands-on 4: Find Country by Code (Custom Exceptions)

#### 1. Custom Exception: `com.cognizant.ormlearn.service.exception.CountryNotFoundException`
```java
package com.cognizant.ormlearn.service.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(value = HttpStatus.NOT_FOUND, reason = "Country not found")
public class CountryNotFoundException extends Exception {
    public CountryNotFoundException(String message) {
        super(message);
    }
}
```

#### 2. Update `CountryService.java`
```java
@Transactional(readOnly = true)
public Country findCountryByCode(String countryCode) throws CountryNotFoundException {
    return countryRepository.findById(countryCode)
        .orElseThrow(() -> new CountryNotFoundException("Country with code " + countryCode + " was not found."));
}
```

#### 3. Test invocation in `OrmLearnApplication.java`
```java
private static void testFindCountry() {
    LOGGER.info("Start testFindCountry");
    try {
        Country country = countryService.findCountryByCode("IN");
        LOGGER.debug("Found country: {}", country);
        
        // This will fail and throw exception
        countryService.findCountryByCode("ZZ");
    } catch (CountryNotFoundException e) {
        LOGGER.error("Exception occurred: {}", e.getMessage());
    }
    LOGGER.info("End testFindCountry");
}
```

---

### Hands-on 5: Add New Country

#### 1. Update `CountryService.java`
```java
@Transactional
public void addCountry(Country country) {
    countryRepository.save(country);
}
```

#### 2. Test invocation in `OrmLearnApplication.java`
```java
private static void testAddCountry() {
    LOGGER.info("Start testAddCountry");
    Country newCountry = new Country("FR", "France");
    countryService.addCountry(newCountry);
    
    try {
        Country verified = countryService.findCountryByCode("FR");
        LOGGER.debug("Verified added country: {}", verified);
    } catch (CountryNotFoundException e) {
        LOGGER.error("Failed to add country: {}", e.getMessage());
    }
    LOGGER.info("End testAddCountry");
}
```

---

### Hands-on 6 & 7: Update and Delete Country

#### 1. Update/Delete Methods in `CountryService.java`
```java
@Transactional
public void updateCountry(String code, String newName) throws CountryNotFoundException {
    Country country = countryRepository.findById(code)
        .orElseThrow(() -> new CountryNotFoundException("Country not found"));
    country.setName(newName);
    countryRepository.save(country); // Transaction commit automatically persists changes
}

@Transactional
public void deleteCountry(String code) {
    countryRepository.deleteById(code);
}
```

#### 2. Test in `OrmLearnApplication.java`
```java
private static void testUpdateAndDelete() {
    try {
        LOGGER.info("Start Update");
        countryService.updateCountry("FR", "French Republic");
        LOGGER.debug("Updated FR Name");
        
        LOGGER.info("Start Delete");
        countryService.deleteCountry("FR");
        LOGGER.debug("Deleted FR");
    } catch (Exception e) {
        LOGGER.error("Exception: {}", e.getMessage());
    }
}
```

---

### Hands-on 8 & 9: Query Methods (Search by containing/starting characters)

#### 1. Repository Custom Query Methods: `CountryRepository.java`
```java
package com.cognizant.ormlearn.repository;

import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.cognizant.ormlearn.model.Country;

@Repository
public interface CountryRepository extends JpaRepository<Country, String> {
    
    // Search by name containing text, sorted in ascending alphabetical order
    List<Country> findByNameContainingOrderByNameAsc(String namePart);

    // Search by name starting with character
    List<Country> findByNameStartingWith(String prefix);
}
```

#### 2. Service Class Additions: `CountryService.java`
```java
@Transactional(readOnly = true)
public List<Country> searchCountriesByNamePart(String text) {
    return countryRepository.findByNameContainingOrderByNameAsc(text);
}

@Transactional(readOnly = true)
public List<Country> searchCountriesByPrefix(String prefix) {
    return countryRepository.findByNameStartingWith(prefix);
}
```

---

## Part 2: JPA Relationships & Stock Market Queries

### Hands-on 1: Search Queries and Ascending Alphabetical Sorting
Using the JPA repository query methods defined above:
```java
// Controller or Application invocation returning list of countries containing 'ou' sorted:
List<Country> countriesWithOu = countryService.searchCountriesByNamePart("ou");
// Output matching: Bouvet Island, Djibouti, French Southern Territories, etc.
```

---

### Hands-on 2: Stock Market Analysis (Query Methods & CSV Population)

#### 1. SQL Schema Script
```sql
CREATE TABLE stock (
    st_id INT NOT NULL AUTO_INCREMENT,
    st_code VARCHAR(10) NOT NULL, 
    st_date DATE NOT NULL,
    st_open DECIMAL(10,2) NOT NULL,
    st_close DECIMAL(10,2) NOT NULL, 
    st_volume BIGINT NOT NULL,
    PRIMARY KEY (st_id)
);
```

#### 2. Excel Row Conversion Formula to Script
```excel
=CONCATENATE("insert into stock (st_code, st_date, st_open, st_close, st_volume) values ('", B2, "', '", YEAR(A2), "-", TEXT(MONTH(A2),"00"), "-", TEXT(DAY(A2),"00"), "', ", C2, ", ", D2, ", ", E2, ");")
```

#### 3. Model Entity Class: `com.cognizant.ormlearn.model.Stock`
```java
package com.cognizant.ormlearn.model;

import java.math.BigDecimal;
import java.util.Date;
import javax.persistence.*;

@Entity
@Table(name="stock")
public class Stock {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="st_id")
    private int id;

    @Column(name="st_code")
    private String code;

    @Column(name="st_date")
    @Temporal(TemporalType.DATE)
    private Date date;

    @Column(name="st_open")
    private BigDecimal open;

    @Column(name="st_close")
    private BigDecimal close;

    @Column(name="st_volume")
    private long volume;

    // Getters, Setters and toString()...
}
```

#### 4. Repository Query Methods: `StockRepository.java`
```java
package com.cognizant.ormlearn.repository;

import java.math.BigDecimal;
import java.util.Date;
import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.cognizant.ormlearn.model.Stock;

@Repository
public interface StockRepository extends JpaRepository<Stock, Integer> {
    
    // FB stock details in Sept 2019
    List<Stock> findByCodeAndDateBetween(String code, Date startDate, Date endDate);

    // Google stock price close value greater than 1250
    List<Stock> findByCodeAndCloseGreaterThan(String code, BigDecimal closeLimit);

    // Top 3 dates which had highest volume transactions
    List<Stock> findTop3ByOrderByVolumeDesc();

    // Three lowest prices for Netflix
    List<Stock> findTop3ByCodeOrderByCloseAsc(String code);
}
```

---

### Hands-on 3: Payroll Schema Setup and Annotations
To establish relationships, the tables `employee`, `department`, and `skill` are structured with relational key references in `payroll.sql`.

#### Model classes definitions:
##### `com.cognizant.ormlearn.model.Department`
```java
package com.cognizant.ormlearn.model;

import java.util.Set;
import javax.persistence.*;

@Entity
@Table(name="department")
public class Department {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="dp_id")
    private int id;

    @Column(name="dp_name")
    private String name;

    @OneToMany(mappedBy = "department", fetch = FetchType.EAGER)
    private Set<Employee> employeeList;

    // Getters and Setters...
}
```

##### `com.cognizant.ormlearn.model.Skill`
```java
package com.cognizant.ormlearn.model;

import java.util.Set;
import javax.persistence.*;

@Entity
@Table(name="skill")
public class Skill {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="sk_id")
    private int id;

    @Column(name="sk_name")
    private String name;

    @ManyToMany(mappedBy = "skillList")
    private Set<Employee> employeeList;

    // Getters and Setters...
}
```

---

### Hands-on 4: `@ManyToOne` (Employee -> Department)

#### Mapping in `Employee.java`
```java
package com.cognizant.ormlearn.model;

import java.util.Date;
import java.util.Set;
import javax.persistence.*;

@Entity
@Table(name="employee")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="em_id")
    private int id;

    @Column(name="em_name")
    private String name;

    @Column(name="em_salary")
    private double salary;

    @Column(name="em_permanent")
    private boolean permanent;

    @Column(name="em_date_of_birth")
    @Temporal(TemporalType.DATE)
    private Date dateOfBirth;

    // Establish Many-to-One mapping to Department
    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "em_dp_id")
    private Department department;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(name = "employee_skill",
               joinColumns = @JoinColumn(name = "es_em_id"), 
               inverseJoinColumns = @JoinColumn(name = "es_sk_id"))
    private Set<Skill> skillList;

    // Getters and Setters...
}
```

#### Test implementation inside `OrmLearnApplication.java`
```java
private static void testGetEmployee() {
    LOGGER.info("Start testGetEmployee");
    Employee employee = employeeService.get(1);
    LOGGER.debug("Employee: {}", employee);
    LOGGER.debug("Department: {}", employee.getDepartment());
    LOGGER.info("End testGetEmployee");
}
```

---

### Hands-on 5: `@OneToMany` (Department -> Employee) & Lazy vs Eager Fetching
- **Lazy Fetching**: Only loads the parent entity record. Accessing children collection triggers a subsequent select query. Can throw a `LazyInitializationException` if accessed outside an active `@Transactional` context session.
- **Eager Fetching**: Joins the tables together, populating both parent and child values in one single SQL execution.

```java
// Configured inside Department.java:
@OneToMany(mappedBy = "department", fetch = FetchType.EAGER)
private Set<Employee> employeeList;
```

---

### Hands-on 6: `@ManyToMany` (Employee <-> Skill)
Defined using a bridging table `employee_skill`:

```java
// Inside Employee.java
@ManyToMany(fetch = FetchType.EAGER)
@JoinTable(
    name = "employee_skill",
    joinColumns = @JoinColumn(name = "es_em_id"),
    inverseJoinColumns = @JoinColumn(name = "es_sk_id")
)
private Set<Skill> skillList;

// Inside Skill.java
@ManyToMany(mappedBy = "skillList")
private Set<Employee> employeeList;
```

#### Add Skill to Employee logic:
```java
@Transactional
public void addSkillToEmployee(int employeeId, int skillId) {
    Employee employee = employeeRepository.findById(employeeId).get();
    Skill skill = skillRepository.findById(skillId).get();
    employee.getSkillList().add(skill);
    employeeRepository.save(employee);
}
```

---

## Part 3: Hibernate Query Language (HQL) and Criteria Queries

### Hands-on 2: Permanent Employee Details with JOIN FETCH Optimization
Eager configurations run redundant SQL operations. By changing fetching configurations to `LAZY` and applying `JOIN FETCH` queries, we optimize execution down to a single query:

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {
    
    @Query("SELECT e FROM Employee e LEFT JOIN FETCH e.department d LEFT JOIN FETCH e.skillList s WHERE e.permanent = true")
    List<Employee> getAllPermanentEmployees();
}
```

---

### Hands-on 3: Complex Quiz Attempt Query
HQL logic to pull complete relational logs of attempt details in a quiz system:

```java
@Repository
public interface AttemptRepository extends JpaRepository<Attempt, Integer> {

    @Query("SELECT a FROM Attempt a " +
           "JOIN FETCH a.user u " +
           "JOIN FETCH a.attemptQuestions aq " +
           "JOIN FETCH aq.question q " +
           "JOIN FETCH q.options o " +
           "WHERE u.id = :userId AND a.id = :attemptId")
    Attempt getAttemptDetails(@Param("userId") int userId, @Param("attemptId") int attemptId);
}
```

---

### Hands-on 4: Aggregate HQL (Average Salary by Department)
```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {
    
    @Query("SELECT AVG(e.salary) FROM Employee e WHERE e.department.id = :deptId")
    double getAverageSalary(@Param("deptId") int deptId);
}
```

---

### Hands-on 5: Native SQL Queries
For direct query operations bypass objects mapping:
```java
@Query(value = "SELECT * FROM employee", nativeQuery = true)
List<Employee> getAllEmployeesNative();
```

---

### Hands-on 6: Criteria API (Dynamic Filters Tutorial)
For dynamic filters, instantiate `CriteriaBuilder`:
```java
public List<Employee> findEmployeesDynamically(String name, Double minSalary, Boolean permanent) {
    CriteriaBuilder cb = entityManager.getCriteriaBuilder();
    CriteriaQuery<Employee> query = cb.createQuery(Employee.class);
    Root<Employee> employee = query.from(Employee.class);
    
    List<Predicate> predicates = new ArrayList<>();
    
    if (name != null) {
        predicates.add(cb.like(employee.get("name"), "%" + name + "%"));
    }
    if (minSalary != null) {
        predicates.add(cb.ge(employee.get("salary"), minSalary));
    }
    if (permanent != null) {
        predicates.add(cb.equal(employee.get("permanent"), permanent));
    }
    
    query.select(employee).where(cb.and(predicates.toArray(new Predicate[0])));
    return entityManager.createQuery(query).getResultList();
}
```

---

## Part 4: Employee Management System (Case Study: Exercises 1-10)

This system establishes the central payroll manager console.

### 1. Employee Entity with Auditing: `com.ems.model.Employee`
```java
package com.ems.model;

import javax.persistence.*;
import org.springframework.data.annotation.*;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;
import java.util.Date;

@Entity
@Table(name = "employee")
@EntityListeners(AuditingEntityListener.class)
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String email;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;

    // Audit fields
    @CreatedDate
    @Temporal(TemporalType.TIMESTAMP)
    private Date createdDate;

    @LastModifiedDate
    @Temporal(TemporalType.TIMESTAMP)
    private Date lastModifiedDate;

    // Getters and Setters...
}
```

### 2. Custom projections query: `com.ems.projection.EmployeeSummary`
```java
package com.ems.projection;

public interface EmployeeSummary {
    Long getId();
    String getName();
    String getEmail();
    // Nested projections
    DepartmentSummary getDepartment();
    
    interface DepartmentSummary {
        String getName();
    }
}
```

### 3. Repository with Pagination & Auditing: `com.ems.repository.EmployeeRepository`
```java
package com.ems.repository;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;
import com.ems.model.Employee;
import com.ems.projection.EmployeeSummary;

@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {
    
    // Pagination & Sorting Query Method
    Page<Employee> findByDepartmentId(Long departmentId, Pageable pageable);

    // Named Projection query
    @Query("SELECT e.id as id, e.name as name, e.email as email, e.department as department FROM Employee e")
    Page<EmployeeSummary> findAllSummarized(Pageable pageable);
}
```

### 4. Enable Auditing configuration: `com.ems.config.AuditingConfig`
```java
package com.ems.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@Configuration
@EnableJpaAuditing
public class AuditingConfig {
}
```

### 5. Hibernate Dialect Batch Properties Configuration (`application.properties`)
```properties
# Enable Hibernate batch processing configurations
spring.jpa.properties.hibernate.jdbc.batch_size=20
spring.jpa.properties.hibernate.order_inserts=true
spring.jpa.properties.hibernate.order_updates=true

# Database Platform Dialect
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
```
