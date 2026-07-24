# Engineering Concepts (Data Structures, Algorithms & Design Patterns) Solutions

This document contains the complete Java source code solutions and theoretical complexity analyses for the exercises in the **Algorithms & Data Structures** and **Design Patterns and Principles** lab manuals.

---

## Part 1: Algorithms & Data Structures

### Exercise 1: Inventory Management System

#### Product Class (`Product.java`)
```java
public class Product {
    private String productId;
    private String productName;
    private int quantity;
    private double price;

    public Product(String productId, String productName, int quantity, double price) {
        this.productId = productId;
        this.productName = productName;
        this.quantity = quantity;
        this.price = price;
    }

    // Getters and Setters
    public String getProductId() { return productId; }
    public void setProductId(String productId) { this.productId = productId; }

    public String getProductName() { return productName; }
    public void setProductName(String productName) { this.productName = productName; }

    public int getQuantity() { return quantity; }
    public void setQuantity(int quantity) { this.quantity = quantity; }

    public double getPrice() { return price; }
    public void setPrice(double price) { this.price = price; }
}
```

#### Inventory Management (`Inventory.java`)
```java
import java.util.HashMap;

public class Inventory {
    // HashMap is chosen because it offers O(1) average time complexity for insertions, deletions, and search lookups.
    private HashMap<String, Product> productMap;

    public Inventory() {
        this.productMap = new HashMap<>();
    }

    // Add Product - O(1) Time Complexity
    public void addProduct(Product product) {
        productMap.put(product.getProductId(), product);
    }

    // Update Product - O(1) Time Complexity
    public void updateProduct(String productId, Product updatedProduct) {
        if (productMap.containsKey(productId)) {
            productMap.put(productId, updatedProduct);
        } else {
            System.out.println("Product not found in inventory.");
        }
    }

    // Delete Product - O(1) Time Complexity
    public void deleteProduct(String productId) {
        productMap.remove(productId);
    }

    // Retrieve Product - O(1) Time Complexity
    public Product getProduct(String productId) {
        return productMap.get(productId);
    }
}
```

#### Complexity Analysis
- **ArrayList vs HashMap**: 
  - In an `ArrayList`, searching for an item takes $O(n)$ time because we must iterate through the list sequentially. Deleting or updating also takes $O(n)$ time due to search lookup.
  - In a `HashMap`, search, insertion, and deletion operations occur in $O(1)$ average time complexity using key hashes.
- **Optimization**: For inventory structures that are dynamically queried by a unique identifier, hash-maps are optimal.

---

### Exercise 2: E-commerce Platform Search Function

#### Product Class (`Product.java`)
```java
public class Product implements Comparable<Product> {
    private String productId;
    private String productName;
    private String category;

    public Product(String productId, String productName, String category) {
        this.productId = productId;
        this.productName = productName;
        this.category = category;
    }

    public String getProductId() { return productId; }
    public String getProductName() { return productName; }
    public String getCategory() { return category; }

    @Override
    public int compareTo(Product other) {
        return this.productId.compareTo(other.productId);
    }
}
```

#### Search Implementation (`SearchAlgorithms.java`)
```java
import java.util.Arrays;

public class SearchAlgorithms {

    // Linear Search - O(n) Time Complexity
    public static Product linearSearch(Product[] products, String targetId) {
        for (Product p : products) {
            if (p.getProductId().equals(targetId)) {
                return p;
            }
        }
        return null;
    }

    // Binary Search - O(log n) Time Complexity (Requires sorted array)
    public static Product binarySearch(Product[] products, String targetId) {
        int low = 0;
        int high = products.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            int comparison = products[mid].getProductId().compareTo(targetId);

            if (comparison == 0) {
                return products[mid];
            } else if (comparison < 0) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return null;
    }
}
```

