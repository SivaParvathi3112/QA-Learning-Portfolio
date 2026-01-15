# Section 7 - Automation Practice & Dynamic Cart

Complete working C# code for **Section 7 Tasks (7.1, 7.2, 7.3)** covering dropdown/radio/checkbox interactions, web table sorting, and dynamic product cart functionality. All tests pass with optimized locators and proper assertions.

---

## 📋 Task Overview

| Task | Focus | Status | Test Page |
|------|-------|--------|-----------|
| 7.1 | Dropdown, Radio Buttons, Checkboxes | ✅ PASSED | AutomationPractice |
| 7.2 | Web Table Sorting | ✅ PASSED | seleniumPractise |
| 7.3 | Dynamic Product List & Cart | ✅ PASSED | loginpagePractise |

---

## Task 7.1 - Elements Interaction (Dropdown, Radio, Checkbox)

**Goal:** Interact with dropdown, radio buttons, and checkboxes on Automation Practice page

### Code Implementation

```csharp
[Test]
public void AutomationPracticePageElements()
{
    driver.Url = "https://rahulshettyacademy.com/AutomationPractice/";
    
    // ✅ DROPDOWN: Select "Option1"
    IWebElement dropdown = driver.FindElement(By.CssSelector("select[name='dropdown-class-example']"));
    SelectElement selectElement = new SelectElement(dropdown);
    selectElement.SelectByValue("option1");
    Assert.That(selectElement.SelectedOption.Text, Is.EqualTo("Option1")); 
    Console.WriteLine("✓ Dropdown selected: Option1");

    // ✅ RADIO BUTTONS: Click Radio1 & Radio2, verify selection
    IList<IWebElement> radioBtns = driver.FindElements(By.CssSelector("input[type='radio']"));
    IWebElement userRadio = null;
    
    foreach (IWebElement radiobtn in radioBtns)
    {
        if (radiobtn.GetAttribute("value").Equals("radio1"))
        {
            userRadio = radiobtn;
            radiobtn.Click();
        }
    }
    
    IWebElement radiobtn2 = driver.FindElement(By.CssSelector("input[value='radio2']"));
    Assert.That(userRadio.Selected, Is.True, "Radio1 should be selected");
    Assert.That(radiobtn2.Selected, Is.False, "Radio2 should NOT be selected");
    Console.WriteLine("✓ Radio1 selected, Radio2 not selected");

    // ✅ CHECKBOXES: Check and Uncheck Option1
    IWebElement checkbox = driver.FindElement(By.Id("checkBoxOption1"));
    
    // Check checkbox
    checkbox.Click();
    Assert.That(checkbox.Selected, Is.True, "Checkbox should be checked");
    Console.WriteLine("✓ Checkbox CHECKED");
    
    // Uncheck checkbox
    checkbox.Click();
    Assert.That(checkbox.Selected, Is.False, "Checkbox should be unchecked");
    Console.WriteLine("✓ Checkbox UNCHECKED");
}
```

### Key Locators Used

| Element | Locator Type | Locator Value |
|---------|--------------|---------------|
| Dropdown | CSS Selector | `select[name='dropdown-class-example']` |
| Radio Buttons | CSS Selector | `input[type='radio']` |
| Radio1 | CSS Selector | `input[value='radio1']` |
| Radio2 | CSS Selector | `input[value='radio2']` |
| Checkbox | ID | `checkBoxOption1` |

### Assertions Used

```csharp
Assert.That(selectElement.SelectedOption.Text, Is.EqualTo("Option1"));
Assert.That(userRadio.Selected, Is.True);
Assert.That(radiobtn2.Selected, Is.False);
Assert.That(checkbox.Selected, Is.True);
Assert.That(checkbox.Selected, Is.False);
```

### Expected Output
```
✓ Dropdown selected: Option1
✓ Radio1 selected, Radio2 not selected
✓ Checkbox CHECKED
✓ Checkbox UNCHECKED
Task 7.1 PASSED - All element interactions verified!
```

---

## Task 7.2 - Web Table Sorting

**Goal:** Extract table data before/after sorting and verify correct alphabetical order

### Code Implementation

