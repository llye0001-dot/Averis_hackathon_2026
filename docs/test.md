# Software Quality & EML Parser Test Report

## 1. Executive Summary
This document logs the Quality Assurance (QA) testing performed on the **SDOC (Shipping Document Checker)** web interface. The primary focus of this test suite was evaluating the `.eml` file upload functionality, input sanitization, error handling, and Git integration workflows.

---

## 2. Test Execution & Setup

### Environment Details
* **Target Application URL:** `main.d3pt38qur6i911.amplifyapp.com`
* **Test Platform:** WSL (Ubuntu) / Windows 11
* **Tools Used:** VS Code, Python 3, Git, Google Chrome

---

## 3. Test Cases & Evidence

### Test Case 1: Interface & Inbox KPI Verification
* **Objective:** Verify that the primary navigation, status counters, and "Upload .eml" action triggers are visible and functional on the SDOC Inbox page.
* **Status:** `PASSED`
* **Evidence:**

![SDOC Dashboard View](../apk/screenshot_inbox.png)
*(Image Reference: `apk/screenshot_inbox.png` or `docs/screenshots/inbox_overview.png`)*

---

### Test Case 2: Synthetic EML Test File Generation
* **Objective:** Automatically produce test `.eml` files covering Clean Docs, Data Mismatches, Script Injection (XSS), and Malformed Headers using a Python script.
* **Script Location:** `tests/create_test_eml.py`
* **Generated Artifacts:**
  * `tests/emltestsfiles/clean_shipping_doc.eml`
  * `tests/emltestsfiles/mismatch_test.eml`
  * `tests/emltestsfiles/script_injection_xss.eml`
  * `tests/emltestsfiles/malformed_missing_headers.eml`
* **Status:** `PASSED`
* **Evidence:**

![VS Code EML Script Execution](../apk/screenshot_script.png)
*(Image Reference: `apk/screenshot_script.png`)*

---

### Test Case 3: Security & XSS Payload Validation
* **Objective:** Inspect generated `.eml` file contents to ensure XSS vectors (`<script>alert('XSS')</script>`, `<img src=x onerror=...>`) are properly structured before uploading to verify UI sanitization.
* **Status:** `PASSED`
* **Evidence:**

![XSS EML Inspection](../apk/screenshot_xss.png)
*(Image Reference: `apk/screenshot_xss.png`)*

---

### Test Case 4: Git Identity Configuration Check
* **Objective:** Attempt to commit test scripts and verification files to Source Control and handle missing Git identity errors (`fatal: no email was given`).
* **Resolution:** Executed `git config --global user.email` and `git config --global user.name` in terminal.
* **Status:** `RESOLVED`

---

## 4. Summary of Test Results

| Test ID | Test Scenario | Input Data | Result | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **TC-01** | UI Inspection | Website Home | **PASS** | Dashboard KPIs and controls rendered. |
| **TC-02** | Test Data Creation | `create_test_eml.py` | **PASS** | 4 `.eml` files generated successfully. |
| **TC-03** | Security Payload | `script_injection_xss.eml` | **PASS** | XSS payload script embedded as expected for upload testing. |
| **TC-04** | Source Control | VS Code Git Commit | **PASS** | Git user configuration resolved, changes staged and committed. |