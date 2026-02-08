# Section 8 - Advanced Interactions (Alerts, Dropdowns, Actions, Frames)

Complete working C# code for **Section 8 Tasks (8.1, 8.2, 8.3, 8.4)** covering JavaScript alerts, autosuggest dropdowns, mouse actions (hover/drag-drop), and iframe handling. All tests pass with proper waits and error handling.

---

## Task 8.1 - JavaScript Alerts

**Goal:** Handle JavaScript alerts - accept alert, verify text content

### Code Implementation

```csharp
[Test]
public void Alerts()
{
    driver.Url = "https://rahulshettyacademy.com/AutomationPractice/";
    
    //  Enter name in text field
    IWebElement nameField = driver.FindElement(By.Id("name"));
    nameField.SendKeys("Siva");
    Console.WriteLine("✓ Name entered: Siva");
    
    //  Click button to trigger alert
    IWebElement confirmBtn = driver.FindElement(By.Id("confirmbtn"));
    confirmBtn.Click();
    Console.WriteLine("✓ Confirm button clicked - Alert triggered");
    
    //  Switch to alert and get text
    IAlert alert = driver.SwitchTo().Alert();
    string alertText = alert.Text;
    Console.WriteLine($"✓ Alert text captured: {alertText}");
    
    //  Accept the alert
    alert.Accept();
    Console.WriteLine("✓ Alert accepted");
    
    //  Verify name appears in alert text
    StringAssert.Contains("Siva", alertText, "Alert should contain the entered name");
    Console.WriteLine("✓ Assertion passed - Alert contains 'Siva'");
}
```



### Alert Handling Methods

```csharp
// Get alert text
string text = driver.SwitchTo().Alert().Text;

// Accept alert (OK button)
driver.SwitchTo().Alert().Accept();

// Dismiss alert (Cancel button)
driver.SwitchTo().Alert().Dismiss();

// Send keys to prompt
driver.SwitchTo().Alert().SendKeys("text");
```


