# .NET Interview Questions

This repository contains a comprehensive list of commonly asked .NET interview questions, categorized for ease of reference.

---

## A. C# & OOP Questions

1. **Try, Catch, and Finally**

In C#, the `try-catch-finally` block is used for exception handling. It allows you to write code that handles runtime errors gracefully without crashing the program. Here's how each part works:

- **`try` Block**: Contains the code that may throw an exception.
- **`catch` Block**: Catches and handles exceptions if they occur.
- **`finally` Block**: Executes code after the `try` and `catch` blocks, regardless of whether an exception was thrown or not. It's typically used for cleanup operations like closing a file or releasing resources.

#### Example:
```csharp
using System;

class Program {
    static void Main() {
        try {
            int numerator = 10;
            int denominator = 0;

            // This will throw a DivideByZeroException
            int result = numerator / denominator;
            Console.WriteLine("Result: " + result);
        }
        catch (DivideByZeroException ex) {
            // Handles the specific exception
            Console.WriteLine($"Error: {ex.Message}");
        }
        finally {
            // Always executes
            Console.WriteLine("Execution completed.");
        }
    }
}

2. **Difference between Abstract Class and Interface**

### Difference Between Abstract Class and Interface

In C#, both abstract classes and interfaces are used to define contracts for classes to implement, but they differ in functionality and use cases.

#### Key Differences:

| Feature                  | Abstract Class                          | Interface                               |
|--------------------------|-----------------------------------------|----------------------------------------|
| **Implementation**       | Can have some methods implemented.     | Cannot have implemented methods (pre-C# 8.0). |
| **Fields**               | Can have fields.                       | Cannot have fields.                    |
| **Inheritance**          | Can inherit from one abstract class or interface. | Can inherit from multiple interfaces.  |
| **Access Modifiers**     | Members can have access modifiers.      | Members cannot have access modifiers.  |
| **Constructors**         | Can have constructors.                 | Cannot have constructors.              |

#### When to Use:
- Use **abstract classes** when:
  - You want to provide a base class with some shared implementation.
  - The relationship is hierarchical and "is-a" type.
- Use **interfaces** when:
  - You want to define a contract without any implementation.
  - Multiple inheritance is required.

#### Example:
```csharp
// Abstract Class Example
abstract class Animal {
    public abstract void Speak();
    public void Eat() {
        Console.WriteLine("Eating...");
    }
}

class Dog : Animal {
    public override void Speak() {
        Console.WriteLine("Dog barks.");
    }
}

// Interface Example
interface IAnimal {
    void Speak();
}

class Cat : IAnimal {
    public void Speak() {
        Console.WriteLine("Cat meows.");
    }
}

// Usage
class Program {
    static void Main() {
        Animal dog = new Dog();
        dog.Speak(); // Output: Dog barks.
        dog.Eat();   // Output: Eating...

        IAnimal cat = new Cat();
        cat.Speak(); // Output: Cat meows.
    }
}


3. **What is a Sealed Class?**

### What is a Sealed Class?

In C#, a `sealed` class is a class that cannot be inherited. It is used to prevent other classes from deriving from it. 

#### Use Cases:
- To restrict inheritance for security or design reasons.
- To improve performance by notifying the compiler that no further inheritance will occur.

#### Syntax:
```csharp
sealed class MyClass {
    public void Display() {
        Console.WriteLine("This is a sealed class.");
    }
}


4. **How to Solve Multiple Class Inheritance in C#**

### How to Solve Multiple Class Inheritance in C#

C# does not support multiple inheritance with classes due to the ambiguity it can introduce, such as the **diamond problem**. However, this can be solved by using interfaces. Interfaces allow a class to inherit from multiple sources while avoiding the complexities of multiple class inheritance.

#### Example:
```csharp
using System;

// Define two interfaces
interface IAnimal {
    void Eat();
}

interface IMammal {
    void Walk();
}

// Implement both interfaces in a single class
class Dog : IAnimal, IMammal {
    public void Eat() {
        Console.WriteLine("Dog is eating.");
    }