#### Complexity Analysis
- **Linear Search**: $O(n)$ worst-case time complexity. Suitable for unsorted, small datasets.
- **Binary Search**: $O(\log n)$ worst-case time complexity. Highly efficient for large datasets but requires sorted inputs. Sorting the array initially requires $O(n \log n)$ time.

---

### Exercise 3: Sorting Customer Orders

#### Order Class (`Order.java`)
```java
public class Order {
    private String orderId;
    private String customerName;
    private double totalPrice;

    public Order(String orderId, String customerName, double totalPrice) {
        this.orderId = orderId;
        this.customerName = customerName;
        this.totalPrice = totalPrice;
    }

    public String getOrderId() { return orderId; }
    public String getCustomerName() { return customerName; }
    public double getTotalPrice() { return totalPrice; }
}
```

#### Sorting Implementation (`SortAlgorithms.java`)
```java
public class SortAlgorithms {

    // Bubble Sort - O(n^2) Time Complexity
    public static void bubbleSort(Order[] orders) {
        int n = orders.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (orders[j].getTotalPrice() > orders[j + 1].getTotalPrice()) {
                    // Swap
                    Order temp = orders[j];
                    orders[j] = orders[j + 1];
                    orders[j + 1] = temp;
                }
            }
        }
    }

    // Quick Sort - O(n log n) Average Time Complexity
    public static void quickSort(Order[] orders, int low, int high) {
        if (low < high) {
            int pivotIndex = partition(orders, low, high);
            quickSort(orders, low, pivotIndex - 1);
            quickSort(orders, pivotIndex + 1, high);
        }
    }

    private static int partition(Order[] orders, int low, int high) {
        double pivot = orders[high].getTotalPrice();
        int i = (low - 1);
        for (int j = low; j < high; j++) {
            if (orders[j].getTotalPrice() <= pivot) {
                i++;
                Order temp = orders[i];
                orders[i] = orders[j];
                orders[j] = temp;
            }
        }
        Order temp = orders[i + 1];
        orders[i + 1] = orders[high];
        orders[high] = temp;
        return i + 1;
    }
}
```

#### Complexity Analysis
- **Bubble Sort**: $O(n^2)$ time complexity in average and worst cases. Impractical for production systems.
- **Quick Sort**: $O(n \log n)$ average time complexity. It uses divide-and-conquer partitioning, making it the preferred choice for sorting orders on e-commerce platforms.

---

### Exercise 4: Employee Management System

#### Employee Class (`Employee.java`)
```java
public class Employee {
    private String employeeId;
    private String name;
    private String position;
    private double salary;

    public Employee(String employeeId, String name, String position, double salary) {
        this.employeeId = employeeId;
        this.name = name;
        this.position = position;
        this.salary = salary;
    }

    public String getEmployeeId() { return employeeId; }
    public String getName() { return name; }
    public String getPosition() { return position; }
    public double getSalary() { return salary; }
}
```

#### Employee Array Management (`EmployeeManager.java`)
```java
public class EmployeeManager {
    private Employee[] employees;
    private int size;
    private int capacity;

    public EmployeeManager(int capacity) {
        this.capacity = capacity;
        this.employees = new Employee[capacity];
        this.size = 0;
    }

    // Add Employee - O(1) if capacity exists
    public boolean addEmployee(Employee emp) {
        if (size < capacity) {
            employees[size] = emp;
            size++;
            return true;
        }
        System.out.println("Array capacity full.");
        return false;
    }

    // Search Employee - O(n)
    public Employee searchEmployee(String empId) {
        for (int i = 0; i < size; i++) {
            if (employees[i].getEmployeeId().equals(empId)) {
                return employees[i];
            }
        }
        return null;
    }

    // Traverse Employees - O(n)
    public void traverseEmployees() {
        for (int i = 0; i < size; i++) {
            System.out.println(employees[i].getName() + " - " + employees[i].getPosition());
        }
    }

    // Delete Employee - O(n) due to search & element shifting
    public boolean deleteEmployee(String empId) {
        int index = -1;
        for (int i = 0; i < size; i++) {
            if (employees[i].getEmployeeId().equals(empId)) {
                index = i;
                break;
            }
        }
        if (index != -1) {
            for (int i = index; i < size - 1; i++) {
                employees[i] = employees[i + 1];
            }
            employees[size - 1] = null;
            size--;
            return true;
        }
        return false;
    }
}
```