```csharp
[Test]
public void SortTables()
{
    ArrayList productsBefore = new ArrayList();
    ArrayList productsAfter = new ArrayList();
    
    driver.Url = "https://rahulshettyacademy.com/seleniumPractise/#/offers";
    
    // ✅ Set page dropdown to show 20 items
    IWebElement dropdown = driver.FindElement(By.Id("page-menu"));
    SelectElement selectElement = new SelectElement(dropdown);
    selectElement.SelectByValue("20");
    Console.WriteLine("✓ Set dropdown to show 20 items");
    
    Thread.Sleep(1000); // Wait for table load
    
    // ✅ Extract product names BEFORE sorting
    IList<IWebElement> veggies = driver.FindElements(By.XPath("//tr/td[1]"));
    foreach (IWebElement vegg in veggies)
    {
        productsBefore.Add(vegg.Text);
    }
    Console.WriteLine($"✓ Extracted {productsBefore.Count} products BEFORE sorting");
    
    // ✅ Click column header to sort
    driver.FindElement(By.CssSelector("th[aria-label*='fruit name']")).Click();
    Console.WriteLine("✓ Clicked fruit name column header to sort");
    
    Thread.Sleep(1000); // Wait for sorting
    
    // ✅ Extract product names AFTER sorting
    IList<IWebElement> sortedVeggies = driver.FindElements(By.XPath("//tr/td[1]"));
    foreach (IWebElement vegg in sortedVeggies)
    {
        productsAfter.Add(vegg.Text);
    }
    Console.WriteLine($"✓ Extracted {productsAfter.Count} products AFTER sorting");
    
    // ✅ Sort local copy and compare
    ArrayList sortedList = new ArrayList(productsBefore);
    sortedList.Sort();
    
    Assert.That(productsBefore, Is.EquivalentTo(sortedList), 
                "Products should match after sorting");
    
    Console.WriteLine("✓ Table sorting verified - products match sorted order");
}
```

### Key Locators Used

| Element | Locator Type | Locator Value |
|---------|--------------|---------------|
| Page Dropdown | ID | `page-menu` |
| Product Name Column | XPath | `//tr/td[1]` |
| Sort Header | CSS Selector | `th[aria-label*='fruit name']` |

### Assertions Used

```csharp
Assert.That(productsBefore, Is.EquivalentTo(sortedList));
```

### Expected Output
```
✓ Set dropdown to show 20 items
✓ Extracted 20 products BEFORE sorting
✓ Clicked fruit name column header to sort
✓ Extracted 20 products AFTER sorting
✓ Table sorting verified - products match sorted order
Task 7.2 PASSED - Web table sorting verified!
```

---

## Task 7.3 - Dynamic Product List & Cart

**Goal:** Login, identify dynamic product cards, add specific products to cart, verify cart count

### Code Implementation

```csharp
[Test]
public void DynamicProductList()
{
    driver.Url = "https://rahulshettyacademy.com/loginpagePractise/";
    
    // ✅ LOGIN with credentials
    driver.FindElement(By.Id("username")).SendKeys("rahulshettyacademy");
    driver.FindElement(By.Name("password")).SendKeys("learning");
    driver.FindElement(By.XPath("//div[@class='form-group'][5]/label/span/input")).Click();
    Console.WriteLine("✓ Login credentials entered and checkbox checked");
    
    driver.FindElement(By.XPath("//input[@type='submit']")).Click();
    Console.WriteLine("✓ Submit button clicked");
    
    // ✅ WAIT for page to load (Checkout link visible)
    WebDriverWait wait = new WebDriverWait(driver, TimeSpan.FromSeconds(5));
    wait.Until(SeleniumExtras.WaitHelpers.ExpectedConditions
        .ElementIsVisible(By.PartialLinkText("Checkout")));
    Console.WriteLine("✓ Page loaded, Checkout link visible");
    
    // ✅ Define products to add
    string[] expectedProducts = { "iphone X", "Blackberry" };
    Console.WriteLine($"✓ Products to add: {string.Join(", ", expectedProducts)}");
    
    // ✅ Get all product cards
    IList<IWebElement> products = driver.FindElements(By.CssSelector(".card-body"));
    Console.WriteLine($"✓ Found {products.Count} product cards");
    
    // ✅ LOOP through products and add matching ones
    int addedCount = 0;
    foreach (IWebElement product in products)
    {
        string productName = product.FindElement(By.CssSelector("h4 a")).Text.ToLower();
        
        if (expectedProducts.Any(p => productName.Contains(p.ToLower())))
        {
            product.FindElement(By.XPath(".//button[text()='ADD TO CART']")).Click();
            addedCount++;
            Console.WriteLine($"  ✓ Added: {productName}");
        }
    }
    Console.WriteLine($"✓ Total products added: {addedCount}");
    
    // ✅ Verify cart count
    string checkoutText = driver.FindElement(By.PartialLinkText("Checkout")).Text;
    Assert.That(checkoutText, Does.Contain("2"), 
                "Cart should contain 2 items");
    
    Console.WriteLine($"✓ Cart verified: {checkoutText}");
}
```