    public void Walk() {
        Console.WriteLine("Dog is walking.");
    }
}

class Program {
    static void Main() {
        Dog myDog = new Dog();
        myDog.Eat();   // Output: Dog is eating.
        myDog.Walk();  // Output: Dog is walking.
    }
}


5. **Access Modifiers: Public, Protected, Private**

# Access Modifiers in C#

Access modifiers in C# determine the visibility and accessibility of class members (fields, properties, methods). The three primary access modifiers are **Public**, **Protected**, and **Private**.

## 1. Public

Members declared as `public` are accessible from any other code in the same assembly or another assembly that references it.

### Example:
```csharp
public class Car
{
    public string Model { get; set; }

    public void Drive()
    {
        Console.WriteLine("Driving the car.");
    }
}

## 2. Protected

Members declared as `protected` are accessible only within their own class and by derived classes (subclasses).

### Example:
```csharp
public class Vehicle
{
    protected string EngineType { get; set; }

    protected void StartEngine()
    {
        Console.WriteLine("Engine started.");
    }
}

public class Car : Vehicle
{
    public void ShowEngineType()
    {
        EngineType = "V8";
        Console.WriteLine("Engine type: " + EngineType);
        StartEngine();
    }
}

## 3. Private

Members declared as `private` are accessible only within the body of the class in which they are declared. They are not visible to derived classes or any other classes.

### Example:
```csharp
public class BankAccount
{
    private decimal Balance { get; set; }

    public void Deposit(decimal amount)
    {
        if (amount > 0)
        {
            Balance += amount;
            Console.WriteLine($"Deposited: {amount}");
        }
    }

    public decimal GetBalance()
    {
        return Balance;
    }
}

## 4. Internal

Members declared as `internal` are accessible only within the same assembly. This means they are not accessible from other assemblies.

### Example:
```csharp
internal class InternalClass
{
    internal void DisplayMessage()
    {
        Console.WriteLine("This is an internal class method.");
    }
}

## 5. Protected Internal

Members declared as `protected internal` are accessible from the current assembly and from derived classes in other assemblies.

### Example:
```csharp
public class BaseClass
{
    protected internal string InternalProtectedProperty { get; set; }
}

public class DerivedClass : BaseClass
{
    public void ShowProperty()
    {
        InternalProtectedProperty = "Accessible from derived class.";
        Console.WriteLine(InternalProtectedProperty);
    }
}

## 6. Private Protected

Members declared as `private protected` are accessible only within the same assembly and in the same class or a derived class.

### Example:
```csharp
public class BaseClass
{
    private protected string InternalProtectedProperty { get; set; }
}

public class DerivedClass : BaseClass
{
    public void ShowProperty()
    {
        InternalProtectedProperty = "Accessible from derived class.";
        Console.WriteLine(InternalProtectedProperty);
    }
}

## 7. File

Members declared with the `file` access modifier are accessible only within the same file.

### Example:
```csharp
class FileClass
{
    internal void FileMethod()
    {
        Console.WriteLine("This method is accessible only within the same file.");
    }
}

// This method is not accessible from outside this file.



```markdown
## Summary of C# Access Modifiers

- **Public**: Accessible from anywhere.
- **Protected**: Accessible only within the class and derived classes.
- **Private**: Accessible only within the class itself.
- **Internal**: Accessible only within the same assembly.
- **Protected Internal**: Accessible from the current assembly and derived classes in other assemblies.
- **Private Protected**: Accessible only within the same assembly and in the same class or a derived class.
- **File**: Accessible only within the same file.

>>> The record modifier on a type causes the compiler to synthesize extra members. The record modifier doesn't affect the default accessibility for either a record class or a record struct.



6. **Difference Between String and StringBuilder**

## 6. Difference Between String and StringBuilder

In C#, `String` and `StringBuilder` are used to manipulate strings, but they have some key differences:

| Feature          | String                            | StringBuilder                      |
|------------------|-----------------------------------|------------------------------------|
| **Mutability**    | Immutable (cannot be changed)    | Mutable (can be modified)         |
| **Performance**   | Slower for frequent modifications | Faster for frequent modifications  |
| **Memory Usage**  | Creates new objects for changes  | Reuses the same object for changes |
| **Usage**         | Suitable for fixed string values  | Suitable for dynamic string manipulations |

