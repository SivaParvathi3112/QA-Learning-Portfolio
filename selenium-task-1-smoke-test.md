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
            string pageTitle = driver.Title;
            Assert.That(pageTitle, Does.Contain("Google"));
        }

        [TearDown]
        public void TearDown()
        {
            driver.Quit();
        }
    }
}
```



## Key Learning Points

1. **Assertions are essential** – A test without an assertion doesn't actually verify anything.
2. **Test names matter** – `GooglePageTitleTest()` is better than `Test1()` because it explains what's being tested.
3. **Get data first, assert second** – Always retrieve the value, then check it.
4. **Use `Quit()` not `Close()`** – Better practice for full cleanup.

---

