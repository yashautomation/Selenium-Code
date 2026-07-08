# Most Commonly Asked Selenium Interview Questions with Java Code

## Table of Contents
1. [Selenium Basics](#selenium-basics)
2. [WebDriver & Browser Management](#webdriver--browser-management)
3. [Locators & Finding Elements](#locators--finding-elements)
4. [Interactions & Actions](#interactions--actions)
5. [Waits & Synchronization](#waits--synchronization)
6. [Handle Multiple Windows/Tabs](#handle-multiple-windowstabs)
7. [Alerts & Dropdown Handling](#alerts--dropdown-handling)
8. [Advanced Interactions](#advanced-interactions)
9. [JavaScriptExecutor](#javascriptexecutor)
10. [Screenshots & Reporting](#screenshots--reporting)

---

## Selenium Basics

### Q1: What is Selenium? What are its advantages?
**Answer:** Selenium is an open-source automation testing tool used for testing web applications across different browsers and platforms. It supports multiple programming languages including Java, Python, C#, Ruby, PHP, and JavaScript.

**Advantages:**
- Free and open-source
- Cross-platform and cross-browser support
- Multiple language support
- Easy to learn and implement
- Active community support

---

### Q2: What are the different Selenium versions?
**Answer:**
- **Selenium IDE** - Record and playback tool (browser extension)
- **Selenium RC (Remote Control)** - Deprecated, used Java Server Gateway
- **Selenium WebDriver** - Current standard, direct communication with browser
- **Selenium Grid** - Parallel execution across multiple machines

---

## WebDriver & Browser Management

### Q3: How do you initialize different browsers in Selenium?

```java
// Chrome Browser
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.WebDriver;

public class BrowserInit {
    public static void main(String[] args) {
        // Chrome
        WebDriver driver = new ChromeDriver();
        
        // Firefox
        // WebDriver driver = new FirefoxDriver();
        
        // Edge
        // WebDriver driver = new EdgeDriver();
        
        // Safari
        // WebDriver driver = new SafariDriver();
        
        driver.get("https://www.google.com");
        driver.quit();
    }
}
```

---

### Q4: How do you handle browser window management?

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class WindowManagement {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // Maximize window
        driver.manage().window().maximize();
        
        // Get window size
        int width = driver.manage().window().getSize().getWidth();
        int height = driver.manage().window().getSize().getHeight();
        System.out.println("Width: " + width + ", Height: " + height);
        
        // Get window position
        int x = driver.manage().window().getPosition().getX();
        int y = driver.manage().window().getPosition().getY();
        System.out.println("X: " + x + ", Y: " + y);
        
        // Set window position
        driver.manage().window().setPosition(new Point(100, 100));
        
        // Set window size
        driver.manage().window().setSize(new Dimension(800, 600));
        
        driver.quit();
    }
}
```

---

## Locators & Finding Elements

### Q5: What are the different types of locators in Selenium?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class Locators {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // 1. ID Locator
        WebElement element1 = driver.findElement(By.id("elementId"));
        
        // 2. NAME Locator
        WebElement element2 = driver.findElement(By.name("elementName"));
        
        // 3. CLASS NAME Locator
        WebElement element3 = driver.findElement(By.className("className"));
        
        // 4. TAG NAME Locator
        WebElement element4 = driver.findElement(By.tagName("button"));
        
        // 5. LINK TEXT Locator
        WebElement element5 = driver.findElement(By.linkText("Click Here"));
        
        // 6. PARTIAL LINK TEXT Locator
        WebElement element6 = driver.findElement(By.partialLinkText("Click"));
        
        // 7. CSS SELECTOR Locator
        WebElement element7 = driver.findElement(By.cssSelector("input[name='username']"));
        WebElement element8 = driver.findElement(By.cssSelector(".className"));
        WebElement element9 = driver.findElement(By.cssSelector("#elementId"));
        
        // 8. XPATH Locator
        WebElement element10 = driver.findElement(By.xpath("//input[@name='username']"));
        WebElement element11 = driver.findElement(By.xpath("//button[text()='Submit']"));
        WebElement element12 = driver.findElement(By.xpath("//input[@type='email' and @name='email']"));
        
        // Find multiple elements
        List<WebElement> elements = driver.findElements(By.tagName("button"));
        
        driver.quit();
    }
}
```

---

### Q6: What is XPath? Types of XPath?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class XPathExamples {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // ABSOLUTE XPATH
        // /html/body/div[1]/form/input[1]
        WebElement element1 = driver.findElement(By.xpath("/html/body/div[1]/form/input[1]"));
        
        // RELATIVE XPATH (Preferred)
        // //input[@id='username']
        WebElement element2 = driver.findElement(By.xpath("//input[@id='username']"));
        
        // XPath with multiple attributes
        WebElement element3 = driver.findElement(By.xpath("//input[@type='password' and @name='pwd']"));
        
        // XPath with contains() function
        WebElement element4 = driver.findElement(By.xpath("//button[contains(@class, 'btn')]"));
        
        // XPath with text()
        WebElement element5 = driver.findElement(By.xpath("//button[text()='Login']"));
        
        // XPath with contains() for text
        WebElement element6 = driver.findElement(By.xpath("//a[contains(text(), 'Login')]"));
        
        // XPath using parent-child relationship
        WebElement element7 = driver.findElement(By.xpath("//form/input[@type='email']"));
        
        // XPath using following-sibling
        WebElement element8 = driver.findElement(By.xpath("//label[text()='Username']/following-sibling::input"));
        
        // XPath with position
        WebElement element9 = driver.findElement(By.xpath("//input[1]"));
        WebElement element10 = driver.findElement(By.xpath("//input[last()]"));
        
        driver.quit();
    }
}
```

---

## Interactions & Actions

### Q7: How do you perform mouse and keyboard actions?

```java
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class ActionsExample {
    public static void main(String[] args) throws InterruptedException {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        WebElement element = driver.findElement(By.id("element"));
        Actions actions = new Actions(driver);
        
        // 1. Click
        actions.click(element).perform();
        
        // 2. Double Click
        actions.doubleClick(element).perform();
        
        // 3. Right Click (Context Click)
        actions.contextClick(element).perform();
        
        // 4. Hover over element
        actions.moveToElement(element).perform();
        
        // 5. Drag and Drop
        WebElement source = driver.findElement(By.id("source"));
        WebElement target = driver.findElement(By.id("target"));
        actions.dragAndDrop(source, target).perform();
        
        // 6. Click and Hold
        actions.clickAndHold(element).perform();
        Thread.sleep(2000);
        actions.release().perform();
        
        // 7. Send Keys
        actions.sendKeys(element, "Text to send").perform();
        
        // 8. Key Down and Key Up
        actions.keyDown(Keys.SHIFT).sendKeys("hello").keyUp(Keys.SHIFT).perform();
        
        // 9. Multiple Actions Chained
        actions.moveToElement(element)
                .click()
                .sendKeys("Username")
                .keyDown(Keys.TAB)
                .sendKeys("Password")
                .keyUp(Keys.TAB)
                .build()
                .perform();
        
        driver.quit();
    }
}
```

---

### Q8: How do you handle text input and form filling?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class FormFilling {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com/login");
        
        // Find username field
        WebElement username = driver.findElement(By.id("username"));
        
        // Send text to input field
        username.sendKeys("testuser");
        
        // Get the text that was entered
        String enteredText = username.getAttribute("value");
        System.out.println("Entered text: " + enteredText);
        
        // Clear the field
        username.clear();
        
        // Clear and type new text
        username.clear();
        username.sendKeys("newuser");
        
        // Get visible text
        WebElement label = driver.findElement(By.id("label"));
        String labelText = label.getText();
        System.out.println("Label text: " + labelText);
        
        // Submit form by finding submit button
        WebElement submitBtn = driver.findElement(By.xpath("//button[@type='submit']"));
        submitBtn.click();
        
        driver.quit();
    }
}
```

---

## Waits & Synchronization

### Q9: What are different types of waits in Selenium?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.openqa.selenium.support.ui.ExpectedConditions;
import java.time.Duration;

public class WaitsExample {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        
        // 1. IMPLICIT WAIT - Applied to all findElement calls
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
        
        // 2. EXPLICIT WAIT - WebDriverWait (Recommended)
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        
        driver.get("https://www.example.com");
        
        // Wait until element is present
        WebElement element = wait.until(
            ExpectedConditions.presenceOfElementLocated(By.id("elementId"))
        );
        
        // Wait until element is visible
        WebElement visibleElement = wait.until(
            ExpectedConditions.visibilityOfElementLocated(By.id("elementId"))
        );
        
        // Wait until element is clickable
        WebElement clickableElement = wait.until(
            ExpectedConditions.elementToBeClickable(By.id("buttonId"))
        );
        clickableElement.click();
        
        // Wait until element is invisible/not visible
        wait.until(ExpectedConditions.invisibilityOfElementLocated(By.id("loader")));
        
        // Wait until text is present in element
        wait.until(ExpectedConditions.textToBePresentInElementLocated(
            By.id("messageId"), "Success"
        ));
        
        // Wait until element value changes
        wait.until(ExpectedConditions.textToBePresentInElementValue(
            By.id("inputId"), "Expected Value"
        ));
        
        // Wait for title
        wait.until(ExpectedConditions.titleIs("Expected Title"));
        
        // Wait for title contains
        wait.until(ExpectedConditions.titleContains("Part of Title"));
        
        // Wait for URL contains
        wait.until(ExpectedConditions.urlContains("/dashboard"));
        
        // Wait for alert to be present
        wait.until(ExpectedConditions.alertIsPresent());
        
        // 3. FLUENT WAIT - Custom polling strategy
        FluentWait<WebDriver> fluentWait = new FluentWait<>(driver)
            .withTimeout(Duration.ofSeconds(15))
            .pollingEvery(Duration.ofMillis(500))
            .ignoring(NoSuchElementException.class);
        
        WebElement fluentElement = fluentWait.until(
            ExpectedConditions.presenceOfElementLocated(By.id("elementId"))
        );
        
        // 4. HARDCODED WAIT (Not recommended, but sometimes necessary)
        try {
            Thread.sleep(3000); // Sleep for 3 seconds
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        driver.quit();
    }
}
```

---

### Q10: What is the difference between Implicit Wait and Explicit Wait?

```java
/*
IMPLICIT WAIT:
- Applied globally to all findElement calls
- Waits for max time if element not found
- Once set, it's active for entire WebDriver lifetime
- Default timeout is 0 seconds
- Cannot change wait condition
- Slower overall execution

EXPLICIT WAIT:
- Applied to specific elements/conditions
- Waits until condition is met or timeout occurs
- Can specify custom conditions
- Better performance
- Recommended approach
*/

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.openqa.selenium.support.ui.ExpectedConditions;
import java.time.Duration;

public class WaitComparison {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        
        // IMPLICIT WAIT - Global setting
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
        
        // EXPLICIT WAIT - Specific to elements
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        
        driver.get("https://www.example.com");
        
        // Example 1: Implicit Wait (applies to all findElement)
        try {
            driver.findElement(By.id("element1")); // Waits up to 10 seconds
        } catch (Exception e) {
            System.out.println("Element not found after implicit wait");
        }
        
        // Example 2: Explicit Wait (specific condition)
        try {
            wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("element2")));
        } catch (Exception e) {
            System.out.println("Element not visible after explicit wait");
        }
        
        driver.quit();
    }
}
```

---

## Handle Multiple Windows/Tabs

### Q11: How do you handle multiple windows or tabs?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import java.util.Set;

public class MultipleWindowsHandling {
    public static void main(String[] args) throws InterruptedException {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // Get current window handle
        String parentWindow = driver.getWindowHandle();
        System.out.println("Parent Window: " + parentWindow);
        
        // Open new tab/window (using keyboard shortcut)
        Actions actions = new Actions(driver);
        actions.keyDown(Keys.CONTROL).click(driver.findElement(By.linkText("Link"))).keyUp(Keys.CONTROL).perform();
        
        // Wait for new window to open
        Thread.sleep(2000);
        
        // Get all window handles
        Set<String> allWindows = driver.getWindowHandles();
        System.out.println("Total Windows: " + allWindows.size());
        
        // Switch to new window/tab
        for (String window : allWindows) {
            if (!window.equals(parentWindow)) {
                driver.switchTo().window(window);
                System.out.println("Switched to window: " + window);
                System.out.println("Current URL: " + driver.getCurrentUrl());
                
                // Perform operations in new window
                WebElement element = driver.findElement(By.id("elementId"));
                element.click();
                break;
            }
        }
        
        // Switch back to parent window
        driver.switchTo().window(parentWindow);
        System.out.println("Switched back to parent window");
        System.out.println("Current URL: " + driver.getCurrentUrl());
        
        // Close current window/tab
        driver.close();
        
        // Close all windows
        driver.quit();
    }
}
```

---

## Alerts & Dropdown Handling

### Q12: How do you handle alerts in Selenium?

```java
import org.openqa.selenium.Alert;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class AlertsHandling {
    public static void main(String[] args) throws InterruptedException {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // Find button that triggers alert
        WebElement alertButton = driver.findElement(By.id("alertButton"));
        alertButton.click();
        
        // Wait for alert to appear
        Thread.sleep(1000);
        
        // Switch to alert
        Alert alert = driver.switchTo().alert();
        
        // 1. Simple Alert - Just OK button
        String alertText = alert.getText();
        System.out.println("Alert Text: " + alertText);
        alert.accept(); // Click OK
        
        // 2. Confirmation Alert - OK and Cancel buttons
        WebElement confirmButton = driver.findElement(By.id("confirmButton"));
        confirmButton.click();
        Thread.sleep(1000);
        
        Alert confirmAlert = driver.switchTo().alert();
        confirmAlert.dismiss(); // Click Cancel
        // confirmAlert.accept(); // Click OK
        
        // 3. Prompt Alert - Text input + OK and Cancel
        WebElement promptButton = driver.findElement(By.id("promptButton"));
        promptButton.click();
        Thread.sleep(1000);
        
        Alert promptAlert = driver.switchTo().alert();
        promptAlert.sendKeys("Test Text"); // Enter text in prompt
        promptAlert.accept(); // Click OK
        
        driver.quit();
    }
}
```

---

### Q13: How do you handle dropdown selections?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;

public class DropdownHandling {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // Find dropdown element
        WebElement dropdownElement = driver.findElement(By.id("dropdown"));
        
        // Create Select object
        Select dropdown = new Select(dropdownElement);
        
        // 1. SELECT BY VALUE
        dropdown.selectByValue("value2");
        
        // 2. SELECT BY VISIBLE TEXT
        dropdown.selectByVisibleText("Option 2");
        
        // 3. SELECT BY INDEX
        dropdown.selectByIndex(1); // 0-based index
        
        // 4. GET ALL OPTIONS
        java.util.List<WebElement> options = dropdown.getOptions();
        System.out.println("Total options: " + options.size());
        for (WebElement option : options) {
            System.out.println("Option: " + option.getText());
        }
        
        // 5. GET SELECTED OPTION
        WebElement selectedOption = dropdown.getFirstSelectedOption();
        System.out.println("Selected: " + selectedOption.getText());
        
        // 6. IS MULTIPLE SELECT?
        boolean isMultiple = dropdown.isMultiple();
        System.out.println("Is Multiple: " + isMultiple);
        
        // 7. DESELECT OPTIONS (for multi-select)
        if (isMultiple) {
            dropdown.selectByValue("value1");
            dropdown.selectByValue("value2");
            dropdown.deselectByValue("value1");
            dropdown.deselectAll();
        }
        
        driver.quit();
    }
}
```

---

### Q14: How do you handle custom dropdowns (not using <select> tag)?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import java.util.List;

public class CustomDropdownHandling {
    public static void main(String[] args) throws InterruptedException {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // Method 1: Click dropdown to open it, then select option
        WebElement dropdownButton = driver.findElement(By.className("dropdown-btn"));
        dropdownButton.click();
        Thread.sleep(1000);
        
        // Find and click specific option
        List<WebElement> options = driver.findElements(By.className("dropdown-option"));
        for (WebElement option : options) {
            if (option.getText().equals("Option 2")) {
                option.click();
                break;
            }
        }
        
        // Method 2: Using Actions for hover dropdowns
        WebElement parentDropdown = driver.findElement(By.className("parent-dropdown"));
        WebElement childOption = driver.findElement(By.className("child-option"));
        
        org.openqa.selenium.interactions.Actions actions = 
            new org.openqa.selenium.interactions.Actions(driver);
        actions.moveToElement(parentDropdown)
               .moveToElement(childOption)
               .click()
               .perform();
        
        // Method 3: Using XPath to select option
        String optionText = "Desired Option";
        WebElement option = driver.findElement(
            By.xpath("//div[@class='dropdown-option' and contains(text(), '" + optionText + "')]")
        );
        option.click();
        
        driver.quit();
    }
}
```

---

## Advanced Interactions

### Q15: How do you scroll down the page?

```java
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class ScrollingExample {
    public static void main(String[] args) throws InterruptedException {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        JavascriptExecutor js = (JavascriptExecutor) driver;
        
        // 1. Scroll down by pixel
        js.executeScript("window.scrollBy(0, 500);");
        Thread.sleep(2000);
        
        // 2. Scroll to bottom of page
        js.executeScript("window.scrollTo(0, document.body.scrollHeight);");
        Thread.sleep(2000);
        
        // 3. Scroll to specific element
        WebElement element = driver.findElement(By.id("footer"));
        js.executeScript("arguments[0].scrollIntoView();", element);
        Thread.sleep(2000);
        
        // 4. Scroll to specific element with offset
        js.executeScript("arguments[0].scrollIntoView(true);", element); // Top alignment
        js.executeScript("arguments[0].scrollIntoView(false);", element); // Bottom alignment
        
        // 5. Scroll up
        js.executeScript("window.scrollBy(0, -500);");
        
        // 6. Scroll horizontally
        js.executeScript("window.scrollBy(300, 0);");
        
        // 7. Get current scroll position
        Long scrollPosition = (Long) js.executeScript("return window.pageYOffset;");
        System.out.println("Scroll Position: " + scrollPosition);
        
        driver.quit();
    }
}
```

---

### Q16: How do you work with frames and iFrames?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class FramesHandling {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // Method 1: Switch to frame by index
        driver.switchTo().frame(0); // First frame
        
        // Perform operations in frame
        WebElement element = driver.findElement(By.id("elementInFrame"));
        element.click();
        
        // Switch back to main page
        driver.switchTo().defaultContent();
        
        // Method 2: Switch to frame by name or ID
        driver.switchTo().frame("frameName");
        WebElement element2 = driver.findElement(By.id("elementId"));
        element2.click();
        driver.switchTo().defaultContent();
        
        // Method 3: Switch to frame by WebElement
        WebElement frameElement = driver.findElement(By.xpath("//iframe[@id='frameId']"));
        driver.switchTo().frame(frameElement);
        WebElement element3 = driver.findElement(By.id("elementId"));
        element3.click();
        driver.switchTo().defaultContent();
        
        // Method 4: Handle nested frames
        driver.switchTo().frame(0); // Outer frame
        driver.switchTo().frame(0); // Inner frame (nested)
        WebElement nestedElement = driver.findElement(By.id("nestedElementId"));
        nestedElement.click();
        
        // Go back to outer frame
        driver.switchTo().parentFrame();
        
        // Go back to main page
        driver.switchTo().defaultContent();
        
        // Method 5: Get all frames count
        java.util.List<WebElement> frames = driver.findElements(By.tagName("iframe"));
        System.out.println("Total frames: " + frames.size());
        
        driver.quit();
    }
}
```

---

## JavaScriptExecutor

### Q17: What is JavaScriptExecutor and how do you use it?

```java
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.By;

public class JavaScriptExecutorExample {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        JavascriptExecutor js = (JavascriptExecutor) driver;
        
        // 1. Execute JavaScript and get return value
        Object result = js.executeScript("return 2 + 2;");
        System.out.println("Result: " + result); // Output: 4
        
        // 2. Click element using JavaScript
        WebElement element = driver.findElement(By.id("elementId"));
        js.executeScript("arguments[0].click();", element);
        
        // 3. Enter text in element
        WebElement textInput = driver.findElement(By.id("inputId"));
        js.executeScript("arguments[0].value='Text Here';", textInput);
        
        // 4. Set attribute value
        js.executeScript("arguments[0].setAttribute('value', 'New Value');", element);
        
        // 5. Get attribute value
        String attrValue = (String) js.executeScript("return arguments[0].getAttribute('value');", element);
        System.out.println("Attribute Value: " + attrValue);
        
        // 6. Get element text
        String text = (String) js.executeScript("return arguments[0].innerText;", element);
        System.out.println("Element Text: " + text);
        
        // 7. Highlight element
        js.executeScript("arguments[0].style.border='3px solid red';", element);
        
        // 8. Hide element
        js.executeScript("arguments[0].style.display='none';", element);
        
        // 9. Show hidden element
        js.executeScript("arguments[0].style.display='block';", element);
        
        // 10. Remove element
        js.executeScript("arguments[0].remove();", element);
        
        // 11. Disable element
        js.executeScript("arguments[0].disabled=true;", element);
        
        // 12. Get page title
        String title = (String) js.executeScript("return document.title;");
        System.out.println("Page Title: " + title);
        
        // 13. Get page URL
        String url = (String) js.executeScript("return window.location.href;");
        System.out.println("Page URL: " + url);
        
        // 14. Scroll to element
        js.executeScript("arguments[0].scrollIntoView(true);", element);
        
        // 15. Execute async script
        js.executeAsyncScript("var callback = arguments[arguments.length - 1];" +
                              "setTimeout(function() {callback('result');}, 5000);");
        
        driver.quit();
    }
}
```

---

## Screenshots & Reporting

### Q18: How do you take screenshots?

```java
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.apache.commons.io.FileUtils;
import java.io.File;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class ScreenshotExample {
    public static void main(String[] args) throws IOException {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // Method 1: Basic screenshot
        TakesScreenshot screenshot = (TakesScreenshot) driver;
        File srcFile = screenshot.getScreenshotAs(OutputType.FILE);
        File destFile = new File("screenshot.png");
        FileUtils.copyFile(srcFile, destFile);
        System.out.println("Screenshot saved: " + destFile.getAbsolutePath());
        
        // Method 2: Screenshot with timestamp
        String timestamp = new SimpleDateFormat("yyyy-MM-dd HH-mm-ss").format(new Date());
        File timestampFile = new File("screenshot_" + timestamp + ".png");
        FileUtils.copyFile(srcFile, timestampFile);
        
        // Method 3: Screenshot in specific directory
        String screenshotDir = "screenshots/";
        new File(screenshotDir).mkdirs();
        File dirFile = new File(screenshotDir + "screenshot_" + timestamp + ".png");
        FileUtils.copyFile(srcFile, dirFile);
        
        // Method 4: Screenshot as Base64
        String base64Screenshot = screenshot.getScreenshotAs(OutputType.BASE64);
        System.out.println("Base64 Screenshot length: " + base64Screenshot.length());
        
        // Method 5: Screenshot of specific element
        WebElement element = driver.findElement(By.id("elementId"));
        File elementScreenshot = element.getScreenshotAs(OutputType.FILE);
        FileUtils.copyFile(elementScreenshot, new File("element_screenshot.png"));
        
        driver.quit();
    }
}
```

---

### Q19: How do you create a test case? Basic structure?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.*;

public class SampleTestCase {
    
    WebDriver driver;
    
    // Setup - runs before each test
    @BeforeTest
    public void setUp() {
        System.out.println("Browser Launch");
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }
    
    // Test method
    @Test
    public void loginTest() {
        System.out.println("Test Execution");
        
        // Navigate to URL
        driver.get("https://www.example.com/login");
        
        // Find elements and perform actions
        WebElement usernameField = driver.findElement(By.id("username"));
        WebElement passwordField = driver.findElement(By.id("password"));
        WebElement loginButton = driver.findElement(By.id("loginBtn"));
        
        // Input data
        usernameField.sendKeys("testuser");
        passwordField.sendKeys("password123");
        
        // Click login
        loginButton.click();
        
        // Wait for page to load
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        // Verify result
        String expectedUrl = "https://www.example.com/dashboard";
        String actualUrl = driver.getCurrentUrl();
        Assert.assertEquals(actualUrl, expectedUrl, "Login failed");
        
        System.out.println("Test Passed");
    }
    
    // Another test
    @Test
    public void logoutTest() {
        System.out.println("Logout Test Execution");
        
        driver.get("https://www.example.com/dashboard");
        
        WebElement logoutButton = driver.findElement(By.xpath("//button[text()='Logout']"));
        logoutButton.click();
        
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        String expectedUrl = "https://www.example.com/login";
        String actualUrl = driver.getCurrentUrl();
        Assert.assertEquals(actualUrl, expectedUrl, "Logout failed");
    }
    
    // Cleanup - runs after each test
    @AfterTest
    public void tearDown() {
        System.out.println("Browser Close");
        driver.quit();
    }
}
```

