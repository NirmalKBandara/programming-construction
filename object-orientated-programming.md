---
# Object-Oriented Programming Guide
---
## Basic Concepts

**Object** → A real-world entity with states and behaviors.

**Class** → A blueprint for creating objects.

- An object is an instance of a class
- Procedural programming is a type of computer programming that follows the top-down approach

### Basic Class Example

```java
class Car {
    // Attributes of the class (usually private)
    String brand;
    int speed;
    
    // Constructor -> used to create the object
    Car(String brand, int speed) {
        this.brand = brand;
        this.speed = speed;
    }
    
    // Methods of the class
    void displayInfo() {
        System.out.println(brand + " car is driving at " + speed + "km/h");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating a 'new' Car instance (object)
        Car myCar = new Car("Audi", 180);
        // A method that belongs to the car object
        myCar.displayInfo();
    }
}
```

## Object Structure

An object has:
- **State** → Attributes (data and stored variables)
  - Complex attributes can contain collections and/or references
- **Behaviors** → Methods (what they can do)
  - These can also change the object's state

### Important Keywords and Concepts

- **'this' keyword** refers to the current instance of the class
- **Instance variables** → Stored in Heap memory (Dynamic memory allocation in Java)
- **Static members** → Belong to the class (shared among all objects)
  - Therefore, objects shouldn't call static methods; they should be called by the class itself
  - Example: `Math.sqrt(25)` instead of `myObject.sqrt(25)`

**Note:** 'this' and 'super' keywords cannot be used inside a static method.

## The Four Pillars of OOP

### 1. Encapsulation
Hiding data and allowing controlled access using getters and setters.

```java
public class BankAccount {
    private double balance;        // Hidden from outside access
    private String accountNumber;  // Protected internal data
    private String pin;           // Secure and private
    
    // Getter methods
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public double getBalance() {
        return balance;
    }
    
    // Setter methods with validation
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
}
```

**Benefits:**
- Changing the class internals doesn't affect code outside the class
- Changing method implementation doesn't affect the client
- Reduces complexity → Easier maintenance
- Fields can be made read-only or write-only

### 2. Inheritance
Acquiring properties from a super class (Parent → Child).

This allows child classes to inherit the characteristics of the parent class (aka Super class). Child classes can extend the parent class or implement parent interfaces.

**Use this to build "IS-A" relationships. Don't use it to build "HAS-A" relationships.**

#### Interface Implementation
```java
interface Animal {
    void alive();
}

class Cat implements Animal {
    public void alive() {
        System.out.println("Alive");
    }
}
// Overriding or method implementing is a must
```

#### Class Extension
```java
class Animal {
    public void alive() {
        System.out.println("Yes");
    }
}

class Cat extends Animal {
    public void alive() {
        System.out.println("Alive");
    }
}
// Overriding or method implementing is not necessary
```

**Inheritance Rules:**
- Subclass (child class) inherits all public and protected members of its parent (not private ones)
- Inherited methods can be used directly
- Can override methods in the super class

#### Method Hiding vs Method Overriding

| Aspect | Method Hiding (Static) | Method Overriding (Instance) |
|--------|------------------------|------------------------------|
| When | Compile-time | Runtime |
| Based on | Reference type | Actual object type |
| Keyword | No special keyword | @Override |
| Polymorphism | No | Yes |
| Access to parent | ParentClass.method() | super.method() |

**Note:** Static methods are resolved at compile time based on the reference type, not the actual object type. If we call a static method through an object, it will call the parent class static method, not the child class static method.

#### Constructor Invocation

**Implicit Constructor Invocation:**
```java
class Animal {
    protected String species;
    protected int age;
    
    // No-argument constructor
    public Animal() {
        this.species = "Unknown";
        this.age = 0;
        System.out.println("Animal constructor called");
    }
}

class Dog extends Animal {
    private String breed;
    
    public Dog(String breed) {
        // Java implicitly calls Animal() here - you don't see it
        // super(); ← This happens automatically
        
        this.breed = breed;
        System.out.println("Dog constructor called");
    }
}
```

**Explicit Constructor Invocation:**
```java
class Animal {
    protected String species;
    protected int age;
    
    // No-argument constructor
    public Animal() {
        this.species = "Unknown";
        this.age = 0;
        System.out.println("Animal default constructor");
    }
    
    // Parameterized constructor
    public Animal(String species, int age) {
        this.species = species;
        this.age = age;
        System.out.println("Animal constructor: " + species + ", age " + age);
    }
}

class Dog extends Animal {
    private String breed;
    
    public Dog(String breed) {
        super();  // Explicit call to Animal()
        this.breed = breed;
        System.out.println("Dog constructor: " + breed);
    }
    
    public Dog(String species, int age, String breed) {
        super(species, age);  // Explicit call to Animal(String, int)
        this.breed = breed;
        System.out.println("Dog constructor: " + breed);
    }
}
```

### Interface vs Class
**Interface:** A blueprint for classes
- Defines methods that a class must have but doesn't provide implementation
- A class can only extend one class at once but can implement multiple interfaces at once

### 3. Abstraction
The concept of hiding the implementation details of a class and showing only necessary functionality.

**Example:** TV remote - user doesn't know what's happening inside but only knows how to use it.

We know what an object does and how to use it, but don't know how it does what it does.

**Benefits:**
- Manages complexity
- Manages change
- Enables code reuse