---

### Exercise 5: Task Management System

#### Task Class (`Task.java`)
```java
public class Task {
    private String taskId;
    private String taskName;
    private String status;

    public Task(String taskId, String taskName, String status) {
        this.taskId = taskId;
        this.taskName = taskName;
        this.status = status;
    }

    public String getTaskId() { return taskId; }
    public String getTaskName() { return taskName; }
    public String getStatus() { return status; }
}
```

#### Task Singly Linked List (`TaskList.java`)
```java
public class TaskList {
    private class Node {
        Task task;
        Node next;
        Node(Task task) { this.task = task; this.next = null; }
    }

    private Node head = null;

    // Add Task (At Head) - O(1)
    public void addTask(Task task) {
        Node newNode = new Node(task);
        newNode.next = head;
        head = newNode;
    }

    // Search Task - O(n)
    public Task searchTask(String taskId) {
        Node current = head;
        while (current != null) {
            if (current.task.getTaskId().equals(taskId)) {
                return current.task;
            }
            current = current.next;
        }
        return null;
    }

    // Traverse Tasks - O(n)
    public void traverseTasks() {
        Node current = head;
        while (current != null) {
            System.out.println(current.task.getTaskName() + " [" + current.task.getStatus() + "]");
            current = current.next;
        }
    }

    // Delete Task - O(n)
    public boolean deleteTask(String taskId) {
        if (head == null) return false;
        
        if (head.task.getTaskId().equals(taskId)) {
            head = head.next;
            return true;
        }

        Node current = head;
        while (current.next != null) {
            if (current.next.task.getTaskId().equals(taskId)) {
                current.next = current.next.next;
                return true;
            }
            current = current.next;
        }
        return false;
    }
}
```

---

### Exercise 6: Library Management System

#### Book Class (`Book.java`)
```java
public class Book {
    private String bookId;
    private String title;
    private String author;

    public Book(String bookId, String title, String author) {
        this.bookId = bookId;
        this.title = title;
        this.author = author;
    }

    public String getBookId() { return bookId; }
    public String getTitle() { return title; }
    public String getAuthor() { return author; }
}
```

#### Library Search (`Library.java`)
```java
public class Library {

    // Linear Search by Title - O(n)
    public static Book linearSearchByTitle(Book[] books, String title) {
        for (Book b : books) {
            if (b.getTitle().equalsIgnoreCase(title)) {
                return b;
            }
        }
        return null;
    }

    // Binary Search by Title - O(log n) (Assumes list is sorted by title)
    public static Book binarySearchByTitle(Book[] books, String title) {
        int low = 0;
        int high = books.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            int comp = books[mid].getTitle().compareToIgnoreCase(title);

            if (comp == 0) {
                return books[mid];
            } else if (comp < 0) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return null;
    }
}
```

---

### Exercise 7: Financial Forecasting

#### Recursive Forecast (`FinancialForecasting.java`)
```java
public class FinancialForecasting {

    // Recursive formula: Value(n) = Value(n-1) * (1 + growthRate)
    // Time Complexity: O(n)
    // Space Complexity: O(n) due to call stack recursion depth
    public static double calculateFutureValue(double currentValue, double growthRate, int years) {
        if (years == 0) {
            return currentValue;
        }
        return calculateFutureValue(currentValue * (1 + growthRate), growthRate, years - 1);
    }

    // Optimized Iterative Approach to avoid stack overflow (O(n) time, O(1) space)
    public static double calculateFutureValueIterative(double currentValue, double growthRate, int years) {
        double value = currentValue;
        for (int i = 0; i < years; i++) {
            value *= (1 + growthRate);
        }
        return value;
    }
}
```

