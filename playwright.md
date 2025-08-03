# 📚 Complete Playwright POM Testing Guide - Student Notes

## 🎯 How to Use This Guide

**Dear Students,**

This is your complete reference for mastering Playwright with Page Object Model. Each section builds on the previous one, so follow in order. Every code example is explained in detail - copy, paste, and modify them for your own projects!

---

## 1. Introduction to Playwright Locators and Actions

### 🔍 **What Are Locators?**

**Locators** are like GPS coordinates for web elements. They tell Playwright exactly which button, input field, or text to interact with on a webpage.

#### **1.1 Built-in Locators (Your Best Friends!)**

These are Playwright's smart locators that make your tests more reliable:

```javascript
// 🎯 BY ROLE - Most reliable because it's based on accessibility
await page.getByRole('button', { name: 'Submit' });
// ↑ Finds ANY button with text "Submit" - even if CSS changes!

await page.getByRole('textbox', { name: 'Email' });
// ↑ Finds email input field - works with <input>, <textarea>, etc.

await page.getByRole('link', { name: 'Home' });
// ↑ Finds links - super reliable for navigation
```

**💡 Why use getByRole?**
- Works even if developers change CSS classes
- Follows accessibility standards
- More reliable than CSS selectors

```javascript
// 🏷️ BY LABEL - Perfect for form fields
await page.getByLabel('Username');
// ↑ Finds input field associated with "Username" label

await page.getByLabel('Password');
// ↑ Works with <label for="password"> or aria-label
```

```javascript
// 📝 BY PLACEHOLDER - When there's placeholder text
await page.getByPlaceholder('Enter your email');
// ↑ Finds <input placeholder="Enter your email">
```

```javascript
// 📄 BY TEXT - Finds elements containing specific text
await page.getByText('Welcome to our site');
// ↑ Finds any element with this text

await page.getByText('Login', { exact: true });
// ↑ exact: true means ONLY "Login", not "Login Now"
```

```javascript
// 🏷️ BY TEST ID - For elements marked specifically for testing
await page.getByTestId('login-button');
// ↑ Finds <button data-testid="login-button">
// Developers add these specifically for tests!
```

#### **1.2 CSS and XPath Locators (Use Sparingly)**

```javascript
// 🎨 CSS Selectors (like styling CSS)
await page.locator('.login-form');        // Class selector
await page.locator('#submit-btn');        // ID selector
await page.locator('[data-testid="user-menu"]'); // Attribute selector
await page.locator('input[type="email"]'); // Element + attribute
```

```javascript
// 🔍 XPath (Very powerful but complex)
await page.locator('xpath=//button[contains(text(), "Submit")]');
// ↑ Finds button containing text "Submit"

await page.locator('//div[@class="error-message"]');
// ↑ Finds div with specific class
```

**⚠️ Student Tip:** Use built-in locators first, CSS/XPath as backup!

#### **1.3 Filtering and Chaining (Super Powerful!)**

```javascript
// 🔍 Filter by text content
await page.getByRole('listitem').filter({ hasText: 'Product 1' });
// ↑ From all list items, find the one with "Product 1"

// 🔍 Filter by other locators
await page.getByRole('listitem').filter({
  has: page.getByRole('button', { name: 'Delete' })
});
// ↑ Find list item that CONTAINS a Delete button

// ⛓️ Chain locators (look inside elements)
await page.locator('.product-card').getByRole('button', { name: 'Add to Cart' });
// ↑ Find "Add to Cart" button INSIDE product card

// 🔢 Handle multiple matches
await page.getByRole('button', { name: 'Delete' }).nth(0);  // First one
await page.getByRole('button', { name: 'Delete' }).last(); // Last one
await page.getByRole('button', { name: 'Delete' }).first(); // Same as nth(0)
```

### 🖱️ **1.2 Common Actions - Making Things Happen**

#### **Click Actions**

```javascript
// 👆 Basic click (most common)
await page.getByRole('button', { name: 'Submit' }).click();

// 👆 Advanced click options
await page.getByRole('button', { name: 'Submit' }).click({
  button: 'right',    // Right-click for context menu
  clickCount: 2,      // Double-click
  force: true         // Click even if element is hidden/covered
});

// 🎯 Click at specific position (rare but useful)
await page.getByRole('button', { name: 'Submit' }).click({
  position: { x: 10, y: 10 }  // 10px from top-left of element
});
```

#### **Input Actions**

```javascript
// ✏️ Fill input fields (clears first, then types)
await page.getByLabel('Username').fill('john.doe');
await page.getByLabel('Password').fill('password123');

// 🧹 Clear field manually
await page.getByLabel('Email').clear();
await page.getByLabel('Email').fill('new@email.com');

// ⌨️ Type with delay (looks more human-like)
await page.getByLabel('Search').type('playwright', { delay: 100 });
// ↑ 100ms delay between each keystroke

// ⌨️ Press special keys
await page.getByLabel('Search').press('Enter');
await page.keyboard.press('Escape');  // Global keyboard press
await page.keyboard.press('Control+A'); // Keyboard shortcuts
```

#### **Selection Actions**

```javascript
// 📋 Dropdown selections (multiple ways)
await page.getByLabel('Country').selectOption('US');              // By value
await page.getByLabel('Country').selectOption({ label: 'United States' }); // By visible text
await page.getByLabel('Country').selectOption({ index: 0 });     // By position

// ☑️ Checkboxes
await page.getByRole('checkbox', { name: 'Subscribe' }).check();   // Check it
await page.getByRole('checkbox', { name: 'Subscribe' }).uncheck(); // Uncheck it

// 🔘 Radio buttons
await page.getByRole('radio', { name: 'Male' }).check();
```

