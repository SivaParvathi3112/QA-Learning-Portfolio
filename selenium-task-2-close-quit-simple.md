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
