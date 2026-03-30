Playwright CLI + Playwright Test — Setup & Usage (PDP Experiment)

Purpose
-------
This note documents how to set up and use Playwright Test and Playwright CLI
locally on macOS and Windows for PDP learning and experimentation.
It is written so the steps can be reproduced end-to-end.


Prerequisites (All Platforms)
-----------------------------
- Node.js 18+
- npm (comes with Node)
- Git

Verify Node:
node -v
npm -v


Platform-Specific Setup
-----------------------

macOS
-----
- Supported on Intel and Apple Silicon (M1/M2/M3)
- Recommended install method:

brew install node


Windows
-------
- Supported on Windows 10 / 11
- Install Node.js LTS (18+) from https://nodejs.org
- During installation, ensure "Add to PATH" is checked

Verify installation (Command Prompt or PowerShell):
node -v
npm -v

Commands can be run in:
- Command Prompt
- PowerShell
- VS Code integrated terminal


Repo Setup (Local Machine)
--------------------------
The repo lives in GitHub, but all commands are run locally after cloning.

Clone the repo:

macOS / Linux:
cd ~/projects
git clone <YOUR_GITHUB_REPO_URL>
cd <REPO_FOLDER_NAME>

Windows (PowerShell / CMD):
cd %USERPROFILE%
git clone <YOUR_GITHUB_REPO_URL>
cd <REPO_FOLDER_NAME>

You are in the correct folder if this shows package.json:
ls


package.json (Minimal Working Example)
-------------------------------------
Ensure package.json is valid JSON.
An incomplete script (for example: "playwright test --") will break npm install.

{
  "name": "pdp-playwright-ai-learning",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "test": "playwright test",
    "test:ui": "playwright test --ui",
    "report": "playwright show-report"
  },
  "devDependencies": {
    "@playwright/test": "^1.50.0",
    "@playwright/cli": "^0.1.0"
  }
}


Install Dependencies
--------------------
From the repo root:

npm install

If npm install fails:
rm -rf node_modules package-lock.json
npm cache clean --force
npm install

Windows note:
- Use PowerShell instead of Git Bash if permission errors occur.


Install Playwright Browsers
---------------------------
Required once per machine.

npx playwright install

This installs Chromium, WebKit, and Firefox.


Using Playwright CLI (Core PDP Learning Goal)
---------------------------------------------
Playwright CLI is separate from Playwright Test.
It drives a real browser via terminal commands and generates Playwright code.

Important:
- Use: npx @playwright/cli
- Do NOT rely on npx playwright-cli unless installed globally


Open a Website
--------------
npx @playwright/cli open https://www.activeandfit.com

Optional (show browser UI):
npx @playwright/cli open https://www.activeandfit.com --headed


Take a Snapshot (Critical Step)
-------------------------------
npx @playwright/cli snapshot

This prints an accessibility snapshot with element references like:

e1 [link "Log In"]
e2 [button "Search"]

These e# references are used for interactions.


Interact with the Page
----------------------
Use the element references from the snapshot.

Example:
npx @playwright/cli click e1

Each action prints generated Playwright TypeScript code.
Copy this code into a test file, for example:
tests/ai-generated/example-ai.spec.ts


Optional: Save Snapshot as YAML
-------------------------------
npx @playwright/cli snapshot --filename=homepage.yaml

This creates an artifact that can be committed as learning evidence.
