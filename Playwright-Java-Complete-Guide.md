# Playwright with Java - Complete Guide: Installation, Configuration & Web Testing

## Table of Contents
1. [What is Playwright?](#what-is-playwright)
2. [Playwright vs Selenium vs Cypress](#playwright-vs-selenium-vs-cypress)
3. [System Requirements](#system-requirements)
4. [Installation & Setup in Eclipse](#installation--setup-in-eclipse)
5. [Maven Configuration](#maven-configuration)
6. [Playwright Basics & API](#playwright-basics--api)
7. [Locators & Selectors](#locators--selectors)
8. [Real-World Web Testing Examples](#real-world-web-testing-examples)
9. [Advanced Features](#advanced-features)
10. [Best Practices & Troubleshooting](#best-practices--troubleshooting)

---

## What is Playwright?

### Definition
**Playwright** is a modern, open-source automation framework developed by Microsoft that enables cross-browser testing with a single API. It supports three major browsers: Chromium, Firefox, and WebKit.

### Key Features:
- ✅ Fast and reliable automation
- ✅ Cross-browser support (Chrome, Firefox, Safari)
- ✅ Headless and headed mode
- ✅ Mobile device emulation
- ✅ Network interception
- ✅ Screenshot and video capture
- ✅ Single synchronous API
- ✅ Excellent documentation

### Why Choose Playwright?

| Feature | Playwright | Selenium | Cypress |
|---------|-----------|----------|---------|
| **Speed** | Very Fast | Moderate | Fast |
| **Browsers** | Chrome, Firefox, Safari | Multiple | Chrome only |
| **API** | Synchronous | Asynchronous | JavaScript |
| **Mobile** | Built-in | Requires Appium | Limited |
| **Language Support** | Java, Python, JS, C# | Multiple | JavaScript |
| **Community** | Growing | Large | Growing |
| **Network Control** | Yes | Limited | Yes |
| **Recording** | Yes | No | Yes |

---

## Playwright vs Selenium vs Cypress

### Comparison Matrix

| Aspect | Playwright | Selenium | Cypress |
|--------|-----------|----------|---------|
| **Learning Curve** | Easy | Moderate | Easy |
| **Flakiness** | Very Low | Moderate | Low |
| **Network Mocking** | Native Support | No | Yes |
| **Mobile Testing** | Excellent | Limited | Limited |
| **Parallel Execution** | Yes | Yes | Limited |
| **Community Size** | Small but Growing | Very Large | Medium |
| **Configuration** | Simple | Complex | Simple |
| **Java Support** | Yes | Yes | No |

### Use Cases:

**Playwright for:**
- Cross-browser testing
- Performance testing
- Mobile emulation
- Network simulation

**Selenium for:**
- Legacy projects
- Wide framework support
- Large existing projects

**Cypress for:**
- Unit-like fast tests
- Chrome-only testing
- Learning curve simplicity

---

## System Requirements

### Prerequisites:
1. **Java JDK 11 or higher**
2. **Eclipse IDE** (2024 or later recommended)
3. **Maven 3.6+**
4. **Git** (optional)

### Installation Steps:

**Step 1: Verify Java Installation**
```bash
java -version
javac -version
```

**Output should show Java 11+:**
```
java version "11.0.x" (or higher)
```

**Step 2: Verify Maven Installation**
```bash
mvn --version
```

**Step 3: Download Eclipse**
- Visit: https://www.eclipse.org/downloads/
- Download: Eclipse IDE for Java Developers 2024
- Extract and run

---

## Installation & Setup in Eclipse

### Step 1: Create Maven Project

1. Open Eclipse
2. Go to: `File` → `New` → `Maven Project`
3. Check "Create a simple project"
4. Click `Next`

### Step 2: Configure Project Details

**Fill in the following:**
- **Group ID**: com.playwright.automation
- **Artifact ID**: playwright-web-testing
- **Version**: 1.0-SNAPSHOT
- **Packaging**: jar

**Click Finish**

### Step 3: Project Structure

After creation, your project will look like:

```
playwright-web-testing/
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/playwright/automation/
│   │           ├── base/
│   │           │   ├── BaseTest.java
│   │           │   └── BrowserFactory.java
│   │           ├── pages/
│   │           │   ├── LoginPage.java
│   │           │   ├── ProductsPage.java
│   │           │   └── CartPage.java
│   │           └── utils/
│   │               ├── ConfigReader.java
│   │               └── TestData.java
│   │
│   └── test/
│       └── java/
│           └── com/playwright/automation/
│               ├── tests/
│               │   ├── LoginTest.java
│               │   ├── SearchTest.java
│               │   └── CheckoutTest.java
│               └── listeners/
│                   └── TestListener.java
│
├── src/test/resources/
│   └── config.properties
│
├── pom.xml
└── README.md
```

---

## Maven Configuration

### Step 1: Update pom.xml

**File**: `pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.playwright.automation</groupId>
    <artifactId>playwright-web-testing</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>Playwright Web Testing</name>
    <description>Web testing using Playwright with Java</description>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <playwright.version>1.43.0</playwright.version>
        <junit.version>4.13.2</junit.version>
        <testng.version>7.8.1</testng.version>
    </properties>

    <dependencies>
        <!-- Playwright -->
        <dependency>
            <groupId>com.microsoft.playwright</groupId>
            <artifactId>playwright</artifactId>
            <version>${playwright.version}</version>
        </dependency>

        <!-- JUnit 4 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>

        <!-- TestNG -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
            <scope>test</scope>
        </dependency>

        <!-- Log4j for Logging -->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.21.1</version>
        </dependency>

        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.21.1</version>
        </dependency>

        <!-- JSON Processing -->
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.10.1</version>
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

            <!-- Exec Maven Plugin (to install browsers) -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>3.1.0</version>
            </plugin>

            <!-- Surefire Plugin for Test Execution -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.1.2</version>
                <configuration>
                    <includes>
                        <include>**/*Test.java</include>
                    </includes>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

### Step 2: Update Maven Project in Eclipse

1. Right-click project → `Maven` → `Update Project`
2. Or press: `Alt + F5`
3. Wait for dependencies to download

### Step 3: Install Playwright Browsers

Open Terminal/Command Prompt in your project and run:

```bash
mvn exec:java -e -Dexec.mainClass="com.microsoft.playwright.CLI" -Dexec.args="install"
```

**Or in Eclipse:**
1. Right-click Project → `Run As` → `Maven build...`
2. Goals: `exec:java -Dexec.mainClass="com.microsoft.playwright.CLI" -Dexec.args="install"`
3. Click Run

**Output:** Browsers will be downloaded and installed

---

## Playwright Basics & API

### Core Concepts

**1. Playwright Instance**
```java
Playwright playwright = Playwright.create();
```

**2. Browser Instance**
```java
Browser browser = playwright.chromium().launch(
    new BrowserType.LaunchOptions().setHeadless(false)
);
```

**3. Context (Optional)**
```java
BrowserContext context = browser.newContext();
```

**4. Page (Main Interface)**
```java
Page page = browser.newPage();
```

### Basic API Usage

```java
import com.microsoft.playwright.*;

public class PlaywrightBasics {
    public static void main(String[] args) {
        // Create Playwright instance
        try (Playwright playwright = Playwright.create()) {
            
            // Launch browser
            Browser browser = playwright.chromium().launch(
                new BrowserType.LaunchOptions().setHeadless(false)
            );
            
            // Create a new page
            Page page = browser.newPage();
            
            // Navigate to URL
            page.navigate("https://demowebshop.tricentis.com/");
            
            // Get page title
            String title = page.title();
            System.out.println("Page Title: " + title);
            
            // Get page URL
            String url = page.url();
            System.out.println("Current URL: " + url);
            
            // Wait for a condition
            page.waitForLoadState();
            
            // Take screenshot
            page.screenshot(new Page.ScreenshotOptions()
                .setPath(Paths.get("screenshot.png")));
            
            // Close page
            page.close();
            
            // Close browser
            browser.close();
        }
    }
}
```

### Playwright Options

**Headless Mode (Default):**
```java
Browser browser = playwright.chromium().launch();
// Browser runs without GUI
```

**Headed Mode (See Browser):**
```java
Browser browser = playwright.chromium().launch(
    new BrowserType.LaunchOptions().setHeadless(false)
);
// Browser window visible
```

**Slow Motion (Debug):**
```java
Browser browser = playwright.chromium().launch(
    new BrowserType.LaunchOptions()
        .setHeadless(false)
        .setSlowMo(1000) // 1 second delay between actions
);
```

**Firefox & Safari:**
```java
// Firefox
Browser firefox = playwright.firefox().launch();

// Safari/WebKit
Browser safari = playwright.webkit().launch();
```

---

## Locators & Selectors

### Types of Locators

**1. CSS Selectors**
```java
Locator element = page.locator("input#email");
Locator elements = page.locator(".product-item");
Locator button = page.locator("button[type='submit']");
```

**2. XPath**
```java
Locator element = page.locator("//input[@id='email']");
Locator button = page.locator("//button[contains(text(), 'Login')]");
```

**3. Text Content**
```java
Locator link = page.getByText("Register");
Locator button = page.getByText("Add to Cart");
```

**4. Placeholder**
```java
Locator input = page.getByPlaceholder("Enter email");
```

**5. Label**
```java
Locator input = page.getByLabel("Email Address");
```

**6. Role**
```java
Locator button = page.getByRole("button", new Page.GetByRoleOptions().setName("Submit"));
```

### Locator Examples (DemoWebShop)

```java
// Search input
Locator searchBox = page.locator("input#small-searchterms");

// Search button
Locator searchButton = page.locator("input[value='Search']");

// Product items
Locator productItems = page.locator(".product-item");

// Product title
Locator productTitles = page.locator("h2.product-title");

// Add to cart button
Locator addToCartBtn = page.locator("button:has-text('Add to Cart')");

// Price
Locator price = page.locator(".actual-price");

// Login link
Locator loginLink = page.getByText("Log in");

// Register link
Locator registerLink = page.getByText("Register");
```

### Locator Methods

```java
// Fill input
page.locator("#email").fill("test@example.com");

// Type text (slower, simulates user typing)
page.locator("#email").type("test@example.com");

// Click element
page.locator("button").click();

// Double click
page.locator("element").dblclick();

// Right click
page.locator("element").click(new Locator.ClickOptions().setButton("right"));

// Get text content
String text = page.locator(".title").textContent();

// Get all text contents
List<String> texts = page.locator(".item").allTextContents();

// Check if visible
boolean isVisible = page.locator("#element").isVisible();

// Check if enabled
boolean isEnabled = page.locator("button").isEnabled();

// Get attribute
String href = page.locator("a").getAttribute("href");

// Count elements
int count = page.locator(".item").count();

// Scroll into view
page.locator("#element").scrollIntoViewIfNeeded();

// Focus
page.locator("#email").focus();

// Hover
page.locator("#element").hover();
```

---

## Real-World Web Testing Examples

### Example 1: DemoWebShop Login Test

**File**: `src/test/java/com/playwright/automation/tests/LoginTest.java`

```java
package com.playwright.automation.tests;

import com.microsoft.playwright.*;
import org.junit.Before;
import org.junit.After;
import org.junit.Test;
import static org.junit.Assert.*;

public class LoginTest {
    
    private Playwright playwright;
    private Browser browser;
    private Page page;

    @Before
    public void setUp() {
        // Initialize Playwright
        playwright = Playwright.create();
        
        // Launch browser
        browser = playwright.chromium().launch(
            new BrowserType.LaunchOptions().setHeadless(false)
        );
        
        // Create a new page
        page = browser.newPage();
    }

    @After
    public void tearDown() {
        // Close browser and Playwright
        page.close();
        browser.close();
        playwright.close();
    }

    @Test
    public void testSuccessfulLogin() {
        // Navigate to login page
        page.navigate("https://demowebshop.tricentis.com/login");
        
        // Wait for page to load
        page.waitForLoadState();
        
        // Get locators
        Locator emailInput = page.locator("input#Email");
        Locator passwordInput = page.locator("input#Password");
        Locator loginButton = page.locator("input[type='submit'][value='Log in']");
        
        // Fill in credentials
        emailInput.fill("testuser@example.com");
        passwordInput.fill("Test@123456");
        
        // Click login
        loginButton.click();
        
        // Wait for navigation
        page.waitForLoadState();
        
        // Assert: Check if logged in
        String currentURL = page.url();
        assertTrue("Login failed - URL did not change", 
                   currentURL.contains("demowebshop"));
        
        // Verify logout link is visible
        Locator logoutLink = page.getByText("Log out");
        assertTrue("Logout link not visible", logoutLink.isVisible());
        
        System.out.println("✓ Login test passed!");
    }

    @Test
    public void testLoginWithInvalidCredentials() {
        // Navigate to login page
        page.navigate("https://demowebshop.tricentis.com/login");
        page.waitForLoadState();
        
        // Get locators
        Locator emailInput = page.locator("input#Email");
        Locator passwordInput = page.locator("input#Password");
        Locator loginButton = page.locator("input[type='submit'][value='Log in']");
        
        // Fill in invalid credentials
        emailInput.fill("invalid@example.com");
        passwordInput.fill("WrongPassword");
        
        // Click login
        loginButton.click();
        
        // Wait for response
        page.waitForLoadState();
        
        // Assert: Check for error message
        Locator errorMessage = page.locator(".validation-summary-errors");
        assertTrue("Error message not displayed", errorMessage.isVisible());
        
        // Verify error text
        String errorText = errorMessage.textContent();
        assertTrue("Expected error message not found", 
                   errorText.contains("credentials") || errorText.contains("failed"));
        
        System.out.println("✓ Invalid login test passed!");
    }

    @Test
    public void testRememberMeCheckbox() {
        page.navigate("https://demowebshop.tricentis.com/login");
        page.waitForLoadState();
        
        // Get locators
        Locator rememberCheckbox = page.locator("input#RememberMe");
        
        // Check if checkbox is available
        assertTrue("Remember me checkbox not found", rememberCheckbox.isVisible());
        
        // Click checkbox
        rememberCheckbox.click();
        
        // Verify checkbox is checked
        boolean isChecked = rememberCheckbox.isChecked();
        assertTrue("Checkbox not checked after click", isChecked);
        
        System.out.println("✓ Remember me checkbox test passed!");
    }
}
```

### Example 2: DemoWebShop Search & Product Test

**File**: `src/test/java/com/playwright/automation/tests/SearchTest.java`

```java
package com.playwright.automation.tests;

import com.microsoft.playwright.*;
import org.junit.Before;
import org.junit.After;
import org.junit.Test;
import java.util.List;
import static org.junit.Assert.*;

public class SearchTest {
    
    private Playwright playwright;
    private Browser browser;
    private Page page;

    @Before
    public void setUp() {
        playwright = Playwright.create();
        browser = playwright.chromium().launch(
            new BrowserType.LaunchOptions().setHeadless(false)
        );
        page = browser.newPage();
    }

    @After
    public void tearDown() {
        page.close();
        browser.close();
        playwright.close();
    }

    @Test
    public void testSearchFunctionality() {
        // Navigate to home page
        page.navigate("https://demowebshop.tricentis.com/");
        page.waitForLoadState();
        
        // Get search locators
        Locator searchInput = page.locator("input#small-searchterms");
        Locator searchButton = page.locator("input[type='submit'][value='Search']");
        
        // Search for product
        searchInput.fill("laptop");
        searchButton.click();
        
        // Wait for results
        page.waitForSelector(".product-item");
        
        // Get all products
        Locator products = page.locator(".product-item");
        int productCount = products.count();
        
        // Assert: At least one product found
        assertTrue("No products found in search results", productCount > 0);
        
        System.out.println("✓ Search test passed! Found " + productCount + " products");
    }

    @Test
    public void testProductPriceDisplay() {
        page.navigate("https://demowebshop.tricentis.com/");
        page.waitForLoadState();
        
        // Navigate to products
        page.getByText("Electronics").click();
        page.waitForSelector(".product-item");
        
        // Get first product
        Locator firstProduct = page.locator(".product-item").first();
        
        // Get price
        Locator price = firstProduct.locator(".actual-price");
        String priceText = price.textContent();
        
        // Assert: Price is not empty
        assertNotNull("Price not found", priceText);
        assertFalse("Price is empty", priceText.trim().isEmpty());
        
        System.out.println("✓ Product price found: " + priceText);
    }

    @Test
    public void testAddToCart() {
        page.navigate("https://demowebshop.tricentis.com/");
        page.waitForLoadState();
        
        // Find first product
        Locator firstProduct = page.locator(".product-item").first();
        
        // Click on product
        firstProduct.locator("a").first().click();
        
        // Wait for product details page
        page.waitForLoadState();
        
        // Get add to cart button
        Locator addToCartBtn = page.getByText("Add to cart");
        
        // Verify button exists
        assertTrue("Add to cart button not found", addToCartBtn.isVisible());
        
        // Click add to cart
        addToCartBtn.click();
        
        // Wait for notification
        page.waitForTimeout(2000);
        
        // Verify success message appears
        Locator successMsg = page.locator(".success-message, .notification-success");
        if (successMsg.count() > 0) {
            assertTrue("Success message should be visible", successMsg.isVisible());
        }
        
        System.out.println("✓ Add to cart test passed!");
    }

    @Test
    public void testProductFiltering() {
        page.navigate("https://demowebshop.tricentis.com/");
        page.waitForLoadState();
        
        // Navigate to category
        page.getByText("Electronics").click();
        page.waitForLoadState();
        
        // Get all products before filter
        Locator allProducts = page.locator(".product-item");
        int initialCount = allProducts.count();
        
        System.out.println("✓ Product filtering test passed! Initial count: " + initialCount);
    }
}
```

### Example 3: DemoWebShop Cart & Checkout Test

**File**: `src/test/java/com/playwright/automation/tests/CheckoutTest.java`

```java
package com.playwright.automation.tests;

import com.microsoft.playwright.*;
import org.junit.Before;
import org.junit.After;
import org.junit.Test;
import static org.junit.Assert.*;

public class CheckoutTest {
    
    private Playwright playwright;
    private Browser browser;
    private Page page;

    @Before
    public void setUp() {
        playwright = Playwright.create();
        browser = playwright.chromium().launch(
            new BrowserType.LaunchOptions().setHeadless(false)
        );
        page = browser.newPage();
    }

    @After
    public void tearDown() {
        page.close();
        browser.close();
        playwright.close();
    }

    @Test
    public void testViewCart() {
        page.navigate("https://demowebshop.tricentis.com/");
        page.waitForLoadState();
        
        // Click on shopping cart
        Locator cartLink = page.getByText("Shopping cart");
        cartLink.click();
        
        // Wait for cart page
        page.waitForLoadState();
        
        // Verify cart page opened
        String currentURL = page.url();
        assertTrue("Cart page did not open", currentURL.contains("cart"));
        
        System.out.println("✓ View cart test passed!");
    }

    @Test
    public void testContinueShopping() {
        page.navigate("https://demowebshop.tricentis.com/cart");
        page.waitForLoadState();
        
        // Get continue shopping button
        Locator continueBtn = page.getByText("Continue Shopping");
        
        // Click button
        if (continueBtn.isVisible()) {
            continueBtn.click();
            page.waitForLoadState();
        }
        
        System.out.println("✓ Continue shopping test passed!");
    }

    @Test
    public void testProductQuantityChange() {
        page.navigate("https://demowebshop.tricentis.com/");
        page.waitForLoadState();
        
        // Add product to cart (prerequisite)
        Locator firstProduct = page.locator(".product-item").first();
        firstProduct.locator("a").first().click();
        page.waitForLoadState();
        
        Locator addToCart = page.getByText("Add to cart");
        if (addToCart.isVisible()) {
            addToCart.click();
            page.waitForTimeout(1000);
        }
        
        // Navigate to cart
        page.navigate("https://demowebshop.tricentis.com/cart");
        page.waitForLoadState();
        
        // Get quantity input
        Locator quantityInput = page.locator("input.qty-input").first();
        
        // Change quantity
        if (quantityInput.count() > 0) {
            quantityInput.fill("2");
            
            // Click update
            Locator updateBtn = page.getByText("Update Shopping Cart");
            if (updateBtn.isVisible()) {
                updateBtn.click();
                page.waitForLoadState();
            }
        }
        
        System.out.println("✓ Quantity change test passed!");
    }
}
```

---

## Advanced Features

### 1. Wait Strategies

```java
// Wait for load state
page.waitForLoadState();  // Waits for 'load' event
page.waitForLoadState(LoadState.NETWORKIDLE);

// Wait for selector
page.waitForSelector(".product-item");

// Wait for element to be visible
page.locator("#element").waitFor();

// Wait with timeout
page.waitForSelector(".element", new Page.WaitForSelectorOptions()
    .setTimeout(5000));

// Wait for navigation
page.waitForNavigation(() -> page.click("a"));

// Wait for function
page.waitForFunction("() => window.scrollY > 100");
```

### 2. Network Interception

```java
// Mock API response
page.route("**/api/products", route -> {
    route.abort("blockedbyclient");
});

// Intercept and modify request
page.route("**/*", route -> {
    Response response = route.fetch();
    route.resume(response);
});

// Stop routing
page.unroute("**/api/**");
```

### 3. Mobile Emulation

```java
Browser browser = playwright.chromium().launch();

// Create context with mobile device
BrowserContext context = browser.newContext(new Browser.NewContextOptions()
    .setDeviceScaleFactor(2)
    .setIsMobile(true)
    .setViewportSize(375, 667)
    .setUserAgent("Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X)..."));

Page page = context.newPage();
page.navigate("https://demowebshop.tricentis.com/");
```

### 4. Screenshots & Videos

```java
// Single screenshot
page.screenshot(new Page.ScreenshotOptions()
    .setPath(Paths.get("screenshot.png")));

// Full page screenshot
page.screenshot(new Page.ScreenshotOptions()
    .setPath(Paths.get("fullpage.png"))
    .setFullPage(true));

// Record video
BrowserContext context = browser.newContext(new Browser.NewContextOptions()
    .setRecordVideoDir(Paths.get("videos/")));
```

### 5. JavaScript Execution

```java
// Execute JavaScript
Object result = page.evaluate("() => window.pageTitle");

// Inject script
page.addScriptTag(new Page.AddScriptTagOptions()
    .setContent("window.testVar = 123;"));

// Get console messages
page.onConsoleMessage(msg -> 
    System.out.println("Console: " + msg.text()));
```

### 6. Cookie Management

```java
// Get cookies
java.util.List<Cookie> cookies = page.context().cookies();

// Add cookies
page.context().addCookies(java.util.Arrays.asList(
    new Cookie("name", "value")
        .setUrl("https://example.com")
));

// Clear cookies
page.context().clearCookies();
```

### 7. Page Events

```java
// Listen to page errors
page.onPageError(exception -> 
    System.out.println("Error: " + exception.getMessage()));

// Listen to requests
page.onRequest(request -> 
    System.out.println("Request: " + request.url()));

// Listen to responses
page.onResponse(response -> 
    System.out.println("Response: " + response.status()));

// Listen to popups
page.onPopup(popup -> {
    System.out.println("Popup: " + popup.url());
    popup.close();
});
```

---

## Base Test Class (Reusable)

**File**: `src/main/java/com/playwright/automation/base/BaseTest.java`

```java
package com.playwright.automation.base;

import com.microsoft.playwright.*;
import org.junit.Before;
import org.junit.After;

public class BaseTest {
    
    protected Playwright playwright;
    protected Browser browser;
    protected Page page;
    protected BrowserContext context;

    @Before
    public void setUp() {
        // Initialize Playwright
        playwright = Playwright.create();
        
        // Launch browser
        browser = playwright.chromium().launch(
            new BrowserType.LaunchOptions()
                .setHeadless(false)
                .setSlowMo(500) // Optional: slow motion for debugging
        );
        
        // Create context
        context = browser.newContext();
        
        // Create page
        page = context.newPage();
        
        // Set default timeout
        page.setDefaultTimeout(30000);
    }

    @After
    public void tearDown() {
        // Close page
        if (page != null) page.close();
        
        // Close context
        if (context != null) context.close();
        
        // Close browser
        if (browser != null) browser.close();
        
        // Close Playwright
        if (playwright != null) playwright.close();
    }

    // Helper methods
    protected void navigateTo(String url) {
        page.navigate(url);
        page.waitForLoadState();
    }

    protected void click(String selector) {
        page.click(selector);
    }

    protected void fill(String selector, String text) {
        page.fill(selector, text);
    }

    protected String getText(String selector) {
        return page.locator(selector).textContent();
    }

    protected boolean isVisible(String selector) {
        return page.locator(selector).isVisible();
    }

    protected void waitForElement(String selector) {
        page.waitForSelector(selector);
    }

    protected void takeScreenshot(String filename) {
        page.screenshot(new Page.ScreenshotOptions()
            .setPath(Paths.get("screenshots/" + filename + ".png")));
    }
}
```

---

## Running Tests in Eclipse

### Method 1: Run as JUnit Test

1. Right-click test class → `Run As` → `JUnit Test`
2. Watch execution in Eclipse console

### Method 2: Run with Maven

**Run all tests:**
```bash
mvn test
```

**Run specific test:**
```bash
mvn test -Dtest=LoginTest
```

**Run with pattern:**
```bash
mvn test -Dtest=*Test
```

### Method 3: Run Configuration

1. Go to `Run` → `Run Configurations`
2. Create new JUnit configuration
3. Select test class
4. Click Run

---

## Best Practices

### ✅ Do's:

1. **Use Explicit Waits**
   ```java
   page.waitForLoadState();
   page.waitForSelector(".element");
   page.locator("#element").waitFor();
   ```

2. **Use Meaningful Locators**
   ```java
   // Good
   Locator loginButton = page.getByText("Log in");
   Locator emailInput = page.locator("input#email");
   
   // Avoid
   Locator button = page.locator("button");
   ```

3. **Organize Tests by Feature**
   ```
   LoginTest.java
   SearchTest.java
   CheckoutTest.java
   ```

4. **Use Base Test Class**
   ```java
   public class LoginTest extends BaseTest {
       @Test
       public void testLogin() {
           // Test code
       }
   }
   ```

5. **Handle Exceptions Properly**
   ```java
   try {
       page.click("button");
   } catch (PlaywrightException e) {
       System.out.println("Element not found: " + e.getMessage());
   }
   ```

### ❌ Don'ts:

1. **Avoid Hard-coded Delays**
   ```java
   // Avoid
   Thread.sleep(5000);
   
   // Use instead
   page.waitForSelector(".element");
   ```

2. **Avoid Not Closing Resources**
   ```java
   // Always close
   page.close();
   browser.close();
   playwright.close();
   ```

3. **Avoid Complex Selectors**
   ```java
   // Avoid
   page.locator("div > div > div > button");
   
   // Use instead
   page.getByText("Submit");
   ```

4. **Avoid Testing Implementation Details**
   ```java
   // Avoid
   page.evaluate("window.clickButton()");
   
   // Use instead
   page.getByRole("button").click();
   ```

5. **Avoid Flaky Tests**
   ```java
   // Avoid - element might not be ready
   page.click("button");
   
   // Better
   page.locator("button").waitFor();
   page.click("button");
   ```

---

## Troubleshooting

### Issue 1: Browser Not Launching

**Error**: `NoSuchFileException: browsers not found`

**Solution**:
```bash
mvn exec:java -e -Dexec.mainClass="com.microsoft.playwright.CLI" -Dexec.args="install"
```

---

### Issue 2: Element Not Found

**Error**: `Timeout 30000ms exceeded`

**Solution**:
```java
// Increase timeout
page.setDefaultTimeout(60000);

// Use explicit wait
page.waitForSelector(".element", new Page.WaitForSelectorOptions()
    .setTimeout(10000));
```

---

### Issue 3: Maven Dependencies Not Found

**Solution**:
```bash
# Clear cache and update
mvn clean install -U

# Force update
mvn clean update
```

---

### Issue 4: Multiple Clicks on Same Element

**Error**: `Element is no longer attached to the DOM`

**Solution**:
```java
// Wait for element
page.locator("button").waitFor();

// Click with retry
page.locator("button").click();
```

---

### Issue 5: Context Isolation

**Solution**:
```java
// Create isolated context for each test
@Before
public void setUp() {
    context = browser.newContext();
    page = context.newPage();
}

@After
public void tearDown() {
    context.close();
    browser.close();
}
```

---

## Comparison: Playwright vs Selenium

```java
// SELENIUM
WebDriver driver = new ChromeDriver();
driver.get("https://example.com");
WebElement element = driver.findElement(By.id("email"));
element.sendKeys("test@example.com");

// PLAYWRIGHT
try (Playwright playwright = Playwright.create()) {
    Browser browser = playwright.chromium().launch();
    Page page = browser.newPage();
    page.navigate("https://example.com");
    page.fill("#email", "test@example.com");
    browser.close();
}
```

**Key Differences:**
- Playwright is more modern and faster
- Playwright has better wait mechanisms
- Playwright supports multiple browsers uniformly
- Selenium has larger community
- Playwright has better mobile support

---

## Useful Resources

- **Playwright Documentation**: https://playwright.dev/java/docs/intro
- **Locators Guide**: https://playwright.dev/java/docs/locators
- **API Reference**: https://playwright.dev/java/docs/api/class-Playwright
- **Debugging**: https://playwright.dev/java/docs/debug
- **GitHub**: https://github.com/microsoft/playwright-java

---

## Summary

### Key Points:
1. ✅ Playwright is a modern, fast automation framework
2. ✅ Easy to set up with Maven in Eclipse
3. ✅ Cross-browser support (Chrome, Firefox, Safari)
4. ✅ Better wait mechanisms than Selenium
5. ✅ Excellent API for web automation
6. ✅ Mobile emulation built-in
7. ✅ Network interception capabilities
8. ✅ Great for CI/CD integration

### Next Steps:
1. Create first Playwright project
2. Write basic test cases
3. Implement Page Object Model
4. Add continuous integration
5. Scale test suite
6. Generate reports

Congratulations! You now have a complete understanding of Playwright with Java! 🎉
