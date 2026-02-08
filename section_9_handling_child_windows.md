# Section 9 - Window Handles & Child Windows

Complete working C# code for **Section 9 Tasks (9.1)** covering browser window switching, handle management, and data extraction from child windows. Demonstrates advanced window manipulation and parsing techniques.


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
    
    //   Click link that opens new window
    IWebElement blinkingLink = driver.FindElement(By.ClassName("blinkingText"));
    blinkingLink.Click();
    Console.WriteLine("✓ Clicked blinking text link");
    
    //   Get parent window handle BEFORE new window opens
    string parentWindowId = driver.CurrentWindowHandle;
    Console.WriteLine($"✓ Parent window handle stored: {parentWindowId}");
    
    //   Verify window count (should be 2)
    Assert.That(driver.WindowHandles.Count, Is.EqualTo(2), 
                "Should have 2 windows open");
    Console.WriteLine($"✓ Window count verified: {driver.WindowHandles.Count}");
    
    //   Get child window handle
    string childWindowId = driver.WindowHandles[1];
    Console.WriteLine($"✓ Child window handle identified: {childWindowId}");
    
    //  Switch to child window
    driver.SwitchTo().Window(childWindowId);
    Console.WriteLine("✓ Switched to child window");
    
    //  Extract text from child window
    IWebElement redText = driver.FindElement(By.ClassName("red"));
    string fullText = redText.Text;
    Console.WriteLine($"✓ Text extracted: {fullText}");
    
    // Example text: "Please email us at mentor@rahulshettyacademy.com with below template"
    
    //  Parse email using String operations
    string[] splitAtText = fullText.Split("at");
    string afterAt = splitAtText[1].Trim(); // " mentor@rahulshettyacademy.com with below..."
    
    string[] words = afterAt.Split(" ");
    string extractedEmail = words[0]; // "mentor@rahulshettyacademy.com"
    
    Console.WriteLine($"✓ Email parsed: {extractedEmail}");
    
    //  Verify extracted email
    Assert.That(extractedEmail, Is.EqualTo(expectedEmail), 
                "Extracted email should match expected");
    Console.WriteLine("✓ Email verification passed");
    
    //  Switch back to parent window
    driver.SwitchTo().Window(parentWindowId);
    Console.WriteLine("✓ Switched back to parent window");
    
    //  Populate username field with extracted email
    IWebElement usernameField = driver.FindElement(By.Id("username"));
    usernameField.SendKeys(extractedEmail);
    Console.WriteLine($"✓ Username field populated with: {extractedEmail}");
    
    //  Verify field value
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

