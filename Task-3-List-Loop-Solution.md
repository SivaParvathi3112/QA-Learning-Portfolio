# Task 3 – List and Loop – Solution

## Objective
Practise collections and `foreach` loops.

---


##  Solution

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