---
---

## Part 2: Design Patterns and Principles

### Exercise 1: Singleton Pattern

Ensure a class has only one instance.

```java
public class Logger {
    // Private static instance
    private static Logger instance;

    // Private constructor prevents external instantiation
    private Logger() {}

    // Thread-safe double-checked locking getInstance
    public static Logger getInstance() {
        if (instance == null) {
            synchronized (Logger.class) {
                if (instance == null) {
                    instance = new Logger();
                }
            }
        }
        return instance;
    }

    public void log(String message) {
        System.out.println("[LOG]: " + message);
    }
}
```

---

### Exercise 2: Factory Method Pattern

Decouple document creation logic.

```java
// Interface Product
interface Document {
    void open();
}

// Concrete Products
class PdfDocument implements Document {
    public void open() { System.out.println("Opening PDF Document."); }
}
class WordDocument implements Document {
    public void open() { System.out.println("Opening Word Document."); }
}

// Creator Abstract Class
abstract class DocumentFactory {
    public abstract Document createDocument();
}

// Concrete Creators
class PdfDocumentFactory extends DocumentFactory {
    public Document createDocument() { return new PdfDocument(); }
}
class WordDocumentFactory extends DocumentFactory {
    public Document createDocument() { return new WordDocument(); }
}
```

---

### Exercise 3: Builder Pattern

Construct complex objects step-by-step.

```java
public class Computer {
    // Required attributes
    private String HDD;
    private String RAM;
    
    // Optional attributes
    private boolean isGraphicsCardEnabled;
    private boolean isBluetoothEnabled;

    private Computer(Builder builder) {
        this.HDD = builder.HDD;
        this.RAM = builder.RAM;
        this.isGraphicsCardEnabled = builder.isGraphicsCardEnabled;
        this.isBluetoothEnabled = builder.isBluetoothEnabled;
    }

    public static class Builder {
        private String HDD;
        private String RAM;
        private boolean isGraphicsCardEnabled;
        private boolean isBluetoothEnabled;

        public Builder(String hdd, String ram) {
            this.HDD = hdd;
            this.RAM = ram;
        }

        public Builder setGraphicsCardEnabled(boolean enabled) {
            this.isGraphicsCardEnabled = enabled;
            return this;
        }

        public Builder setBluetoothEnabled(boolean enabled) {
            this.isBluetoothEnabled = enabled;
            return this;
        }

        public Computer build() {
            return new Computer(this);
        }
    }
}
```

---

### Exercise 4: Adapter Pattern

Wrap incompatible interfaces.

```java
// Target Interface
interface PaymentProcessor {
    void processPayment(double amount);
}

// Adaptee 1
class PayPalGateway {
    public void makePayment(double amount) { System.out.println("PayPal payment of $" + amount); }
}

// Adapter 1
class PayPalAdapter implements PaymentProcessor {
    private PayPalGateway gateway;
    public PayPalAdapter(PayPalGateway g) { this.gateway = g; }
    public void processPayment(double amount) { gateway.makePayment(amount); }
}
```

---

### Exercise 5: Decorator Pattern

Attach responsibilities to objects dynamically.

```java
// Component Interface
interface Notifier {
    void send(String message);
}

// Concrete Component
class EmailNotifier implements Notifier {
    public void send(String msg) { System.out.println("Sending Email: " + msg); }
}

// Decorator Base
abstract class NotifierDecorator implements Notifier {
    protected Notifier decoratedNotifier;
    public NotifierDecorator(Notifier n) { this.decoratedNotifier = n; }
    public void send(String msg) { decoratedNotifier.send(msg); }
}

// Concrete Decorator
class SMSDecorator extends NotifierDecorator {
    public SMSDecorator(Notifier n) { super(n); }
    public void send(String msg) {
        super.send(msg);
        System.out.println("Sending SMS: " + msg);
    }
}
```

