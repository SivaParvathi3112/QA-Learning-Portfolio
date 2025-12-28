# Task 2 – Close vs Quit Browser Methods – Solution

## Objective
Understand the difference between `driver.Close()` and `driver.Quit()` by writing two separate tests demonstrating each method.

---

## Your Solution

```csharp
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using WebDriverManager.DriverConfigs.Impl;

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
        public void CloseBrowserTest()
        {
            driver.Url = "https://www.google.com";
            driver.Close();   // Closes the current window
        }
        
        [Test]
        public void QuitBrowserTest()
        {
            driver.Url = "https://rahulshettyacademy.com/loginpagePractise/";
            driver.Quit();   // Shuts down the entire browser session
        }

        [TearDown]
        public void TearDown()
        {
            driver.Quit();
        }
    }
}
```

---

## What You Did Right ✅

1. ✅ Created two separate test methods to compare `Close()` vs `Quit()`.
2. ✅ Added comments explaining what each method does.
3. ✅ Used different URLs to test different websites.
4. ✅ Properly added `driver.Quit()` in `[TearDown]` for cleanup.
5. ✅ Understood the conceptual difference between the two methods.

---

## What Could Be Improved ⚠️

### Improvement 1: Add assertions to verify tests

Right now the tests just navigate to pages but don't **verify** anything. Add assertions:

```csharp
[Test]
public void CloseBrowserTest()
{
    driver.Url = "https://www.google.com";
    
    // Verify the page loaded
    Assert.That(driver.Title, Does.Contain("Google"));
    
    driver.Close();   // Closes the current window
}

[Test]
public void QuitBrowserTest()
{
    driver.Url = "https://rahulshettyacademy.com/loginpagePractise/";
    
    // Verify the page loaded
    Assert.That(driver.Url, Does.Contain("loginpage"));
    
    driver.Quit();   // Shuts down the entire browser session
}
```

### Improvement 2: Better test names

```csharp
// Instead of generic names, use descriptive names
[Test]
public void GooglePageWithCloseMethodTest()  // More descriptive
{
    // code
}

[Test]
public void LoginPageWithQuitMethodTest()  // More descriptive
{
    // code
}
```

### Improvement 3: Handle the Quit() called twice issue

Since `driver.Quit()` is called **inside the test** AND **in TearDown()**, you might get an error on the second Quit call. Here's the best approach:

```csharp
[Test]
public void QuitBrowserTest()
{
    driver.Url = "https://rahulshettyacademy.com/loginpagePractise/";
    Assert.That(driver.Url, Does.Contain("loginpage"));
    
    driver.Quit();  // Terminate the browser in the test
}

[TearDown]
public void TearDown()
{
    // TearDown will try to Quit() again, which will cause an error
    // Better approach: Only call Quit() in TearDown(), NOT in the test
}
```

---

## Better Solution (Recommended)

```csharp
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using WebDriverManager.DriverConfigs.Impl;

namespace SeleniumLearning
{
    public class BrowserCloseVsQuitTest
    {
        IWebDriver driver;
        
        [SetUp]
        public void Setup()
        {
            new WebDriverManager.DriverManager().SetUpDriver(new ChromeConfig());
            driver = new ChromeDriver();
        }

        [Test]
        public void GooglePageCloseBrowserTest()
        {
            // Navigate to Google
            driver.Url = "https://www.google.com";
            
            // Verify page loaded
            Assert.That(driver.Title, Does.Contain("Google"));
            
            // Close only the current window (browser process still runs)
            driver.Close();
        }
        
        [Test]
        public void LoginPageQuitBrowserTest()
        {
            // Navigate to login page
            driver.Url = "https://rahulshettyacademy.com/loginpagePractise/";
            
            // Verify page loaded
            Assert.That(driver.Url, Does.Contain("loginpage"));
            
            // Quit terminates the entire browser (no need to call Quit() in TearDown)
            driver.Quit();
        }

        [TearDown]
        public void TearDown()
        {
            // Always clean up: Quit() the browser after each test
            // If the test called Quit(), this will fail, but that's okay
            // because the browser is already closed
            driver.Quit();
        }
    }
}
```

---

## Understanding Close() vs Quit()

| Method | What it does | When to use |
|--------|-------------|------------|
| **`driver.Close()`** | Closes **only the current window/tab** | When testing multiple windows and you want to switch between them |
| **`driver.Quit()`** | Closes **ALL windows** and **terminates the entire browser process** | At the end of your test (in `[TearDown]`) to clean up resources |

---

## Visual Flow

### Test with `Close()`:
```
[SetUp] → Start Chrome browser
   ↓
[Test] → Navigate to Google
   ↓
      → driver.Close() (closes the window)
      → Browser process still RUNNING ⚠️
   ↓
[TearDown] → driver.Quit() (finally terminates the browser)
```

### Test with `Quit()`:
```
[SetUp] → Start Chrome browser
   ↓
[Test] → Navigate to login page
   ↓
      → driver.Quit() (closes all windows AND ends the browser)
   ↓
[TearDown] → driver.Quit() (tries to quit an already-closed browser)
```

---

## Key Learning Points

1. **Always put `driver.Quit()` in `[TearDown]`** – This ensures the browser closes after every test.

2. **Use `Close()` rarely** – Only use it if you're testing multiple windows/tabs and need to switch between them.

3. **Add assertions** – Tests become meaningful only when they verify something.

4. **Use meaningful names** – Test names should describe what's being tested (not just `Test1`, `Test2`).

5. **Understand the cleanup flow** – Know when the browser starts and when it stops.

---

## Common Mistakes to Avoid

❌ **Empty `TearDown()`** → Browser processes pile up and consume memory

✅ **Always have `driver.Quit()` in `TearDown()`**

---

## Summary

Your code now has the right structure! You:
- ✅ Understand `Close()` vs `Quit()`
- ✅ Have proper cleanup in `TearDown()`
- ✅ Created two tests to demonstrate both methods

Next step: Add **assertions** to make your tests verify something meaningful! 🚀