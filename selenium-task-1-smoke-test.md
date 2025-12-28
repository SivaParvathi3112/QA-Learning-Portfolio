# Task 1 – Smoke Test: Open and Close Browser – Solution

## Objective
Write a simple NUnit test that opens Google in Chrome, asserts the page title contains "Google", and closes the browser properly.

---

## Your Solution

```csharp
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using WebDriverManager.DriverConfigs.Impl;
using static System.Net.WebRequestMethods;

namespace SeleniumLearning
{
    public class Tests
    {
        IWebDriver driver;
        
        [SetUp]
        public void Setup()
        {
            new WebDriverManager.DriverManager().SetUpDriver(new ChromeConfig());
            driver = new ChromeDriver();
        }

        [Test]
        public void Test1()
        {
            driver.Url = "https://www.google.com";
        }

        [TearDown]
        public void TearDown()
        {
            driver.Close();
        }
    }
}
```

---

## What You Did Right ✅

1. ✅ Created a test class with `[SetUp]`, `[Test]`, and `[TearDown]` attributes.
2. ✅ Used `WebDriverManager` to handle ChromeDriver setup (excellent practice!).
3. ✅ Navigated to Google using `driver.Url`.
4. ✅ Closed the browser in `TearDown`.

---

## What's Missing ❌

You didn't add an **assertion** to verify that the test actually passed or failed. Right now, the test just opens Google but doesn't check anything.

---

## What is an Assertion?

An **assertion** is a statement that **checks if something is true**. If it's true, the test passes ✅. If it's false, the test fails ❌.

**Example:**
- "Assert that the page title contains 'Google'" → If yes, test passes; if no, test fails.

---

## Improved Solution (With Assertion)

```csharp
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using WebDriverManager.DriverConfigs.Impl;

namespace SeleniumLearning
{
    public class GoogleHomePageTest
    {
        IWebDriver driver;
        
        [SetUp]
        public void Setup()
        {
            new WebDriverManager.DriverManager().SetUpDriver(new ChromeConfig());
            driver = new ChromeDriver();
        }

        [Test]
        public void GooglePageTitleTest()
        {
            // Step 1: Navigate to Google
            driver.Url = "https://www.google.com";
            
            // Step 2: Get the page title
            string pageTitle = driver.Title;
            
            // Step 3: Assert that the title contains "Google"
            Assert.That(pageTitle, Does.Contain("Google"));
        }

        [TearDown]
        public void TearDown()
        {
            driver.Close();
        }
    }
}
```

---

## How the Assertion Works

```csharp
Assert.That(pageTitle, Does.Contain("Google"));
```

**Breaking it down:**
- `Assert.That()` → NUnit method that checks something
- `pageTitle` → The thing we're checking (the actual page title)
- `Does.Contain("Google")` → What we expect (the title should contain the word "Google")

**If the title is `"Google"` or `"Google Search"` → ✅ Test PASSES**  
**If the title is something else → ❌ Test FAILS**

---

## What Changed?

| Original | Improved | Why |
|----------|----------|-----|
| `driver.Url = "..."` | `driver.Url = "..."`; then `string pageTitle = driver.Title;` | We need to **get** the title first to check it |
| No assertion | `Assert.That(pageTitle, Does.Contain("Google"));` | Assertion **verifies** the page loaded correctly |
| `Test1()` | `GooglePageTitleTest()` | Better test name (describes what it tests) |
| Used `driver.Close()` | Could use `driver.Quit()` | `Quit()` is safer for cleanup (closes all windows) |

---

## Alternative Assertion Methods (All do the same thing)

```csharp
// Method 1: Using Does.Contain() (Recommended - Modern)
Assert.That(driver.Title, Does.Contain("Google"));

// Method 2: Using string method Contains()
Assert.IsTrue(driver.Title.Contains("Google"));

// Method 3: Using Assert.Contains() (Simple)
Assert.IsTrue(driver.Title.Contains("Google"), 
    "Page title does not contain 'Google'");
```

**For interviews and industry standard:** Use **Method 1** with `Does.Contain()` – it's the most modern NUnit syntax.

---

## Key Learning Points

1. **Assertions are essential** – A test without an assertion doesn't actually verify anything.
2. **Test names matter** – `GooglePageTitleTest()` is better than `Test1()` because it explains what's being tested.
3. **Get data first, assert second** – Always retrieve the value, then check it.
4. **Use `Quit()` not `Close()`** – Better practice for full cleanup.

---

## Error Handling Note

If your test fails, the output will show:
```
Expected: string containing "Google"
But was: "some other title"
```

This helps you debug quickly.

---

## Summary

Your code was **90% correct**! You just needed to:
1. Get the page title: `string pageTitle = driver.Title;`
2. Add the assertion: `Assert.That(pageTitle, Does.Contain("Google"));`

These two lines transform a test that does nothing into a test that **verifies Google loaded correctly**. 🚀