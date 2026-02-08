# Section 6: Implicit & Explicit Waits - Complete Guide

##  Section 6 Overview

**Topics Covered:**
- Implicit Wait (Global timeout)
- Explicit Wait (Specific conditions)
- Handling async elements

---

## 🎯 Your Complete Code

### Full Wait_Tasks.cs Class

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium;
using WebDriverManager.DriverConfigs.Impl;
using OpenQA.Selenium.Support.UI;

namespace SeleniumLearning
{
    internal class Wait_Tasks
    {
        IWebDriver driver;
        
        [SetUp]
        public void SetUp()
        {
            _ = new WebDriverManager.DriverManager().SetUpDriver(new ChromeConfig());
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
            driver.Url = "https://rahulshettyacademy.com/loginpagePractise/";
            
            // ============ IMPLICIT WAIT ============
            // Declared globally - applies to ALL FindElement() calls
            // Waits UP TO 5 seconds if element not found immediately
            driver.Manage().Timeouts().ImplicitWait = TimeSpan.FromSeconds(5);
        }

        [Test]
        public void Implicit_Explicit_Waits()
        {
            Console.WriteLine("=== Test Start: Implicit & Explicit Waits ===\n");

            // Step 1: Enter Username (implicit wait handles if page slow)
            Console.WriteLine("Step 1: Finding & typing username...");
            driver.FindElement(By.Id("username")).SendKeys("Sivaparvathi");
            Console.WriteLine(" Username entered: Sivaparvathi\n");

            // Step 2: Enter Password (implicit wait applies here too)
            Console.WriteLine("Step 2: Finding & typing password...");
            driver.FindElement(By.Name("password")).SendKeys("12345");
            Console.WriteLine(" Password entered: 12345\n");

            // Step 3: Click Terms Checkbox (implicit wait)
            Console.WriteLine("Step 3: Clicking terms checkbox...");
            driver.FindElement(By.XPath("//div[@class='form-group'][5]/label/span/input")).Click();
            Console.WriteLine(" Terms checkbox clicked\n");

            // Step 4: Click Login Button (implicit wait)
            Console.WriteLine("Step 4: Clicking login button...");
            driver.FindElement(By.XPath("//input[@type = 'submit']")).Click();
            Console.WriteLine(" Login button clicked\n");

            // ============ EXPLICIT WAIT ============
            // Wait for SPECIFIC condition - element value contains "Sign In"
            Console.WriteLine("Step 5: Waiting for specific condition...");
            WebDriverWait wait = new WebDriverWait(driver, TimeSpan.FromSeconds(5));
            
            // This waits for SignInBtn element's VALUE to contain "Sign In"
            wait.Until(SeleniumExtras.WaitHelpers.ExpectedConditions
                .TextToBePresentInElementValue(
                    driver.FindElement(By.Id("SignInBtn")), 
                    "Sign In"
                ));
            Console.WriteLine(" SignInBtn found with correct text\n");

            // Step 6: Get and verify error message
            Console.WriteLine("Step 6: Getting error message...");
            string ErrorMessage = driver.FindElement(By.ClassName("alert")).Text;
            Console.WriteLine(" Error Message: " + ErrorMessage);
            
            // Verify error message exists (login failed as expected)
            Assert.That(ErrorMessage, Does.Contain("password"));
            Console.WriteLine(" Error message verified\n");

            Console.WriteLine("=== Test Complete: PASSED ===");
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

## 📊 Concept Breakdown

### **Implicit Wait (SetUp)**

```csharp
driver.Manage().Timeouts().ImplicitWait = TimeSpan.FromSeconds(5);
```

**What it does:**
- Sets a **GLOBAL** timeout of 5 seconds
- Applies to **ALL** `FindElement()` calls
- If element not found immediately, waits UP TO 5 seconds
- Returns element as soon as found (doesn't always wait 5 seconds)

**Where it applies:**
```csharp
// All of these use implicit wait (5 seconds each):
driver.FindElement(By.Id("username"))           // Waits max 5 sec
driver.FindElement(By.Name("password"))         // Waits max 5 sec
driver.FindElement(By.XPath("//div[@class='form-group'][5]/label/span/input"))  // Waits max 5 sec
driver.FindElement(By.XPath("//input[@type = 'submit']"))  // Waits max 5 sec
```

**Pros:**
- ✅ Simple (one line in SetUp)
- ✅ Applies globally
- ✅ No code repetition

**Cons:**
- ❌ Fixed timeout for all elements
- ❌ Not flexible

---

### **Explicit Wait (In Test)**

```csharp
WebDriverWait wait = new WebDriverWait(driver, TimeSpan.FromSeconds(5));
wait.Until(SeleniumExtras.WaitHelpers.ExpectedConditions
    .TextToBePresentInElementValue(
        driver.FindElement(By.Id("SignInBtn")), 
        "Sign In"
    ));
```

**What it does:**
- Creates a **SPECIFIC** wait for ONE condition
- Waits UP TO 5 seconds for that condition
- Checks the condition repeatedly until it's true or timeout

**Condition Used: `TextToBePresentInElementValue`**
- Waits for an element's **VALUE attribute** to contain specific text
- Example: Waiting for button value to change to "Sign In"

**Flow:**
```
1. Create wait object (5 seconds)
2. Find SignInBtn element
3. Check if its VALUE contains "Sign In"
4. If YES → Return element immediately ✅
5. If NO → Keep checking every 500ms
6. If 5 seconds pass and still NO → Timeout ❌
```

**Pros:**
- ✅ Flexible (specific conditions)
- ✅ Can target specific elements
- ✅ More reliable than implicit wait

**Cons:**
- ❌ More code
- ❌ Must write for each condition

---



---

## 📋 When Implicit Wait is Used

Implicit wait applies to:
- `FindElement()` 
- `FindElements()`
- Any search for element on page

Does NOT apply to:
- `GetAttribute()`
- `SendKeys()`
- `Click()`
- Text assertions

---

## 📋 When Explicit Wait is Used

Explicit wait is used when:
- ✅ Element takes time to appear (AJAX)
- ✅ Need to wait for specific text change
- ✅ Element value changes dynamically
- ✅ Need to wait for specific condition

**Your example:** Waiting for button text to show "Sign In" after login attempt

---

## 💡 Key Learnings

### **Implicit Wait:**
- Declared in `[SetUp]`
- Global (all FindElements)
- 5 seconds timeout
- Automatic for all element searches

### **Explicit Wait:**
- Declared in test method when needed
- Specific to one condition
- 5 seconds timeout
- Must specify expected condition

### **Expected Condition Used:**
`TextToBePresentInElementValue()` - Waits for element value attribute to contain text

---
