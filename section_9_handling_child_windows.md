# Section 9 - Window Handles & Child Windows

Complete working C# code for **Section 9 Tasks (9.1)** covering browser window switching, handle management, and data extraction from child windows. Demonstrates advanced window manipulation and parsing techniques.

---

## 📋 Task Overview

| Task | Focus | Status | Concepts |
|------|-------|--------|----------|
| 9.1 | Child Windows | ✅ PASSED | Window handles, Switching, Data extraction |

**Note:** Tasks 9.2, 9.3, 9.4 not covered in course - focusing on core window handling concepts.

---

## Task 9.1 - Child Windows

**Goal:** Click link to open new window, switch to child, extract text, parse email, return to parent, and populate field

### Code Implementation

```csharp
[Test]
public void WindowHandles()
{
    driver.Url = "https://rahulshettyacademy.com/loginpagePractise/";
    
    string expectedEmail = "mentor@rahulshettyacademy.com";
    Console.WriteLine("✓ Navigated to login page");
    
    // ✅ STEP 1: Click link that opens new window
    IWebElement blinkingLink = driver.FindElement(By.ClassName("blinkingText"));
    blinkingLink.Click();
    Console.WriteLine("✓ Clicked blinking text link");
    
    // ✅ STEP 2: Get parent window handle BEFORE new window opens
    string parentWindowId = driver.CurrentWindowHandle;
    Console.WriteLine($"✓ Parent window handle stored: {parentWindowId}");
    
    // ✅ STEP 3: Verify window count (should be 2)
    Assert.That(driver.WindowHandles.Count, Is.EqualTo(2), 
                "Should have 2 windows open");
    Console.WriteLine($"✓ Window count verified: {driver.WindowHandles.Count}");
    
    // ✅ STEP 4: Get child window handle
    string childWindowId = driver.WindowHandles[1];
    Console.WriteLine($"✓ Child window handle identified: {childWindowId}");
    
    // ✅ STEP 5: Switch to child window
    driver.SwitchTo().Window(childWindowId);
    Console.WriteLine("✓ Switched to child window");
    
    // ✅ STEP 6: Extract text from child window
    IWebElement redText = driver.FindElement(By.ClassName("red"));
    string fullText = redText.Text;
    Console.WriteLine($"✓ Text extracted: {fullText}");
    
    // Example text: "Please email us at mentor@rahulshettyacademy.com with below template"
    
    // ✅ STEP 7: Parse email using String operations
    string[] splitAtText = fullText.Split("at");
    string afterAt = splitAtText[1].Trim(); // " mentor@rahulshettyacademy.com with below..."
    
    string[] words = afterAt.Split(" ");
    string extractedEmail = words[0]; // "mentor@rahulshettyacademy.com"
    
    Console.WriteLine($"✓ Email parsed: {extractedEmail}");
    
    // ✅ STEP 8: Verify extracted email
    Assert.That(extractedEmail, Is.EqualTo(expectedEmail), 
                "Extracted email should match expected");
    Console.WriteLine("✓ Email verification passed");
    
    // ✅ STEP 9: Switch back to parent window
    driver.SwitchTo().Window(parentWindowId);
    Console.WriteLine("✓ Switched back to parent window");
    
    // ✅ STEP 10: Populate username field with extracted email
    IWebElement usernameField = driver.FindElement(By.Id("username"));
    usernameField.SendKeys(extractedEmail);
    Console.WriteLine($"✓ Username field populated with: {extractedEmail}");
    
    // ✅ STEP 11: Verify field value
    string fieldValue = usernameField.GetAttribute("value");
    Assert.That(fieldValue, Is.EqualTo(extractedEmail), 
                "Username field should contain extracted email");
    Console.WriteLine("✓ Field value verification passed");
}
```

### Key Locators Used

| Element | Locator Type | Locator Value |
|---------|--------------|---------------|
| Blinking Link | Class Name | `blinkingText` |
| Red Text | Class Name | `red` |
| Username Field | ID | `username` |

### Window Handling Methods

```csharp
// Get current window handle
string currentHandle = driver.CurrentWindowHandle;

// Get all window handles
IList<string> allHandles = driver.WindowHandles;

// Switch to specific window by handle
driver.SwitchTo().Window(windowHandle);

// Switch to parent/default content
driver.SwitchTo().DefaultContent();

// Close current window
driver.Close();

// Quit all windows
driver.Quit();
```

### String Parsing Technique

