# Procura Final Submission — Teammate Handoff & Verification Checklist

**Module:** SE3090 Software Testing and Quality Evaluation / Software Engineering  
**Application:** Procura Mobile (Flutter) & Enterprise Platform  
**Target Submission Artifact:** `app-release.apk` (via GitHub Release)  
**Handoff Repository:** `https://github.com/AlgoDove/flutter-ku-da`  

---

## 1. Assignment Rubric Guidance & Context

> **Note on Rubric Interpretation:**  
> The SE3090 assignment rubric was reviewed via Gemini (which had access to the rubric document). Its reported interpretation indicates that the rubric requires a **runnable Android APK** and **clear installation instructions**, and does not explicitly mandate production keystore signing (the debug-signed release APK satisfies the runnable APK requirement).  
> *Note:* This represents a reported rubric interpretation, not an independent legal or academic guarantee.

---

## 2. On-Device Verification Checklist (To Be Performed by Submitter)

Before finalizing and submitting the assignment deliverables, the teammate responsible for submission must complete the following checks on a physical Android device:

- [ ] **Download Release APK:** Download `app-release.apk` directly from the release link:  
  `https://github.com/AlgoDove/flutter-ku-da/releases/download/v1.0.0-candidate/app-release.apk`
- [ ] **Physical Device Installation:** Install the APK on a physical Android phone (running Android 5.0 or newer).
  - [ ] Successfully permit "Install unknown apps" / sideloading.
  - [ ] Successfully bypass Google Play Protect unverified developer prompt ("Install anyway").
  - [ ] Verify the Procura application opens to the Login screen without crashes.
- [ ] **Network Connectivity Verification:**
  - [ ] Confirm mobile device has an active internet connection (Wi-Fi or mobile data).
  - [ ] Open `https://procura-production-98b4.up.railway.app/health` in device browser to verify backend is awake (`Healthy`).
- [ ] **Authentication & Workflow Smoke Test:**
  - [ ] Log in using the authorized **EMPLOYEE** test credentials (from private team credentials sheet).
  - [ ] Verify the Procurement Requests list loads live data from the server.
  - [ ] Open "New Request" $\rightarrow$ verify switching to "Manual Request" displays the form.
  - [ ] Verify the Material Date Picker opens and prevents selection of dates before tomorrow.
  - [ ] Fill in a sample requisition, add at least one line item, and submit the draft.
  - [ ] Verify the newly created request appears in the list.
- [ ] **Role Permissions & Boundaries Check:**
  - [ ] Verify employee cannot access restricted management/approval actions.
  - [ ] Log in with the **PROCUREMENT_OFFICER** account and verify vendor directory access.

---

## 3. Submission Package & Deliverables Checklist

- [ ] **APK Deliverable:**
  - [ ] Verify the GitHub Release page is accessible to evaluators:  
    `https://github.com/AlgoDove/flutter-ku-da/releases/tag/v1.0.0-candidate`
  - [ ] Alternatively, attach `app-release.apk` directly to the LMS submission zip if required by the assignment guidelines.
- [ ] **Installation Instructions:**
  - [ ] Include `docs/FLUTTER_INSTALLATION.md` or paste its contents into the submission annex/appendix.
- [ ] **Evaluator Test Credentials:**
  - [ ] Ensure valid, active test account credentials (email and password for each tested role: EMPLOYEE, PROCUREMENT_OFFICER, MANAGER, ADMIN) are included in the **confidential submission document / LMS text box**.
  - [ ] **CRITICAL:** Do NOT publish test passwords, database connection strings, or JWT secrets in public GitHub repositories.
- [ ] **Consolidated Report Details:**
  - [ ] Verify official **Group Number / Group ID** is recorded correctly on the title page.
  - [ ] Verify individual student names, registration numbers, and component ownership are accurate.
  - [ ] Verify the **Demo Video URL** is valid, unlisted/publicly accessible, and embedded in the report.
  - [ ] Ensure backend live base URL (`https://procura-production-98b4.up.railway.app`) is cited.
  - [ ] Note that Swagger is disabled in production and the `/health` endpoint serves as the public health check.
