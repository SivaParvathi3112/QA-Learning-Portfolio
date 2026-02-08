# Task 4 – Cross-Browser Testing: Chrome, Firefox, and Edge – Solution

## Objective
Write automated tests that run the same test case on three different browsers (Chrome, Firefox, and Edge) to verify cross-browser compatibility.

---

## Your Solution

```csharp
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium.Edge;
using OpenQA.Selenium.Firefox;
using WebDriverManager.DriverConfigs.Impl;

namespace SeleniumLearning
{
    public class ChromeTests
    {
        IWebDriver driver;
        
        [SetUp]
        public void Setup()
        {
            new WebDriverManager.DriverManager().SetUpDriver(new ChromeConfig());
            driver = new ChromeDriver();
        }

        [Test]
        public void ChromeHomePageTest()
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

    public class FirefoxTests
    {
        IWebDriver driver;
        
        [SetUp]
        public void Setup()
        {
            new WebDriverManager.DriverManager().SetUpDriver(new FirefoxConfig());
            driver = new FirefoxDriver();
        }

        [Test]
        public void FirefoxHomePageTest()
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

    public class EdgeTests
    {
        IWebDriver driver;
        
        [SetUp]
        public void Setup()
        {
            new WebDriverManager.DriverManager().SetUpDriver(new EdgeConfig());
            driver = new EdgeDriver();
        }

        [Test]
        public void EdgeHomePageTest()
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

---





```












## Key Learning Points

1. **Separate test classes per browser** – Clear, organized, easy to maintain.

2. **One `Quit()` in TearDown** – Not in the test method.

3. **Meaningful test names** – Include browser name for clarity.

4. **Same logic, multiple browsers** – DRY principle (Don't Repeat Yourself).

5. **WebDriverManager handles drivers** – Automatic driver management.

---