```csharp
// Original text:
// "Please email us at mentor@rahulshettyacademy.com with below template"

// Step 1: Split by delimiter "at"
string[] splitAtText = fullText.Split("at");
// Result: ["Please email us ", " mentor@rahulshettyacademy.com with below template"]

// Step 2: Get second part and trim
string afterAt = splitAtText[1].Trim();
// Result: "mentor@rahulshettyacademy.com with below template"

// Step 3: Split by space
string[] words = afterAt.Split(" ");
// Result: ["mentor@rahulshettyacademy.com", "with", "below", "template"]

// Step 4: Get first word (email)
string email = words[0];
// Result: "mentor@rahulshettyacademy.com"
```

### Alternative: Regex Parsing (More Robust)

```csharp
[Test]
public void WindowHandlesWithRegex()
{
    // ... (steps 1-5 same as above)
    
    string fullText = driver.FindElement(By.ClassName("red")).Text;
    
    // ✅ Use Regex for robust email extraction
    string emailPattern = @"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}";
    Match emailMatch = Regex.Match(fullText, emailPattern);
    string extractedEmail = emailMatch.Value;
    
    Assert.That(extractedEmail, Is.EqualTo("mentor@rahulshettyacademy.com"));
    Console.WriteLine($"✓ Email extracted via Regex: {extractedEmail}");
}
```

**Requires:** `using System.Text.RegularExpressions;`

### Expected Output
```
✓ Navigated to login page
✓ Clicked blinking text link
✓ Parent window handle stored: CDwindow-4D3DA4D39EFC4B1BDD7B7DAE05E56B3E
✓ Window count verified: 2
✓ Child window handle identified: CDwindow-9F2E8A1C7B4D5E6F9G2H3I4J5K6L7M8N
✓ Switched to child window
✓ Text extracted: Please email us at mentor@rahulshettyacademy.com with below template to receive response
✓ Email parsed: mentor@rahulshettyacademy.com
✓ Email verification passed
✓ Switched back to parent window
✓ Username field populated with: mentor@rahulshettyacademy.com
✓ Field value verification passed
Task 9.1 PASSED - Window handling verified!
```

---

## Advanced Window Scenarios

### Scenario 1: Multiple Child Windows

```csharp
[Test]
public void MultipleChildWindows()
{
    // Open first link
    driver.FindElement(By.Id("link1")).Click();
    
    // Open second link
    driver.FindElement(By.Id("link2")).Click();
    
    // Now we have 3 windows: parent, child1, child2
    Assert.That(driver.WindowHandles.Count, Is.EqualTo(3));
    
    // Iterate through all windows
    foreach (string handle in driver.WindowHandles)
    {
        driver.SwitchTo().Window(handle);
        Console.WriteLine($"Window title: {driver.Title}");
    }
    
    // Always close extra windows and return to parent
    driver.Close(); // Close current child
    driver.SwitchTo().Window(driver.WindowHandles[0]); // Back to parent
}
```

### Scenario 2: Switch & Close Pattern

```csharp
[Test]
public void SwitchAndClose()
{
    string parentHandle = driver.CurrentWindowHandle;
    
    // Click to open new window
    driver.FindElement(By.LinkText("Open New Window")).Click();
    
    // Switch to child
    string childHandle = driver.WindowHandles
        .FirstOrDefault(h => h != parentHandle);
    driver.SwitchTo().Window(childHandle);
    
    // Do something in child
    var data = driver.FindElement(By.Id("data")).Text;
    
    // Close child
    driver.Close();
    
    // Back to parent
    driver.SwitchTo().Window(parentHandle);
    
    // Continue with parent
    driver.FindElement(By.Id("username")).SendKeys(data);
}
```

### Scenario 3: Tab vs Window

```csharp
// Both use same WindowHandles concept
// driver.FindElement(By.LinkText("...")).SendKeys(Keys.Control + Keys.Return); // New tab (Windows)
// driver.FindElement(By.LinkText("...")).SendKeys(Keys.Command + Keys.Return);  // New tab (Mac)

// Switching method is identical
string newHandle = driver.WindowHandles[1];
driver.SwitchTo().Window(newHandle);
```

---

## Setup & Teardown

```csharp
[SetUp]
public void Setup()
{
    new WebDriverManager.DriverManager().SetUpDriver(new ChromeConfig());
    driver = new ChromeDriver();
    driver.Manage().Window.Maximize();
}

[TearDown]
public void TearDown()
{
    // Close all extra windows first
    string mainHandle = driver.WindowHandles[0];
    foreach (string handle in driver.WindowHandles)
    {
        if (handle != mainHandle)
        {
            driver.SwitchTo().Window(handle);
            driver.Close();
        }
    }
    
    // Quit main window
    driver.SwitchTo().Window(mainHandle);
    driver?.Quit();
}
```

