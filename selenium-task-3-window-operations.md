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
            driver.Quit();  // Closes the current window
        }
    }
}
```














```




## Key Learning Points

1. **Always use `Quit()` in `TearDown()`** – Ensures proper cleanup.

2. **Maximize() is the default** – Most tests use Maximize() for consistency.

3. **FullScreen() hides browser UI** – Useful for testing full webpage layout.

4. **Add assertions** – Verify the page loaded before applying window operations.

5. **Use separate test methods** – One operation per test makes it easier to understand.