#### **File Upload**

```javascript
// 📁 Upload single file
await page.getByLabel('Upload file').setInputFiles('path/to/file.pdf');

// 📁 Upload multiple files
await page.getByLabel('Upload files').setInputFiles([
  'path/to/file1.pdf',
  'path/to/file2.pdf'
]);

// 🗑️ Clear uploaded files
await page.getByLabel('Upload file').setInputFiles([]);
```

### ⏰ **1.3 Waiting and Assertions - Making Tests Reliable**

#### **Wait for Elements**

```javascript
// ⌛ Wait for element to appear
await page.getByRole('button', { name: 'Submit' }).waitFor();

// ⌛ Wait for specific states
await page.getByRole('dialog').waitFor({ state: 'visible' });  // Wait until visible
await page.getByRole('dialog').waitFor({ state: 'hidden' });   // Wait until hidden
await page.getByRole('dialog').waitFor({ state: 'attached' }); // Wait until in DOM

// ⏰ Custom timeout (default is 30 seconds)
await page.getByText('Loading...').waitFor({ timeout: 10000 }); // 10 seconds
```

#### **Assertions (Checking if things are correct)**

```javascript
import { expect } from '@playwright/test';

// 👁️ Visibility checks
await expect(page.getByText('Welcome')).toBeVisible();
await expect(page.getByText('Error')).toBeHidden();

// 📝 Text checks
await expect(page.getByRole('heading')).toHaveText('Dashboard');
await expect(page.getByRole('heading')).toContainText('Dash'); // Partial match
await expect(page.getByRole('textbox')).toHaveValue('john.doe');

// 🏷️ Attribute checks
await expect(page.getByRole('button')).toBeEnabled();
await expect(page.getByRole('button')).toBeDisabled();
await expect(page.getByRole('link')).toHaveAttribute('href', '/home');

// 🔢 Count checks
await expect(page.getByRole('listitem')).toHaveCount(5);        // Exactly 5
await expect(page.getByRole('listitem')).toHaveCount({ min: 1 }); // At least 1
```

---

## 2. Page Object Model (POM) Fundamentals

### 🏗️ **What is Page Object Model?**

**Imagine your test code like a house:**
- **Without POM:** Everything in one messy room
- **With POM:** Each page is a separate, organized room

**Benefits:**
- **Maintainability:** Change UI once, update one file
- **Reusability:** Use same page object in multiple tests
- **Readability:** Tests read like English sentences
- **Organization:** Code is clean and professional

### 🏠 **2.2 Understanding BasePage (The Foundation)**

BasePage is like the foundation of your house - it has common features every page needs.

```javascript
// pages/BasePage.js
export class BasePage {
  constructor(page) {
    this.page = page;  // Store Playwright page object
  }

  // 🌐 Navigation methods (every page needs these)
  async goto(url) {
    await this.page.goto(url);
  }

  async getTitle() {
    return await this.page.title();
  }

  async getCurrentUrl() {
    return this.page.url();
  }

  // ⏳ Waiting methods (very important for stability)
  async waitForPageLoad() {
    await this.page.waitForLoadState('networkidle');
    // ↑ Wait until no network requests for 500ms
  }

  async waitForElement(locator, timeout = 5000) {
    await locator.waitFor({ timeout });
  }

  // 🖱️ Common interaction methods (DRY principle)
  async clickElement(locator) {
    await this.waitForElement(locator);  // Always wait first!
    await locator.click();
  }

  async fillInput(locator, text) {
    await this.waitForElement(locator);
    await locator.clear();  // Clear existing text
    await locator.fill(text);
  }

  async selectDropdown(locator, value) {
    await this.waitForElement(locator);
    await locator.selectOption(value);
  }

  // ✅ Assertion helpers (reusable checks)
  async verifyElementVisible(locator) {
    await expect(locator).toBeVisible();
  }

  async verifyElementText(locator, expectedText) {
    await expect(locator).toHaveText(expectedText);
  }

  async verifyPageTitle(expectedTitle) {
    await expect(this.page).toHaveTitle(expectedTitle);
  }

  // 📸 Debugging helpers
  async takeScreenshot(name) {
    await this.page.screenshot({ path: `screenshots/${name}.png` });
  }

  async scrollToElement(locator) {
    await locator.scrollIntoViewIfNeeded();
  }

  // 🍪 Browser state helpers
  async clearCookies() {
    await this.page.context().clearCookies();
  }

  async setLocalStorage(key, value) {
    await this.page.evaluate(({ key, value }) => {
      localStorage.setItem(key, value);
    }, { key, value });
  }
}
```

**💡 Student Note:** BasePage = Your testing Swiss Army knife!

### 📄 **2.3 Creating Specific Page Objects**

#### **LoginPage Example (Inherits from BasePage)**