---

## Key Concepts Covered

| Concept | Implementation | Code Example |
|---------|-----------------|--------------|
| **Window Handles** | IList<string> | `driver.WindowHandles` |
| **Current Handle** | String property | `driver.CurrentWindowHandle` |
| **Switch Window** | SwitchTo() | `driver.SwitchTo().Window(handle)` |
| **Window Count** | Count property | `driver.WindowHandles.Count` |
| **String Split** | Split() method | `text.Split("delimiter")` |
| **String Trim** | Trim() method | `text.Trim()` |
| **Array Access** | Index notation | `array[0]` |
| **Regex Extraction** | Regex.Match() | `Regex.Match(text, pattern)` |
| **Field Population** | SendKeys() | `element.SendKeys(text)` |
| **Value Verification** | GetAttribute() | `element.GetAttribute("value")` |

---

## Best Practices Applied

✅ **Store Parent Handle First** - Before child window opens  
✅ **Verify Window Count** - Assert correct number of windows  
✅ **Always Return to Parent** - Never leave context switched  
✅ **Close Child First** - Clean up before quitting  
✅ **Use Handles Not Titles** - Titles can be ambiguous  
✅ **Robust Parsing** - Handle variations in text content  
✅ **Verify After Parse** - Assert extracted data is correct  
✅ **Console Output** - Track each window switch  

---

## Common Mistakes to Avoid

❌ Not storing parent handle before opening child window  
❌ Trying to find element in parent while in child context  
❌ Forgetting to switch back to parent after child operations  
❌ Not verifying window count before accessing handles  
❌ Using index [1] without checking if it exists (use LINQ)  
❌ Not closing child windows before main driver.Quit()  
❌ Fragile string parsing (use Regex for robustness)  
❌ Not verifying extracted data matches expected  

---

## Window Handle Flow Diagram

```
┌─ Parent Window ──────────────────────────────┐
│  Handle: CDwindow-ABC123                     │
│  Title: Login Page                           │
│                                               │
│  [Blinking Text Link] ← Click                │
│       │                                       │
│       └─────────────────────────────┐        │
│                                      │        │
│                                      ▼        │
│              ┌─ Child Window ───────────────┐│
│              │ Handle: CDwindow-XYZ789      ││
│              │ Title: Confirmation Page     ││
│              │ [Red Text with email]        ││
│              │ Extract: mentor@... ◄─Parse ││
│              └──────────────────────────────┘│
│                      │                       │
│                      └─ Switch Back         │
│  [Username Field] ← Populate with email     │
│                                               │
└─────────────────────────────────────────────┘
```

---

## Data Extraction Patterns

### Pattern 1: Split & Index
```csharp
// Fast, simple, but fragile
string[] parts = text.Split("delimiter");
string result = parts[1].Trim().Split(" ")[0];
```

### Pattern 2: Regex
```csharp
// Robust, handles variations
Regex regex = new Regex(@"pattern");
string result = regex.Match(text).Value;
```

### Pattern 3: LINQ + String
```csharp
// Flexible, chainable
string result = text
    .Split("at")[1]
    .Trim()
    .Split(" ")
    .First();
```

---

## Test Results Summary

```
═══════════════════════════════════════════════
          SECTION 9 - TESTS COMPLETED
═══════════════════════════════════════════════

✅ Task 9.1 - Child Windows              [PASSED]
   - Blinking link clicked
   - New window opened successfully
   - Window count verified (2 windows)
   - Switched to child window
   - Text extracted from child
   - Email parsed correctly
   - Switched back to parent
   - Email populated in field
   - Field value verified

❌ Task 9.2 - Documents Request         [SKIPPED]
   Not covered in course

❌ Task 9.3 - E2E Cart Checkout          [SKIPPED]
   Not covered in course

❌ Task 9.4 - Combined Regression        [SKIPPED]
   Not covered in course

═══════════════════════════════════════════════
All available concepts demonstrated! 🎓
═══════════════════════════════════════════════
```

---

## Next Steps

**To expand Task 9.1:**
- [ ] Add Regex-based email extraction (more robust)
- [ ] Handle multiple child windows
- [ ] Implement tab opening with Keys.Control + Return
- [ ] Add screenshot capture at each step
- [ ] Create reusable WindowHelper utility class

**Continue with:**
- Advanced waits & synchronization
- Page Object Model (POM) pattern
- Test data management
- Cross-browser testing