---

### Q20: What are TestNG annotations?

```java
import org.testng.annotations.*;

public class TestNGAnnotations {
    
    // Runs once before all tests in the class
    @BeforeClass
    public void beforeClass() {
        System.out.println("Before Class");
    }
    
    // Runs before each test method
    @BeforeMethod
    public void beforeMethod() {
        System.out.println("Before Method");
    }
    
    // Actual test method
    @Test
    public void testMethod1() {
        System.out.println("Test Method 1");
    }
    
    @Test
    public void testMethod2() {
        System.out.println("Test Method 2");
    }
    
    // Runs after each test method
    @AfterMethod
    public void afterMethod() {
        System.out.println("After Method");
    }
    
    // Runs once after all tests in the class
    @AfterClass
    public void afterClass() {
        System.out.println("After Class");
    }
    
    // Runs once before all tests in suite
    @BeforeSuite
    public void beforeSuite() {
        System.out.println("Before Suite");
    }
    
    // Runs once after all tests in suite
    @AfterSuite
    public void afterSuite() {
        System.out.println("After Suite");
    }
    
    // Test with parameters
    @Test(priority = 1)
    public void test1() {
        System.out.println("Test 1 - Priority 1");
    }
    
    @Test(priority = 2)
    public void test2() {
        System.out.println("Test 2 - Priority 2");
    }
    
    // Disable test
    @Test(enabled = false)
    public void disabledTest() {
        System.out.println("This test is disabled");
    }
    
    // Test with timeout (in milliseconds)
    @Test(timeOut = 5000)
    public void testWithTimeout() {
        System.out.println("Test with timeout");
    }
}
```