**Abstract Classes:**
- Super class defines common attributes and behaviors that multiple child classes share
- Super class can't be instantiated on its own, so it serves as a blueprint for subclasses
- Shows what must be implemented but not how - this is done using abstract methods

```java
abstract class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    // Abstract method - must be implemented by subclasses
    public abstract double calculateArea();
    
    // Concrete method - can be used by all subclasses
    public void displayColor() {
        System.out.println("Color: " + color);
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width, height;
    
    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
}
```

**Marker Interfaces:** Interfaces with no abstract method definitions.
- Empty interface used to indicate metadata or special behavior to the compiler or runtime environment

**Purpose and Benefits:**
- **Metadata Provider:** Provide metadata about a class without forcing implementation of specific methods
- **Type Safety:** Enable compile-time type checking to ensure objects have certain characteristics
- **Framework Integration:** Allow frameworks and libraries to identify and handle classes differently based on these markers

**Note:** 
- Static methods (in interfaces) cannot be overridden by implementing classes
- This provides backward compatibility (static and default methods can be added to existing interfaces)
- All methods in the interface are public by default

### 4. Polymorphism
One method, different implementations (Method overloading & overriding).

The ability of an object to take on many forms. This way, subclasses of a class can define their own unique behaviors and yet share some of the same functionality of the parent class.

**"IS-A" test** can check interfaces and polymorphism.

```java
// Base class
class Animal {
    public void makeSound() {
        System.out.println("The animal makes a sound");
    }
}

// Derived classes
class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("The dog barks");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("The cat meows");
    }
}

class Bird extends Animal {
    @Override
    public void makeSound() {
        System.out.println("The bird chirps");
    }
}

// Polymorphism in action
public class PolymorphismExample {
    public static void main(String[] args) {
        Animal[] animals = {
            new Dog(),
            new Cat(),
            new Bird()
        };
        
        // Same method call, different behaviors
        for (Animal animal : animals) {
            animal.makeSound(); // Polymorphic behavior
        }
    }
}
```

**Dynamic Binding:** When a function call is resolved at runtime instead of compile time, it enables runtime polymorphism using virtual functions.

## Casting

### Upcasting
Moving up in the inheritance chain:
```java
Animal animal = new Dog(); // Implicit upcasting
```
Properties of the super class are inherited by subclasses automatically (implicit cast).

### Downcasting
Converting the type of an object to a subclass:
```java
Animal animal = new Dog();
Dog dog = (Dog) animal; // Explicit downcasting
```
Cast will be successful if the object is already an instance of the target class or a subclass of it.

```java
// Safe downcasting with instanceof check
if (animal instanceof Dog) {
    Dog dog = (Dog) animal;
    // Now safe to use dog-specific methods
}
```

## Design Principles

### Cohesion
The degree to which elements of a module, class, or function work together to achieve a single purpose.

**High Cohesion Example:**
```java
class MathUtils {
    public static double add(double a, double b) {
        return a + b;
    }
    
    public static double subtract(double a, double b) {
        return a - b;
    }
    
    public static double multiply(double a, double b) {
        return a * b;
    }
    
    public static double divide(double a, double b) {
        return a / b;
    }
}
// All methods are related to mathematical operations - HIGH COHESION
```

**Low Cohesion Example:**
```java
class Cat {
    public void meow() {
        System.out.println("Meow");
    }
    
    public void bark() { // This doesn't belong here!
        System.out.println("Woof");
    }
    
    public void calculateTax() { // This also doesn't belong!
        // tax calculation logic
    }
}
// Methods are not related to the Cat's purpose - LOW COHESION
```

### Coupling
How tightly a class or routine is related to other classes or routines. Coupling must be kept loose.

The degree of dependency between modules, classes, or components in a system.

**Tight Coupling (Bad):**
```java
class Order {
    private EmailService emailService = new EmailService(); // Direct dependency
    
    public void processOrder() {
        // process order logic
        emailService.sendEmail("Order processed"); // Tightly coupled
    }
}
```

**Loose Coupling (Good):**
```java
interface NotificationService {
    void sendNotification(String message);
}

class EmailService implements NotificationService {
    @Override
    public void sendNotification(String message) {
        // send email
    }
}

class Order {
    private NotificationService notificationService;
    
    public Order(NotificationService notificationService) {
        this.notificationService = notificationService; // Dependency injection
    }
    
    public void processOrder() {
        // process order logic
        notificationService.sendNotification("Order processed"); // Loosely coupled
    }
}
```

## Access Modifiers

| Modifier | Class | Package | Subclass | World |
|----------|-------|---------|----------|-------|
| Public | Yes | Yes | Yes | Yes |
| Protected | Yes | Yes | Yes | No |
| No modifier | Yes | Yes | No | No |
| Private | Yes | No | No | No |

### C# Access Modifiers

- **Public:** Accessible from anywhere in the program
- **Private:** Only accessible within the same class or struct (default for class members)
- **Protected:** Accessible within the same class and by derived classes
- **Internal:** Accessible anywhere within the same assembly (default for top-level types)
- **Protected Internal:** Combines protected and internal - accessible within the same assembly OR by derived classes
- **Private Protected:** Accessible within the same class AND by derived classes, but only within the same assembly

## Summary

**Cohesion and Coupling:**
- **High Cohesion** = Good (elements work together toward a single purpose)
- **Low Coupling** = Good (minimal dependencies between classes)
- **Goal:** Achieve high cohesion within classes and low coupling between classes for maintainable, flexible code
