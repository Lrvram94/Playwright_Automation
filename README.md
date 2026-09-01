# Playwright UI Automation Project

[![Playwright Tests](https://github.com/Lrvram94/Playwright_Automation/actions/workflows/playwright-ci.yml/badge.svg)](https://github.com/Lrvram94/Playwright_Automation/actions/workflows/playwright-ci.yml)

A TypeScript-based Playwright automation framework using the Page Object Model pattern with CI/CD integration.

## Project Structure

```
├── .github/workflows/     # CI/CD workflows
├── src/
│   ├── pages/            # Page Object Model classes
│   │   ├── BasePage.ts   # Base page with common functionality
│   │   ├── SauceDemoObjects.ts # SauceDemo page objects
│   │   └── TradeMeObjects.ts   # TradeMe page objects
│   ├── utils/            # Utility functions and test helpers
│   │   └── test-utils.ts # Common test utilities
│   ├── data/             # Centralized test data
│   │   └── test-data.ts  # URLs, credentials, test data
│   └── config/           # Environment configuration
│       └── config.ts     # Timeouts, settings, environment config
├── tests/
│   └── ui tests/         # UI automation test specs
│       ├── SauceDemo.test.ts  # E-commerce automation tests
│       └── TradeMe.test.ts    # Marketplace automation tests
├── test-results/         # Test execution results
├── playwright-report/    # HTML test reports
├── playwright.config.ts  # Playwright configuration
├── package.json          # Dependencies and scripts
└── README.md            # This file
```
## Features

- **Page Object Model** - Maintainable test structure
- **TypeScript Support** - Full type safety
- **CI/CD Integration** - GitHub Actions workflows
- **Live Application Testing** - SauceDemo & TradeMe automation
- **Professional Reporting** - HTML reports with screenshots

## Getting Started

### 1. **Install Dependencies:**
```bash
npm install
```

### 2. **Install Playwright Browsers:**
```bash
npm run install:browsers
```

### 3. **Run Tests Locally:**
```bash
npm test                    # Run all tests headlessly
npm run test:headed         # Run with visible browser
npm run test:debug          # Debug mode with DevTools
npm run test:ui             # Interactive UI test runner
```

### 4. **View Test Results:**
```bash
npm run test:report         # Open HTML test report
```

## Available Tests

- **SauceDemo E-commerce**: Login, product selection, cart operations, checkout
- **TradeMe Marketplace**: Search functionality, listing interactions, navigation

### Run Specific Tests
```bash
npm run test:smoke                              # Smoke tests only
npm test -- tests/ui\ tests/SauceDemo.test.ts  # SauceDemo tests
npm test -- tests/ui\ tests/TradeMe.test.ts    # TradeMe tests
npm test -- --grep "login"                     # Tests matching "login"
```

## CI/CD Integration

- **Main CI Pipeline** - Runs on push to `main`/`develop`
- **PR Validation** - Runs smoke tests on pull requests
- **Manual Triggers** - Available in GitHub Actions tab

## Development Patterns

### Page Object Structure
All page objects extend `BasePage` and use getter methods:
```typescript
export class YourPage extends BasePage {
  get baseUrl(): string { return 'https://example.com'; }
  get submitButton(): Locator { return this.page.locator('#submit'); }
}
```

### Adding New Tests
1. Create page object in `src/pages/` extending `BasePage`
2. Add test data in `src/data/test-data.ts`
3. Write test spec in `tests/` subdirectory
4. Use codegen for recording: `npm run test:codegen`

## Debugging
```bash
npm run test:debug          # Step through tests interactively
npm run test:headed         # Watch tests execute in browser
npm run test:ui             # Visual test runner
npm run test:codegen        # Generate tests from interactions
```

## Support
- View test reports: `npm run test:report`
- Check CI logs in GitHub Actions tab
- Use debug mode for troubleshooting