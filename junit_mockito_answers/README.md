# JUnit, Mockito and SLF4J Testing Solutions

This document contains the complete Java source code implementations for basic/advanced JUnit testing, Mockito mocks, Spring Boot Test integrations, and SLF4J logging exercises.

---

## Table of Contents
1. [JUnit Basic Testing](#junit-basic-testing)
2. [Advanced JUnit Testing](#advanced-junit-testing)
3. [Mockito Mocking and Stubbing](#mockito-mocking-and-stubbing)
4. [Advanced Mockito Mocking](#advanced-mockito-mocking)
5. [JUnit Spring Test Integrations](#junit-spring-test-integrations)
6. [Mockito Mock Dependencies in Spring](#mockito-mock-dependencies-in-spring)
7. [SLF4J Logging](#slf4j-logging)

---

## JUnit Basic Testing

### Exercise 1: Setting Up JUnit
Maven dependency in `pom.xml`:
```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>
```

### Exercise 2 & 3: Writing Basic Tests & Assertions
```java
import org.junit.Test;
import static org.junit.Assert.*;

public class AssertionsTest {
    @Test
    public void testAssertions() {
        // Assert Equals
        assertEquals(5, 2 + 3);

        // Assert True
        assertTrue(5 > 3);

        // Assert False
        assertFalse(5 < 3);

        // Assert Null
        assertNull(null);

        // Assert Not Null
        assertNotNull(new Object());
    }
}
```

### Exercise 4: Arrange-Act-Assert (AAA) Pattern & Test Fixtures
```java
import org.junit.Before;
import org.junit.After;
import org.junit.Test;
import java.util.ArrayList;
import java.util.List;
import static org.junit.Assert.*;

public class ListTestFixture {
    private List<String> list;

    @Before
    public void setUp() {
        // Arrange
        list = new ArrayList<>();
        list.add("Item1");
        list.add("Item2");
    }

    @Test
    public void testListSize() {
        // Act
        int size = list.size();

        // Assert
        assertEquals(2, size);
    }

    @After
    public void tearDown() {
        list.clear();
        list = null;
    }
}
```

---

## Advanced JUnit Testing

### Exercise 1: Parameterized Tests
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import static org.junit.jupiter.api.Assertions.assertTrue;

class EvenChecker {
    public boolean isEven(int number) {
        return number % 2 == 0;
    }
}

public class EvenCheckerTest {
    private final EvenChecker checker = new EvenChecker();

    @ParameterizedTest
    @ValueSource(ints = {2, 4, 6, 8, 10})
    public void testIsEven(int number) {
        assertTrue(checker.isEven(number));
    }
}
```

### Exercise 2: Test Suites and Categories
```java
import org.junit.platform.suite.api.SelectClasses;
import org.junit.platform.suite.api.Suite;

@Suite
@SelectClasses({ AssertionsTest.class, EvenCheckerTest.class })
public class AllTests {
    // Acts as a test holder suite for bulk execution
}
```

### Exercise 3: Test Execution Order
```java
import org.junit.jupiter.api.MethodOrderer;
import org.junit.jupiter.api.Order;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestMethodOrder;

@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
public class OrderedTests {

    @Test
    @Order(1)
    public void firstTest() {
        System.out.println("Executing first step");
    }

    @Test
    @Order(2)
    public void secondTest() {
        System.out.println("Executing second step");
    }
}
```

### Exercise 4: Exception Testing
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertThrows;

class ExceptionThrower {
    public void throwException() {
        throw new IllegalArgumentException("Invalid arguments provided");
    }
}

public class ExceptionThrowerTest {
    @Test
    public void testThrowException() {
        ExceptionThrower thrower = new ExceptionThrower();
        assertThrows(IllegalArgumentException.class, () -> {
            thrower.throwException();
        });
    }
}
```

### Exercise 5: Timeout and Performance Testing
```java
import org.junit.jupiter.api.Test;
import java.time.Duration;
import static org.junit.jupiter.api.Assertions.assertTimeout;

class PerformanceTester {
    public void performTask() throws InterruptedException {
        Thread.sleep(50); // Simulate processing delay
    }
}

public class PerformanceTesterTest {
    @Test
    public void testPerformanceTimeout() {
        PerformanceTester tester = new PerformanceTester();
        assertTimeout(Duration.ofMillis(100), () -> {
            tester.performTask();
        });
    }
}
```

---

## Mockito Mocking and Stubbing

### Exercise 1 & 2: Mocking, Stubbing & Verifying Interactions
```java
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

interface ExternalApi {
    String getData();
}

class MyService {
    private ExternalApi api;
    public MyService(ExternalApi api) { this.api = api; }
    public String fetchData() { return api.getData(); }
}

public class MyServiceTest {
    @Test
    public void testExternalApi() {
        // Arrange
        ExternalApi mockApi = mock(ExternalApi.class);
        when(mockApi.getData()).thenReturn("Mock Data");
        MyService service = new MyService(mockApi);

        // Act
        String result = service.fetchData();

        // Assert
        assertEquals("Mock Data", result);
        
        // Verify Interaction
        verify(mockApi).getData();
    }
}
```

### Exercise 3 & 4: Argument Matching & Void Methods
```java
import static org.mockito.Mockito.*;
import org.junit.jupiter.api.Test;

interface EmailService {
    void sendEmail(String recipient, String message);
}

public class EmailServiceTest {
    @Test
    public void testArgumentMatchingAndVoid() {
        EmailService mockEmail = mock(EmailService.class);
        
        // Stub void method using doNothing/doThrow (optional since doNothing is default)
        doNothing().when(mockEmail).sendEmail(anyString(), anyString());

        // Invoke method
        mockEmail.sendEmail("john@example.com", "Test Notification");

        // Verify with arguments matchers
        verify(mockEmail).sendEmail(eq("john@example.com"), anyString());
    }
}
```

### Exercise 5 & 6: Multiple Return Values & Verifying Order
```java
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import org.mockito.InOrder;

interface CodeService {
    String generateCode();
}

public class CodeServiceTest {
    @Test
    public void testMultipleReturnsAndOrder() {
        CodeService service = mock(CodeService.class);
        when(service.generateCode())
            .thenReturn("Code1")
            .thenReturn("Code2");

        assertEquals("Code1", service.generateCode());
        assertEquals("Code2", service.generateCode());

        // Verify interaction order
        InOrder inOrder = inOrder(service);
        inOrder.verify(service, times(2)).generateCode();
    }
}
```

### Exercise 7: Handling Void Methods with Exceptions
```java
import static org.mockito.Mockito.*;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertThrows;

interface DatabaseWriter {
    void writeData(String key, String value) throws RuntimeException;
}

public class DatabaseWriterTest {
    @Test
    public void testVoidMethodException() {
        DatabaseWriter mockWriter = mock(DatabaseWriter.class);
        doThrow(new RuntimeException("Database error")).when(mockWriter).writeData(anyString(), anyString());

        assertThrows(RuntimeException.class, () -> {
            mockWriter.writeData("key", "value");
        });
    }
}
```

---

## Advanced Mockito Mocking

### Exercise 1: Mocking Databases & Repositories
```java
interface Repository {
    String getData();
}

class Service {
    private Repository repo;
    public Service(Repository r) { this.repo = r; }
    public String processData() { return "Processed " + repo.getData(); }
}
// Solution Code is equivalent to MyServiceTest above using mock(Repository.class)
```

### Exercise 2 & 3: Mocking External Services (REST APIs) and File I/O
```java
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

interface RestClient {
    String getResponse();
}

class ApiService {
    private RestClient client;
    public ApiService(RestClient c) { this.client = c; }
    public String fetchData() { return "Fetched " + client.getResponse(); }
}

public class ApiServiceTest {
    @Test
    public void testServiceWithMockRestClient() {
        RestClient mockRestClient = mock(RestClient.class);
        when(mockRestClient.getResponse()).thenReturn("Mock Response");
        ApiService apiService = new ApiService(mockRestClient);
        
        String result = apiService.fetchData();
        assertEquals("Fetched Mock Response", result);
    }
}
```

### Exercise 4 & 5: Mocking Network Interactions & Multiple Returns
```java
interface NetworkClient {
    String connect();
}

class NetworkService {
    private NetworkClient client;
    public NetworkService(NetworkClient c) { this.client = c; }
    public String connectToServer() { return "Connected to " + client.connect(); }
}
// Tested using mock(NetworkClient.class) and stubbing when().thenReturn().
```

---

## JUnit Spring Test Integrations

### Exercise 1 & 2: Service Method Test & Mocking Repository in Service
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import java.util.Optional;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
public class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    public void testGetUserById() {
        User user = new User(1L, "Alice");
        when(userRepository.findById(1L)).thenReturn(Optional.of(user));

        User result = userService.getUserById(1L);
        assertNotNull(result);
        assertEquals("Alice", result.getName());
    }
}
```

### Exercise 3 & 5: Testing Controller (GET & POST) with MockMvc
```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(UserController.class)
public class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    public void testGetUserEndpoint() throws Exception {
        User user = new User(1L, "Bob");
        when(userService.getUserById(1L)).thenReturn(user);

        mockMvc.perform(get("/users/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("Bob"));
    }

    @Test
    public void testCreateUserEndpoint() throws Exception {
        User user = new User(1L, "Charlie");
        when(userService.saveUser(any(User.class))).thenReturn(user);

        mockMvc.perform(post("/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\": \"Charlie\"}"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.id").value(1))
                .andExpect(jsonPath("$.name").value("Charlie"));
    }
}
```

### Exercise 8: Test Controller Exception Handling (@ControllerAdvice)
```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.MockMvc;
import java.util.NoSuchElementException;

import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(UserController.class)
public class UserControllerExceptionTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    public void testGetUserNotFound() throws Exception {
        when(userService.getUserById(99L)).thenThrow(new NoSuchElementException("User not found"));

        mockMvc.perform(get("/users/99"))
                .andExpect(status().isNotFound())
                .andExpect(content().string("User not found"));
    }
}
```

---

## Mockito Mock Dependencies in Spring

### Exercise 1: Mocking Service Dependency in Controller Test
Utilizes `@MockBean` inside `@WebMvcTest` as documented in `UserControllerTest` above.

### Exercise 2: Mocking Repository in Service Test
Utilizes mockito `@Mock` for repository and `@InjectMocks` on the service as documented in `UserServiceTest` above.

### Exercise 3: Mocking Service Dependency in Integration Test
```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringBootTest
@AutoConfigureMockMvc
public class UserIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    public void testGetIntegratedUserWithMockedService() throws Exception {
        User user = new User(1L, "David");
        when(userService.getUserById(1L)).thenReturn(user);

        mockMvc.perform(get("/users/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("David"));
    }
}
```

---

## SLF4J Logging

### Exercise 1 & 2: Error, Warn & Parameterized Logging
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class LoggingExample {
    private static final Logger logger = LoggerFactory.getLogger(LoggingExample.class);

    public static void main(String[] args) {
        logger.error("This is an error message");
        logger.warn("This is a warning message");

        String username = "Alice";
        int id = 101;
        // Parameterized Logging
        logger.debug("Logged in user: {} with ID: {}", username, id);
    }
}
```

### Exercise 3: Using Different Appenders (`logback.xml`)
Configuration file `src/main/resources/logback.xml`:
```xml
<configuration>
    <!-- Console Appender -->
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- File Appender -->
    <appender name="file" class="ch.qos.logback.core.FileAppender">
        <file>app.log</file>
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- Root level logs routing -->
    <root level="debug">
        <appender-ref ref="console" />
        <appender-ref ref="file" />
    </root>
</configuration>
```