```javascript
// pages/LoginPage.js
import { BasePage } from './BasePage.js';
import { expect } from '@playwright/test';

export class LoginPage extends BasePage {
  constructor(page) {
    super(page);  // Call parent constructor

    // 🎯 Page-specific locators (define once, use everywhere)
    this.usernameInput = page.getByLabel('Username');
    this.passwordInput = page.getByLabel('Password');
    this.loginButton = page.getByRole('button', { name: 'Login' });
    this.errorMessage = page.getByRole('alert');
    this.forgotPasswordLink = page.getByRole('link', { name: 'Forgot Password?' });
    this.rememberMeCheckbox = page.getByRole('checkbox', { name: 'Remember me' });

    // 📍 Page URL
    this.url = '/login';
  }

  // 🌐 Navigation method
  async navigateToLogin() {
    await this.goto(this.url);
    await this.verifyPageTitle('Login - MyApp');
  }

  // 🔐 Main login method
  async login(username, password, rememberMe = false) {
    await this.fillInput(this.usernameInput, username);
    await this.fillInput(this.passwordInput, password);

    if (rememberMe) {
      await this.rememberMeCheckbox.check();
    }

    await this.clickElement(this.loginButton);
  }

  // 🎯 Convenience method for valid login
  async loginWithValidCredentials(username = 'testuser', password = 'password123') {
    await this.login(username, password);
    await this.waitForPageLoad();
  }

  // ❌ Error verification
  async verifyLoginError(expectedError) {
    await this.verifyElementVisible(this.errorMessage);
    await this.verifyElementText(this.errorMessage, expectedError);
  }

  // 🔗 Additional actions
  async clickForgotPassword() {
    await this.clickElement(this.forgotPasswordLink);
  }

  // ✅ State checks
  async isLoginButtonEnabled() {
    return await this.loginButton.isEnabled();
  }

  // 🧹 Utility methods
  async clearLoginForm() {
    await this.usernameInput.clear();
    await this.passwordInput.clear();
  }
}
```

**💡 Student Notes:**
- **Constructor:** Sets up all locators once
- **Methods:** Each method does one clear thing
- **Naming:** Methods read like English sentences

#### **DashboardPage Example**

```javascript
// pages/DashboardPage.js
import { BasePage } from './BasePage.js';
import { expect } from '@playwright/test';

export class DashboardPage extends BasePage {
  constructor(page) {
    super(page);

    // 🏠 Header elements
    this.welcomeMessage = page.getByTestId('welcome-message');
    this.userMenu = page.getByTestId('user-menu');
    this.logoutButton = page.getByRole('button', { name: 'Logout' });

    // 🧭 Navigation elements
    this.profileLink = page.getByRole('link', { name: 'Profile' });
    this.settingsLink = page.getByRole('link', { name: 'Settings' });
    this.reportsLink = page.getByRole('link', { name: 'Reports' });

    // 📊 Content area
    this.statsCards = page.locator('.stats-card');
    this.recentActivityList = page.getByTestId('recent-activity');

    this.url = '/dashboard';
  }

  // ✅ Verification methods
  async verifyDashboardLoaded() {
    await this.verifyPageTitle('Dashboard - MyApp');
    await this.verifyElementVisible(this.welcomeMessage);
  }

  // 📝 Data extraction methods
  async getWelcomeMessage() {
    return await this.welcomeMessage.textContent();
  }

  // 🚪 User actions
  async logout() {
    await this.clickElement(this.userMenu);
    await this.clickElement(this.logoutButton);
    await this.waitForPageLoad();
  }

  async navigateToProfile() {
    await this.clickElement(this.profileLink);
    await this.waitForPageLoad();
  }

  // 📊 Data methods
  async getStatsCardCount() {
    return await this.statsCards.count();
  }

  async getStatsCardValue(cardIndex) {
    const card = this.statsCards.nth(cardIndex);
    const valueElement = card.locator('.stats-value');
    return await valueElement.textContent();
  }

  async verifyRecentActivity() {
    await this.verifyElementVisible(this.recentActivityList);
    const activityItems = this.recentActivityList.locator('li');
    await expect(activityItems).toHaveCount({ min: 1 });
  }
}
```

### 🧪 **2.4 Writing Tests with POM**

```javascript
// tests/auth.spec.js
import { test, expect } from '@playwright/test';
import { LoginPage } from '../pages/LoginPage.js';
import { DashboardPage } from '../pages/DashboardPage.js';

test.describe('Authentication Tests', () => {
  let loginPage;
  let dashboardPage;

  // 🏁 Setup before each test
  test.beforeEach(async ({ page }) => {
    loginPage = new LoginPage(page);        // Create page objects
    dashboardPage = new DashboardPage(page);
    await loginPage.navigateToLogin();      // Navigate to starting point
  });

  test('should login with valid credentials', async () => {
    // 🎯 Test steps read like English!
    await loginPage.loginWithValidCredentials();

    await dashboardPage.verifyDashboardLoaded();
    const welcomeText = await dashboardPage.getWelcomeMessage();
    expect(welcomeText).toContain('Welcome, testuser');
  });

  test('should show error for invalid credentials', async () => {
    await loginPage.login('invalid', 'invalid');

    await loginPage.verifyLoginError('Invalid username or password');
  });

  test('should redirect to forgot password page', async () => {
    await loginPage.clickForgotPassword();

    await expect(loginPage.page).toHaveURL(/.*forgot-password/);
  });

  test('should remember login preference', async () => {
    await loginPage.login('testuser', 'password123', true);  // rememberMe = true

    await dashboardPage.verifyDashboardLoaded();
    // Additional assertions for remember me functionality
  });
});
```

**🎯 Student Benefits of This Approach:**
- **Readable:** Tests read like user stories
- **Maintainable:** Change locator once, fix all tests
- **Reusable:** Same methods across multiple tests
- **Professional:** Industry-standard pattern

