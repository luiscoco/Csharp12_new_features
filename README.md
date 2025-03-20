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

**When should you use Primary Constructors?**

a) For concise type definitions, reducing boilerplate code.

b) Clearly declaring required parameters for type initialization.

c) Keeping class/struct definitions readable and maintainable.

**Summary and Benefits of Primary Constructors in C# 12**:

a) Conciseness: Reduces boilerplate significantly.

b) Clarity: Clearly defines what is required to create instances.

c) Maintainability: Improves readability, especially in simple type definitions.

d)|Flexibility: Available for all class and struct types (not just records).

Primary constructors greatly simplify the definition of simple classes and structs, streamlining object initialization and making your code more expressive and clear.

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

In **C# 12**, Collection Expressions introduce concise and expressive **syntax for creating common collection types** like arrays, lists, and spans directly with minimal verbosity.

**Previously**, to create collections, you wrote:

```csharp
int[] numbers = new int[] { 1, 2, 3 };
List<string> names = new List<string> { "John", "Alice" };
Span<char> chars = new char[] { 'a', 'b', 'c' };
```

**Now with C# 12**, you can write them more concisely with collection expressions:

```csharp
int[] numbers = [1, 2, 3];
List<string> names = ["John", "Alice"];
Span<char> chars = ['a', 'b', 'c'];
```

**Key points about Collection Expressions**:

a) Supports **inline initialization** with simple syntax ([element1, element2, ...]).

b) Supports multiple collection types out-of-the-box:

**Arrays** (int[], string[], etc.)
**Lists** (List<T>)
**Spans** (Span<T>, ReadOnlySpan<T>)

c) **Spread operator (..)** allows merging collections concisely.

**Summary of Collection Expressions (C# 12)**:

a) Clear, concise syntax for **collection creation**:

```csharp
int[] nums = [1, 2, 3];            // Arrays
List<string> list = ["a", "b"];    // Lists
Span<char> span = ['x', 'y'];      // Spans
```

b) **Spread operator (..)** to merge collections:

```csharp
int[] merged = [..arr1, ..arr2];
```

c) Flexible usage as arguments, initializers, etc.

C# 12's collection expressions streamline the creation of common collection types, making your code concise, readable, and efficient.

### 2.1. Example 1: Basic Usage (Array, List, Span)

```csharp
using System;

class Program
{
    static void Main()
    {
        // Creating an array using collection expression
        int[] numbers = [1, 2, 3, 4, 5];
        Console.WriteLine($"Array: {string.Join(", ", numbers)}");

        // Creating a List using collection expression
        List<string> fruits = ["Apple", "Banana", "Cherry"];
        Console.WriteLine($"List: {string.Join(", ", fruits)}");

        // Creating a Span using collection expression
        Span<char> letters = ['X', 'Y', 'Z'];
        Console.WriteLine($"Span: {letters.ToString()}");
    }
}
```

### 2.2. Example 2: Jagged Arrays

```csharp
using System;

class Program
{
    static void Main()
    {
        // Create jagged 2D array directly
        int[][] matrix =
        [
            [1, 2, 3],
            [4, 5, 6],
            [7, 8, 9]
        ];

        // Display jagged array
        for (int i = 0; i < matrix.Length; i++)
        {
            Console.WriteLine($"Row {i}: {string.Join(", ", matrix[i])}");
        }
    }
}
```

### 2.3. Example 3: Using Spread Operator (..)

```csharp
using System;

class Program
{
    static void Main()
    {
        int[] first = [1, 2, 3];
        int[] second = [4, 5];
        int[] third = [6, 7, 8, 9];

        // Merge multiple arrays into one using spread operator
        int[] mergedArray = [.. first, .. second, .. third];

        Console.WriteLine($"Merged Array: {string.Join(", ", mergedArray)}");
    }
}
```

### 2.4. Example 4: Collection Expressions in Method Calls

```csharp
using System;

class Program
{
    static void PrintItems(IEnumerable<string> items)
    {
        foreach (var item in items)
            Console.WriteLine($"Item: {item}");
    }

    static void Main()
    {
        PrintItems(["Laptop", "Tablet", "Smartphone"]);
    }
}
```


## 3. ref readonly parameters




## 4. Default lambda parameters






## 5. Alias any type





## 6. Inline arrays






## 7. Experimental attribute






## 8. Interceptors






## 4. 