---

## Additional Common Questions

### Q21: How do you handle SSL certificate issues?

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;

public class SSLCertificateHandling {
    public static void main(String[] args) {
        // Method 1: Using ChromeOptions to ignore SSL
        ChromeOptions options = new ChromeOptions();
        options.setAcceptInsecureCerts(true);
        WebDriver driver = new ChromeDriver(options);
        
        driver.get("https://www.example.com");
        System.out.println("Page loaded with SSL certificate ignored");
        
        driver.quit();
    }
}
```

---

### Q22: How do you set browser capabilities?

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;

public class BrowserCapabilities {
    public static void main(String[] args) {
        ChromeOptions options = new ChromeOptions();
        
        // Add arguments
        options.addArguments("--start-maximized");
        options.addArguments("--disable-notifications");
        options.addArguments("--disable-popup-blocking");
        options.addArguments("--disable-images"); // Disable loading images
        options.addArguments("--headless"); // Run in headless mode
        options.addArguments("--disable-extensions");
        options.addArguments("--no-sandbox");
        options.addArguments("--disable-dev-shm-usage");
        
        // Add user data directory (to maintain cookies/cache)
        options.addArguments("user-data-dir=/path/to/chrome/profile");
        
        // Add preferences
        options.addPreferences("profile.default_content_settings.popups", 0);
        options.addPreferences("profile.managed_default_content_settings.images", 2);
        
        // Accept insecure certificates
        options.setAcceptInsecureCerts(true);
        
        // Enable downloads to specific directory
        options.addPreferences("download.default_directory", "/path/to/downloads");
        
        WebDriver driver = new ChromeDriver(options);
        driver.get("https://www.example.com");
        
        driver.quit();
    }
}
```

