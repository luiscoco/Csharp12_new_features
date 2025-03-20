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

In **earlier C# versions**, you had these parameter modifiers:

**ref**: Parameters passed by reference (modifiable).

**in**: Parameters passed by reference but read-only. Can accept both variables and value expressions.

With **C# 12**, there's a new modifier:

**ref readonly**:

Parameters passed by reference and explicitly read-only, but must be passed a variable (not a direct value expression).

This modifier gives more clarity and precision for your APIs, specifically when:

You want to ensure the argument is **passed by reference without modification**.

You explicitly require the caller to pass a variable, making it clear that the method uses references (for performance) and guarantees no mutation.

![image](https://github.com/user-attachments/assets/c8a3f78b-4ed8-419c-b2e4-b33874e47b82)

**ref readonly** combines aspects of both **ref** (requires variable) and **in** (readonly).

**When to use ref readonly parameters?**

a) When you explicitly want callers to **pass variables only**, enforcing that **no temporary expressions are allowed**.

b) When you want explicit clarity and performance optimization, indicating **passing by reference without allowing modification**.

c) When **updating legacy APIs** (ref parameters that aren't modified) without breaking existing callers.

### 3.1. Example 1: Basic usage of 'ref readonly' parameter

```csharp
using System;

class Program
{
    static void DisplayCoordinates(ref readonly Point point)
    {
        Console.WriteLine($"Coordinates: ({point.X}, {point.Y})");
        
        // point.X = 10;  // Compiler Error! "Cannot modify ref readonly parameter"
    }

    static void Main()
    {
        Point p = new Point(5, 10);
        
        DisplayCoordinates(ref p);  // Must pass with 'ref' keyword
    }
}

struct Point
{
    public int X;
    public int Y;

    public Point(int x, int y) => (X, Y) = (x, y);
}
```

### 3.2. Example 2: Contrast with 'in' parameter

```csharp
using System;

class Program
{
    static void MethodWithIn(in int value)
    {
        Console.WriteLine($"Value (in): {value}");
    }

    static void MethodWithRefReadonly(ref readonly int value)
    {
        Console.WriteLine($"Value (ref readonly): {value}");
    }

    static void Main()
    {
        int num = 42;

        MethodWithIn(num);             // Can pass directly (value or variable)
        MethodWithIn(100);             // Literal allowed

        MethodWithRefReadonly(ref num); // Variable with ref keyword
        // MethodWithRefReadonly(ref 100); Error: Literal NOT allowed
    }
}
```

### 3.3. Example 3: Updating existing API clearly (no breaking changes)

Let's say you had a previous API using **ref** but **without modifying the parameter**:

**Before (C#11 and earlier)**:

```csharp
public void ProcessData(ref int data)
{
    // Method never actually modifies data
    Console.WriteLine($"Data is: {data}");
}
```

**After C#12**, clearly expressing intent:

```csharp
public void ProcessData(ref readonly int data)
{
    // Explicitly guarantees data isn't modified
    Console.WriteLine($"Data is: {data}");
}
```

**Important**:

Updating your API to use **ref readonly** doesn't cause breaking changes for existing callers, since they already **pass a variable by ref**.

It clarifies intent, **preventing accidental modifications**.

### 3.4. Practical Real-world scenario (Performance-oriented APIs):

```csharp
using System;

class Program
{
    // Method that clearly states it takes a reference to a ReadOnlySpan
    static void PrintFirstElement<T>(ref readonly ReadOnlySpan<T> span)
    {
        if (span.Length > 0)
            Console.WriteLine($"First Element: {span[0]}");
        else
            Console.WriteLine("Span is empty");
    }

    static void Main()
    {
        ReadOnlySpan<int> numbers = stackalloc[] { 10, 20, 30 };
        
        PrintFirstElement(ref numbers);  // must pass with 'ref' keyword
    }
}
```

## 4. Default lambda parameters

**Before C# 12**, if you wanted a **lambda expression** with **default parameter values**, you needed to use methods or local functions, since lambda parameters didn't support defaults directly.

Now, **with C# 12**, you can clearly and concisely define default parameter values directly in lambda expressions, exactly like methods or local functions:

New syntax:

```csharp
var lambda = (int x, int y = 10) => x + y;
```

Here, y has a default value (10), so callers can omit it if desired.

**Key points about Default Lambda Parameters**:

The syntax matches exactly how you declare defaults in methods:

```csharp
(paramType param = defaultValue) => { ... }
```

When calling lambdas, parameters with defaults can be omitted.

Greatly simplifies certain scenarios, avoiding unnecessary method or local function definitions.

**Restrictions and rules**:

Same rules apply as for methods and local functions:

Default parameters must come after non-default ones.

a) Correct: (int x, int y = 10)

b) Incorrect: (int x = 10, int y)

