# Procura Mobile (Android) — Submission Handoff & Distribution Repository

**Project:** Procura — AI-Assisted Enterprise Procurement Management System  
**Academic Module:** SE3090 Software Testing & Quality Evaluation / Software Engineering  
**Handoff Repository:** [AlgoDove/flutter-ku-da](https://github.com/AlgoDove/flutter-ku-da)  

---

## 1. Purpose of This Repository

This dedicated handoff repository serves as the official distribution and evaluation hub for the **Procura Android Mobile Application (`app-release.apk`)** and its supporting academic submission documentation. 

It provides group members and academic evaluators with:
1. Direct access to download the pre-compiled, release-configured Android APK without requiring local Flutter SDK installation.
2. Verified artifact checksums and build audit logs.
3. Step-by-step sideloading and installation instructions for Android devices.
4. Pre-submission verification checklists for on-device testing.

---

## 2. Release Artifact & Download Links

The production release APK is hosted as an asset on GitHub Releases:

* **Release Page:** [Procura Flutter APK — Submission Candidate (v1.0.0-candidate)](https://github.com/AlgoDove/flutter-ku-da/releases/tag/v1.0.0-candidate)
* **Direct Download Link:** [app-release.apk (51.4 MB)](https://github.com/AlgoDove/flutter-ku-da/releases/download/v1.0.0-candidate/app-release.apk)
* **Exact File Size:** `53,872,838 bytes` (~51.38 MB)
* **SHA-256 Checksum:**  
  `9376476D9D1D90AEBA16DB298212C5252C092331887F7ABE04B9D02FF2244611`

---

## 3. Quick Installation Summary

1. **Download:** Open the [Direct APK Download Link](https://github.com/AlgoDove/flutter-ku-da/releases/download/v1.0.0-candidate/app-release.apk) on your Android device (Android 5.0+).
2. **Allow Sideloading:** When prompted, open **Settings** and toggle **Allow from this source** to ON.
3. **Play Protect Warning:** If Google Play Protect prompts regarding an unverified developer certificate, tap **More details** $\rightarrow$ **Install anyway**.
4. **Launch:** Tap **Install**, then open **Procura** from your app drawer.

> For comprehensive installation instructions and detailed troubleshooting, refer to [docs/FLUTTER_INSTALLATION.md](docs/FLUTTER_INSTALLATION.md).

---

## 4. Backend Connectivity & Infrastructure

* **Internet Connection Mandatory:**  
  The Procura mobile application communicates dynamically with the live cloud backend over secure HTTPS. **An active Wi-Fi or mobile data connection is required at all times.** The app does not operate in offline mode or use local mock fixtures.
* **Production API Base URL:**  
  `https://procura-production-98b4.up.railway.app`
* **Health Check Probe:**  
  `https://procura-production-98b4.up.railway.app/health` (Returns `Healthy`)
* **Swagger UI Notice:**  
  In accordance with production hardening standards, Swagger interactive UI (`/swagger`) is disabled on the production Railway instance. All API endpoints operate under the standard `/api/` routing structure.

---

## 5. Documentation Directory

Detailed documentation is available in the [`docs/`](docs/) directory:

| Document | Description |
| :--- | :--- |
| **[`docs/FLUTTER_INSTALLATION.md`](docs/FLUTTER_INSTALLATION.md)** | Full installation walkthrough, sideloading configuration, device permissions, and troubleshooting guide. |
| **[`docs/SUBMISSION_CHECKLIST.md`](docs/SUBMISSION_CHECKLIST.md)** | Mandatory pre-submission verification checklist, on-device smoke test procedures, and report deliverable requirements. |
| **[`docs/ARTIFACT_VERIFICATION.md`](docs/ARTIFACT_VERIFICATION.md)** | Technical build audit: compilation parameters, Gradle configuration, SHA-256 hash, and test coverage logs. |

---

## 6. Current Verification Status & Testing Audit

The application artifact has undergone rigorous automated testing and live API contract verification:

* **Static Code Analysis:** `flutter analyze` completed with **0 issues**.
* **Automated Test Suite:** `flutter test` passed **23 / 23 automated tests** (covering UI widgets, JWT claim decoding, DTO models serialization, and environment configuration).
* **Live API Integration:** The APK was verified against the deployed Railway instance using live HTTP integration tests:
  - User authentication and role token generation (Employee & Procurement Officer).
  - Retrieval and creation of procurement requisitions.
  - Requisition submission into approval workflow.
  - Certified vendor listing and role-based access control (RBAC) restrictions.
* **Physical Device Testing Status:**  
  **Outstanding / To be executed by submission team.** No physical Android phone or running emulator was attached during the build audit session. The submitter must perform on-device verification on physical hardware prior to final submission as outlined in [docs/SUBMISSION_CHECKLIST.md](docs/SUBMISSION_CHECKLIST.md).

---

## 7. Known Issues & Operational Limitations

1. **DEF-APP-002 (Server-Side Past Date Persistence):**  
   The deployed backend currently permits past dates on raw HTTP requests due to a missing model validation attribute on `RequiredByDate`. In the mobile application, this is mitigated client-side by restricting the native Material Date Picker (`firstDate: DateTime.now()`), preventing users from selecting past dates.
2. **Empty User Body in Login Response:**  
   The backend `/api/auth/login` endpoint returns an empty `user: {}` object. The mobile app decodes user ID and role directly from the signed JWT payload.
3. **Cloud Container Cold Starts:**  
   The backend is hosted on a Railway cloud instance. When idle, the container may take 15–30 seconds to spin up on the first incoming network request. Subsequent requests respond normally.

---

## 8. Test Credentials Notice

* **No Credentials in Repository:**  
  In strict accordance with security policies, **no passwords, user accounts, JWT secrets, or database credentials are stored in this repository**.
* **Obtaining Credentials:**  
  Teammates and academic evaluators must obtain authorized test account credentials (for Employee, Procurement Officer, Manager, and Admin roles) through the **confidential submission sheet / LMS private submission field**.
