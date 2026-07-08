# Cucumber BDD Framework - Complete Guide with Eclipse Setup & Web Testing

## Table of Contents
1. [What is Cucumber BDD?](#what-is-cucumber-bdd)
2. [Cucumber vs Selenium](#cucumber-vs-selenium)
3. [Installation & Setup](#installation--setup)
4. [Project Structure](#project-structure)
5. [Feature Files & Gherkin Syntax](#feature-files--gherkin-syntax)
6. [Step Definitions](#step-definitions)
7. [Test Runner](#test-runner)
8. [Real-World Web Testing Examples](#real-world-web-testing-examples)
9. [Best Practices](#best-practices)
10. [Troubleshooting](#troubleshooting)

---

## What is Cucumber BDD?

### Definition
**Cucumber** is a Behavior Driven Development (BDD) testing framework that allows you to write test scenarios in plain English using Gherkin language. It bridges the gap between technical and non-technical stakeholders.

### Key Features:
- ✅ Write tests in plain English (Gherkin language)
- ✅ Business analysts can understand test scenarios
- ✅ Better code reusability
- ✅ Excellent reporting and documentation
- ✅ Supports multiple programming languages (Java, Python, Ruby, etc.)

### Why Use Cucumber?
1. **Readability** - Business users can read and understand tests
2. **Maintainability** - Easy to update test scenarios
3. **Reusability** - Reuse step definitions across multiple scenarios
4. **Documentation** - Tests serve as living documentation
5. **Collaboration** - Non-developers can participate in test design

---

## Cucumber vs Selenium

| Feature | Cucumber | Selenium |
|---------|----------|----------|
| **Type** | BDD Framework | Web Automation Tool |
| **Language** | Gherkin (Plain English) | Java, Python, C#, etc. |
| **Readability** | Very High (non-technical) | Low (technical) |
| **Purpose** | Test Behavior & Scenarios | Browser Automation |
| **Reporting** | Built-in detailed reports | Requires additional plugins |
| **Learning Curve** | Easy | Medium |
| **Scope** | Functional Testing | Cross-browser Testing |

### Common Use Case:
**Cucumber + Selenium** = BDD framework for web application testing
- Cucumber writes test scenarios in Gherkin
- Selenium automates browser interactions
- Together they provide complete test automation solution

---

## Installation & Setup

### Prerequisites:
1. **Java JDK 11 or higher** installed
2. **Eclipse IDE** installed
3. **Maven** installed (optional, but recommended)
4. **Chrome WebDriver** (for Selenium)

### Step 1: Download & Install Java

**Check if Java is installed:**
```bash
java -version
javac -version
```

**If not installed:**
- Download Java JDK 11+ from: https://www.oracle.com/java/technologies/downloads/
- Set JAVA_HOME environment variable
- Add Java/bin to PATH

**Verify installation:**
```bash
echo %JAVA_HOME%  # Windows
echo $JAVA_HOME   # Mac/Linux
```

---

### Step 2: Install Eclipse IDE

1. Download Eclipse IDE for Java Developers from: https://www.eclipse.org/downloads/
2. Extract and run eclipse.exe (Windows) or eclipse (Mac/Linux)
3. Choose a workspace location
4. Click Launch

---

### Step 3: Install Maven (Optional but Recommended)

**Check if Maven is installed:**
```bash
mvn --version
```

**If not installed:**
1. Download Maven from: https://maven.apache.org/download.cgi
2. Extract to a location (e.g., C:\maven)
3. Set M2_HOME environment variable
4. Add Maven/bin to PATH

**Verify:**
```bash
mvn --version
```

---

### Step 4: Create Maven Project in Eclipse

**Steps:**
1. Open Eclipse
2. Go to `File` → `New` → `Maven Project`
3. Check "Create a simple project" (Skip archetype selection)
4. Click Next
5. Fill in Project details:
   - **Group ID**: com.automation.cucumber
   - **Artifact ID**: cucumber-web-testing
   - **Version**: 1.0-SNAPSHOT
6. Click Finish

---

### Step 5: Add Cucumber Dependencies to pom.xml

**File**: `pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.automation.cucumber</groupId>
    <artifactId>cucumber-web-testing</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- Cucumber Java -->
        <dependency>
            <groupId>io.cucumber</groupId>
            <artifactId>cucumber-java</artifactId>
            <version>7.15.0</version>
        </dependency>

        <!-- Cucumber JUnit 5 -->
        <dependency>
            <groupId>io.cucumber</groupId>
            <artifactId>cucumber-junit-platform-engine</artifactId>
            <version>7.15.0</version>
            <scope>test</scope>
        </dependency>

        <!-- JUnit 5 -->
        <dependency>
            <groupId>org.junit.platform</groupId>
            <artifactId>junit-platform-suite</artifactId>
            <version>1.10.0</version>
            <scope>test</scope>
        </dependency>

        <!-- Selenium WebDriver -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.15.0</version>
        </dependency>

        <!-- WebDriverManager (Auto-downloads drivers) -->
        <dependency>
            <groupId>io.github.bonigarcia</groupId>
            <artifactId>webdrivermanager</artifactId>
            <version>5.6.3</version>
        </dependency>

        <!-- JUnit 4 (for legacy projects) -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Maven Compiler Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>11</source>
                    <target>11</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

**After updating pom.xml:**
- Right-click project → Maven → Update Project
- Wait for dependencies to download

---

### Step 6: Install Cucumber Eclipse Plugin (Optional)

For Gherkin syntax highlighting:

1. Go to `Help` → `Eclipse Marketplace`
2. Search for "Cucumber"
3. Install "Cucumber Eclipse Plugin"
4. Restart Eclipse

---

## Project Structure

```
cucumber-web-testing/
├── src/
│   ├── main/
│   │   └── java/
│   │       └── utils/
│   │           ├── DriverFactory.java
│   │           ├── WebActions.java
│   │           └── ConfigReader.java
│   │
│   └── test/
│       ├── java/
│       │   ├── stepdefinitions/
│       │   │   ├── LoginSteps.java
│       │   │   ├── ProductSteps.java
│       │   │   ├── CartSteps.java
│       │   │   └── CommonSteps.java
│       │   │
│       │   ├── runners/
│       │   │   └── TestRunner.java
│       │   │
│       │   └── hooks/
│       │       └── Hooks.java
│       │
│       └── resources/
│           ├── features/
│           │   ├── login.feature
│           │   ├── shopping.feature
│           │   └── checkout.feature
│           │
│           └── config.properties
│
├── pom.xml
└── README.md
```

---

## Feature Files & Gherkin Syntax

### Gherkin Keywords

| Keyword | Purpose |
|---------|---------|
| **Feature** | Describes what feature is being tested |
| **Scenario** | Describes a specific test case |
| **Given** | Precondition - initial state |
| **When** | Action - what user does |
| **Then** | Expected result - assertion |
| **And** | Additional Given/When/Then |
| **But** | Negative case of Given/When/Then |
| **Background** | Common steps for all scenarios |

### Example 1: Simple Login Feature

**File**: `src/test/resources/features/login.feature`

```gherkin
Feature: User Login Functionality
  As a registered user
  I want to log in to the application
  So that I can access my account

  Background:
    Given I open the web browser
    And I navigate to the login page

  Scenario: Successful login with valid credentials
    When I enter username "testuser@email.com"
    And I enter password "Test@123"
    And I click on the login button
    Then I should be logged in successfully
    And I should see the dashboard

  Scenario: Login fails with invalid credentials
    When I enter username "invalid@email.com"
    And I enter password "wrongpassword"
    And I click on the login button
    Then I should see an error message "Invalid credentials"

  Scenario Outline: Login with multiple users
    When I enter username "<username>"
    And I enter password "<password>"
    And I click on the login button
    Then I should see the message "<result>"

    Examples:
      | username        | password   | result                    |
      | user1@email.com | Pass@123   | Login successful          |
      | user2@email.com | Pass@456   | Login successful          |
      | invalid@email   | WrongPass  | Invalid credentials       |
```

### Example 2: E-Commerce Feature

**File**: `src/test/resources/features/shopping.feature`

```gherkin
Feature: E-Commerce Shopping Cart
  As a customer
  I want to add products to my cart
  So that I can purchase them

  Scenario: Add product to cart
    Given I am on the products page
    When I search for "laptop"
    And I select the first product
    And I click "Add to Cart"
    Then the product should be added to my cart
    And I should see a success message
    And the cart count should increase to 1

  Scenario: Remove product from cart
    Given I have 2 products in my cart
    When I click on the cart icon
    And I click the remove button for the first product
    Then the product should be removed from cart
    And the cart count should decrease to 1

  Scenario: Checkout process
    Given I have products in my cart
    When I click on the checkout button
    And I enter shipping address
    And I select payment method "Credit Card"
    And I click "Place Order"
    Then I should see the confirmation page
    And I should receive an order confirmation email
```

---

## Step Definitions

### Utilities & Base Classes

**File**: `src/main/java/utils/DriverFactory.java`

```java
package utils;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import io.github.bonigarcia.wdm.WebDriverManager;

public class DriverFactory {
    
    private static WebDriver driver;

    // Initialize WebDriver
    public static WebDriver initializeDriver(String browser) {
        switch (browser.toLowerCase()) {
            case "chrome":
                WebDriverManager.chromedriver().setup();
                driver = new ChromeDriver();
                break;
            case "firefox":
                WebDriverManager.firefoxdriver().setup();
                driver = new FirefoxDriver();
                break;
            default:
                WebDriverManager.chromedriver().setup();
                driver = new ChromeDriver();
        }
        
        driver.manage().window().maximize();
        return driver;
    }

    // Get WebDriver instance
    public static WebDriver getDriver() {
        return driver;
    }

    // Close WebDriver
    public static void quitDriver() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```

**File**: `src/main/java/utils/WebActions.java`

```java
package utils;

import org.openqa.selenium.*;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import java.time.Duration;

public class WebActions {
    
    private WebDriver driver;
    private WebDriverWait wait;

    public WebActions(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    // Navigate to URL
    public void navigateTo(String url) {
        driver.navigate().to(url);
        System.out.println("Navigated to: " + url);
    }

    // Find element with wait
    public WebElement findElement(By locator) {
        return wait.until(ExpectedConditions.presenceOfElementLocated(locator));
    }

    // Click element
    public void click(By locator) {
        WebElement element = wait.until(ExpectedConditions.elementToBeClickable(locator));
        element.click();
        System.out.println("Clicked on: " + locator);
    }

    // Send text
    public void sendKeys(By locator, String text) {
        WebElement element = findElement(locator);
        element.clear();
        element.sendKeys(text);
        System.out.println("Entered text: " + text);
    }

    // Get text
    public String getText(By locator) {
        return findElement(locator).getText();
    }

    // Is element displayed
    public boolean isDisplayed(By locator) {
        try {
            return findElement(locator).isDisplayed();
        } catch (TimeoutException e) {
            return false;
        }
    }

    // Wait for element
    public void waitForElement(By locator) {
        wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
    }

    // Get current URL
    public String getCurrentURL() {
        return driver.getCurrentUrl();
    }

    // Scroll to element
    public void scrollToElement(By locator) {
        WebElement element = findElement(locator);
        ((JavascriptExecutor) driver).executeScript("arguments[0].scrollIntoView(true);", element);
    }
}
```

### Login Step Definitions

**File**: `src/test/java/stepdefinitions/LoginSteps.java`

```java
package stepdefinitions;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import io.cucumber.java.en.*;
import utils.DriverFactory;
import utils.WebActions;
import org.junit.Assert;

public class LoginSteps {
    
    private WebDriver driver = DriverFactory.getDriver();
    private WebActions actions = new WebActions(driver);

    // Login page locators
    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton = By.id("loginButton");
    private By errorMessage = By.className("error-message");
    private By welcomeMessage = By.id("welcomeMessage");
    private By dashboard = By.id("dashboard");

    @Given("I open the web browser")
    public void i_open_the_web_browser() {
        System.out.println("Opening web browser...");
        // Browser is already initialized in Hooks
    }

    @And("I navigate to the login page")
    public void i_navigate_to_the_login_page() {
        String loginPageURL = "https://demo.testproject.io/web/";
        actions.navigateTo(loginPageURL);
        System.out.println("Navigated to login page");
    }

    @When("I enter username {string}")
    public void i_enter_username(String username) {
        actions.sendKeys(usernameField, username);
    }

    @And("I enter password {string}")
    public void i_enter_password(String password) {
        actions.sendKeys(passwordField, password);
    }

    @And("I click on the login button")
    public void i_click_on_the_login_button() {
        actions.click(loginButton);
        System.out.println("Clicked login button");
    }

    @Then("I should be logged in successfully")
    public void i_should_be_logged_in_successfully() {
        actions.waitForElement(dashboard);
        Assert.assertTrue("Dashboard is not displayed", actions.isDisplayed(dashboard));
        System.out.println("Login successful!");
    }

    @And("I should see the dashboard")
    public void i_should_see_the_dashboard() {
        Assert.assertTrue("Dashboard not visible", actions.isDisplayed(dashboard));
    }

    @Then("I should see an error message {string}")
    public void i_should_see_an_error_message(String message) {
        actions.waitForElement(errorMessage);
        String actualMessage = actions.getText(errorMessage);
        Assert.assertTrue("Error message not found", actualMessage.contains(message));
        System.out.println("Error message displayed: " + actualMessage);
    }

    @Then("I should see the message {string}")
    public void i_should_see_the_message(String expectedMessage) {
        if (expectedMessage.contains("successful")) {
            Assert.assertTrue("Dashboard not displayed after login", 
                            actions.isDisplayed(dashboard));
        } else if (expectedMessage.contains("Invalid")) {
            Assert.assertTrue("Error message not displayed", 
                            actions.isDisplayed(errorMessage));
        }
    }
}
```

### E-Commerce Step Definitions

**File**: `src/test/java/stepdefinitions/ShoppingSteps.java`

```java
package stepdefinitions;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import io.cucumber.java.en.*;
import utils.DriverFactory;
import utils.WebActions;
import org.junit.Assert;

public class ShoppingSteps {
    
    private WebDriver driver = DriverFactory.getDriver();
    private WebActions actions = new WebActions(driver);
    private int cartCount = 0;

    // Locators
    private By searchBox = By.id("searchBox");
    private By searchButton = By.id("searchButton");
    private By firstProduct = By.xpath("(//div[@class='product-item'])[1]");
    private By addToCartButton = By.id("addToCart");
    private By cartIcon = By.id("cartIcon");
    private By cartCount = By.id("cartCount");
    private By successMessage = By.className("success-message");
    private By removeButton = By.className("remove-btn");
    private By checkoutButton = By.id("checkout");

    @Given("I am on the products page")
    public void i_am_on_the_products_page() {
        String productsURL = "https://demo.testproject.io/web/";
        actions.navigateTo(productsURL);
        System.out.println("On products page");
    }

    @When("I search for {string}")
    public void i_search_for(String productName) {
        actions.sendKeys(searchBox, productName);
        actions.click(searchButton);
        System.out.println("Searched for: " + productName);
    }

    @And("I select the first product")
    public void i_select_the_first_product() {
        actions.click(firstProduct);
        System.out.println("Selected first product");
    }

    @And("I click {string}")
    public void i_click(String buttonText) {
        if (buttonText.equals("Add to Cart")) {
            actions.click(addToCartButton);
            System.out.println("Clicked Add to Cart");
        } else if (buttonText.equals("Checkout")) {
            actions.click(checkoutButton);
            System.out.println("Clicked Checkout");
        }
    }

    @Then("the product should be added to my cart")
    public void the_product_should_be_added_to_my_cart() {
        Assert.assertTrue("Success message not displayed", 
                         actions.isDisplayed(successMessage));
        System.out.println("Product added to cart");
    }

    @And("I should see a success message")
    public void i_should_see_a_success_message() {
        String message = actions.getText(successMessage);
        Assert.assertTrue("Success message not visible", 
                         message.contains("Added") || message.contains("success"));
    }

    @And("the cart count should increase to {int}")
    public void the_cart_count_should_increase_to(int expectedCount) {
        String cartCountText = actions.getText(this.cartCount);
        int actualCount = Integer.parseInt(cartCountText);
        Assert.assertEquals("Cart count mismatch", expectedCount, actualCount);
    }

    @Given("I have {int} products in my cart")
    public void i_have_products_in_my_cart(int count) {
        // Add products to cart (simplified)
        for (int i = 0; i < count; i++) {
            actions.click(addToCartButton);
        }
        cartCount = count;
    }

    @When("I click on the cart icon")
    public void i_click_on_the_cart_icon() {
        actions.click(cartIcon);
        System.out.println("Clicked cart icon");
    }

    @And("I click the remove button for the first product")
    public void i_click_the_remove_button_for_the_first_product() {
        actions.click(removeButton);
        System.out.println("Clicked remove button");
    }

    @Then("the product should be removed from cart")
    public void the_product_should_be_removed_from_cart() {
        // Verify product removed (implementation depends on UI)
        System.out.println("Product removed from cart");
    }

    @And("the cart count should decrease to {int}")
    public void the_cart_count_should_decrease_to(int expectedCount) {
        String cartCountText = actions.getText(this.cartCount);
        int actualCount = Integer.parseInt(cartCountText);
        Assert.assertEquals("Cart count mismatch", expectedCount, actualCount);
    }

    @Given("I have products in my cart")
    public void i_have_products_in_my_cart() {
        // Assume products are already in cart from previous steps
        System.out.println("Products in cart");
    }

    @When("I click on the checkout button")
    public void i_click_on_the_checkout_button() {
        actions.click(checkoutButton);
    }

    @And("I enter shipping address")
    public void i_enter_shipping_address() {
        By addressField = By.id("shippingAddress");
        actions.sendKeys(addressField, "123 Main Street, New York, NY 10001");
        System.out.println("Entered shipping address");
    }

    @And("I select payment method {string}")
    public void i_select_payment_method(String method) {
        By paymentMethod = By.id("paymentMethod");
        actions.sendKeys(paymentMethod, method);
        System.out.println("Selected payment method: " + method);
    }

    @And("I click {string} button")
    public void i_click_button(String buttonName) {
        if (buttonName.equals("Place Order")) {
            By placeOrderButton = By.id("placeOrder");
            actions.click(placeOrderButton);
        }
    }

    @Then("I should see the confirmation page")
    public void i_should_see_the_confirmation_page() {
        By confirmationPage = By.id("orderConfirmation");
        Assert.assertTrue("Confirmation page not displayed", 
                         actions.isDisplayed(confirmationPage));
    }

    @And("I should receive an order confirmation email")
    public void i_should_receive_an_order_confirmation_email() {
        // In real scenario, verify email or check DB
        System.out.println("Order confirmation email sent");
    }
}
```

### Common Steps & Hooks

**File**: `src/test/java/stepdefinitions/CommonSteps.java`

```java
package stepdefinitions;

import org.openqa.selenium.WebDriver;
import io.cucumber.java.en.*;
import utils.DriverFactory;
import utils.WebActions;

public class CommonSteps {
    
    private WebDriver driver = DriverFactory.getDriver();
    private WebActions actions = new WebActions(driver);

    @Given("I wait for {int} seconds")
    public void i_wait_for_seconds(int seconds) {
        try {
            Thread.sleep(seconds * 1000L);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    @Then("I verify the page title is {string}")
    public void i_verify_the_page_title_is(String title) {
        String actualTitle = driver.getTitle();
        org.junit.Assert.assertEquals("Page title mismatch", title, actualTitle);
    }

    @And("I close the browser")
    public void i_close_the_browser() {
        DriverFactory.quitDriver();
        System.out.println("Browser closed");
    }
}
```

**File**: `src/test/java/hooks/Hooks.java`

```java
package hooks;

import io.cucumber.java.Before;
import io.cucumber.java.After;
import org.openqa.selenium.WebDriver;
import utils.DriverFactory;

public class Hooks {

    private WebDriver driver;

    @Before
    public void setUp() {
        System.out.println("========== TEST STARTED ==========");
        // Initialize WebDriver before each scenario
        driver = DriverFactory.initializeDriver("chrome");
    }

    @After
    public void tearDown() {
        System.out.println("========== TEST ENDED ==========");
        // Quit WebDriver after each scenario
        DriverFactory.quitDriver();
    }

    @After
    public void captureScreenshot() {
        // Optionally capture screenshot on failure
        System.out.println("Screenshot captured (if failed)");
    }
}
```

---

## Test Runner

**File**: `src/test/java/runners/TestRunner.java`

```java
package runners;

import org.junit.platform.suite.api.*;

import static io.cucumber.junit.platform.engine.Constants.*;

@Suite
@SelectClasspathResource("features")
@ConfigurationParameter(key = PLUGIN_PROPERTY_NAME, value = 
    "pretty, " +
    "html:target/cucumber-reports/index.html, " +
    "json:target/cucumber-reports/cucumber.json")
@ConfigurationParameter(key = GLUE_PROPERTY_NAME, value = 
    "stepdefinitions, hooks")
@ConfigurationParameter(key = FEATURES_PROPERTY_NAME, value = 
    "src/test/resources/features")
public class TestRunner {
}
```

**Alternative for JUnit 4:**

```java
package runners;

import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;
import org.junit.runner.RunWith;

@RunWith(Cucumber.class)
@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"stepdefinitions", "hooks"},
    plugin = {
        "pretty",
        "html:target/cucumber-reports/index.html",
        "json:target/cucumber-reports/cucumber.json"
    },
    monochrome = true
)
public class TestRunner {
}
```

---

## Real-World Web Testing Examples

### Example 1: DemoWebShop Login Testing

**Feature File**: `src/test/resources/features/demowebshop_login.feature`

```gherkin
Feature: DemoWebShop Login Functionality

  Background:
    Given I open the Chrome browser
    And I navigate to DemoWebShop login page

  Scenario: Successful login with valid credentials
    When I enter the email "testuser@example.com"
    And I enter the password "Password123"
    And I click the login button
    Then I should be logged in successfully
    And I should see my account page
    And I should see logout option

  Scenario: Login fails with wrong password
    When I enter the email "testuser@example.com"
    And I enter the password "WrongPassword"
    And I click the login button
    Then I should see an error message about invalid credentials

  Scenario Outline: Login with various credentials
    When I enter the email "<email>"
    And I enter the password "<password>"
    And I click the login button
    Then I should see the result "<result>"

    Examples:
      | email                    | password       | result           |
      | existing@example.com     | ValidPass123   | Login successful |
      | nonexistent@example.com  | AnyPassword123 | User not found   |
      | testuser@example.com     | WrongPassword  | Invalid password |
```

**Step Definition for DemoWebShop**:

```java
package stepdefinitions;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import io.cucumber.java.en.*;
import utils.DriverFactory;
import utils.WebActions;
import org.junit.Assert;

public class DemoWebShopLoginSteps {
    
    private WebDriver driver = DriverFactory.getDriver();
    private WebActions actions = new WebActions(driver);

    private By emailField = By.id("Email");
    private By passwordField = By.id("Password");
    private By rememberMeCheckbox = By.id("RememberMe");
    private By loginButton = By.xpath("//button[contains(text(), 'Log in')]");
    private By logoutLink = By.className("logout");
    private By myAccountLink = By.xpath("//a[contains(text(), 'My account')]");
    private By errorMessage = By.className("error");

    @Given("I open the Chrome browser")
    public void i_open_the_chrome_browser() {
        System.out.println("Chrome browser opened");
    }

    @And("I navigate to DemoWebShop login page")
    public void i_navigate_to_demowebshop_login_page() {
        actions.navigateTo("https://demowebshop.tricentis.com/login");
        System.out.println("Navigated to DemoWebShop login page");
    }

    @When("I enter the email {string}")
    public void i_enter_the_email(String email) {
        actions.sendKeys(emailField, email);
    }

    @And("I enter the password {string}")
    public void i_enter_the_password(String password) {
        actions.sendKeys(passwordField, password);
    }

    @And("I click the login button")
    public void i_click_the_login_button() {
        actions.click(loginButton);
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    @Then("I should be logged in successfully")
    public void i_should_be_logged_in_successfully() {
        String currentURL = actions.getCurrentURL();
        Assert.assertTrue("Not logged in", currentURL.contains("demowebshop"));
        System.out.println("Login successful!");
    }

    @And("I should see my account page")
    public void i_should_see_my_account_page() {
        Assert.assertTrue("Account page not visible", 
                         actions.isDisplayed(myAccountLink));
    }

    @And("I should see logout option")
    public void i_should_see_logout_option() {
        Assert.assertTrue("Logout option not visible", 
                         actions.isDisplayed(logoutLink));
    }

    @Then("I should see an error message about invalid credentials")
    public void i_should_see_an_error_message_about_invalid_credentials() {
        Assert.assertTrue("Error message not displayed", 
                         actions.isDisplayed(errorMessage));
    }

    @Then("I should see the result {string}")
    public void i_should_see_the_result(String result) {
        if (result.contains("successful")) {
            Assert.assertTrue("Login failed", actions.isDisplayed(logoutLink));
        } else {
            Assert.assertTrue("Error message not shown", 
                            actions.isDisplayed(errorMessage));
        }
    }
}
```

---

## Best Practices

### ✅ Do's:

1. **Write Clear Scenarios**
   ```gherkin
   # Good
   Scenario: User can add product to cart
     Given I am on the products page
     When I add a product to cart
     Then the product should be in my cart

   # Avoid
   Scenario: Test something
   ```

2. **Use Meaningful Locators**
   ```java
   // Good
   private By loginButton = By.id("loginButton");

   // Avoid
   private By button = By.xpath("//button[2]");
   ```

3. **Keep Steps Reusable**
   ```java
   // Reusable
   @When("I enter {string} in {string}")
   public void i_enter_text_in_field(String text, String field) {
       // Generic implementation
   }
   ```

4. **Use Background for Common Steps**
   ```gherkin
   Background:
     Given I open the browser
     And I navigate to login page
   ```

5. **Implement Page Object Model**
   - Separate page elements from step definitions
   - Makes tests more maintainable

### ❌ Don'ts:

1. **Avoid Hard-coded Test Data**
   ```java
   // Avoid
   actions.sendKeys(emailField, "testuser@example.com");

   // Better - use external data or params
   ```

2. **Don't Mix Technical and Business Language**
   ```gherkin
   # Avoid
   Scenario: Click button and verify element by ID
     When I click element with id "btn123"
     Then element with xpath "//div[2]" should be visible
   ```

3. **Avoid Dependent Scenarios**
   ```gherkin
   # Each scenario should be independent
   # Avoid relying on previous scenario's results
   ```

4. **Don't Create Complex Feature Files**
   - Keep them focused and readable
   - One feature per file

5. **Avoid Flaky Tests**
   - Use proper waits (WebDriverWait)
   - Don't use Thread.sleep()

---

## Running Tests in Eclipse

### Method 1: Run as JUnit Test

1. Right-click `TestRunner.java`
2. Select `Run As` → `JUnit Test`
3. Watch test execution in Eclipse console

### Method 2: Run with Maven

```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=TestRunner

# Run with specific tag
mvn test -Dcucumber.filter.tags="@smoke"
```

### Method 3: Using Tags in Feature Files

**Feature File:**
```gherkin
@smoke @regression
Feature: Login Tests

  @smoke
  Scenario: Valid login
    ...

  @regression
  Scenario: Invalid login
    ...
```

**Run specific tags:**
```bash
mvn test -Dcucumber.filter.tags="@smoke"
```

---

## Cucumber Reports

### HTML Report

After running tests, open the report:
```
target/cucumber-reports/index.html
```

### JSON Report

For further analysis:
```
target/cucumber-reports/cucumber.json
```

### Generate PDF Report (Optional)

Add plugin to pom.xml:
```xml
<dependency>
    <groupId>net.masterthought</groupId>
    <artifactId>cucumber-reporting</artifactId>
    <version>7.15.0</version>
</dependency>
```

---

## Troubleshooting

### Issue 1: WebDriver Not Found

**Error**: `WebDriver is not initialized`

**Solution**:
```java
// Ensure Hooks.java @Before runs before steps
@Before
public void setUp() {
    driver = DriverFactory.initializeDriver("chrome");
}
```

---

### Issue 2: Step Definitions Not Found

**Error**: `No steps found matching...`

**Solution**:
```java
// Ensure @SelectClasspathResource or glue path is correct
@ConfigurationParameter(key = GLUE_PROPERTY_NAME, 
    value = "stepdefinitions, hooks")
```

---

### Issue 3: Element Not Found

**Error**: `NoSuchElementException`

**Solution**:
```java
// Use explicit waits instead of implicit waits
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.presenceOfElementLocated(locator));
```

---

### Issue 4: Maven Dependencies Not Downloading

**Solution**:
```bash
# Update Maven project
mvn clean install

# Force update
mvn clean install -U
```

---

### Issue 5: Gherkin Syntax Not Highlighted

**Solution**:
1. Install Cucumber Eclipse Plugin
2. Right-click .feature file → Open With → Gherkin Editor
3. Restart Eclipse

---

## Advanced Features

### Parameterization with Scenario Outline

```gherkin
Scenario Outline: Login with multiple users
  When I enter username "<username>"
  And I enter password "<password>"
  Then I should see "<result>"

  Examples:
    | username | password | result |
    | user1    | pass1    | success |
    | user2    | pass2    | success |
```

### Hooks for Setup/Teardown

```java
@Before
public void beforeScenario() {
    // Setup before each scenario
}

@After
public void afterScenario() {
    // Cleanup after each scenario
}
```

### Data-Driven Testing

```java
@When("I enter user data from row {int}")
public void i_enter_user_data_from_row(int rowNumber) {
    // Read data from Excel/CSV
    // Enter data in form
}
```

---

## Summary

### Key Points:
1. ✅ Cucumber uses Gherkin for readable test scenarios
2. ✅ Feature files describe "what" to test
3. ✅ Step definitions describe "how" to test
4. ✅ Hooks handle setup and teardown
5. ✅ Test Runner connects everything together
6. ✅ Works seamlessly with Selenium for web automation
7. ✅ Generates comprehensive reports
8. ✅ Supports data-driven testing with Scenario Outline
9. ✅ Promotes collaboration between QA, Developers, and Business

### Next Steps:
1. Create your first feature file
2. Implement step definitions with Selenium
3. Run tests with TestRunner
4. Analyze reports
5. Expand test coverage
6. Implement CI/CD integration

---

## Useful Resources

- **Cucumber Documentation**: https://cucumber.io/docs/cucumber/
- **Selenium Documentation**: https://www.selenium.dev/documentation/
- **Gherkin Syntax**: https://cucumber.io/docs/gherkin/
- **WebDriverManager**: https://github.com/bonigarcia/webdrivermanager
- **JUnit 5**: https://junit.org/junit5/docs/current/user-guide/

---

Congratulations! You now have a complete understanding of Cucumber BDD Framework and can create robust web automation tests! 🎉
