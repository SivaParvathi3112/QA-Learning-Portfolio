# Task 1 – Hello User – Solution

## Objective
Practise basic input/output and running a simple console app.

---

## Your Solution (Good! ✅)

```csharp
using System;

public class HelloWorld
{
    public static void Main(string[] args)
    {
        string name;
        name = Console.ReadLine();
        Console.WriteLine("Hello " + name);
    }
}
```

---

## Improved Solution (Modern C#)

```csharp
using System;

public class HelloWorld
{
    public static void Main(string[] args)
    {
        Console.WriteLine("Enter your name:");
        string name = Console.ReadLine();
        Console.WriteLine($"Hello, {name}");
    }
}
```



## Output

```text
Enter your name:
Sanjay
Hello, Sanjay
```

---

## Key Learning

- Always prompt the user before expecting input.
- Use **string interpolation** with `$"..."` for embedding variables in strings.
- This modern syntax is more readable and is the standard in modern C# (6.0+).