---

## 3. Test Labels and Categories

### 🏷️ **Why Use Test Labels?**

Labels are like tags on your clothes - they help you organize and find what you need quickly!

**Benefits:**
- **Run specific tests:** Only smoke tests on every commit
- **Organize by priority:** Run critical tests first
- **Filter by type:** API tests vs UI tests
- **CI/CD integration:** Different stages run different tests

### 🎯 **3.1 Common Test Labels**

```javascript
// 💨 Smoke Tests - Quick checks that basic functionality works
test.describe('Smoke Tests @smoke', () => {
  test('should load login page @smoke @critical', async ({ page }) => {
    const loginPage = new LoginPage(page);
    await loginPage.navigateToLogin();
    // Quick check that page loads
  });
});

// 🔄 Regression Tests - Comprehensive feature testing
test.describe('User Management @regression', () => {
  test('should create user @regression @medium', async ({ page }) => {
    // Detailed user creation test
  });
});

// 🔗 Integration Tests - Cross-system functionality
test.describe('Payment Integration @integration', () => {
  test('should process payment @integration @high', async ({ page }) => {
    // Test payment system integration
  });
});
```

**💡 Label Strategy:**
- `@smoke` - Fast, basic functionality
- `@regression` - Thorough feature testing
- `@integration` - Cross-system tests
- `@e2e` - Complete user journeys

### 📊 **3.2 Priority Levels**

```javascript
// 🔴 High Priority - Business critical
test.describe('High Priority Tests', () => {
  test('user login @high @critical @smoke', async ({ page }) => {
    // Can't use app without login!
  });

  test('checkout process @high @critical @e2e', async ({ page }) => {
    // Revenue depends on this!
  });
});

// 🟡 Medium Priority - Important but not critical
test.describe('Medium Priority Tests', () => {
  test('user profile update @medium @regression', async ({ page }) => {
    // Nice to have, but app works without it
  });
});

// 🟢 Low Priority - Nice to have
test.describe('Low Priority Tests', () => {
  test('ui color theme change @low @ui', async ({ page }) => {
    // Cosmetic features
  });
});
```

### 🎯 **3.3 Test Categories and Tags**

#### **Functional Categories**

```javascript
// 🔐 Authentication tests
test.describe('Authentication @auth', () => {
  test('login @auth @smoke', async ({ page }) => {
    const loginPage = new LoginPage(page);
    await loginPage.navigateToLogin();
    await loginPage.loginWithValidCredentials();
  });

  test('logout @auth @smoke', async ({ page }) => {
    // Login first, then test logout
  });

  test('password reset @auth @regression', async ({ page }) => {
    // Test forgot password flow
  });
});

// 🛣️ E2E user journeys
test.describe('User Journey @e2e', () => {
  test('complete purchase flow @e2e @critical', async ({ page }) => {
    // Login → Browse → Add to Cart → Checkout → Payment
  });

  test('user onboarding @e2e @medium', async ({ page }) => {
    // Registration → Email Verification → Profile Setup
  });
});

// 🔌 API integration tests
test.describe('API Integration @api', () => {
  test('user data sync @api @regression', async ({ page }) => {
    // Test that UI updates when API data changes
  });
});

// ⚡ Performance tests
test.describe('Performance @performance', () => {
  test('page load time @performance @slow', async ({ page }) => {
    const startTime = Date.now();
    await page.goto('/dashboard');
    const loadTime = Date.now() - startTime;
    expect(loadTime).toBeLessThan(3000); // Less than 3 seconds
  });
});
```

#### **Browser-specific Tags**

```javascript
test.describe('Cross-browser Tests', () => {
  test('feature works in Chrome @chrome', async ({ page }) => {
    // Chrome-specific test
  });

  test('feature works in Firefox @firefox', async ({ page }) => {
    // Firefox-specific test
  });

  test('feature works in Safari @safari', async ({ page }) => {
    // Safari-specific test
  });

  test('mobile responsive @mobile', async ({ page }) => {
    // Mobile device testing
  });
});
```

### ⚙️ **3.4 Configuration for Test Execution**

#### **playwright.config.js Setup**

```javascript
// playwright.config.js
import { defineConfig } from '@playwright/test';

export default defineConfig({
  // 🎯 Define different test projects
  projects: [
    {
      name: 'smoke-tests',
      grep: /@smoke/,           // Only run tests with @smoke tag
      use: {
        browserName: 'chromium',
        headless: true          // Fast execution
      },
    },
    {
      name: 'regression-tests',
      grep: /@regression/,
      use: {
        browserName: 'chromium',
        headless: false         // See what's happening
      },
    },
    {
      name: 'critical-tests',
      grep: /@critical/,
      use: {
        browserName: 'chromium',
        headless: true
      },
    }
  ],

  // 📊 Test reporting
  reporter: [
    ['html'],                                    // Beautiful HTML report
    ['junit', { outputFile: 'test-results/junit.xml' }] // CI/CD compatible
  ],
});
```

#### **Running Specific Test Categories**

```bash
# 💨 Run smoke tests only (fast feedback)
npx playwright test --grep "@smoke"

# 🔴 Run critical tests (most important)
npx playwright test --grep "@critical"

# 🔄 Run regression tests but skip slow ones
npx playwright test --grep "@regression" --grep-invert "@slow"

# 📊 Run tests by priority
npx playwright test --grep "@high"

# 🎯 Run specific project configuration
npx playwright test --project smoke-tests

# 🌐 Run auth tests in all browsers
npx playwright test --grep "@auth"
```