### Example:
```csharp
// Using String
string str = "Hello";
str += " World"; // Creates a new string object
Console.WriteLine(str); // Output: Hello World

// Using StringBuilder
StringBuilder sb = new StringBuilder("Hello");
sb.Append(" World"); // Modifies the existing StringBuilder object
Console.WriteLine(sb.ToString()); // Output: Hello World


7. **.NET Generics**

## 7. .NET Generics

Generics in .NET allow you to define classes, interfaces, and methods with a placeholder for the data type. This feature enables you to create reusable code components while providing type safety. Generics help reduce code duplication and improve performance by avoiding boxing and unboxing.

### Benefits of Using Generics:
- **Type Safety**: Ensures compile-time type checking.
- **Code Reusability**: Allows you to create a single class or method that works with any data type.
- **Performance**: Reduces the need for boxing/unboxing when working with value types.

### Example:
```csharp
// Generic class
public class GenericList<T>
{
    private List<T> items = new List<T>();

    public void Add(T item)
    {
        items.Add(item);
    }

    public T Get(int index)
    {
        return items[index];
    }
}

// Usage of GenericList
class Program
{
    static void Main(string[] args)
    {
        // Using GenericList with integers
        GenericList<int> intList = new GenericList<int>();
        intList.Add(1);
        intList.Add(2);
        Console.WriteLine(intList.Get(0)); // Output: 1

        // Using GenericList with strings
        GenericList<string> stringList = new GenericList<string>();
        stringList.Add("Hello");
        stringList.Add("World");
        Console.WriteLine(stringList.Get(1)); // Output: World
    }
}


8. **List, LinkedList, Queue, and Stack**

## 8. List, LinkedList, Queue, and Stack

In .NET, different collection types are available to store and manipulate data. Below are some commonly used collection types:

### 1. List
- A `List<T>` is a dynamic array that can grow in size and provides fast access to elements.
- It is suitable for scenarios where you need fast index-based access.

#### Example:
```csharp
List<int> numbers = new List<int>();
numbers.Add(1);
numbers.Add(2);
numbers.Add(3);
Console.WriteLine(numbers[1]); // Output: 2

### 2. LinkedList

A `LinkedList<T>` is a doubly linked list that allows for efficient insertion and removal of elements. It is useful when you frequently add or remove items from the beginning or end of the collection.

#### Example:
```csharp
LinkedList<string> names = new LinkedList<string>();
names.AddLast("Alice");
names.AddLast("Bob");
names.AddFirst("Charlie");
Console.WriteLine(names.First.Value); // Output: Charlie


### 3. Queue

A `Queue<T>` is a first-in, first-out (FIFO) collection that allows you to add items at the end and remove them from the front. It is ideal for scenarios where you need to process items in the order they were added.

#### Example:
```csharp
Queue<string> queue = new Queue<string>();
queue.Enqueue("Task 1");
queue.Enqueue("Task 2");
Console.WriteLine(queue.Dequeue()); // Output: Task 1

### 4. Stack

A `Stack<T>` is a last-in, first-out (LIFO) collection that allows you to add and remove items from the top of the stack. It is useful for scenarios like undo functionality, where the most recently added item needs to be processed first.

#### Example:
```csharp
Stack<int> stack = new Stack<int>();
stack.Push(1);
stack.Push(2);
Console.WriteLine(stack.Pop()); // Output: 2


### Summary

- **List**: Dynamic array with fast index-based access.
- **LinkedList**: Doubly linked list for efficient insertions and deletions.
- **Queue**: FIFO collection for processing items in the order added.
- **Stack**: LIFO collection for processing the most recently added items first.


9. **Difference Between LinkedList and ArrayList**

## 9. Difference Between LinkedList and ArrayList

