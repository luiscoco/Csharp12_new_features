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

**Best Practices & Important Notes**:

a) Use semantic and meaningful alias names for clarity.

b) Clearly document alias usages to avoid confusion.

**When Should You Use Alias Any Type?**

a) To simplify and clarify complex or repeated type declarations.

b) To improve maintainability (refactor type changes easily).

c )To provide clearer semantic meaning to your types.

**Summary & Benefits of "Alias Any Type" (C# 12)**:

a) Improves readability: Gives clear semantic names.

b) Simplifies complex code: Reduces boilerplate.

c) Supports advanced scenarios: Tuples, arrays, pointers, and complex generics can now be aliased clearly.

This feature significantly enhances the readability and maintainability of your code, especially when dealing with complex or repetitive type declarations.

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
### 5.3. Example 3: Aliasing Unsafe Pointer Types

```csharp
using System;

// unsafe pointer alias:
using IntPointer = int*;

class Program
{
    unsafe static void Main()
    {
        int number = 100;
        IntPointer ptr = &number;

        Console.WriteLine($"Value pointed by ptr: {*ptr}");
    }
}
```

### 5.4. Example 4: Aliasing Complex Generic Types

```csharp
using System.Collections.Generic;

// Complex generic type alias:
using NameLookup = Dictionary<int, List<string>>;

class Program
{
    static void Main()
    {
        NameLookup lookup = new()
        {
            [1] = ["Alice", "Anna"],
            [2] = ["Bob", "Bill"]
        };

        foreach (var (key, names) in lookup)
        {
            Console.WriteLine($"ID {key}: {string.Join(", ", names)}");
        }
    }
}
```

## 6. Inline arrays

Inline arrays are a new **C# 12** feature designed primarily for **advanced performance optimization scenarios**. 

They allow **defining fixed-size arrays** directly **within struct types**, providing performance similar to unsafe fixed-size buffers, but with safer, clearer semantics.

**Who uses inline arrays?**

a) Primarily used by runtime developers, library authors, or performance-critical application developers.

b) Most application developers typically consume inline arrays (via Span<T> or ReadOnlySpan<T>), rather than defining them directly.

**What do inline arrays look like?**

Inline arrays are declared by annotating a struct with [InlineArray(N)], specifying the array size:

```csharp
[System.Runtime.CompilerServices.InlineArray(10)]
public struct Buffer
{
    private int _element0; // Compiler automatically generates the rest
}
```

**_element0** is required for type inference.

The **compiler auto-generates additional elements** (_element1, _element2, ...) transparently, matching the declared size (10 in this example).

**Key characteristics of Inline Arrays**:

a) Fixed size: Defined at compile-time via attribute.

b) Performance optimized: Similar performance to unsafe fixed buffers, but safer and clearer.

c) Transparent usage: Can be indexed (buffer[i]) and iterated like normal arrays.

**Performance Considerations & Compiler Optimizations**:

a) Inline arrays give the compiler explicit knowledge of size at compile-time.

b) Compiler may optimize allocations and indexing more effectively compared to dynamic arrays.

c) Avoids heap allocation, maintaining data on the stack for better cache locality and performance.

**When to use Inline Arrays**:

a) When you need fixed-size, performance-oriented buffers with safer semantics than unsafe fixed-size buffers.

b) In high-performance scenarios (runtime internals, library development, game engines, mathematical computations).

**Limitations & Considerations**:

a) Inline arrays are fixed-size; cannot dynamically resize.

b) Typically struct-based, best suited for performance-sensitive stack scenarios.

c) Be cautious of large inline arrays, as they increase stack size and may impact performance negatively.

**Summary & Benefits of Inline Arrays (C# 12)**:

a) High performance: Similar speed to unsafe fixed-size buffers.

b) Safety & clarity: No unsafe context needed.

c) Transparent and simple usage: Easily indexed and iterable.

Inline arrays represent a safe, efficient, and performant alternative for fixed-size buffer scenarios, primarily enhancing libraries and performance-critical code.

### 6.1. Example 1: Declaring and Using an Inline Array Struct

```csharp
using System;
using System.Runtime.CompilerServices;

[InlineArray(5)]
public struct FixedBuffer
{
    private int _element0;
}

class Program
{
    static void Main()
    {
        var buffer = new FixedBuffer();

        // Assign values (indexed access)
        for (int i = 0; i < 5; i++)
            buffer[i] = (i + 1) * 10;

        // Iterate and display values
        foreach (var value in buffer)
            Console.WriteLine(value);
    }
}
```

