# WinSol CO Mobile Automation
**Do not write the code yourself.** Let Claude CLI write it. That is its entire purpose.

You have created the empty files (0 bytes). Claude CLI needs to open them and fill them with the actual TypeScript code. 

If you try to write the code manually right now, you will get stuck on Appium locators, driver configurations, and Excel parsing. Claude CLI will do all of that for you in about 3 minutes.

Here is exactly what you do next:

### Step 1: Put your APK and Excel in the folders
*(Make sure they are actually in the local folders, not just on your desktop)*
```cmd
copy apps\winsol_co_app.apk apps\
copy test-data\WinSol_7_Automation_Sheets.xlsx test-data\
```
*(If they are in Downloads, change `Desktop` to `Downloads`)*

Verify they are there:
```cmd
dir apps
dir test-data
```
You must see the files and their sizes (not 0 bytes).

### Step 2: Start Claude CLI
```cmd
claude
```

### Step 3: Paste this prompt

Copy this entire block and paste it into Claude CLI. It tells Claude exactly which files to fill and forces the break test.

```cmd
I have created the folder structure and empty files for a Mobile-only Appium 2.x + TypeScript test framework. The APK is at apps/winsol_co_app.apk and the Excel is at test-data/WinSol_7_Automation_Sheets.xlsx. The emulator emulator-5554 is running.

Write the code into the existing empty files in this exact order:

1. Configuration:
- package.json — deps: webdriverio, xlsx, mocha, chai, ts-node, typescript. Script: "test:co": "npx mocha -r ts-node/register tests/mobile/co-ui.spec.ts --timeout 60000".
- tsconfig.json — Node target, CommonJS.
- capabilities.json — UiAutomator2, appium:app: "./apps/winsol_co_app.apk", appium:udid: "emulator-5554", autoGrantPermissions: true, noReset: false.
- CLAUDE.md — UI-only rules, accessibilityId locators, no sleeps, Excel sheet "1. CO (Mobile App)" only.

2. Utilities:
- utils/excelReader.ts — reads sheet "1. CO (Mobile App)". Returns testCaseId, module, testSteps, testData, expectedResult, and excelRow (the real row number).
- utils/excelWriter.ts — accepts status: 'PASS' | 'FAIL'. Default FAIL. Finds row by scanning column A for the Test Case ID. Writes to columns J, N, O, P, Q.
- utils/driverFactory.ts — Appium session manager.

3. Page Objects (Create all 16 files in pages/mobile/ and pages/mobile/tabs/):
- pages/mobile/coLoginPage.ts — Login, OTP digits 1-4, verify button.
- pages/mobile/prospectPage.ts — Add prospect, name, phone, purpose, amount, save.
- pages/mobile/preAssessmentPage.ts — CBC score, willingness, capacity, grade badge.
- pages/mobile/appraisalWizard.ts — Master Facade loading tabs 1 through 8.
- pages/mobile/tabs/clientInfoTab.ts — Tab 1 Khmer name, Latin name, ID, GPS button.
- pages/mobile/tabs/loanDetailsTab.ts — Tab 2 product type, amount, term, rate slider.
- pages/mobile/tabs/cashFlowTab.ts — Tab 3 income, expense, DSCR gauge.
- pages/mobile/tabs/guarantorTab.ts — Tab 4 relative switch, name, phone.
- pages/mobile/tabs/collateralTab.ts — Tab 5 market value, LTV badge, hard title input.
- pages/mobile/tabs/referenceTab.ts — Tab 6 reference name, relationship, verified checkbox.
- pages/mobile/tabs/documentsTab.ts — Tab 7 ID card upload, title deed upload, camera button.
- pages/mobile/tabs/summaryGateTab.ts — Tab 8 BR-01 badge, BTL reviewer select, submit button.
- pages/mobile/titleTransferPage.ts — Title deed number, cadastral office, hypothec reg button.
- pages/mobile/scheduleTaskPage.ts — Calendar view, task card, check-in button.
- pages/mobile/refinancePage.ts — Refinance switch, existing loan ID, withdrawal reason.
- pages/mobile/offlineSyncPage.ts — Offline status, queue count badge, sync now button.

4. Test Spec:
- tests/mobile/co-ui.spec.ts — loops through Excel, dispatches by tc.module using the Page Objects you created, asserts tc.expectedResult appears on screen (XPath with escaped quotes, 5s wait). Takes screenshot on failure. Writes back to Excel in a finally block.

5. Install dependencies:
Run npm install.

6. THE BREAK TEST (MANDATORY - DO NOT SKIP):
- Open the Excel file and change the Expected Result for TC-CO-001 to ZZZ_FAKE_TEXT.
- Run ONLY that test: npx mocha -r ts-node/register tests/mobile/co-ui.spec.ts --grep "TC-CO-001" --timeout 60000
- Paste the output.
- If it PASSES, you must fix excelWriter.ts and co-ui.spec.ts until it FAILS. Re-run until it fails.
- Once it fails, revert the Excel change.

7. Only after the break test fails, run the full suite: npm run test:co.

Show me every command and terminal output as you execute. Do not skip step 6.

```
That is the only thing I need to see. Once we confirm the break test **fails** (as it should), your harness is officially real and we can celebrate.
## 📁 Complete Project Structure
```cmd
winsol-co-mobile-automation/
│
├── apps/
│   └── winsol_co_app.apk                    ← [You put this here] Android App Binary
│
├── test-data/
│   └── WinSol_7_Automation_Sheets.xlsx      ← [You put this here] Your 1,159 test cases
│
├── pages/
│   └── mobile/
│       ├── coLoginPage.ts                   ← [Claude writes] Login & OTP
│       ├── prospectPage.ts                  ← [Claude writes] Prospect forms
│       ├── preAssessmentPage.ts             ← [Claude writes] Scorecard
│       ├── appraisalWizard.ts               ← [Claude writes] Master Facade for 8 tabs
│       ├── titleTransferPage.ts             ← [Claude writes] Hypothec & Title
│       ├── scheduleTaskPage.ts              ← [Claude writes] Field tasks
│       ├── refinancePage.ts                 ← [Claude writes] Refinance
│       ├── offlineSyncPage.ts               ← [Claude writes] Offline Sync
│       │
│       └── tabs/                            ← [Claude creates this folder]
│           ├── clientInfoTab.ts             ← [Claude writes] Tab 1: Demographics & GPS
│           ├── loanDetailsTab.ts            ← [Claude writes] Tab 2: Loan Terms
│           ├── cashFlowTab.ts               ← [Claude writes] Tab 3: DSCR & Income
│           ├── guarantorTab.ts              ← [Claude writes] Tab 4: Guarantors
│           ├── collateralTab.ts             ← [Claude writes] Tab 5: Valuation
│           ├── referenceTab.ts              ← [Claude writes] Tab 6: References
│           ├── documentsTab.ts              ← [Claude writes] Tab 7: Uploads
│           └── summaryGateTab.ts            ← [Claude writes] Tab 8: BR-01 Gate
│
├── tests/
│   └── mobile/
│       └── co-ui.spec.ts                    ← [Claude writes] The main Excel loop & Break Test
│
├── utils/
│   ├── driverFactory.ts                     ← [Claude writes] Appium Session Setup
│   ├── excelReader.ts                       ← [Claude writes] Reads Excel + Finds exact Row
│   └── excelWriter.ts                       ← [Claude writes] Writes PASS/FAIL to Excel
│
├── screenshots/                             ← [Empty folder] Claude saves failed screenshots here
├── reports/                                 ← [Empty folder] Optional HTML reports
│
├── capabilities.json                        ← [Claude writes] Appium Device Settings
├── CLAUDE.md                                ← [Claude writes] Agent Rules
├── package.json                             ← [Claude writes] NPM Dependencies
├── tsconfig.json                            ← [Claude writes] TypeScript Config
└── wdio.conf.ts                             ← [Claude writes] WebdriverIO Runner Setup
```