| Feature                | LinkedList<T>                                   | ArrayList                                   |
|------------------------|-------------------------------------------------|---------------------------------------------|
| **Data Structure**     | Doubly linked list                              | Resizable array                             |
| **Access Time**        | O(n) for access by index                       | O(1) for access by index                   |
| **Insertion/Deletion** | O(1) for adding/removing elements              | O(n) for adding/removing elements          |
| **Memory Usage**       | More memory overhead due to node references    | Less memory overhead but can lead to fragmentation |
| **Type Safety**        | Strongly typed (generics)                       | Non-generic, stores objects as `object`   |
| **Use Case**           | Best for scenarios where frequent insertions/removals occur | Best for scenarios requiring fast access by index |

### Summary

- **LinkedList<T>** is suitable for scenarios where frequent insertions and deletions occur.
- **ArrayList** provides faster access times at the cost of slower insertions and deletions, but it lacks type safety and is less efficient in memory usage.


10. **Polymorphism with Examples**

## 10. Polymorphism with Examples

Polymorphism is a fundamental concept in object-oriented programming that allows methods to do different things based on the object that it is acting upon. In C#, polymorphism can be achieved through method overriding and method overloading.

### Types of Polymorphism

1. **Compile-Time Polymorphism (Method Overloading)**:
   - This occurs when multiple methods in the same class have the same name but different parameters.

#### Example:
```csharp
public class MathOperations
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public double Add(double a, double b)
    {
        return a + b;
    }
}

MathOperations math = new MathOperations();
Console.WriteLine(math.Add(5, 10)); // Output: 15
Console.WriteLine(math.Add(5.5, 10.2)); // Output: 15.7


11. **Keywords: Override, New, and Virtual**

### Run-Time Polymorphism (Method Overriding)

Run-time polymorphism occurs when a derived class overrides a method of its base class. The appropriate method is determined at runtime.

#### Example:
```csharp
public class Animal
{
    public virtual void Sound()
    {
        Console.WriteLine("Animal makes a sound");
    }
}

public class Dog : Animal
{
    public override void Sound()
    {
        Console.WriteLine("Dog barks");
    }
}

public class Cat : Animal
{
    public override void Sound()
    {
        Console.WriteLine("Cat meows");
    }
}

Animal myDog = new Dog();
Animal myCat = new Cat();
myDog.Sound(); // Output: Dog barks
myCat.Sound(); // Output: Cat meows


### Summary

Polymorphism allows for method overloading (compile-time) and method overriding (run-time). It enhances code flexibility and reusability by enabling a single interface to represent different underlying forms (data types).


12. **Overloading and Constructor Overloading**

## 12. Overloading and Constructor Overloading

### Method Overloading

Method overloading occurs when multiple methods in the same class have the same name but different parameters (type, number, or both). This allows methods to perform similar but slightly different tasks based on the input parameters.

#### Example:
```csharp
public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public double Add(double a, double b)
    {
        return a + b;
    }

    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }
}

Calculator calc = new Calculator();
Console.WriteLine(calc.Add(5, 10)); // Output: 15
Console.WriteLine(calc.Add(5.5, 10.2)); // Output: 15.7
Console.WriteLine(calc.Add(1, 2, 3)); // Output: 6


13. **Reflection**

### Constructor Overloading

Constructor overloading is similar to method overloading but specifically pertains to constructors. Multiple constructors can be defined in a class, allowing different ways to initialize objects based on the arguments passed.

#### Example:
```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    // Default constructor
    public Person()
    {
        Name = "Unknown";
        Age = 0;
    }

    // Parameterized constructor
    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
}

Person person1 = new Person(); // Calls default constructor
Person person2 = new Person("Alice", 30); // Calls parameterized constructor

Console.WriteLine($"{person1.Name}, Age: {person1.Age}"); // Output: Unknown, Age: 0
Console.WriteLine($"{person2.Name}, Age: {person2.Age}"); // Output: Alice, Age: 30

### Summary

- **Overloading** allows multiple methods with the same name but different parameters within a class.
- **Constructor Overloading** provides different ways to initialize an object based on the parameters passed to the constructor.


14. **LINQ and LINQ to SQL**

### LINQ and LINQ to SQL

