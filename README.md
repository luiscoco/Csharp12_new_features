# Csharp 12 New Features Samples

For more information abuot this post visit this web page:

https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-12

## 1. Primary constructors

**Before C# 12**, primary constructors were limited to **record types** (e.g., record Person(string Name)), providing concise syntax for parameter declarations and initialization.

With **C# 12**, this concept is extended beyond records. 

Now, you can define primary constructors in any **class** or **struct**. 

This simplifies your code significantly, allowing concise and expressive type definitions.

**Key points about Primary Constructors in C# 12**:

**Concise Syntax**: Declare constructor parameters directly on the class or struct declaration.

**Parameters Scope**: Primary constructor parameters are accessible within the entire class or struct body.

**Explicit Constructor Rule**: All explicitly declared constructors must call the primary constructor (this(...)) to ensure primary parameters are assigned.

**Implicit Constructor**:

Classes with a primary constructor no longer have an implicit parameterless constructor.

Structs with primary constructors have an implicit parameterless constructor initializing fields to their default values.

**Automatic Properties**:

For record types (class or struct), primary constructor parameters automatically become public properties.

For non-record classes or structs, primary constructor parameters are NOT automatically properties (unless you explicitly declare them).

### 1.1. Example 1: Primary Constructor in a Non-record Class

```csharp
using System;

public class Person(string firstName, string lastName, int age)
{
    // You must explicitly create properties or methods to expose parameters
    public string FirstName { get; } = firstName;
    public string LastName { get; } = lastName;
    public int Age { get; } = age;

    public void Introduce() =>
        Console.WriteLine($"Hello, I'm {FirstName} {LastName}, and I'm {Age} years old.");
}

class Program
{
    static void Main()
    {
        Person person = new("John", "Doe", 30);
        person.Introduce();
    }
}
```

### 1.2. Example 2: Primary Constructor in a Non-record Struct

```csharp
using System;

public struct Point(double x, double y)
{
    // Explicitly declared properties
    public double X { get; } = x;
    public double Y { get; } = y;

    public void PrintCoordinates() =>
        Console.WriteLine($"Coordinates: ({X}, {Y})");
}

class Program
{
    static void Main()
    {
        Point p = new(10.5, 20.0);
        p.PrintCoordinates();
    }
}
```

### 1.3. Example 3: Explicit Constructor calling the Primary Constructor

```csharp
using System;

public class Vehicle(string brand, string model)
{
    public string Brand { get; } = brand;
    public string Model { get; } = model;

    // Additional constructor calling primary constructor
    public Vehicle(string brand) : this(brand, "Unknown")
    {
    }

    public void DisplayInfo() =>
        Console.WriteLine($"Vehicle: {Brand} {Model}");
}

class Program
{
    static void Main()
    {
        Vehicle car1 = new("Toyota", "Corolla");
        Vehicle car2 = new("Ford");

        car1.DisplayInfo();
        car2.DisplayInfo();
    }
}
```


## 2. Collection expressions





## 3. ref readonly parameters




## 4. Default lambda parameters






## 5. Alias any type





## 6. Inline arrays






## 7. Experimental attribute






## 8. Interceptors






## 4. 