**💡 Student Workflow:**
1. **Development:** Run `@smoke` tests frequently
2. **Feature complete:** Run `@regression` tests
3. **Before release:** Run all tests
4. **CI/CD:** Different stages run different labels

---

## 4. Advanced POM Patterns

### 🧩 **4.1 Component-Based Page Objects**

**Problem:** Large pages with repeated elements (navigation, modals, etc.)
**Solution:** Break into reusable components!

```javascript
// components/NavigationComponent.js
export class NavigationComponent {
  constructor(page) {
    this.page = page;

    // 🧭 Navigation elements
    this.navbar = page.locator('.navbar');
    this.homeLink = this.navbar.getByRole('link', { name: 'Home' });
    this.productsLink = this.navbar.getByRole('link', { name: 'Products' });
    this.profileDropdown = this.navbar.getByTestId('profile-dropdown');
    this.cartIcon = this.navbar.getByTestId('cart-icon');
    this.searchBox = this.navbar.getByPlaceholder('Search products...');
  }

  // 🏠 Navigation methods
  async navigateToHome() {
    await this.homeLink.click();
    await this.page.waitForLoadState('networkidle');
  }

  async navigateToProducts() {
    await this.productsLink.click();
    await this.page.waitForLoadState('networkidle');
  }

  async openProfileMenu() {
    await this.profileDropdown.click();
  }

  async searchFor(searchTerm) {
    await this.searchBox.fill(searchTerm);
    await this.searchBox.press('Enter');
  }

  async getCartItemCount() {
    const cartBadge = this.cartIcon.locator('.badge');
    return await cartBadge.textContent();
  }
}

// pages/HomePage.js - Using the component
import { BasePage } from './BasePage.js';
import { NavigationComponent } from '../components/NavigationComponent.js';

export class HomePage extends BasePage {
  constructor(page) {
    super(page);

    // 🧩 Include navigation component
    this.navigation = new NavigationComponent(page);

    // 🏠 Page-specific elements
    this.heroSection = page.locator('.hero');
    this.featuredProducts = page.locator('.featured-products');
    this.newsletterSignup = page.getByTestId('newsletter-signup');
  }

  async verifyHomePageLoaded() {
    await this.verifyElementVisible(this.heroSection);
    await this.verifyElementVisible(this.featuredProducts);
  }

  async signUpForNewsletter(email) {
    const emailInput = this.newsletterSignup.getByLabel('Email');
    const submitButton = this.newsletterSignup.getByRole('button', { name: 'Subscribe' });

    await emailInput.fill(email);
    await submitButton.click();
  }
}
```

**🎯 Benefits:**
- **Reusable:** Navigation works on every page
- **Maintainable:** Change navigation once, fixes everywhere
- **Organized:** Separate concerns clearly

### 📊 **4.2 Data-Driven Testing with POM**

**Problem:** Testing same functionality with different data
**Solution:** Parameterized tests!

```javascript
// tests/data-driven-login.spec.js
import { test } from '@playwright/test';
import { LoginPage } from '../pages/LoginPage.js';

// 📋 Test data array
const loginTestData = [
  {
    username: '',
    password: '',
    expectedError: 'Username is required',
    description: 'empty username'
  },
  {
    username: 'valid@email.com',
    password: '',
    expectedError: 'Password is required',
    description: 'empty password'
  },
  {
    username: 'invalid@email.com',
    password: 'wrongpassword',
    expectedError: 'Invalid credentials',
    description: 'invalid credentials'
  },
  {
    username: 'user@domain-that-does-not-exist.com',
    password: 'password123',
    expectedError: 'User not found',
    description: 'non-existent user'
  }
];

// 🔄 Generate tests from data
loginTestData.forEach(({ username, password, expectedError, description }) => {
  test(`should show error for ${description} @negative @regression`, async ({ page }) => {
    const loginPage = new LoginPage(page);

    await loginPage.navigateToLogin();
    await loginPage.login(username, password);
    await loginPage.verifyLoginError(expectedError);
  });
});

// 📋 External data file example
// test-data/users.json
const validUsers = [
  { username: 'admin@test.com', password: 'admin123', role: 'admin' },
  { username: 'user@test.com', password: 'user123', role: 'user' },
  { username: 'manager@test.com', password: 'manager123', role: 'manager' }
];

// Using external data
import userData from '../test-data/users.json';

userData.forEach(({ username, password, role }) => {
  test(`${role} user can login @smoke @auth`, async ({ page }) => {
    const loginPage = new LoginPage(page);
    const dashboardPage = new DashboardPage(page);

    await loginPage.navigateToLogin();
    await loginPage.login(username, password);
    await dashboardPage.verifyDashboardLoaded();

    // Verify role-specific elements
    if (role === 'admin') {
      await expect(dashboardPage.adminPanel).toBeVisible();
    }
  });
});
```

**💡 Student Benefits:**
- **Efficiency:** One test template, multiple scenarios
- **Coverage:** Test edge cases systematically
- **Maintenance:** Update logic once, applies to all data

### 🏭 **4.3 Page Factory Pattern**

**Problem:** Managing multiple page objects gets messy
**Solution:** Centralized page creation!