**LINQ (Language Integrated Query)** is a powerful feature in C# that allows developers to write queries directly in the C# language, providing a consistent model for working with data across various types of data sources, including arrays, collections, XML, and databases. LINQ queries are type-safe and can be compiled at compile-time, allowing for better performance and error checking.

#### Example:
```csharp
using System;
using System.Collections.Generic;
using System.Linq;

public class Program
{
    public static void Main()
    {
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
        var evenNumbers = from num in numbers
                          where num % 2 == 0
                          select num;

        foreach (var num in evenNumbers)
        {
            Console.WriteLine(num); // Output: 2, 4
        }
    }
}

### LINQ and LINQ to SQL

**LINQ (Language Integrated Query)** is a powerful feature in C# that allows developers to write queries directly in the C# language, providing a consistent model for working with data across various types of data sources, including arrays, collections, XML, and databases. LINQ queries are type-safe and can be compiled at compile-time, allowing for better performance and error checking.

**LINQ to SQL** is a component of LINQ that allows developers to query a SQL Server database using LINQ syntax. It provides a mapping between the database tables and C# classes, making data access more intuitive and type-safe.

#### Example:
```csharp
using System;
using System.Data.Linq;
using System.Linq;

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Program
{
    public static void Main()
    {
        DataContext db = new DataContext("your_connection_string");
        var products = from p in db.GetTable<Product>()
                       where p.Name.Contains("example")
                       select p;

        foreach (var product in products)
        {
            Console.WriteLine(product.Name);
        }
    }
}

### Summary

LINQ allows querying various data sources using a unified syntax in C#.  
LINQ to SQL enables querying SQL Server databases using LINQ, providing strong type safety and ease of use.



15. **Difference Between .NET Versions**

### Difference Between .NET Versions

.NET has evolved over the years, introducing various frameworks and versions. Here’s a brief overview of the key differences:

| Version         | Release Year | Key Features                                           | Platform Support           |
|------------------|--------------|-------------------------------------------------------|-----------------------------|
| **.NET Framework** | 2002         | Full-featured framework for Windows applications.     | Windows only                |
| **.NET Core**     | 2016         | Cross-platform, modular, and lightweight.             | Windows, Linux, macOS      |
| **.NET 5**        | 2020         | Unifies .NET Framework and .NET Core; single platform. | Windows, Linux, macOS, iOS, Android |
| **.NET 6**        | 2021         | Long-Term Support (LTS), improved performance, and new features. | Windows, Linux, macOS, iOS, Android |
| **.NET 7**        | 2022         | Enhanced performance, support for cloud and microservices. | Windows, Linux, macOS, iOS, Android |

### Summary

- **.NET Framework**: Primarily for Windows; robust features for desktop and web applications.
- **.NET Core**: Cross-platform and modular; suitable for modern cloud-based applications.
- **.NET 5 and later**: Unified platform for building any application type across all platforms.


16. **Design Patterns**


### Design Patterns

Design patterns are reusable solutions to common software design problems. They help improve code readability, maintainability, and scalability. Here are some common design patterns:

| Pattern              | Description                                                | Example                                         |
|----------------------|------------------------------------------------------------|-------------------------------------------------|
| **Singleton**        | Ensures a class has only one instance and provides a global point of access to it. | Managing a connection pool.                     |
| **Factory Method**   | Defines an interface for creating an object, but lets subclasses alter the type of objects that will be created. | Creating different types of shapes (Circle, Square) based on input. |
| **Observer**         | A one-to-many dependency between objects so that when one object changes state, all its dependents are notified. | Event handling systems where multiple listeners are notified of an event. |
| **Strategy**         | Defines a family of algorithms, encapsulates each one, and makes them interchangeable. | Sorting algorithms (QuickSort, MergeSort) that can be selected at runtime. |
| **Decorator**        | Adds behavior or responsibilities to individual objects dynamically without affecting the behavior of other objects. | Adding features to UI components (like adding scrollbars to windows). |

### Summary

- Design patterns are best practices for solving common design issues.
- They promote code reuse and can help with the organization of complex software systems.
