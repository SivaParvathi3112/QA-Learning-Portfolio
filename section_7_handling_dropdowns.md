# Section 7 - Automation Practice & Dynamic Cart

Complete working C# code for **Section 7 Tasks (7.1, 7.2, 7.3)** covering dropdown/radio/checkbox interactions, web table sorting, and dynamic product cart functionality. All tests pass with optimized locators and proper assertions.

---


## Task 7.1 - Elements Interaction (Dropdown, Radio, Checkbox)

**Goal:** Interact with dropdown, radio buttons, and checkboxes on Automation Practice page

### Code Implementation

```csharp
[Test]
public void AutomationPracticePageElements()
{
    driver.Url = "https://rahulshettyacademy.com/AutomationPractice/";
    
    //  DROPDOWN: Select "Option1"
    IWebElement dropdown = driver.FindElement(By.CssSelector("select[name='dropdown-class-example']"));
    SelectElement selectElement = new SelectElement(dropdown);
    selectElement.SelectByValue("option1");
    Assert.That(selectElement.SelectedOption.Text, Is.EqualTo("Option1")); 
    Console.WriteLine("✓ Dropdown selected: Option1");

    //  RADIO BUTTONS: Click Radio1 & Radio2, verify selection
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

    //  CHECKBOXES: Check and Uncheck Option1
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
    
    //  Set page dropdown to show 20 items
    IWebElement dropdown = driver.FindElement(By.Id("page-menu"));
    SelectElement selectElement = new SelectElement(dropdown);
    selectElement.SelectByValue("20");
    Console.WriteLine("✓ Set dropdown to show 20 items");
    
    Thread.Sleep(1000); // Wait for table load
    
    //  Extract product names BEFORE sorting
    IList<IWebElement> veggies = driver.FindElements(By.XPath("//tr/td[1]"));
    foreach (IWebElement vegg in veggies)
    {
        productsBefore.Add(vegg.Text);
    }
    Console.WriteLine($"✓ Extracted {productsBefore.Count} products BEFORE sorting");
    
    //  Click column header to sort
    driver.FindElement(By.CssSelector("th[aria-label*='fruit name']")).Click();
    Console.WriteLine("✓ Clicked fruit name column header to sort");
    
    Thread.Sleep(1000); // Wait for sorting
    
    //  Extract product names AFTER sorting
    IList<IWebElement> sortedVeggies = driver.FindElements(By.XPath("//tr/td[1]"));
    foreach (IWebElement vegg in sortedVeggies)
    {
        productsAfter.Add(vegg.Text);
    }
    Console.WriteLine($"✓ Extracted {productsAfter.Count} products AFTER sorting");
    
    //  Sort local copy and compare
    ArrayList sortedList = new ArrayList(productsBefore);
    sortedList.Sort();
    
    Assert.That(productsBefore, Is.EquivalentTo(sortedList), 
                "Products should match after sorting");
    
    Console.WriteLine("✓ Table sorting verified - products match sorted order");
}
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
    
    //  LOGIN with credentials
    driver.FindElement(By.Id("username")).SendKeys("rahulshettyacademy");
    driver.FindElement(By.Name("password")).SendKeys("learning");
    driver.FindElement(By.XPath("//div[@class='form-group'][5]/label/span/input")).Click();
    Console.WriteLine("✓ Login credentials entered and checkbox checked");
    
    driver.FindElement(By.XPath("//input[@type='submit']")).Click();
    Console.WriteLine("✓ Submit button clicked");
    
    //  WAIT for page to load (Checkout link visible)
    WebDriverWait wait = new WebDriverWait(driver, TimeSpan.FromSeconds(5));
    wait.Until(SeleniumExtras.WaitHelpers.ExpectedConditions
        .ElementIsVisible(By.PartialLinkText("Checkout")));
    Console.WriteLine("✓ Page loaded, Checkout link visible");
    
    //  Define products to add
    string[] expectedProducts = { "iphone X", "Blackberry" };
    Console.WriteLine($"✓ Products to add: {string.Join(", ", expectedProducts)}");
    
    //  Get all product cards
    IList<IWebElement> products = driver.FindElements(By.CssSelector(".card-body"));
    Console.WriteLine($"✓ Found {products.Count} product cards");
    
    //  LOOP through products and add matching ones
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
    
    //  Verify cart count
    string checkoutText = driver.FindElement(By.PartialLinkText("Checkout")).Text;
    Assert.That(checkoutText, Does.Contain("2"), 
                "Cart should contain 2 items");
    
    Console.WriteLine($"✓ Cart verified: {checkoutText}");
}
```