---

### Exercise 6: Proxy Pattern

Lazy loading and cache wrapper.

```java
interface Image {
    void display();
}

class RealImage implements Image {
    private String filename;
    public RealImage(String f) {
        this.filename = f;
        loadFromRemoteServer();
    }
    private void loadFromRemoteServer() { System.out.println("Loading " + filename + " from remote..."); }
    public void display() { System.out.println("Displaying " + filename); }
}

class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;

    public ProxyImage(String filename) { this.filename = filename; }

    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename); // Lazy instantiation
        }
        realImage.display();
    }
}
```

---

### Exercise 7: Observer Pattern

One-to-many dependency notification.

```java
import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update(double price);
}

interface Subject {
    void register(Observer o);
    void deregister(Observer o);
    void notifyObservers();
}

class StockMarket implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private double price;

    public void setPrice(double p) { this.price = p; notifyObservers(); }
    public void register(Observer o) { observers.add(o); }
    public void deregister(Observer o) { observers.remove(o); }
    public void notifyObservers() {
        for (Observer o : observers) { o.update(price); }
    }
}

class MobileApp implements Observer {
    public void update(double price) { System.out.println("Mobile App got updated price: $" + price); }
}
```

---

### Exercise 8: Strategy Pattern

Define families of algorithms, encapsulating each.

```java
interface PaymentStrategy {
    void pay(double amount);
}

class CreditCardPayment implements PaymentStrategy {
    public void pay(double amt) { System.out.println("Paid $" + amt + " using Credit Card."); }
}
class PayPalPayment implements PaymentStrategy {
    public void pay(double amt) { System.out.println("Paid $" + amt + " using PayPal."); }
}

class PaymentContext {
    private PaymentStrategy strategy;
    public void setPaymentStrategy(PaymentStrategy s) { this.strategy = s; }
    public void executePayment(double amt) { strategy.pay(amt); }
}
```

---

### Exercise 9: Command Pattern

Encapsulate requests as objects.

```java
// Command Interface
interface Command {
    void execute();
}

// Receiver
class Light {
    public void turnOn() { System.out.println("Light is ON"); }
    public void turnOff() { System.out.println("Light is OFF"); }
}

// Concrete Commands
class LightOnCommand implements Command {
    private Light light;
    public LightOnCommand(Light l) { this.light = l; }
    public void execute() { light.turnOn(); }
}
class LightOffCommand implements Command {
    private Light light;
    public LightOffCommand(Light l) { this.light = l; }
    public void execute() { light.turnOff(); }
}

// Invoker
class RemoteControl {
    private Command slot;
    public void setCommand(Command c) { this.slot = c; }
    public void pressButton() { slot.execute(); }
}
```

---

### Exercise 10: MVC Pattern

Separate Model, View, and Controller.

```java
// Model
class Student {
    private String name;
    private String id;
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
}

// View
class StudentView {
    public void displayStudentDetails(String name, String id) {
        System.out.println("Student Details: " + name + " (ID: " + id + ")");
    }
}

// Controller
class StudentController {
    private Student model;
    private StudentView view;

    public StudentController(Student m, StudentView v) { this.model = m; this.view = v; }
    public void setStudentName(String n) { model.setName(n); }
    public void updateView() { view.displayStudentDetails(model.getName(), model.getId()); }
}
```

---

### Exercise 11: Dependency Injection

Constructor injection patterns.

```java
interface CustomerRepository {
    String findCustomerById(String id);
}

class CustomerRepositoryImpl implements CustomerRepository {
    public String findCustomerById(String id) { return "Customer Account details for " + id; }
}

class CustomerService {
    private CustomerRepository repository;

    // Constructor injection
    public CustomerService(CustomerRepository r) { this.repository = r; }

    public String fetchCustomer(String id) {
        return repository.findCustomerById(id);
    }
}
```