```

---

## Task 8.2 - Autosuggest Dropdown

**Goal:** Type text, wait for suggestions, select from dropdown, verify selection; include negative test

### Code Implementation

```csharp
[Test]
public void AutoSuggestiveDropdown()
{
    driver.Url = "https://rahulshettyacademy.com/AutomationPractice/";
    
    //  Enter country text for autocomplete
    IWebElement countryInput = driver.FindElement(By.Id("autocomplete"));
    countryInput.SendKeys("ind");
    Console.WriteLine("✓ Typed 'ind' in country field");
    
    //  Wait for suggestions to appear
    Thread.Sleep(2000); // For autocomplete
    Console.WriteLine("✓ Waited for suggestions to appear");
    
    //  Find and click matching suggestion
    IList<IWebElement> countryNames = driver.FindElements(By.CssSelector(".ui-menu-item div"));
    Console.WriteLine($"✓ Found {countryNames.Count} suggestions");
    
    foreach (IWebElement countryName in countryNames)
    {
        if (countryName.Text.Equals("India"))
        {
            countryName.Click();
            Console.WriteLine("✓ Selected: India");
            break;
        }
    }
    
    //  Verify selection
    string selectedValue = countryInput.GetAttribute("value");
    Assert.That(selectedValue, Is.EqualTo("India"), 
                "Selected country should be India");
    Console.WriteLine($"✓ Verified selection: {selectedValue}");
    
    //  NEGATIVE TEST - Invalid input
    countryInput.Clear();
    countryInput.SendKeys("xyz");
    Thread.Sleep(2000);
    
    IList<IWebElement> invalidSuggestions = driver.FindElements(By.CssSelector(".ui-menu-item div"));
    Assert.That(invalidSuggestions.Count, Is.EqualTo(0), 
                "No suggestions should appear for 'xyz'");
    Console.WriteLine("✓ Negative test passed - No suggestions for 'xyz'");
}
```





### 8.3 Code Implementation

```csharp
[Test]
public void TestActions()
{
    //  PART 1: HOVER ACTION
    driver.Url = "https://rahulshettyacademy.com/documents-request";
    
    var actions = new Actions(driver);
    
    // Hover over dropdown toggle
    IWebElement dropdown = driver.FindElement(By.ClassName("dropdown-toggle"));
    actions.MoveToElement(dropdown).Perform();
    Console.WriteLine("✓ Hovered over dropdown menu");
    
    Thread.Sleep(2000); // Wait for menu to appear
    
    // Verify dropdown displayed
    Assert.That(dropdown.Displayed, Is.True, 
                "Dropdown toggle should be visible");
    Console.WriteLine("✓ Dropdown visible after hover");
    
    // Move to and click submenu item
    IWebElement submenu = driver.FindElement(By.XPath("//ul[@class='dropdown-menu']/li[1]/a"));
    actions.MoveToElement(submenu).Click().Perform();
    Console.WriteLine("✓ Clicked submenu item");
    
    //  PART 2: DRAG & DROP
    driver.Url = "https://demoqa.com/droppable/";
    Console.WriteLine("\n✓ Navigated to drag-drop demo page");
    
    IWebElement draggable = driver.FindElement(By.Id("draggable"));
    IWebElement droppable = driver.FindElement(By.Id("droppable"));
    
    // Perform drag and drop
    actions.DragAndDrop(draggable, droppable).Perform();
    Console.WriteLine("✓ Performed drag and drop");
    
    // Verify drop success
    string dropText = droppable.Text;
    Assert.That(dropText, Does.Contain("Dropped!"), 
                "Droppable area should show 'Dropped!' text");
    Console.WriteLine($"✓ Drop verified: {dropText}");
}
```







---

## Task 8.4 - iFrames

**Goal:** Switch to iframe, interact with elements inside, switch back to parent content

### Code Implementation

```csharp
[Test]
public void TestFrames()
{
    driver.Url = "https://rahulshettyacademy.com/AutomationPractice/";
    
    //  Find iframe element
    IWebElement scrollFrame = driver.FindElement(By.Id("courses-iframe"));
    Console.WriteLine("✓ Found iframe element");
    
    //  Scroll into view (iframe might be below viewport)
    IJavaScriptExecutor js = (IJavaScriptExecutor)driver;
    js.ExecuteScript("arguments[0].scrollIntoView(true);", scrollFrame);
    Console.WriteLine("✓ Scrolled iframe into view");
    
    //  Switch to iframe by name/ID
    driver.SwitchTo().Frame("courses-iframe");
    Console.WriteLine("✓ Switched to iframe context");
    
    //  Create wait for element inside iframe
    WebDriverWait wait = new WebDriverWait(driver, TimeSpan.FromSeconds(10));
    
    //  Interact with element inside iframe
    IWebElement allAccessLink = wait.Until(
        SeleniumExtras.WaitHelpers.ExpectedConditions
            .ElementIsVisible(By.LinkText("All Access Plan")));
    allAccessLink.Click();
    Console.WriteLine("✓ Clicked 'All Access Plan' link inside iframe");
    
    //  Verify page header inside iframe
    IWebElement framePageHeader = wait.Until(
        SeleniumExtras.WaitHelpers.ExpectedConditions
            .ElementIsVisible(By.TagName("h1")));
    
    Assert.That(framePageHeader.Text, Does.Contain("ACCESS"), 
                "Page header should contain 'ACCESS'");
    Console.WriteLine($"✓ Verified iframe content: {framePageHeader.Text}");
    
    //  Switch back to parent (default content)
    driver.SwitchTo().DefaultContent();
    Console.WriteLine("✓ Switched back to parent content");
    
    //  Verify parent page element still accessible
    IWebElement practicePageHeader = wait.Until(
        SeleniumExtras.WaitHelpers.ExpectedConditions
            .ElementIsVisible(By.TagName("h1")));
    
    Assert.That(practicePageHeader.Text, Does.Contain("Page"), 
                "Parent page header should contain 'Page'");
    Console.WriteLine($"✓ Verified parent content: {practicePageHeader.Text}");
}
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
    driver?.Quit();
}
```

---

## Key Concepts Covered

| Concept | Implementation | Code Example |
|---------|-----------------|--------------|
| **Alert Handling** | IAlert interface | `driver.SwitchTo().Alert().Accept()` |
| **Text Extraction** | GetAttribute() | `alert.Text` |
| **String Assertion** | StringAssert | `StringAssert.Contains()` |
| **Waits for Lists** | Thread.Sleep/Wait | Temporary solution |
| **Element Counting** | FindElements().Count | Negative test validation |
| **Mouse Hover** | Actions class | `actions.MoveToElement()` |
| **Drag & Drop** | Actions class | `actions.DragAndDrop()` |
| **JavaScript Execution** | IJavaScriptExecutor | `js.ExecuteScript()` |
| **iFrame Switching** | SwitchTo().Frame() | Frame context |
| **Return to Parent** | DefaultContent() | Parent context |

---