You cannot skip parameters without naming them explicitly.

**When should you use default lambda parameters?**

a) Simplifying callback logic with optional parameters.

b) Reducing the number of overloaded lambdas.

c) Improving readability and maintainability in functional-style code (LINQ, event handling, delegates).

**Summary & Benefits of Default Lambda Parameters (C# 12)**:

a) Concise syntax: Simple inline definitions of optional parameters.

b) Improved readability: Clearly indicates optional/defaulted behavior.

c) Simpler lambdas: Reduces the need for explicit methods or local functions.

This feature greatly simplifies many scenarios where lambdas previously required complex or verbose patterns, providing clear, concise, and expressive code.

### 4.1. Example 1: Basic usage of default lambda parameters

```csharp
using System;

class Program
{
    static void Main()
    {
        // Lambda expression with default parameters
        var multiply = (int x, int y = 2) => x * y;

        Console.WriteLine(multiply(5));      // y defaults to 2 → 5 * 2 = 10
        Console.WriteLine(multiply(5, 4));   // 5 * 4 = 20
    }
}
```

### 4.2. Example 2: Multiple default parameters

```csharp
using System;

class Program
{
    static void Main()
    {
        var greet = (string name = "User", string greeting = "Hello") 
            => $"{greeting}, {name}!";

        Console.WriteLine(greet());                       // Hello, User!
        Console.WriteLine(greet("Alice"));                // Hello, Alice!
        Console.WriteLine(greet("Bob", "Welcome"));       // Welcome, Bob!
    }
}
```

### 4.3. Example 3: Using lambdas with LINQ and defaults

```csharp
using System;
using System.Linq;

class Program
{
    static void Main()
    {
        var numbers = Enumerable.Range(1, 5); // {1,2,3,4,5}

        // Lambda with default parameter to define multiplication factor
        Func<int, int, int> multiplier = (n, factor = 10) => n * factor;

        var resultWithDefault = numbers.Select(n => multiplier(n));
        var resultWithCustom = numbers.Select(n => multiplier(n, 3));

        Console.WriteLine($"With default factor: {string.Join(", ", resultWithDefault)}");
        Console.WriteLine($"With custom factor: {string.Join(", ", resultWithCustom)}");
    }
}
```

## 5. Alias any type

Previously, **before C# 12**, the using alias directive was limited to **named types**. You couldn't create aliases directly for tuple types, arrays, pointers, or unsafe types.

With **C# 12**, this restriction is removed. You can now create semantic aliases for any type, including:

Tuple types

Array types

Pointer types (unsafe)

Other complex or unnamed types

**Basic Syntax**:

```csharp
using MyPoint = (int X, int Y);
using StringArray = string[];
using IntPointer = int*; // unsafe context required
```

**Key Benefits and Uses**:

a) Improved readability: Give semantic meaning to complex types.

b) Simpler refactoring: Easy to manage type changes from a single alias declaration.

c) Consistency: Use meaningful type names instead of repeating complex definitions.

### 5.1. Example 1: Aliasing Tuple Types

```csharp
using System;

// Tuple type alias:
using Coordinate = (double Latitude, double Longitude);

class Program
{
    static void Main()
    {
        Coordinate location = (34.0522, -118.2437); // Los Angeles

        Console.WriteLine($"Latitude: {location.Latitude}");
        Console.WriteLine($"Longitude: {location.Longitude}");
    }
}
```

### 5.2.  Example 2: Aliasing Array Types

```csharp
using System;

// Array type alias:
using IntArray = int[];

class Program
{
    static void Main()
    {
        IntArray numbers = [1, 2, 3, 4, 5];

        Console.WriteLine(string.Join(", ", numbers));
    }
}
```
### 5.3. 




## 6. Inline arrays






## 7. Experimental attribute






## 8. Interceptors