```javascript
// utils/PageFactory.js
import { LoginPage } from '../pages/LoginPage.js';
import { DashboardPage } from '../pages/DashboardPage.js';
import { ProfilePage } from '../pages/ProfilePage.js';
import { ProductsPage } from '../pages/ProductsPage.js';
import { CheckoutPage } from '../pages/CheckoutPage.js';

export class PageFactory {
  constructor(page) {
    this.page = page;
    // 💾 Cache page objects to avoid recreating
    this._pageCache = {};
  }

  // 🔐 Authentication pages
  getLoginPage() {
    if (!this._pageCache.loginPage) {
      this._pageCache.loginPage = new LoginPage(this.page);
    }
    return this._pageCache.loginPage;
  }

  getRegistrationPage() {
    if (!this._pageCache.registrationPage) {
      this._pageCache.registrationPage = new RegistrationPage(this.page);
    }
    return this._pageCache.registrationPage;
  }

  // 🏠 Main application pages
  getDashboardPage() {
    if (!this._pageCache.dashboardPage) {
      this._pageCache.dashboardPage = new DashboardPage(this.page);
    }
    return this._pageCache.dashboardPage;
  }

  getProfilePage() {
    if (!this._pageCache.profilePage) {
      this._pageCache.profilePage = new ProfilePage(this.page);
    }
    return this._pageCache.profilePage;
  }

  // 🛒 E-commerce pages
  getProductsPage() {
    if (!this._pageCache.productsPage) {
      this._pageCache.productsPage = new ProductsPage(this.page);
    }
    return this._pageCache.productsPage;
  }

  getCheckoutPage() {
    if (!this._pageCache.checkoutPage) {
      this._pageCache.checkoutPage = new CheckoutPage(this.page);
    }
    return this._pageCache.checkoutPage;
  }

  // 🧹 Utility method to clear cache
  clearCache() {
    this._pageCache = {};
  }
}

// Using Page Factory in tests
test('complete user flow @e2e @critical', async ({ page }) => {
  const pageFactory = new PageFactory(page);

  // 🔐 Step 1: Login
  const loginPage = pageFactory.getLoginPage();
  await loginPage.navigateToLogin();
  await loginPage.loginWithValidCredentials();

  // 🏠 Step 2: Verify dashboard
  const dashboardPage = pageFactory.getDashboardPage();
  await dashboardPage.verifyDashboardLoaded();

  // 🛒 Step 3: Browse products
  await dashboardPage.navigation.navigateToProducts();
  const productsPage = pageFactory.getProductsPage();
  await productsPage.verifyProductsPageLoaded();
  await productsPage.addProductToCart('laptop');

  // 💳 Step 4: Checkout
  await productsPage.navigation.goToCart();
  const checkoutPage = pageFactory.getCheckoutPage();
  await checkoutPage.verifyCartContents(['laptop']);
  await checkoutPage.completeCheckout({
    email: 'test@example.com',
    address: '123 Test St',
    cardNumber: '4111111111111111'
  });
});
```

**🎯 Benefits:**
- **Centralized:** One place to manage all pages
- **Cached:** Don't recreate page objects unnecessarily
- **Clean:** Tests focus on logic, not object creation

---

## 5. Best Practices and Tips

### 📝 **5.1 Naming Conventions (Very Important!)**

**Good naming makes code self-documenting:**

```javascript
// ✅ GOOD naming practices
class LoginPage {
  // 🏷️ Locators - noun phrases (what they are)
  usernameInput = page.getByLabel('Username');
  passwordInput = page.getLabel('Password');
  submitButton = page.getByRole('button', { name: 'Submit' });
  errorMessage = page.getByRole('alert');

  // 🎬 Actions - verb phrases (what they do)
  async enterCredentials(username, password) {
    await this.usernameInput.fill(username);
    await this.passwordInput.fill(password);
  }

  async clickSubmit() {
    await this.submitButton.click();
  }

  async verifyLoginSuccess() {
    await expect(this.page).toHaveURL('/dashboard');
  }

  // ❌ Error handling - clear purpose
  async verifyInvalidCredentialsError() {
    await expect(this.errorMessage).toContainText('Invalid username or password');
  }
}

// ❌ BAD naming examples
class LoginPage {
  // Unclear what these are
  field1 = page.locator('#username');
  button = page.locator('button');

  // Unclear what these do
  async doStuff() { }
  async check() { }
  async method1() { }
}
```

**💡 Student Rules:**
- **Locators:** What IS it? (submitButton, usernameInput)
- **Methods:** What does it DO? (clickSubmit, enterUsername)
- **Verifications:** What are you CHECKING? (verifyLoginSuccess)

### 🛡️ **5.2 Error Handling and Debugging**

**Problem:** Tests fail with cryptic messages
**Solution:** Enhanced error handling with debugging info!