---

### Q23: How do you handle file uploads?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class FileUpload {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com/upload");
        
        // Find file input element
        WebElement fileInput = driver.findElement(By.xpath("//input[@type='file']"));
        
        // Method 1: Direct send keys for file path
        String filePath = "/path/to/file.txt";
        fileInput.sendKeys(filePath);
        
        // Method 2: Using absolute path
        String absolutePath = "C:\\Users\\Username\\Desktop\\file.txt";
        fileInput.sendKeys(absolutePath);
        
        // Find and click upload button
        WebElement uploadButton = driver.findElement(By.id("uploadBtn"));
        uploadButton.click();
        
        // Wait for upload to complete
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        driver.quit();
    }
}
```

---

### Q24: How do you handle cookies?

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.Cookie;

public class CookiesHandling {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.example.com");
        
        // Add cookie
        Cookie cookie = new Cookie("cookieName", "cookieValue");
        driver.manage().addCookie(cookie);
        
        // Get specific cookie
        Cookie retrievedCookie = driver.manage().getCookieNamed("cookieName");
        System.out.println("Cookie: " + retrievedCookie.getName() + "=" + retrievedCookie.getValue());
        
        // Get all cookies
        java.util.Set<Cookie> allCookies = driver.manage().getCookies();
        System.out.println("Total cookies: " + allCookies.size());
        for (Cookie c : allCookies) {
            System.out.println(c.getName() + "=" + c.getValue());
        }
        
        // Delete specific cookie
        driver.manage().deleteCookie("cookieName");
        
        // Delete all cookies
        driver.manage().deleteAllCookies();
        
        driver.quit();
    }
}
```

