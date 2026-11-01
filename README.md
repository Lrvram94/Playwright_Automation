---
title: Playwright UI Automation Project
description: Setup and execution instructions for the TypeScript Playwright UI automation project.
---

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
│   └── ui-tests/         # UI automation test specs
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

### 1. Install Prerequisites

- Install [Node.js 22 LTS](https://nodejs.org/en/download), including npm.
- Install [Git](https://git-scm.com/downloads) if you will clone the repository. Git is not needed for a ZIP download.
- Optionally install [Visual Studio Code](https://code.visualstudio.com/) to edit files and use its integrated terminal.
- Use an operating system supported by [Playwright](https://playwright.dev/docs/intro#system-requirements).
- Ensure internet access for package downloads, browser downloads, and the external demo websites.

Open a new terminal after installation and verify Node.js and npm:

```shell
node --version
npm --version
```

If cloning, also verify Git:

```shell
git --version
```

### 2. Get the Project

Clone the repository and enter its folder:

```shell
git clone https://github.com/Lrvram94/Playwright_Automation.git
cd Playwright_Automation
```

Alternatively, open the [GitHub repository](https://github.com/Lrvram94/Playwright_Automation),
select **Code > Download ZIP**, and extract it. Open the extracted project folder in
VS Code using **File > Open Folder**, then select **Terminal > New Terminal**.

Run all remaining commands from the folder containing [package.json](package.json).
For a ZIP download, the extracted folder name may include the branch name.

### 3. Install Project Dependencies

```shell
npm ci
```

This installs the dependency versions recorded in [package-lock.json](package-lock.json).
Keep the lockfile with the project when transferring it to another computer.
Install dependencies on the new computer rather than copying the old computer's dependency folder.

### 4. Install the Playwright Browser

The current configuration runs tests in Chromium only. On Windows or macOS, run:

```shell
npx playwright install chromium
```

On a supported Linux distribution, install Chromium and its system dependencies:

```bash
npx playwright install --with-deps chromium
```

Linux system dependency installation may require administrator privileges.
Playwright needs its own browser binaries even if Google Chrome is already installed.

### 5. Understand Environment Settings

The current tests use application URLs and public demo credentials defined in
[src/data/test-data.ts](src/data/test-data.ts). You do not need a local environment
file to run these tests with their existing demo settings.

[playwright.config.ts](playwright.config.ts) loads a local `.env` file when present.
[.env.example](.env.example) is a shareable template, not a file loaded during tests.
Its application URL and credential variables are not currently read by the test data code,
so filling them in does not override the hardcoded values.

Optionally create a local environment file from the template. Skip this step if
you already have one; do not overwrite existing local settings.

Windows PowerShell:

```powershell
if (-not (Test-Path .env)) { Copy-Item .env.example .env }
```

macOS or Linux:

```bash
if [ ! -e .env ]; then cp .env.example .env; fi
```

Keep real secrets out of the template and source control. The local `.env` file
is excluded by [.gitignore](.gitignore).

### 6. Run Tests

Run all tests without visible browser windows:

```shell
npm test
```

Or run all tests with visible browser windows:

```shell
npm run test:headed
```

Headed mode requires a graphical desktop session. On a server without a display,
use headless mode with `npm test`.

For interactive testing or debugging, choose one of these commands:

```shell
npm run test:ui
npm run test:debug
```

### 7. View Test Results

After a test run, open the HTML report:

```shell
npm run test:report
```

Open the localhost URL printed in the terminal if the browser does not open automatically.
Press **Ctrl+C** in that terminal to stop the report server when finished.
Reports are generated in `playwright-report/`; execution results and failure
attachments are generated in `test-results/`.

## Available Tests

- **SauceDemo E-commerce**: Login, product selection, cart operations, checkout
- **TradeMe Marketplace**: Search functionality, listing interactions, navigation

### Run Specific Tests

```bash
npm run test:smoke                              # Smoke tests only
npm test -- tests/ui-tests/SauceDemo.test.ts     # SauceDemo tests
npm test -- tests/ui-tests/TradeMe.test.ts       # TradeMe tests
npm test -- tests/ui-tests/OrangeHrm.test.ts     # OrangeHRM tests
npm test -- --grep "login"                     # Tests matching "login"
```

## Setup Troubleshooting

| Problem | What to check |
| --- | --- |
| `node` or `npm` is not recognized | Install Node.js with npm, then restart the terminal or VS Code so the updated PATH is loaded. |
| `test:headed` is not recognized | Use `npm run test:headed`. Script names cannot be run as standalone shell commands. |
| npm cannot find the package manifest | Open a terminal in the project root containing [package.json](package.json), not its parent folder. |
| `npm ci` reports a missing or inconsistent lockfile | Obtain matching [package.json](package.json) and [package-lock.json](package-lock.json) from the same project revision. |
| Playwright reports that a browser executable is missing | Run `npx playwright install chromium` on this computer after installing dependencies. |
| Linux reports missing browser libraries | Run `npx playwright install --with-deps chromium` on a supported distribution with the required installation permissions. |
| Headed mode cannot open a browser window | Use a graphical desktop session, or run `npm test` without visible windows. |
| Tests start but fail on navigation, login, or element assertions | Check the report and the target demo website. External-site changes, availability, or test logic can cause failures even when installation is correct. |

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