```javascript
// Enhanced BasePage with smart error handling
export class BasePage {
  constructor(page) {
    this.page = page;
  }

  // 🛡️ Safe click with error context
  async safeClick(locator, timeout = 5000) {
    try {
      await locator.waitFor({ timeout });
      await locator.click();
    } catch (error) {
      // 📸 Take screenshot for debugging
      await this.takeScreenshot(`error-click-${Date.now()}`);

      // 📝 Add helpful context to error
      const elementInfo = await this.getElementInfo(locator);
      throw new Error(`Failed to click element: ${elementInfo}. Original error: ${error.message}`);
    }
  }

  // 🛡️ Safe fill with validation
  async safeFill(locator, text, timeout = 5000) {
    try {
      await locator.waitFor({ timeout });
      await locator.clear();
      await locator.fill(text);

      // ✅ Verify the text was actually entered
      const actualValue = await locator.inputValue();
      if (actualValue !== text) {
        throw new Error(`Expected "${text}" but got "${actualValue}"`);
      }
    } catch (error) {
      await this.takeScreenshot(`error-fill-${Date.now()}`);
      throw new Error(`Failed to fill element with text "${text}": ${error.message}`);
    }
  }

  // 🔍 Get element information for debugging
  async getElementInfo(locator) {
    try {
      const isVisible = await locator.isVisible();
      const isEnabled = await locator.isEnabled();
      const text = await locator.textContent();

      return `Element info - Visible: ${isVisible}, Enabled: ${isEnabled}, Text: "${text}"`;
    } catch {
      return 'Element info not available';
    }
  }

  // 📸 Enhanced screenshot with page info
  async takeScreenshot(name) {
    const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
    const url = this.page.url();
    const filename = `${name}-${timestamp}.png`;

    await this.page.screenshot({
      path: `screenshots/${filename}`,
      fullPage: true
    });

    console.log(`📸 Screenshot saved: ${filename} for URL: ${url}`);
    return filename;
  }

  // 🕐 Wait with retry logic
  async waitForCondition(conditionFn, options = {}) {
    const { timeout = 10000, retryInterval = 500, description = 'condition' } = options;
    const startTime = Date.now();

    while (Date.now() - startTime < timeout) {
      try {
        const result = await conditionFn();
        if (result) {
          return result;
        }
      } catch (error) {
        // Continue retrying
      }

      await this.page.waitForTimeout(retryInterval);
    }

    await this.takeScreenshot(`timeout-${description}`);
    throw new Error(`Timeout waiting for ${description} after ${timeout}ms`);
  }
}

// Using enhanced error handling
test('login with error handling @smoke', async ({ page }) => {
  const loginPage = new LoginPage(page);

  try {
    await loginPage.navigateToLogin();
    await loginPage.safeClick(loginPage.loginButton);  // Will provide detailed error if fails
  } catch (error) {
    console.log('❌ Test failed with helpful context:', error.message);
    throw error;  // Re-throw so test still fails
  }
});
```

### 🌍 **5.3 Environment Configuration**

**Problem:** Different URLs/credentials for dev/staging/production
**Solution:** Environment-based configuration!

```javascript
// config/environment.js
export const environments = {
  // 🔧 Development environment
  dev: {
    baseUrl: 'https://dev-myapp.com',
    apiUrl: 'https://dev-api.myapp.com',
    database: 'dev_database',
    users: {
      admin: { username: 'dev_admin', password: 'dev_admin_pass', role: 'admin' },
      testUser: { username: 'dev_user', password: 'dev_user_pass', role: 'user' },
      manager: { username: 'dev_manager', password: 'dev_manager_pass', role: 'manager' }
    },
    timeouts: {
      default: 5000,
      long: 10000
    }
  },

  // 🚀 Staging environment
  staging: {
    baseUrl: 'https://staging-myapp.com',
    apiUrl: 'https://staging-api.myapp.com',
    database: 'staging_database',
    users: {
      admin: { username: 'staging_admin', password: 'staging_admin_pass', role: 'admin' },
      testUser: { username: 'staging_user', password: 'staging_user_pass', role: 'user' },
      manager: { username: 'staging_manager', password: 'staging_manager_pass', role: 'manager' }
    },
    timeouts: {
      default: 10000,  // Staging might be slower
      long: 20000
    }
  },

  // 🌟 Production environment (for production smoke tests)
  prod: {
    baseUrl: 'https://myapp.com',
    apiUrl: 'https://api.myapp.com',
    database: 'production_database',
    users: {
      // ⚠️ Limited production testing!
      testUser: { username: 'prod_readonly_user', password: 'prod_readonly_pass', role: 'readonly' }
    },
    timeouts: {
      default: 15000,  // Production needs more time
      long: 30000
    }
  }
};

// 🎯 Get current environment
export const getEnvironment = () => {
  const env = process.env.TEST_ENV || 'dev';  // Default to dev

  if (!environments[env]) {
    throw new Error(`Unknown environment: ${env}. Available: ${Object.keys(environments).join(', ')}`);
  }

  return environments[env];
};

// 🔧 Helper functions
export const getBaseUrl = () => getEnvironment().baseUrl;
export const getTestUser = (role = 'testUser') => getEnvironment().users[role];
export const getTimeout = (type = 'default') => getEnvironment().timeouts[type];

// Using in Page Objects
import { getEnvironment, getTestUser } from '../config/environment.js';

export class LoginPage extends BasePage {
  constructor(page) {
    super(page);
    this.environment = getEnvironment();

    // Locators...
    this.usernameInput = page.getByLabel('Username');
    this.passwordInput = page.getByLabel('Password');
    this.loginButton = page.getByRole('button', { name: 'Login' });
  }

  async navigateToLogin() {
    await this.goto(`${this.environment.baseUrl}/login`);
    await this.verifyPageTitle('Login - MyApp');
  }

  async loginWithTestUser(role = 'testUser') {
    const user = getTestUser(role);
    await this.login(user.username, user.password);
  }

  async loginAsAdmin() {
    await this.loginWithTestUser('admin');
  }

  async loginAsManager() {
    await this.loginWithTestUser('manager');
  }
}

// Running tests with different environments
// npm scripts in package.json:
{
  "scripts": {
    "test:dev": "TEST_ENV=dev npx playwright test",
    "test:staging": "TEST_ENV=staging npx playwright test",
    "test:prod": "TEST_ENV=prod npx playwright test --grep @smoke"
  }
}
```