### 6.2. Example 2: Inline Arrays and Span<T>

```csharp
using System;
using System.Runtime.CompilerServices;

[InlineArray(3)]
public struct InlinePoint
{
    private double _element0;
}

class Program
{
    static void Main()
    {
        InlinePoint point = new();

        Span<double> coordinates = point; // Implicit conversion to Span<double>

        coordinates[0] = 1.5;
        coordinates[1] = 3.2;
        coordinates[2] = 4.7;

        Console.WriteLine($"Coordinates: {string.Join(", ", coordinates)}");
    }
}
```

## 7. Experimental attribute

In **C# 12**, the new **ExperimentalAttribute** lets **library authors** explicitly indicate that a particular type, method, or assembly is considered **experimental** and might change or be removed in future versions.

When you mark something as **experimental**, the compiler will **issue clear warnings** whenever developers consume that experimental API.

This helps ensure consumers clearly understand the **risks** and potential **instability** involved with using experimental features.

**Key points about ExperimentalAttribute**:

a) Explicit marking: Clearly marks APIs that might change or disappear.

b) Compiler warnings: Developers receive explicit warnings when using experimental features.

c) Applicable to:

Types (classes, structs, interfaces, enums, delegates, etc.)

Methods

Assemblies (making all contained types experimental)

**When should you use ExperimentalAttribute?**

a) Clearly marking new APIs still under evaluation.

b) Signaling explicitly that APIs might change or disappear in future releases.

c) Preventing accidental reliance on unstable APIs without explicit acknowledgment.

**Best Practices & Recommendations**:

a) Always provide clear identifiers and helpful documentation URLs within the attribute.

b) Clearly document in release notes and API documentation when marking APIs as experimental.

c) Limit the use of Experimental APIs publicly to prevent confusion.
 
**Summary & Benefits of ExperimentalAttribute (C# 12)**:

a) Clear communication: Developers immediately see risks associated with unstable features.

b) Safe experimentation: Allows library authors to release early versions without creating unintended compatibility expectations.

c)Encourages feedback: Developers can test and provide feedback on experimental APIs clearly.

The **ExperimentalAttribute** provides a clear and explicit way for library authors to indicate APIs that are under active development and might change, ensuring a better communication and clearer understanding between library maintainers and their users.

### 7.1. Basic Usage Example

**Step 1: Marking Experimental APIs**

```csharp
using System;
using System.Diagnostics.CodeAnalysis;

// Marking a method as experimental
public class Calculator
{
    [Experimental("EXPERIMENTAL001", UrlFormat = "https://example.com/docs/experimental/{0}")]
    public static int ExperimentalAdd(int a, int b)
    {
        return a + b;
    }
}
```

**Step 2: Consuming Experimental APIs (Compiler Warning)**

When consuming the marked method:

```csharp
class Program
{
    static void Main()
    {
        int result = Calculator.ExperimentalAdd(5, 3); 
        Console.WriteLine(result);
    }
}
```

**Compiler Warning issued**:
```
warning EXPERIMENTAL001: 'Calculator.ExperimentalAdd(int, int)' is for evaluation purposes only and is subject to change or removal in future updates.
See: https://example.com/docs/experimental/EXPERIMENTAL001
```

### 7.2. Example 2: Marking a Whole Assembly as Experimental

You can apply ExperimentalAttribute at the assembly level, making all contained APIs experimental

```csharp
using System.Diagnostics.CodeAnalysis;

[assembly: Experimental("ASSEMBLY_EXPERIMENTAL", UrlFormat = "https://example.com/docs/experimental/{0}")]

namespace ExperimentalLibrary
{
    public class FeatureX
    {
        public void DoSomething() { }
    }

    public struct ExperimentalStruct
    {
        public int Value { get; set; }
    }
}
```

When consuming anything from this assembly:

```csharp
using ExperimentalLibrary;

class Program
{
    static void Main()
    {
        var feature = new FeatureX();
        feature.DoSomething(); // ⚠️ Compiler warning: Experimental!

        ExperimentalStruct es = new ExperimentalStruct(); // ⚠️ Experimental warning
    }
}
```

This clearly notifies developers that all APIs within the assembly are subject to change.

## 8. Interceptors