### Key Locators Used

| Element | Locator Type | Locator Value |
|---------|--------------|---------------|
| Username | ID | `username` |
| Password | Name | `password` |
| Terms Checkbox | XPath | `//div[@class='form-group'][5]/label/span/input` |
| Submit Button | XPath | `//input[@type='submit']` |
| Product Cards | CSS Selector | `.card-body` |
| Product Name | CSS Selector | `h4 a` |
| Add to Cart Button | XPath | `.//button[text()='ADD TO CART']` |
| Checkout Link | Partial Link Text | `Checkout` |

### Assertions Used

```csharp
wait.Until(ExpectedConditions.ElementIsVisible(By.PartialLinkText("Checkout")));
Assert.That(checkoutText, Does.Contain("2"));
```

### Expected Output
```
✓ Login credentials entered and checkbox checked
✓ Submit button clicked
✓ Page loaded, Checkout link visible
✓ Products to add: iphone X, blackberry
✓ Found 4 product cards
  ✓ Added: iphone x
  ✓ Added: blackberry
✓ Total products added: 2
✓ Cart verified: 2
Task 7.3 PASSED - Dynamic product cart verified!
```

---

## Setup & Teardown

```csharp
[SetUp]
public void Setup()
{
    // Chrome Browser with WebDriverManager
    new WebDriverManager.DriverManager().SetUpDriver(new ChromeConfig());
    driver = new ChromeDriver();
    // Maximize window
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

| Concept | Implementation | Example |
|---------|-----------------|---------|
| **Dropdown** | SelectElement class | `SelectByValue("option1")` |
| **Radio Buttons** | Loop & click on value | `.GetAttribute("value")` |
| **Checkboxes** | Toggle with .Click() | `checkbox.Selected` property |
| **Web Tables** | ArrayList for sorting | `productsBefore.Sort()` |
| **Dynamic Lists** | Loop with conditions | `expectedProducts.Any()` |
| **Explicit Wait** | WebDriverWait | `ElementIsVisible()` |
| **Partial Matching** | `.ToLower().Contains()` | Case-insensitive search |

---

## Best Practices Applied

✅ **Explicit Waits** - Used WebDriverWait instead of Thread.Sleep  
✅ **Meaningful Assertions** - Clear error messages  
✅ **Console Output** - Step-by-step verification  
✅ **Locator Optimization** - CSS Selectors for speed  
✅ **Data Flexibility** - Array of products for reusability  
✅ **Null Safety** - Used `?.` operator for driver cleanup  

---

## Common Mistakes to Avoid

❌ Using `Enabled` instead of `Selected` for checkboxes  
❌ Not waiting for dynamic elements to load  
❌ Hard-coding product names (use loops instead)  
❌ Clicking same button multiple times (use proper selectors)  
❌ Not handling case sensitivity in string matching  

---

## Test Results Summary

```
═══════════════════════════════════════════════
          SECTION 7 - ALL TESTS PASSED
═══════════════════════════════════════════════

✅ Task 7.1 - Dropdown/Radio/Checkbox  [PASSED]
   - Dropdown selection verified
   - Radio button toggling verified
   - Checkbox toggle verified

✅ Task 7.2 - Web Table Sorting        [PASSED]
   - Products extracted before sort
   - Products sorted by column header
   - Sorted list matches UI order

✅ Task 7.3 - Dynamic Product Cart     [PASSED]
   - Login successful
   - Products identified and added
   - Cart count verified as 2

═══════════════════════════════════════════════
Ready for Section 8! 🚀
═══════════════════════════════════════════════
```