### 🛠️ **5.4 Test Utilities and Helpers**

**Problem:** Repeating common logic across tests
**Solution:** Utility functions!

```javascript
// utils/TestHelpers.js
export class TestHelpers {
  // 📧 Generate unique test data
  static generateRandomEmail() {
    const timestamp = Date.now();
    const random = Math.random().toString(36).substring(2, 8);
    return `test_${timestamp}_${random}@example.com`;
  }

  static generateRandomString(length = 8) {
    const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    let result = '';
    for (let i = 0; i < length; i++) {
      result += chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return result;
  }

  static generateRandomPhone() {
    return `+1${Math.floor(Math.random() * 9000000000) + 1000000000}`;
  }

  // 🏢 Generate realistic test data
  static generateUserData() {
    const firstNames = ['John', 'Jane', 'Bob', 'Alice', 'Charlie', 'Diana'];
    const lastNames = ['Smith', 'Johnson', 'Williams', 'Brown', 'Jones', 'Garcia'];

    const firstName = firstNames[Math.floor(Math.random() * firstNames.length)];
    const lastName = lastNames[Math.floor(Math.random() * lastNames.length)];

    return {
      firstName,
      lastName,
      email: `${firstName.toLowerCase()}.${lastName.toLowerCase()}.${Date.now()}@test.com`,
      phone: this.generateRandomPhone(),
      password: 'TestPassword123!',
      username: `${firstName.toLowerCase()}${lastName.toLowerCase()}${Math.floor(Math.random() * 1000)}`
    };
  }

  // ⏰ Wait for conditions with timeout
  static async waitForCondition(conditionFn, timeout = 10000, interval = 100) {
    const startTime = Date.now();

    while (Date.now() - startTime < timeout) {
      try {
        const result = await conditionFn();
        if (result) {
          return result;
        }
      } catch (error) {
        // Continue waiting
      }

      await new Promise(resolve => setTimeout(resolve, interval));
    }

    throw new Error(`Condition not met within ${timeout}ms`);
  }

  // 📊 Array utilities
  static shuffleArray(array) {
    const shuffled = [...array];
    for (let i = shuffled.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
    }
    return shuffled;
  }

  static getRandomArrayElement(array) {
    return array[Math.floor(Math.random() * array.length)];
  }

  // 📅 Date utilities
  static getFutureDate(daysFromNow = 7) {
    const date = new Date();
    date.setDate(date.getDate() + daysFromNow);
    return date.toISOString().split('T')[0]; // YYYY-MM-DD format
  }

  static getPastDate(daysAgo = 7) {
    const date = new Date();
    date.setDate(date.getDate() - daysAgo);
    return date.toISOString().split('T')[0];
  }

  // 💳 Credit card test data
  static getValidCreditCard() {
    return {
      number: '4111111111111111',  // Valid test Visa number
      expiry: '12/25',
      cvv: '123',
      name: 'Test User'
    };
  }

  static getInvalidCreditCard() {
    return {
      number: '1234567890123456',  // Invalid number
      expiry: '01/20',              // Expired
      cvv: '000',
      name: 'Test User'
    };
  }

  // 🌐 URL utilities
  static isValidUrl(string) {
    try {
      new URL(string);
      return true;
    } catch (_) {
      return false;
    }
  }

  // 📝 String utilities
  static capitalize(string) {
    return string.charAt(0).toUpperCase() + string.slice(1).toLowerCase();
  }

  static formatCurrency(amount, currency = 'USD') {
    return new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: currency
    }).format(amount);
  }
}

// utils/DatabaseHelpers.js (for API testing)
export class DatabaseHelpers {
  // 🗄️ Database cleanup utilities
  static async cleanupTestUser(email) {
    // This would connect to your test database
    // and clean up test data after tests
    console.log(`Cleaning up test user: ${email}`);
  }

  static async createTestData(userData) {
    // Create test data via API calls
    console.log(`Creating test data:`, userData);
  }

  static async resetDatabase() {
    // Reset database to known state
    console.log('Resetting database to clean state');
  }
}

// Using helpers in tests
import { TestHelpers } from '../utils/TestHelpers.js';

test('user registration with random data @regression', async ({ page }) => {
  const userData = TestHelpers.generateUserData();
  const loginPage = new LoginPage(page);
  const registrationPage = new RegistrationPage(page);

  // Use generated data for registration
  await registrationPage.navigateToRegistration();
  await registrationPage.registerUser(userData);

  // Verify registration success
  await expect(page.getByText(`Welcome, ${userData.firstName}!`)).toBeVisible();
});

test('checkout with valid credit card @e2e @payment', async ({ page }) => {
  const creditCard = TestHelpers.getValidCreditCard();
  const checkoutPage = new CheckoutPage(page);

  // ... add items to cart ...

  await checkoutPage.enterPaymentInfo(creditCard);
  await checkoutPage.completeOrder();

  await expect(page.getByText('Order successful!')).toBeVisible();
});
```


## Some Websites to practice testing
https://www.saucedemo.com/
https://the-internet.herokuapp.com/
https://books.toscrape.com/
https://uitestingplayground.com/
https://www.demoblaze.com/
https://parabank.parasoft.com/
