# Task 3 – List and Loop – Solution

## Objective
Practise collections and `foreach` loops.

---

## Your Solution (Works, but Outdated ⚠️)

```csharp
using System;
using System.Collections;

public class Calculator
{
    public static void Main(string[] args)
    {
        ArrayList CountryNames = new ArrayList();
        CountryNames.Add("India");
        CountryNames.Add("Malta");
        CountryNames.Add("Italy");
        CountryNames.Add("Spain");
        CountryNames.Add("Japan");
        
        foreach(string item in CountryNames)
        {
            Console.WriteLine(item);
        }
    }
}
```

---

## ⚠️ Problem with Your Solution

You used **`ArrayList`**, which is **deprecated** (old, not recommended).

**Why it's a problem:**
- `ArrayList` is **not type-safe** — it can store any type, leading to runtime errors.
- Modern C# uses **`List<T>`** which is type-safe and faster.
- In Selenium automation, you'll work with typed lists like `List<IWebElement>`.

---

## ✅ Improved Solution (Modern C#)

```csharp
using System;
using System.Collections.Generic;

public class CountryList
{
    public static void Main(string[] args)
    {
        List<string> countryNames = new List<string>()
        {
            "India",
            "Malta",
            "Italy",
            "Spain",
            "Japan"
        };
        
        Console.WriteLine("Countries:");
        foreach(string country in countryNames)
        {
            Console.WriteLine($"  - {country}");
        }
    }
}
```

---

## What Changed?

1. **`List<string>` instead of `ArrayList`:**
   ```csharp
   ❌ ArrayList CountryNames = new ArrayList();
   ✅ List<string> countryNames = new List<string>();
   ```
   - `List<T>` is **type-safe** (only stores strings).
   - Faster and more modern.

2. **Collection initializer** (cleaner syntax):
   ```csharp
   List<string> countryNames = new List<string>()
   {
       "India",
       "Malta",
       "Italy",
       "Spain",
       "Japan"
   };
   ```
   Instead of calling `.Add()` five times.

3. **Better naming convention:**
   - ❌ `CountryNames` (PascalCase is for classes, not variables)
   - ✅ `countryNames` (camelCase for local variables)

4. **Added a header and formatting** for readability.

5. **Using `System.Collections.Generic`** instead of `System.Collections`.

---

## Output

```text
Countries:
  - India
  - Malta
  - Italy
  - Spain
  - Japan
```

---

## Key Learning

- **Never use `ArrayList` in modern C#** — always use `List<T>`.
- `List<T>` is **type-safe**, **faster**, and the **industry standard**.
- Collection initializers make code cleaner.
- In Selenium, you'll use `List<IWebElement>` to store multiple web elements from the page.

---

## Extra: How to use List<T> methods

```csharp
List<string> countries = new List<string> { "India", "Malta", "Italy" };

countries.Add("Germany");           // Add item
countries.Remove("Malta");           // Remove item
countries.Count;                     // Get count
countries.Contains("India");         // Check if exists
countries[0];                        // Get first item
countries.Clear();                   // Remove all items
```