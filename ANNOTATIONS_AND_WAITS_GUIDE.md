# Selenium Annotations and Waits - Complete Guide

## Table of Contents
1. [Selenium Annotations Explained](#selenium-annotations-explained)
2. [Selenium Waits Explained](#selenium-waits-explained)
3. [When to Use Which Annotation](#when-to-use-which-annotation)
4. [When to Use Which Wait](#when-to-use-which-wait)
5. [Top 50 Recruiter Questions](#top-50-recruiter-questions)

---

## Selenium Annotations Explained

### What are Annotations?
Annotations in Selenium are metadata that provide information about the test but are not part of the actual code execution. They are used in testing frameworks like TestNG and JUnit to mark methods and classes with special instructions.

### Common Selenium Annotations (TestNG)

#### 1. **@BeforeSuite**
```java
@BeforeSuite
public void setUp() {
    System.out.println("Before Suite - Runs once before entire test suite");
}
```
- Runs once before all tests in the suite
- Ideal for one-time setup like database connections

#### 2. **@BeforeTest**
```java
@BeforeTest
public void beforeTest() {
    System.out.println("Before Test - Runs before each test tag in TestNG XML");
}
```
- Runs before each test tag in TestNG XML file
- Used for test-level setup

#### 3. **@BeforeClass**
```java
@BeforeClass
public void beforeClass() {
    driver = new ChromeDriver();
    System.out.println("Before Class - Runs before first method in class");
}
```
- Runs once before the first test method in the class
- Best for initializing WebDriver

#### 4. **@BeforeMethod**
```java
@BeforeMethod
public void beforeMethod() {
    System.out.println("Before Method - Runs before each test method");
}
```
- Runs before each test method
- Used for resetting state between tests

#### 5. **@Test**
```java
@Test
public void testLogin() {
    System.out.println("Test Method - Actual test execution");
}
```
- Marks the method as a test method
- Executes the actual test logic

#### 6. **@AfterMethod**
```java
@AfterMethod
public void afterMethod() {
    System.out.println("After Method - Runs after each test method");
}
```
- Runs after each test method
- Used for cleanup and reporting

#### 7. **@AfterClass**
```java
@AfterClass
public void afterClass() {
    driver.quit();
    System.out.println("After Class - Runs after all methods in class");
}
```
- Runs once after all test methods in the class
- Best for closing WebDriver

#### 8. **@AfterTest**
```java
@AfterTest
public void afterTest() {
    System.out.println("After Test - Runs after each test tag");
}
```
- Runs after each test tag in TestNG XML
- Used for test-level cleanup

#### 9. **@AfterSuite**
```java
@AfterSuite
public void tearDown() {
    System.out.println("After Suite - Runs once after all tests");
}
```
- Runs once after the entire test suite
- Used for final cleanup

### Annotation Execution Order (TestNG)

```
@BeforeSuite
    ↓
@BeforeTest
    ↓
@BeforeClass
    ↓
@BeforeMethod → @Test → @AfterMethod (repeats for each test)
    ↓
@AfterClass
    ↓
@AfterTest
    ↓
@AfterSuite
```

### Other Important Annotations

#### **@Ignore**
```java
@Ignore
@Test
public void testToIgnore() {
    System.out.println("This test will be ignored");
}
```
- Skips a test method during execution

#### **@DataProvider**
```java
@DataProvider(name = "loginData")
public Object[][] getLoginData() {
    return new Object[][] {
        {"user1", "pass1"},
        {"user2", "pass2"},
        {"user3", "pass3"}
    };
}

@Test(dataProvider = "loginData")
public void testLoginWithData(String username, String password) {
    System.out.println("Testing with " + username + " and " + password);
}
```
- Provides multiple data sets for a test method

#### **@Parameters**
```java
@Parameters({"browser", "url"})
@Test
public void testWithParameters(String browser, String url) {
    System.out.println("Browser: " + browser + ", URL: " + url);
}
```
- Passes parameters from TestNG XML file

---

## Selenium Waits Explained

Waits are used to handle timing issues in Selenium tests. They wait for elements to be available before performing actions.

### Types of Waits

#### 1. **Implicit Wait**
```java
WebDriver driver = new ChromeDriver();
driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
// or in Selenium 4+
driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
```

**Characteristics:**
- Global wait for all elements
- Applied once for the entire driver session
- Default polling interval: 500ms
- Throws `NoSuchElementException` if element not found

**Pros:**
- Simple to implement
- Applied to all elements automatically

**Cons:**
- Makes tests slower (waits the full timeout even if element found quickly)
- Can't handle dynamic waits
- Not suitable for AJAX applications

#### 2. **Explicit Wait (WebDriverWait)**
```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
WebElement element = wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("element")));
element.click();
```

**Characteristics:**
- Waits for specific conditions
- Applied to specific elements
- Default polling interval: 500ms
- Throws `TimeoutException` if condition not met

**Pros:**
- More flexible and precise
- Waits only for required time
- Better performance
- Handles dynamic elements

**Cons:**
- More code to write
- Need to handle exceptions

#### 3. **Fluent Wait**
```java
Wait<WebDriver> wait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(15))
    .pollingEvery(Duration.ofMillis(500))
    .ignoring(NoSuchElementException.class);

WebElement element = wait.until(driver -> driver.findElement(By.id("element")));
```

**Characteristics:**
- Customizable polling interval
- Can ignore specific exceptions
- More control over wait behavior

**Pros:**
- Highly customizable
- Can ignore exceptions
- Custom polling intervals

**Cons:**
- More complex syntax
- Rarely needed in practice

#### 4. **Thread.sleep()**
```java
Thread.sleep(5000); // Wait for 5 seconds
```

**Characteristics:**
- Hard wait
- Pauses execution for specified time
- Not recommended for production

**Pros:**
- Simple

**Cons:**
- Increases test execution time
- No dynamic waiting
- Not suitable for web elements

### Common ExpectedConditions

```java
// Wait for element visibility
wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("element")));

// Wait for element presence
wait.until(ExpectedConditions.presenceOfElementLocated(By.id("element")));

// Wait for element clickability
wait.until(ExpectedConditions.elementToBeClickable(By.id("button")));

// Wait for element invisibility
wait.until(ExpectedConditions.invisibilityOfElementLocated(By.id("element")));

// Wait for element to be selected
wait.until(ExpectedConditions.elementToBeSelected(By.id("checkbox")));

// Wait for text in element
wait.until(ExpectedConditions.textToBePresentInElement(element, "Text"));

// Wait for URL to contain
wait.until(ExpectedConditions.urlContains("google.com"));

// Wait for title
wait.until(ExpectedConditions.titleContains("Title"));

// Wait for number of elements
wait.until(ExpectedConditions.numberOfElementsToBeMoreThan(By.className("item"), 5));

// Wait for alert
wait.until(ExpectedConditions.alertIsPresent());
```

---

## When to Use Which Annotation

| Annotation | When to Use | Example |
|-----------|-----------|---------|
| @BeforeSuite | One-time setup for entire suite (DB connections, config) | Loading test data from database |
| @BeforeTest | Setup before each test block in XML | Starting fresh browser session per test |
| @BeforeClass | Setup before first method in class | Initializing WebDriver |
| @BeforeMethod | Before each test method (resetting state) | Navigating to home page |
| @Test | Mark actual test methods | testLoginFunctionality() |
| @AfterMethod | Cleanup after each test | Taking screenshots on failure |
| @AfterClass | Cleanup after all class methods | Closing WebDriver |
| @AfterTest | Cleanup after test block | Clearing test data |
| @AfterSuite | Final cleanup after all tests | Closing database connections |
| @Ignore | Skip a test temporarily | @Ignore("Pending fix") |
| @DataProvider | Parameterize tests with multiple data sets | Multiple login credentials |
| @Parameters | External parameters from XML | Browser type, Base URL |

---

## When to Use Which Wait

| Wait Type | When to Use | Best For |
|----------|-----------|---------|
| Implicit Wait | General-purpose, simple scripts | Small scripts, all elements same speed |
| Explicit Wait (WebDriverWait) | AJAX applications, dynamic content | Production code, modern web apps |
| Fluent Wait | Custom polling, ignore exceptions | Complex scenarios, special requirements |
| Thread.sleep() | NEVER in production | Learning/debugging only |

### Decision Tree
```
Does element load dynamically?
├─ YES → Use Explicit Wait
└─ NO → 
    └─ Simple script?
       ├─ YES → Implicit Wait is OK
       └─ NO → Use Explicit Wait
```

---

## Top 50 Recruiter Questions

### Annotations (Questions 1-20)

1. **What is an annotation in Selenium/TestNG?**
   - Annotations are metadata that provide information about the test but are not directly part of the code execution. They start with @ symbol.

2. **What is the difference between @BeforeClass and @BeforeMethod?**
   - @BeforeClass runs once before the first test method in the class
   - @BeforeMethod runs before each test method

3. **What is @BeforeSuite used for?**
   - Runs once before the entire test suite. Used for one-time setup like database initialization.

4. **Explain the execution order of TestNG annotations.**
   - @BeforeSuite → @BeforeTest → @BeforeClass → @BeforeMethod → @Test → @AfterMethod → @AfterClass → @AfterTest → @AfterSuite

5. **What is the purpose of @Test annotation?**
   - Marks a method as a test method. TestNG executes all methods marked with @Test.

6. **What is @Ignore annotation used for?**
   - Temporarily skips a test method during execution without removing it from the code.

7. **Can you have multiple @BeforeSuite methods?**
   - No, you should have only one @BeforeSuite in a class. If multiple exist, execution order is unpredictable.

8. **What is @DataProvider annotation?**
   - Provides multiple data sets to a test method. Allows parameterization of tests.

9. **What is @Parameters annotation?**
   - Passes parameters to test methods from TestNG XML configuration file.

10. **What is the difference between @DataProvider and @Parameters?**
    - @DataProvider provides data from Java code; @Parameters provides data from XML configuration.

11. **Can you use @Test and @Ignore on the same method?**
    - Yes, @Ignore takes precedence and the test will be skipped.

12. **What happens if @BeforeClass method fails?**
    - All test methods in that class are skipped, and @AfterClass is executed.

13. **Can @BeforeMethod be used with @DataProvider?**
    - Yes, @BeforeMethod runs before each iteration of parameterized test.

14. **What is the purpose of @AfterClass annotation?**
    - Runs once after all test methods in the class complete. Used for cleanup like closing WebDriver.

15. **Can you have @BeforeClass and @BeforeTest in the same class?**
    - Yes, @BeforeTest runs first, then @BeforeClass.

16. **What is @Factory annotation?**
    - Creates multiple instances of a test class with different parameters.

17. **What happens if @AfterSuite method fails?**
    - It's logged as a failure, but other tests are already completed.

18. **Is @BeforeSuite mandatory?**
    - No, it's optional. Only use if you need one-time setup.

19. **Can @DataProvider method be static?**
    - Yes, @DataProvider can be static. If non-static, a new instance is created.

20. **What is the difference between @BeforeTest and @BeforeClass?**
    - @BeforeTest runs before each test tag in XML; @BeforeClass runs before first method in the Java class.

### Waits (Questions 21-50)

21. **What is a wait in Selenium?**
    - Waits are used to introduce timing delays to allow web elements to load before performing actions.

22. **What is implicit wait?**
    - A global wait applied to all elements in the WebDriver session. Waits up to specified time for element to be present.

23. **What is explicit wait?**
    - A specific wait condition applied to a particular element. Uses WebDriverWait and ExpectedConditions.

24. **What is the difference between implicit and explicit wait?**
    - Implicit: Applied to all elements, set once, waits for presence
    - Explicit: Applied to specific elements, set multiple times, waits for various conditions

25. **What exceptions do implicit and explicit waits throw?**
    - Implicit: NoSuchElementException
    - Explicit: TimeoutException

26. **What is fluent wait?**
    - A wait that allows customizing polling interval and ignoring specific exceptions.

27. **What is the default polling interval for implicit and explicit waits?**
    - Both have default polling interval of 500ms (0.5 seconds).

28. **Can you use both implicit and explicit waits together?**
    - Yes, but it's not recommended. Explicit wait will be applied first.

29. **What is Thread.sleep() in Selenium?**
    - Hard wait that pauses execution for specified milliseconds. Not recommended for production.

30. **What are the disadvantages of implicit wait?**
    - Makes tests slower, can't handle dynamic waits, waits full timeout even if element found quickly.

31. **What is WebDriverWait?**
    - A class that implements explicit wait. Allows waiting for specific conditions with customizable timeout.

32. **What is FluentWait?**
    - Parent class of WebDriverWait. Provides more customization including polling interval and exception handling.

33. **What is ExpectedConditions?**
    - A utility class providing predefined conditions to wait for (visibility, clickability, presence, etc.).

34. **What conditions can ExpectedConditions check?**
    - visibilityOfElementLocated, presenceOfElementLocated, elementToBeClickable, invisibilityOfElementLocated, textToBePresentInElement, urlContains, titleContains, alertIsPresent, etc.

35. **When should you use explicit wait instead of implicit wait?**
    - Always use explicit wait in production code for better control and reliability.

36. **What is the default timeout for implicit wait?**
    - No default timeout. You must set it explicitly using driver.manage().timeouts().implicitlyWait().

37. **Can you change implicit wait timeout during test execution?**
    - Yes, you can set new implicit wait timeout at any point in the test.

38. **What is visibilityOfElementLocated ExpectedCondition?**
    - Waits for element to be present AND visible on the page (not hidden by CSS).

39. **What is presenceOfElementLocated ExpectedCondition?**
    - Waits for element to be present in DOM, regardless of visibility.

40. **What is elementToBeClickable ExpectedCondition?**
    - Waits for element to be visible and enabled (clickable).

41. **What is the difference between visibility and presence of element?**
    - Presence: Element exists in DOM
    - Visibility: Element is displayed on screen and not hidden

42. **What is invisibilityOfElementLocated ExpectedCondition?**
    - Waits for element to become invisible or removed from DOM.

43. **Can you create custom ExpectedConditions?**
    - Yes, you can implement custom ExpectedConditions by extending Function<WebDriver, T>.

44. **What happens if wait timeout expires?**
    - TimeoutException is thrown (for explicit wait) or NoSuchElementException (for implicit wait).

45. **How to wait for AJAX calls to complete?**
    - Use explicit wait with ExpectedCondition checking jQuery ajax status or use waits for element appearance.

46. **What is the best practice for using waits?**
    - Use explicit waits, set appropriate timeouts based on application, avoid mixing implicit and explicit waits.

47. **How to wait for multiple elements?**
    - Use presenceOfAllElementsLocatedBy or numberOfElementsToBeMoreThan ExpectedCondition.

48. **What is fluent wait polling interval?**
    - Time between checking for condition. Default is 500ms, but can be customized.

49. **Can you ignore exceptions in fluent wait?**
    - Yes, using .ignoring(ExceptionClass.class) method.

50. **What is the recommended timeout duration for waits?**
    - 10-15 seconds for most scenarios. Can be higher for slow applications, lower for fast applications.

---

## Complete Example: Using Annotations and Waits Together

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.testng.annotations.*;
import java.time.Duration;

public class LoginTest {
    
    WebDriver driver;
    WebDriverWait wait;
    
    @BeforeSuite
    public void beforeSuite() {
        System.out.println("Setting up test environment");
    }
    
    @BeforeClass
    public void beforeClass() {
        // Initialize WebDriver
        driver = new ChromeDriver();
        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        
        // Set implicit wait
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(5));
    }
    
    @BeforeMethod
    public void beforeMethod() {
        // Navigate to application
        driver.get("https://example.com/login");
    }
    
    @Test(dataProvider = "loginCredentials")
    public void testValidLogin(String username, String password) {
        // Use explicit wait for username field
        WebElement usernameField = wait.until(
            ExpectedConditions.visibilityOfElementLocated(By.id("username"))
        );
        usernameField.sendKeys(username);
        
        // Use explicit wait for password field
        WebElement passwordField = wait.until(
            ExpectedConditions.visibilityOfElementLocated(By.id("password"))
        );
        passwordField.sendKeys(password);
        
        // Use explicit wait for login button to be clickable
        WebElement loginButton = wait.until(
            ExpectedConditions.elementToBeClickable(By.id("login"))
        );
        loginButton.click();
        
        // Wait for success message
        wait.until(ExpectedConditions.textToBePresentInElement(
            driver.findElement(By.id("message")), "Login Successful"
        ));
    }
    
    @Test
    @Ignore
    public void testPendingFeature() {
        // This test will be skipped
    }
    
    @DataProvider(name = "loginCredentials")
    public Object[][] getLoginCredentials() {
        return new Object[][] {
            {"user1@email.com", "password123"},
            {"user2@email.com", "password456"},
            {"user3@email.com", "password789"}
        };
    }
    
    @AfterMethod
    public void afterMethod() {
        System.out.println("Test completed");
    }
    
    @AfterClass
    public void afterClass() {
        driver.quit();
    }
    
    @AfterSuite
    public void afterSuite() {
        System.out.println("Test suite completed");
    }
}
```

---

## Best Practices

### For Annotations:
1. Use @BeforeClass for WebDriver initialization
2. Use @BeforeMethod for test setup/reset
3. Use @AfterClass for WebDriver cleanup
4. Use @AfterMethod for taking screenshots on failure
5. Always use @DataProvider for parameterized tests
6. Don't overuse @BeforeSuite - only for truly one-time setup

### For Waits:
1. Prefer explicit waits over implicit waits
2. Use appropriate ExpectedConditions for your scenario
3. Set realistic timeouts (10-15 seconds typically)
4. Avoid Thread.sleep() in production code
5. Don't mix implicit and explicit waits unnecessarily
6. Use fluent waits only when you need custom polling

---

## Resources
- [TestNG Documentation](https://testng.org/doc/)
- [Selenium Documentation](https://www.selenium.dev/documentation/)
- [ExpectedConditions API](https://www.selenium.dev/selenium/docs/api/java/org/openqa/selenium/support/ui/ExpectedConditions.html)
