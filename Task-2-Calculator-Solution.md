# Task 2 – Simple Calculator Class – Solution

## Objective
Practise classes, methods, and returning values.

---

## Your Solution (Excellent! ✅)

```csharp
using System;

public class Calculator
{
    public int Add(int val1, int val2)
    {
        return val1 + val2;
    }
    
    public int Subtract(int val1, int val2)
    {
        return val1 - val2;
    }
    
    public int Multiply(int val1, int val2)
    {
        return val1 * val2;
    }
    
    public static void Main(string[] args)
    {
        Calculator ObjAdd = new Calculator();
        Console.WriteLine(ObjAdd.Add(10, 20));
        Console.WriteLine(ObjAdd.Subtract(200, 100));
        Console.WriteLine(ObjAdd.Multiply(10, 20));
    }
}
```

---

## Improved Solution (More Readable)

```csharp
using System;

public class Calculator
{
    public int Add(int val1, int val2)
    {
        return val1 + val2;
    }
    
    public int Subtract(int val1, int val2)
    {
        return val1 - val2;
    }
    
    public int Multiply(int val1, int val2)
    {
        return val1 * val2;
    }
    
    public static void Main(string[] args)
    {
        Calculator calculator = new Calculator();
        
        int sum = calculator.Add(10, 20);
        int difference = calculator.Subtract(200, 100);
        int product = calculator.Multiply(10, 20);
        
        Console.WriteLine($"Add(10, 20) = {sum}");
        Console.WriteLine($"Subtract(200, 100) = {difference}");
        Console.WriteLine($"Multiply(10, 20) = {product}");
    }
}
```


## Output

```text
Add(10, 20) = 30
Subtract(200, 100) = 100
Multiply(10, 20) = 200
```

---

## Key Learning

- **Naming matters:** Use `calculator` instead of `ObjAdd`. Lowercase for instance variables, PascalCase for class names.
- **Readability first:** Store intermediate results in variables with meaningful names.
- **Self-documenting code:** Labels in output help anyone understand what the code does.
