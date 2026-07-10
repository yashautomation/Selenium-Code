# Selenium with Java: Frames, Mouse Actions & Attributes

## Table of Contents
1. [Frames in Selenium](#frames-in-selenium)
2. [Mouse Actions](#mouse-actions)
3. [Web Element Attributes](#web-element-attributes)

---

## Frames in Selenium

### What are Frames?
Frames are HTML elements used to divide a web page into multiple sections, each containing a separate HTML document. In Selenium, you cannot directly interact with elements inside frames unless you first switch to that frame.

### Types of Frames:
1. **\<iframe\>** - Inline Frame (most common)
2. **\<frame\>** - Used in frameset (legacy)

### Why Switch to Frames?
When a frame is present on a page, Selenium's focus remains on the main content. To interact with elements inside a frame, you must explicitly switch to it.

### Common Issues:
- **NoSuchElementException** - When trying to access elements without switching to frame
- **Stale Element Reference** - When frame content changes

### Methods to Identify Frames:
1. By Index
2. By Name Attribute
3. By ID Attribute
4. By WebElement

---

## Mouse Actions

### Overview
Mouse actions in Selenium allow you to simulate complex user interactions like:
- Hover (mouseover)
- Click
- Double-click
- Right-click
- Drag and Drop
- Click and Hold

### Supported Actions:
All mouse actions are performed using the **Actions** class from `org.openqa.selenium.interactions` package.

---

## Web Element Attributes

### Common Attributes:
1. **value** - Input field value
2. **placeholder** - Input placeholder text
3. **type** - Input type (text, password, etc.)
4. **class** - CSS class
5. **id** - Element ID
6. **href** - Link URL
7. **title** - Tooltip text
8. **disabled** - Element state
9. **readonly** - Read-only state
10. **required** - Required field indicator

---

# Code Examples

## Installation & Setup

### Maven Dependencies
```xml
<!-- Selenium WebDriver -->
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.15.0</version>
</dependency>

<!-- WebDriverManager (Auto driver management) -->
<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.6.3</version>
</dependency>

<!-- TestNG (Testing Framework) -->
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.8.1</version>
    <scope>test</scope>
</dependency>
```

---

## Complete Code Examples

### 1. Frames Example - Switch to iFrame
```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;
import io.github.bonigarcia.wdm.WebDriverManager;

public class FramesExample {
    WebDriver driver;

    @BeforeMethod
    public void setUp() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
    }

    @Test
    public void switchToFrameByIndex() {
        driver.get("https://www.w3schools.com/html/html_iframe.asp");
        
        // Method 1: Switch to frame by index (0-based)
        driver.switchTo().frame(0);
        System.out.println("Switched to frame by index");
        
        // Now interact with elements inside the frame
        WebElement element = driver.findElement(By.tagName("h1"));
        System.out.println("Element in frame: " + element.getText());
        
        // Switch back to main content
        driver.switchTo().defaultContent();
    }

    @Test
    public void switchToFrameByName() {
        driver.get("https://www.w3schools.com/html/html_iframe.asp");
        
        // Method 2: Switch to frame by name attribute
        driver.switchTo().frame("myFrame");
        System.out.println("Switched to frame by name");
        
        // Interact with frame content
        WebElement content = driver.findElement(By.xpath("//p"));
        System.out.println("Frame content: " + content.getText());
        
        driver.switchTo().defaultContent();
    }

    @Test
    public void switchToFrameById() {
        driver.get("https://www.w3schools.com/html/html_iframe.asp");
        
        // Method 3: Switch to frame by ID
        driver.switchTo().frame("iFrame_id");
        System.out.println("Switched to frame by ID");
        
        driver.switchTo().defaultContent();
    }

    @Test
    public void switchToFrameByWebElement() {
        driver.get("https://www.w3schools.com/html/html_iframe.asp");
        
        // Method 4: Switch to frame by WebElement
        WebElement iframeElement = driver.findElement(By.tagName("iframe"));
        driver.switchTo().frame(iframeElement);
        System.out.println("Switched to frame by WebElement");
        
        driver.switchTo().defaultContent();
    }

    @Test
    public void nestedFrames() {
        driver.get("https://example.com/nested-frames");
        
        // Switch to outer frame first
        driver.switchTo().frame(0);
        System.out.println("Switched to outer frame");
        
        // Switch to nested frame
        driver.switchTo().frame(0);
        System.out.println("Switched to nested frame");
        
        // Interact with element in nested frame
        WebElement element = driver.findElement(By.id("content"));
        System.out.println("Nested frame content: " + element.getText());
        
        // Go back to outer frame
        driver.switchTo().parentFrame();
        
        // Go back to main content
        driver.switchTo().defaultContent();
    }

    @Test
    public void frameHandling_Complete() throws InterruptedException {
        driver.get("https://www.w3schools.com/html/html_iframe.asp");
        
        // Get total number of frames
        int frameCount = driver.findElements(By.tagName("iframe")).size();
        System.out.println("Total frames on page: " + frameCount);
        
        // Iterate through all frames
        for (int i = 0; i < frameCount; i++) {
            driver.switchTo().defaultContent();
            WebElement frameElement = driver.findElements(By.tagName("iframe")).get(i);
            driver.switchTo().frame(frameElement);
            System.out.println("Processing frame: " + (i + 1));
            
            Thread.sleep(1000);
        }
        
        driver.switchTo().defaultContent();
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }
}
```

---

### 2. Mouse Actions Example
```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;
import io.github.bonigarcia.wdm.WebDriverManager;

public class MouseActionsExample {
    WebDriver driver;
    Actions actions;

    @BeforeMethod
    public void setUp() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        actions = new Actions(driver);
    }

    @Test
    public void hoverOverElement() {
        driver.get("https://www.amazon.com");
        
        // Find element to hover
        WebElement accountsMenu = driver.findElement(By.id("nav-link-accountList-nav-line-1"));
        
        // Hover over element
        actions.moveToElement(accountsMenu).perform();
        System.out.println("Hovered over Accounts menu");
        
        // Now click on submenu that appears
        WebElement signInOption = driver.findElement(By.xpath("//span[contains(text(), 'Sign in')]"));
        actions.click(signInOption).perform();
    }

    @Test
    public void doubleClick() {
        driver.get("https://testpages.eviltester.com/styled/index.html");
        
        WebElement button = driver.findElement(By.id("button-id"));
        
        // Double-click on element
        actions.doubleClick(button).perform();
        System.out.println("Double-clicked on button");
    }

    @Test
    public void rightClick() {
        driver.get("https://testpages.eviltester.com/styled/index.html");
        
        WebElement element = driver.findElement(By.id("button-id"));
        
        // Right-click (context click)
        actions.contextClick(element).perform();
        System.out.println("Right-clicked on element");
    }

    @Test
    public void dragAndDrop() {
        driver.get("https://testpages.eviltester.com/styled/drag-drop.html");
        
        WebElement source = driver.findElement(By.id("draggable"));
        WebElement target = driver.findElement(By.id("droppable"));
        
        // Drag and Drop
        actions.dragAndDrop(source, target).perform();
        System.out.println("Dragged and dropped element");
    }

    @Test
    public void dragAndDropByOffset() {
        driver.get("https://testpages.eviltester.com/styled/drag-drop.html");
        
        WebElement element = driver.findElement(By.id("draggable"));
        
        // Drag element by offset (x pixels right, y pixels down)
        actions.dragAndDropBy(element, 100, 50).perform();
        System.out.println("Dragged element by offset");
    }

    @Test
    public void clickAndHold() {
        driver.get("https://testpages.eviltester.com/styled/index.html");
        
        WebElement element = driver.findElement(By.id("button-id"));
        
        // Click and hold
        actions.clickAndHold(element).perform();
        System.out.println("Clicked and held on element");
        
        // Release the hold
        actions.release(element).perform();
        System.out.println("Released the element");
    }

    @Test
    public void moveToElement() {
        driver.get("https://www.example.com");
        
        WebElement element = driver.findElement(By.tagName("a"));
        
        // Move to element (without clicking)
        actions.moveToElement(element).perform();
        System.out.println("Moved to element");
    }

    @Test
    public void sendKeysWithMouseAction() {
        driver.get("https://www.example.com");
        
        WebElement inputField = driver.findElement(By.id("input-id"));
        
        // Click and send keys
        actions.click(inputField).sendKeys("Hello Selenium").perform();
        System.out.println("Clicked and typed text");
    }

    @Test
    public void chainedActions() {
        driver.get("https://testpages.eviltester.com/styled/index.html");
        
        WebElement element1 = driver.findElement(By.id("button1"));
        WebElement element2 = driver.findElement(By.id("button2"));
        WebElement element3 = driver.findElement(By.id("button3"));
        
        // Chain multiple actions together
        actions
            .moveToElement(element1)
            .click()
            .moveToElement(element2)
            .doubleClick()
            .moveToElement(element3)
            .contextClick()
            .perform();
        
        System.out.println("Performed chained actions");
    }

    @Test
    public void keyboardAndMouseCombination() {
        driver.get("https://www.example.com");
        
        WebElement element1 = driver.findElement(By.id("checkbox1"));
        WebElement element2 = driver.findElement(By.id("checkbox2"));
        
        // Hold Ctrl and click multiple elements (select multiple)
        actions
            .click(element1)
            .keyDown(Keys.CONTROL)
            .click(element2)
            .keyUp(Keys.CONTROL)
            .perform();
        
        System.out.println("Selected multiple elements with Ctrl+Click");
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }
}
```

*Note: Add this import for keyboard combinations:*
```java
import org.openqa.selenium.Keys;
```

---

### 3. Web Element Attributes Example
```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;
import io.github.bonigarcia.wdm.WebDriverManager;

public class AttributesExample {
    WebDriver driver;

    @BeforeMethod
    public void setUp() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
    }

    @Test
    public void getAttributeValues() {
        driver.get("https://www.google.com");
        
        WebElement searchBox = driver.findElement(By.name("q"));
        
        // Get various attributes
        String type = searchBox.getAttribute("type");
        String name = searchBox.getAttribute("name");
        String id = searchBox.getAttribute("id");
        String placeholder = searchBox.getAttribute("placeholder");
        String role = searchBox.getAttribute("role");
        
        System.out.println("Type: " + type);
        System.out.println("Name: " + name);
        System.out.println("ID: " + id);
        System.out.println("Placeholder: " + placeholder);
        System.out.println("Role: " + role);
    }

    @Test
    public void getInputFieldAttributes() {
        driver.get("https://testpages.eviltester.com/styled/index.html");
        
        WebElement inputField = driver.findElement(By.id("username"));
        
        // Get input field specific attributes
        String value = inputField.getAttribute("value");
        String type = inputField.getAttribute("type");
        String placeholder = inputField.getAttribute("placeholder");
        String maxLength = inputField.getAttribute("maxlength");
        String required = inputField.getAttribute("required");
        String readonly = inputField.getAttribute("readonly");
        String disabled = inputField.getAttribute("disabled");
        
        System.out.println("Value: " + value);
        System.out.println("Type: " + type);
        System.out.println("Placeholder: " + placeholder);
        System.out.println("Max Length: " + maxLength);
        System.out.println("Required: " + required);
        System.out.println("Readonly: " + readonly);
        System.out.println("Disabled: " + disabled);
    }

    @Test
    public void getClassAttribute() {
        driver.get("https://www.example.com");
        
        WebElement element = driver.findElement(By.tagName("h1"));
        
        // Get class attribute
        String classValue = element.getAttribute("class");
        System.out.println("Class: " + classValue);
    }

    @Test
    public void getHrefAttribute() {
        driver.get("https://www.example.com");
        
        WebElement link = driver.findElement(By.tagName("a"));
        
        // Get href attribute from link
        String href = link.getAttribute("href");
        String title = link.getAttribute("title");
        String target = link.getAttribute("target");
        
        System.out.println("Href: " + href);
        System.out.println("Title: " + title);
        System.out.println("Target: " + target);
    }

    @Test
    public void checkIfAttributeExists() {
        driver.get("https://testpages.eviltester.com/styled/index.html");
        
        WebElement element = driver.findElement(By.id("button-id"));
        
        // Check if attribute exists
        String disabledValue = element.getAttribute("disabled");
        
        if (disabledValue != null) {
            System.out.println("Element is disabled: " + disabledValue);
        } else {
            System.out.println("Element is not disabled");
        }
    }

    @Test
    public void dataAttributes() {
        driver.get("https://example.com");
        
        WebElement element = driver.findElement(By.id("my-element"));
        
        // Get custom data attributes
        String dataId = element.getAttribute("data-id");
        String dataTestid = element.getAttribute("data-testid");
        String dataValue = element.getAttribute("data-value");
        
        System.out.println("Data-ID: " + dataId);
        System.out.println("Data-TestID: " + dataTestid);
        System.out.println("Data-Value: " + dataValue);
    }

    @Test
    public void getTextVsAttributeValue() {
        driver.get("https://www.example.com");
        
        WebElement element = driver.findElement(By.tagName("button"));
        
        // Text content (visible text)
        String textContent = element.getText();
        
        // Value attribute (HTML attribute value)
        String attributeValue = element.getAttribute("value");
        
        System.out.println("Text Content: " + textContent);
        System.out.println("Attribute Value: " + attributeValue);
    }

    @Test
    public void getAttributeWithWait() throws InterruptedException {
        driver.get("https://testpages.eviltester.com/styled/index.html");
        
        WebElement element = driver.findElement(By.id("dynamic-element"));
        
        // Get attribute multiple times
        for (int i = 0; i < 5; i++) {
            String attributeValue = element.getAttribute("data-count");
            System.out.println("Attempt " + (i + 1) + ": " + attributeValue);
            Thread.sleep(1000);
        }
    }

    @Test
    public void cssPropertyVsAttribute() {
        driver.get("https://www.example.com");
        
        WebElement element = driver.findElement(By.tagName("h1"));
        
        // Attributes (from HTML)
        String id = element.getAttribute("id");
        String styleAttr = element.getAttribute("style");
        
        // CSS Properties (computed)
        String color = element.getCssValue("color");
        String fontSize = element.getCssValue("font-size");
        String fontFamily = element.getCssValue("font-family");
        
        System.out.println("ID Attribute: " + id);
        System.out.println("Style Attribute: " + styleAttr);
        System.out.println("Color CSS Property: " + color);
        System.out.println("Font Size CSS Property: " + fontSize);
        System.out.println("Font Family CSS Property: " + fontFamily);
    }

    @Test
    public void practicalExample_FormValidation() {
        driver.get("https://testpages.eviltester.com/styled/forms/input-validation.html");
        
        WebElement emailField = driver.findElement(By.id("email"));
        WebElement passwordField = driver.findElement(By.id("password"));
        WebElement submitButton = driver.findElement(By.id("submit"));
        
        // Check field requirements
        String emailRequired = emailField.getAttribute("required");
        String emailType = emailField.getAttribute("type");
        String passwordRequired = passwordField.getAttribute("required");
        
        System.out.println("Email Required: " + (emailRequired != null ? "Yes" : "No"));
        System.out.println("Email Type: " + emailType);
        System.out.println("Password Required: " + (passwordRequired != null ? "Yes" : "No"));
        
        // Check if button is disabled
        String buttonDisabled = submitButton.getAttribute("disabled");
        System.out.println("Submit Button Disabled: " + (buttonDisabled != null ? "Yes" : "No"));
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }
}
```

---

## Summary Table

| Feature | Method | Example |
|---------|--------|---------|
| **Frames** | `switchTo().frame()` | `driver.switchTo().frame(0)` |
| Nested Frames | `switchTo().parentFrame()` | Go back one level |
| Default Content | `switchTo().defaultContent()` | Go to main page |
| **Mouse Actions** | `new Actions(driver)` | `actions.moveToElement(element)` |
| Hover | `.moveToElement()` | Hover without clicking |
| Double-Click | `.doubleClick()` | Double-click action |
| Right-Click | `.contextClick()` | Context menu |
| Drag-Drop | `.dragAndDrop()` | Drag source to target |
| **Attributes** | `.getAttribute()` | `element.getAttribute("type")` |
| CSS Property | `.getCssValue()` | `element.getCssValue("color")` |

---

## Best Practices

1. **Frames:**
   - Always switch back to default content after frame operations
   - Use explicit waits when frame content loads dynamically
   - Store frame elements if you need to switch multiple times

2. **Mouse Actions:**
   - Always call `.perform()` at the end of action chain
   - Use explicit waits before performing mouse actions
   - Test on different browsers as behavior may vary

3. **Attributes:**
   - Check for null before using attribute values
   - Distinguish between attributes and CSS properties
   - Use `getAttribute("value")` for form inputs, not `getText()`

---

## Common Errors & Solutions

| Error | Cause | Solution |
|-------|-------|----------|
| NoSuchElementException | Not switched to frame | Use `switchTo().frame()` |
| StaleElementReference | Frame reloaded | Re-locate element after frame switch |
| Element not interactable | Element outside viewport | Use `moveToElement()` before interaction |
| Attribute is null | Attribute doesn't exist | Check if attribute exists first |

---

## Resources

- [Selenium Documentation](https://www.selenium.dev/documentation/)
- [Actions Class Javadoc](https://www.selenium.dev/selenium/docs/api/java/org/openqa/selenium/interactions/Actions.html)
- [WebElement Javadoc](https://www.selenium.dev/selenium/docs/api/java/org/openqa/selenium/WebElement.html)