---

### Q25: What is the Page Object Model (POM)?

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;

// Page Object Class
public class LoginPage {
    WebDriver driver;
    
    // Constructor
    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }
    
    // Locators
    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton = By.xpath("//button[@type='submit']");
    private By errorMessage = By.id("errorMsg");
    
    // Methods
    public void enterUsername(String username) {
        driver.findElement(usernameField).sendKeys(username);
    }
    
    public void enterPassword(String password) {
        driver.findElement(passwordField).sendKeys(password);
    }
    
    public void clickLoginButton() {
        driver.findElement(loginButton).click();
    }
    
    public String getErrorMessage() {
        return driver.findElement(errorMessage).getText();
    }
    
    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLoginButton();
    }
}

// Test Class using POM
public class LoginTest {
    public static void main(String[] args) {
        WebDriver driver = new org.openqa.selenium.chrome.ChromeDriver();
        driver.get("https://www.example.com/login");
        
        // Using Page Object Model
        LoginPage loginPage = new LoginPage(driver);
        loginPage.login("testuser", "password123");
        
        driver.quit();
    }
}
```

---

## Summary

This document covers the most commonly asked Selenium interview questions with practical Java Selenium code examples. Key topics include:

1. **Basics** - What Selenium is and its advantages
2. **WebDriver** - Browser initialization and management
3. **Locators** - Different ways to find elements (ID, Name, XPath, CSS, etc.)
4. **Interactions** - Click, sendKeys, Actions API
5. **Waits** - Implicit, Explicit, and Fluent waits
6. **Advanced Topics** - Windows, alerts, frames, JavaScript execution
7. **Best Practices** - Screenshots, Page Object Model, TestNG

**Tips for Interview:**
- Understand the difference between Implicit and Explicit waits
- Know when to use XPath vs CSS Selector
- Be comfortable with Page Object Model pattern
- Understand Selenium Grid for parallel execution
- Know about different exceptions and how to handle them
- Be familiar with TestNG/JUnit for test execution

---

**Last Updated:** 2026
**Created by:** Yash Automation
