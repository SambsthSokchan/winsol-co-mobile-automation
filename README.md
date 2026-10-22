# WinSol CO Mobile Automation
**Do not write the code yourself.** Let Claude CLI write it. That is its entire purpose.

You have created the empty files (0 bytes). Claude CLI needs to open them and fill them with the actual TypeScript code. 

If you try to write the code manually right now, you will get stuck on Appium locators, driver configurations, and Excel parsing. Claude CLI will do all of that for you in about 3 minutes.

Here is exactly what you do next:

### Step 1: Put your APK and Excel in the folders
*(Make sure they are actually in the local folders, not just on your desktop)*
```cmd
copy %USERPROFILE%\Desktop\winsol_co_app.apk apps\
copy %USERPROFILE%\Desktop\WinSol_7_Automation_Sheets.xlsx test-data\
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

---

> **"I have created the folder structure and empty files for a Mobile-only Appium 2.x + TypeScript test framework. The APK is at `apps/winsol_co_app.apk` and the Excel is at `test-data/WinSol_7_Automation_Sheets.xlsx`. The emulator `emulator-5554` is running.**
>
> **Write the code into the empty files in this exact order:**
>
> **1. Configuration:**
> - `package.json` — deps: `webdriverio`, `xlsx`, `mocha`, `chai`, `ts-node`, `typescript`. Script: `"test:co": "npx mocha -r ts-node/register tests/mobile/co-ui.spec.ts --timeout 60000"`.
> - `tsconfig.json` — Node target, CommonJS.
> - `capabilities.json` — UiAutomator2, `appium:app: "./apps/winsol_co_app.apk"`, `appium:udid: "emulator-5554"`, `autoGrantPermissions: true`, `noReset: false`.
> - `CLAUDE.md` — UI-only rules, `accessibilityId` locators, no sleeps, Excel sheet `1. CO (Mobile App)` only.
>
> **2. Utilities:**
> - `utils/excelReader.ts` — reads sheet `1. CO (Mobile App)`. Returns `testCaseId`, `module`, `testSteps`, `testData`, `expectedResult`, and **`excelRow`** (the real row number).
> - `utils/excelWriter.ts` — accepts `status: 'PASS' | 'FAIL'`. **Default FAIL.** Finds row by scanning column A for the Test Case ID. Writes to J, N, O, P, Q.
> - `utils/driverFactory.ts` — Appium session manager.
>
> **3. Page Objects:**
> - `pages/mobile/coLoginPage.ts` — Login + OTP page object using `~` accessibility IDs.
>
> **4. Test Spec:**
> - `tests/mobile/co-ui.spec.ts` — loops through Excel, dispatches by `tc.module`, asserts `tc.expectedResult` appears on screen (XPath + escaped quotes + 5s wait). Screenshot on failure. Writes back to Excel in `finally`.
>
> **5. Install dependencies:**
> `npm install`
>
> **6. THE BREAK TEST (MANDATORY — DO NOT SKIP):**
> - Change `TC-CO-001`'s Expected Result in the Excel file to `ZZZ_FAKE_TEXT`.
> - Run ONLY that test: `npx mocha -r ts-node/register tests/mobile/co-ui.spec.ts --grep "TC-CO-001" --timeout 60000`
> - **Paste the output.**
> - If it PASSES → fix `excelWriter.ts` and `co-ui.spec.ts` until it FAILS. Re-run.
> - Once it fails → revert the Excel change.
>
> **7. Only after the break test fails → run the full suite: `npm run test:co`."**

---

### Step 4: Watch and wait

Claude CLI will start writing the code. It will take about 2–3 minutes. Then it will install the dependencies. Then it will start the break test.

**When it runs the break test, paste the output here.** 

That is the only thing I need to see. Once we confirm the break test **fails** (as it should), your harness is officially real and we can celebrate.
## 📁 The Complete Architecture
