# OOPS Principles in Java - Complete Guide with Code Examples

## Table of Contents
1. [Introduction to OOPS](#introduction-to-oops)
2. [Encapsulation](#encapsulation)
3. [Inheritance](#inheritance)
4. [Polymorphism](#polymorphism)
5. [Abstraction](#abstraction)
6. [Real-World Examples](#real-world-examples)
7. [Best Practices](#best-practices)

---

## Introduction to OOPS

**OOPS (Object-Oriented Programming System)** is a programming paradigm based on the concept of "objects" and "classes". It's one of the most popular and powerful approaches to software development.

### Four Core Principles:
1. **Encapsulation** - Data Hiding
2. **Inheritance** - Code Reusability
3. **Polymorphism** - Multiple Forms
4. **Abstraction** - Hiding Complexity

---

## 1. Encapsulation

### Definition
Encapsulation is the mechanism of bundling data (variables) and methods (functions) together within a single unit (class), and hiding the internal implementation details from the outside world.

### Key Concepts:
- **Private Variables**: Hide data from external access
- **Public Methods**: Provide controlled access through getter and setter methods
- **Data Protection**: Validate data before modifying it

### Example 1: Basic Encapsulation

```java
public class Student {
    // Private variables (data hiding)
    private String name;
    private int age;
    private double gpa;
    
    // Constructor
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
    
    // Getter methods
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public double getGpa() {
        return gpa;
    }
    
    // Setter methods with validation
    public void setName(String name) {
        if (name != null && !name.isEmpty()) {
            this.name = name;
        } else {
            System.out.println("Invalid name!");
        }
    }
    
    public void setAge(int age) {
        if (age > 0 && age < 100) {
            this.age = age;
        } else {
            System.out.println("Invalid age!");
        }
    }
    
    public void setGpa(double gpa) {
        if (gpa >= 0.0 && gpa <= 4.0) {
            this.gpa = gpa;
        } else {
            System.out.println("GPA must be between 0.0 and 4.0");
        }
    }
    
    // Display method
    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age + ", GPA: " + gpa);
    }
}

// Usage Example
public class EncapsulationDemo {
    public static void main(String[] args) {
        Student student = new Student("Yash Automation", 25, 3.8);
        student.displayInfo(); // Name: Yash Automation, Age: 25, GPA: 3.8
        
        // Using setters to modify data
        student.setName("Yash Singh");
        student.setGpa(3.9);
        student.displayInfo(); // Name: Yash Singh, Age: 25, GPA: 3.9
        
        // Invalid data is rejected
        student.setAge(-5); // Invalid age!
        student.setGpa(5.0); // GPA must be between 0.0 and 4.0
    }
}
```

### Benefits of Encapsulation:
✅ **Data Protection** - Private variables prevent unauthorized access  
✅ **Validation** - Setters can validate data before storing  
✅ **Flexibility** - Internal implementation can change without affecting external code  
✅ **Maintainability** - Easier to modify internal logic  

---

## 2. Inheritance

### Definition
Inheritance is the mechanism by which one class (child/subclass) acquires the properties and methods of another class (parent/superclass). It promotes code reusability and establishes an IS-A relationship.

### Types of Inheritance in Java:
1. **Single Inheritance** - One child, one parent
2. **Multilevel Inheritance** - Chain of inheritance (A → B → C)
3. **Hierarchical Inheritance** - Multiple children, one parent
4. **Interface-Based Inheritance** - Implementation of multiple interfaces

### Example 1: Single Inheritance

```java
// Parent Class
public class Vehicle {
    protected String brand;
    protected String color;
    protected int year;
    
    public Vehicle(String brand, String color, int year) {
        this.brand = brand;
        this.color = color;
        this.year = year;
    }
    
    public void displayInfo() {
        System.out.println("Brand: " + brand + ", Color: " + color + ", Year: " + year);
    }
    
    public void start() {
        System.out.println("Vehicle is starting...");
    }
    
    public void stop() {
        System.out.println("Vehicle is stopping...");
    }
}

// Child Class
public class Car extends Vehicle {
    private int numberOfDoors;
    
    public Car(String brand, String color, int year, int numberOfDoors) {
        super(brand, color, year); // Call parent constructor
        this.numberOfDoors = numberOfDoors;
    }
    
    // Override parent method
    @Override
    public void displayInfo() {
        super.displayInfo(); // Call parent method
        System.out.println("Number of Doors: " + numberOfDoors);
    }
    
    // Car-specific method
    public void openTrunk() {
        System.out.println("Trunk is now open!");
    }
}

// Usage Example
public class InheritanceDemo {
    public static void main(String[] args) {
        Car car = new Car("Toyota", "Red", 2024, 4);
        car.displayInfo();
        // Brand: Toyota, Color: Red, Year: 2024
        // Number of Doors: 4
        
        car.start();   // Vehicle is starting...
        car.openTrunk(); // Trunk is now open!
        car.stop();    // Vehicle is stopping...
    }
}
```

### Example 2: Multilevel Inheritance

```java
// Level 1: Parent Class
public class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public void eat() {
        System.out.println(name + " is eating...");
    }
}

// Level 2: Intermediate Class (extends Animal)
public class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }
    
    public void bark() {
        System.out.println(name + " is barking: Woof! Woof!");
    }
}

// Level 3: Child Class (extends Dog)
public class GoldenRetriever extends Dog {
    private String breedType;
    
    public GoldenRetriever(String name, String breedType) {
        super(name);
        this.breedType = breedType;
    }
    
    public void fetch() {
        System.out.println(name + " (Golden Retriever - " + breedType + ") is fetching...");
    }
}

// Usage Example
public class MultilevelInheritanceDemo {
    public static void main(String[] args) {
        GoldenRetriever dog = new GoldenRetriever("Buddy", "Standard");
        dog.eat();   // Buddy is eating...
        dog.bark();  // Buddy is barking: Woof! Woof!
        dog.fetch(); // Buddy (Golden Retriever - Standard) is fetching...
    }
}
```

### Benefits of Inheritance:
✅ **Code Reusability** - Write once, use in multiple places  
✅ **Logical Hierarchy** - Reflects real-world relationships  
✅ **Polymorphism** - Enables dynamic method dispatch  
✅ **Maintainability** - Changes in parent affect all children  

---

## 3. Polymorphism

### Definition
Polymorphism means "many forms". It allows objects of different types to be treated through the same interface. There are two types: **Compile-time (Method Overloading)** and **Runtime (Method Overriding)**.

### Type 1: Compile-Time Polymorphism (Method Overloading)

```java
public class Calculator {
    // Method overloading: same name, different parameters
    
    // Overload 1: Add two integers
    public int add(int a, int b) {
        System.out.println("Adding two integers");
        return a + b;
    }
    
    // Overload 2: Add three integers
    public int add(int a, int b, int c) {
        System.out.println("Adding three integers");
        return a + b + c;
    }
    
    // Overload 3: Add two doubles
    public double add(double a, double b) {
        System.out.println("Adding two doubles");
        return a + b;
    }
    
    // Overload 4: Add String and int
    public String add(String a, int b) {
        System.out.println("Adding String and integer");
        return a + b;
    }
}

// Usage Example
public class MethodOverloadingDemo {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        System.out.println(calc.add(5, 10));           // 15
        System.out.println(calc.add(5, 10, 15));       // 30
        System.out.println(calc.add(5.5, 10.5));       // 16.0
        System.out.println(calc.add("Result: ", 100)); // Result: 100
    }
}
```

### Type 2: Runtime Polymorphism (Method Overriding)

```java
// Parent Class
public class Shape {
    public void draw() {
        System.out.println("Drawing a shape...");
    }
    
    public double calculateArea() {
        return 0;
    }
}

// Child Class 1
public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a circle with radius: " + radius);
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

// Child Class 2
public class Rectangle extends Shape {
    private double length;
    private double width;
    
    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle with length: " + length + ", width: " + width);
    }
    
    @Override
    public double calculateArea() {
        return length * width;
    }
}

// Usage Example
public class MethodOverridingDemo {
    public static void main(String[] args) {
        // Runtime polymorphism: same reference type, different object types
        Shape shape1 = new Circle(5);
        Shape shape2 = new Rectangle(4, 6);
        
        // Calls appropriate method based on object type (not reference type)
        shape1.draw();              // Drawing a circle with radius: 5
        System.out.println("Area: " + shape1.calculateArea()); // Area: 78.53981633974483
        
        shape2.draw();              // Drawing a rectangle with length: 4, width: 6
        System.out.println("Area: " + shape2.calculateArea()); // Area: 24.0
    }
}
```

### Benefits of Polymorphism:
✅ **Flexibility** - Write generic code that works with different types  
✅ **Extensibility** - Add new types without changing existing code  
✅ **Code Reusability** - Use same interface for different implementations  
✅ **Loose Coupling** - Reduces dependencies between classes  

---

## 4. Abstraction

### Definition
Abstraction is the concept of hiding complex implementation details and showing only the essential features to the user. It reduces complexity and improves code readability.

### Methods of Abstraction:
1. **Abstract Classes** - Cannot be instantiated, may have abstract and concrete methods
2. **Interfaces** - Contracts that classes must implement (Java 8+: can have default methods)

### Example 1: Abstract Class

```java
// Abstract Class
public abstract class BankAccount {
    protected String accountNumber;
    protected double balance;
    protected String accountHolder;
    
    public BankAccount(String accountNumber, String accountHolder, double initialBalance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }
    
    // Abstract methods (no implementation)
    public abstract void deposit(double amount);
    public abstract void withdraw(double amount);
    public abstract void calculateInterest();
    
    // Concrete method
    public void displayBalance() {
        System.out.println("Account Holder: " + accountHolder);
        System.out.println("Account Number: " + accountNumber);
        System.out.println("Balance: $" + balance);
    }
}

// Concrete Class 1
public class SavingsAccount extends BankAccount {
    private double interestRate;
    
    public SavingsAccount(String accountNumber, String accountHolder, 
                         double initialBalance, double interestRate) {
        super(accountNumber, accountHolder, initialBalance);
        this.interestRate = interestRate;
    }
    
    @Override
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: $" + amount + " to Savings Account");
        }
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount + " from Savings Account");
        } else {
            System.out.println("Insufficient balance!");
        }
    }
    
    @Override
    public void calculateInterest() {
        double interest = balance * interestRate / 100;
        balance += interest;
        System.out.println("Interest calculated: $" + interest);
    }
}

// Concrete Class 2
public class CheckingAccount extends BankAccount {
    private double overdraftLimit;
    
    public CheckingAccount(String accountNumber, String accountHolder, 
                          double initialBalance, double overdraftLimit) {
        super(accountNumber, accountHolder, initialBalance);
        this.overdraftLimit = overdraftLimit;
    }
    
    @Override
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: $" + amount + " to Checking Account");
        }
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && amount <= (balance + overdraftLimit)) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount + " from Checking Account");
        } else {
            System.out.println("Cannot withdraw! Exceeds limit!");
        }
    }
    
    @Override
    public void calculateInterest() {
        // Checking accounts typically don't earn interest
        System.out.println("Checking account does not earn interest.");
    }
}

// Usage Example
public class AbstractionDemo {
    public static void main(String[] args) {
        // Cannot instantiate abstract class
        // BankAccount account = new BankAccount(...); // ERROR!
        
        BankAccount savingsAccount = new SavingsAccount("SA001", "Yash Singh", 1000, 5.0);
        savingsAccount.displayBalance();    // Display balance
        savingsAccount.deposit(500);        // Deposit: $500 to Savings Account
        savingsAccount.withdraw(200);       // Withdrawn: $200 from Savings Account
        savingsAccount.calculateInterest(); // Interest calculated: $40.0
        savingsAccount.displayBalance();    // Updated balance
        
        System.out.println("\n--- Checking Account ---\n");
        
        BankAccount checkingAccount = new CheckingAccount("CA001", "Alice Johnson", 500, 100);
        checkingAccount.displayBalance();    // Display balance
        checkingAccount.deposit(300);        // Deposit: $300 to Checking Account
        checkingAccount.withdraw(600);       // Withdrawn: $600 from Checking Account
        checkingAccount.calculateInterest(); // Checking account does not earn interest.
    }
}
```

### Example 2: Interface

```java
// Interface
public interface PaymentGateway {
    void processPayment(double amount);
    void refund(double amount);
    String getPaymentStatus();
}

// Implementation 1
public class PayPalGateway implements PaymentGateway {
    private String transactionId;
    private boolean paymentProcessed;
    
    @Override
    public void processPayment(double amount) {
        transactionId = "PP_" + System.currentTimeMillis();
        paymentProcessed = true;
        System.out.println("Processing $" + amount + " via PayPal");
        System.out.println("Transaction ID: " + transactionId);
    }
    
    @Override
    public void refund(double amount) {
        if (paymentProcessed) {
            System.out.println("Refunding $" + amount + " via PayPal");
            paymentProcessed = false;
        }
    }
    
    @Override
    public String getPaymentStatus() {
        return paymentProcessed ? "COMPLETED" : "PENDING";
    }
}

// Implementation 2
public class StripeGateway implements PaymentGateway {
    private String chargeId;
    private boolean paymentProcessed;
    
    @Override
    public void processPayment(double amount) {
        chargeId = "ch_" + System.currentTimeMillis();
        paymentProcessed = true;
        System.out.println("Processing $" + amount + " via Stripe");
        System.out.println("Charge ID: " + chargeId);
    }
    
    @Override
    public void refund(double amount) {
        if (paymentProcessed) {
            System.out.println("Refunding $" + amount + " via Stripe");
            paymentProcessed = false;
        }
    }
    
    @Override
    public String getPaymentStatus() {
        return paymentProcessed ? "SUCCEEDED" : "PENDING";
    }
}

// Usage Example
public class InterfaceDemo {
    public static void main(String[] args) {
        PaymentGateway paypal = new PayPalGateway();
        PaymentGateway stripe = new StripeGateway();
        
        // Process payment with PayPal
        paypal.processPayment(99.99);
        System.out.println("Status: " + paypal.getPaymentStatus());
        paypal.refund(99.99);
        
        System.out.println("\n--- Stripe Payment ---\n");
        
        // Process payment with Stripe
        stripe.processPayment(149.99);
        System.out.println("Status: " + stripe.getPaymentStatus());
    }
}
```

### Benefits of Abstraction:
✅ **Complexity Hiding** - Users don't need to know implementation details  
✅ **Security** - Sensitive data and operations are hidden  
✅ **Flexibility** - Change implementation without affecting users  
✅ **Modularity** - Code is organized into logical units  

---

## 5. Real-World Examples

### Example 1: E-Commerce System

```java
// Abstract class for all products
public abstract class Product {
    protected String productId;
    protected String productName;
    protected double price;
    protected int quantity;
    
    public Product(String productId, String productName, double price, int quantity) {
        this.productId = productId;
        this.productName = productName;
        this.price = price;
        this.quantity = quantity;
    }
    
    public abstract double getDiscount();
    public abstract void updateStock(int quantity);
    
    public void displayInfo() {
        System.out.println("Product ID: " + productId);
        System.out.println("Product Name: " + productName);
        System.out.println("Price: $" + price);
        System.out.println("In Stock: " + quantity);
    }
}

// Electronic Product
public class Electronics extends Product {
    private int warranty; // in months
    
    public Electronics(String productId, String productName, double price, 
                      int quantity, int warranty) {
        super(productId, productName, price, quantity);
        this.warranty = warranty;
    }
    
    @Override
    public double getDiscount() {
        return price * 0.15; // 15% discount
    }
    
    @Override
    public void updateStock(int quantity) {
        this.quantity += quantity;
        System.out.println("Stock updated for Electronics: " + this.quantity);
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Warranty: " + warranty + " months");
    }
}

// Clothing Product
public class Clothing extends Product {
    private String size;
    private String color;
    
    public Clothing(String productId, String productName, double price, 
                   int quantity, String size, String color) {
        super(productId, productName, price, quantity);
        this.size = size;
        this.color = color;
    }
    
    @Override
    public double getDiscount() {
        return price * 0.20; // 20% discount
    }
    
    @Override
    public void updateStock(int quantity) {
        this.quantity += quantity;
        System.out.println("Stock updated for Clothing: " + this.quantity);
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Size: " + size + ", Color: " + color);
    }
}

// Order class (using polymorphism)
public class Order {
    private List<Product> products;
    
    public Order() {
        this.products = new ArrayList<>();
    }
    
    public void addProduct(Product product) {
        products.add(product);
    }
    
    public double calculateTotal() {
        double total = 0;
        for (Product product : products) {
            total += (product.price * product.quantity) - product.getDiscount();
        }
        return total;
    }
    
    public void displayOrderDetails() {
        System.out.println("=== ORDER DETAILS ===");
        for (Product product : products) {
            product.displayInfo();
            System.out.println("Discount: $" + product.getDiscount());
            System.out.println("---");
        }
        System.out.println("Total Amount: $" + calculateTotal());
    }
}

// Usage
public class ECommerceDemo {
    public static void main(String[] args) {
        Order order = new Order();
        
        // Add products
        Product laptop = new Electronics("ELEC001", "Laptop", 999.99, 1, 24);
        Product tshirt = new Clothing("CLOTH001", "T-Shirt", 29.99, 3, "M", "Blue");
        
        order.addProduct(laptop);
        order.addProduct(tshirt);
        
        order.displayOrderDetails();
    }
}
```

### Example 2: Banking System

```java
public interface BankOperations {
    void deposit(double amount);
    void withdraw(double amount);
    double getBalance();
}

public class BankAccount implements BankOperations {
    private String accountNumber;
    private String accountHolder;
    private double balance;
    
    public BankAccount(String accountNumber, String accountHolder, double initialBalance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }
    
    @Override
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposit successful! New balance: $" + balance);
        }
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawal successful! New balance: $" + balance);
        } else {
            System.out.println("Insufficient balance!");
        }
    }
    
    @Override
    public double getBalance() {
        return balance;
    }
    
    public String getAccountInfo() {
        return "Account: " + accountNumber + " | Holder: " + accountHolder + " | Balance: $" + balance;
    }
}
```

---

## 6. Best Practices

### ✅ Do's:
1. **Use Access Modifiers** - Use `private` for data, `public` for interfaces
2. **Follow Single Responsibility** - Each class should have one reason to change
3. **Favor Composition over Inheritance** - Use "HAS-A" instead of "IS-A" when appropriate
4. **Use Interfaces for Contracts** - Define clear contracts through interfaces
5. **Implement Meaningful Names** - Use clear, descriptive class and method names
6. **Override equals() and hashCode()** - For proper object comparison
7. **Document Your Code** - Use Javadoc for public APIs

### ❌ Don'ts:
1. **Avoid Deep Inheritance Hierarchies** - Limit to 3-4 levels
2. **Don't Expose Internal Details** - Keep implementation private
3. **Avoid God Classes** - Classes that do too much
4. **Don't Ignore Exception Handling** - Always handle exceptions
5. **Avoid Tight Coupling** - Minimize dependencies between classes
6. **Don't Violate Liskov Substitution Principle** - Child classes should be substitutable for parents
7. **Avoid Over-Abstraction** - Don't create unnecessary abstract classes

---

## Summary Table

| Principle | Purpose | Keyword | Example |
|-----------|---------|---------|---------|
| **Encapsulation** | Data hiding & protection | `private`, `getter`, `setter` | Bank account balance |
| **Inheritance** | Code reusability | `extends` | Car extends Vehicle |
| **Polymorphism** | Multiple forms | `@Override`, `@Overload` | Shape.draw() |
| **Abstraction** | Hide complexity | `abstract`, `interface` | PaymentGateway interface |

---

## Conclusion

Mastering OOPS principles in Java is essential for writing maintainable, scalable, and professional code. These principles help you:

✅ Write cleaner, more organized code  
✅ Build scalable applications  
✅ Reduce code duplication  
✅ Improve code maintainability  
✅ Implement design patterns effectively  

Practice these concepts regularly, and you'll become an expert in object-oriented design! 🎯
