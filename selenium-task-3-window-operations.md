# Task 3 – Window Operations: Maximize and FullScreen – Solution

## Objective
Practice browser window operations by using `Maximize()` and `FullScreen()` methods to control the browser window size.

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
        public void WindowOperationsTest()
        {
            driver.Url = "https://www.google.com";
            driver.Manage().Window.Maximize();      // Maximize the window
            driver.Manage().Window.FullScreen();    // Put window in fullscreen
        }

        [TearDown]
        public void TearDown()
        {
            driver.Close();  // Closes the current window
        }
    }
}
```

---

## What You Did Right ✅

1. ✅ Correctly used `driver.Manage().Window.Maximize()` to maximize the window.
2. ✅ Correctly used `driver.Manage().Window.FullScreen()` to enter fullscreen mode.
3. ✅ Added comments explaining what each method does.
4. ✅ Navigated to a website before applying window operations.

---

## Issue with Your Code ❌

### Issue: Using `Close()` instead of `Quit()` in TearDown

```csharp
[TearDown]
public void TearDown()
{
    driver.Close();  // ❌ Only closes the current window
}
```

**Problem:**
- `driver.Close()` closes only the **current window**.
- The **browser process keeps running** after the test.
- This wastes memory and can cause issues with subsequent tests.

**Solution:**
- Use `driver.Quit()` in `TearDown()` to **fully terminate the browser**.

```csharp
[TearDown]
public void TearDown()
{
    driver.Quit();  // ✅ Closes all windows and terminates the browser
}
```

---

## Improved Solution

```csharp
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using WebDriverManager.DriverConfigs.Impl;

namespace SeleniumLearning
{
    public class WindowOperationsTest
    {
        IWebDriver driver;
        
        [SetUp]
        public void Setup()
        {
            new WebDriverManager.DriverManager().SetUpDriver(new ChromeConfig());
            driver = new ChromeDriver();
        }

        [Test]
        public void MaximizeWindowTest()
        {
            // Navigate to Google
            driver.Url = "https://www.google.com";
            
            // Verify page loaded
            Assert.That(driver.Title, Does.Contain("Google"));
            
            // Maximize the window for better element visibility
            driver.Manage().Window.Maximize();
            
            // Log for debugging
            Console.WriteLine($"Window Size after Maximize: {driver.Manage().Window.Size}");
        }

        [Test]
        public void FullScreenWindowTest()
        {
            // Navigate to example.com
            driver.Url = "https://www.example.com";
            
            // Verify page loaded
            Assert.That(driver.Url, Does.Contain("example.com"));
            
            // Enter fullscreen mode (hides browser toolbars)
            driver.Manage().Window.FullScreen();
            
            // Log for debugging
            Console.WriteLine($"Window is now in FullScreen mode");
        }

        [TearDown]
        public void TearDown()
        {
            driver.Quit();  // ✅ Properly terminates the entire browser
        }
    }
}
```

---

## What Changed and Why

| Original | Improved | Why |
|----------|----------|-----|
| One test method | Two separate test methods | Each method tests one specific operation |
| `driver.Close()` in TearDown | `driver.Quit()` in TearDown | Proper resource cleanup |
| No assertions | Added `Assert.That()` | Verify tests actually work |
| No logging | Added `Console.WriteLine()` | Helpful for debugging |
| Generic test name | Specific test names | Clear what each test does |

---

## Understanding Window Operations

### `Maximize()` Method

```csharp
driver.Manage().Window.Maximize();
```

**What it does:**
- Maximizes the browser window to fit the screen.
- Window **still has browser toolbars, address bar, and tabs visible**.
- This is the **most common** setup for automated tests.

**When to use:**
- Testing responsive design for desktop screens.
- Ensuring elements are visible and clickable.
- Most realistic user behavior.

**Example output:**
```
Window Size: 1920 x 1040 (approximately)
```

---

### `FullScreen()` Method

```csharp
driver.Manage().Window.FullScreen();
```

**What it does:**
- Makes the browser **completely fullscreen**.
- **Hides** the browser toolbar, address bar, and tabs.
- Only the webpage content is visible.

**When to use:**
- Testing full webpage layout without browser UI distractions.
- Presentation or demo scenarios.
- Testing maximum available screen space.

**Example output:**
```
Window Size: 1920 x 1080 (entire screen)
No toolbars visible
```

---

## Comparison: Maximize vs FullScreen

| Feature | Maximize() | FullScreen() |
|---------|-----------|-------------|
| **Window size** | Fits screen but smaller | Uses entire screen |
| **Toolbars** | Visible | Hidden |
| **Address bar** | Visible | Hidden |
| **Tabs** | Visible | Hidden |
| **Use case** | Default testing | Full layout testing |
| **Realism** | More realistic | Less realistic |

---

## Additional Window Operations (For Learning)

### Get Current Window Size
```csharp
Size windowSize = driver.Manage().Window.Size;
Console.WriteLine($"Width: {windowSize.Width}, Height: {windowSize.Height}");
```

### Set Custom Window Size
```csharp
driver.Manage().Window.Size = new Size(1280, 720);
```

### Get Window Position
```csharp
Point windowPosition = driver.Manage().Window.Position;
Console.WriteLine($"X: {windowPosition.X}, Y: {windowPosition.Y}");
```

---

## Key Learning Points

1. **Always use `Quit()` in `TearDown()`** – Ensures proper cleanup.

2. **Maximize() is the default** – Most tests use Maximize() for consistency.

3. **FullScreen() hides browser UI** – Useful for testing full webpage layout.

4. **Add assertions** – Verify the page loaded before applying window operations.

5. **Use separate test methods** – One operation per test makes it easier to understand.

---

## Why This Matters in Real Testing

- **Responsive Design:** Maximize ensures you test desktop view properly.
- **Element Visibility:** Large windows help ensure all elements are accessible.
- **Consistency:** Tests run in the same window size across machines.
- **Screenshots:** Consistent window size ensures consistent layout in reports.

---

## Summary

Your code was **very close to perfect**! The only improvement needed was:
- Change `driver.Close()` to `driver.Quit()` in `TearDown()`
- Add assertions to verify pages loaded
- Separate into two test methods (one for Maximize, one for FullScreen)

Great work! 🚀