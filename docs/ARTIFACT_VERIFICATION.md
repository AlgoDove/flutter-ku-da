# Procura Android APK — Artifact Verification Record

**Document Version:** 1.0.0  
**Verification Date:** October 9, 2026  
**Target Repository:** `AlgoDove/flutter-ku-da`  

---

## 1. Release Artifact Specification

| Attribute | Verified Value |
| :--- | :--- |
| **Artifact Filename** | `app-release.apk` |
| **Source Build Path** | `frontend-mobile/build/app/outputs/flutter-apk/app-release.apk` |
| **File Size (Exact Bytes)**| **53,872,838 bytes** (~51.38 MB) |
| **SHA-256 Checksum** | `9376476D9D1D90AEBA16DB298212C5252C092331887F7ABE04B9D02FF2244611` |
| **Target OS / Architecture** | Android 5.0+ (API 21+) / Universal (`arm64-v8a`, `armeabi-v7a`, `x86_64`) |
| **Signing Configuration** | Android release build signed with debug certificate (`signingConfigs.debug`) |
| **Configured API URL** | `https://procura-production-98b4.up.railway.app` |

---

## 2. Build Environment & Compilation Parameters

The APK was compiled from the frozen `origin/main` branch (commit `ea73af743a71b602f0594553109922eb2b400770`) in a dedicated, isolated working copy:

* **Flutter SDK:** Flutter 3.47.5 (Channel stable, Dart 3.13.4)
* **Android SDK:** Version 37.0.0 (API Level 34 platform build-tools)
* **Java Runtime:** OpenJDK 25.0.1 (bundled Android Studio JBR)
* **Gradle Version:** Gradle 9.3.1 (Kotlin DSL `build.gradle.kts`)
* **Compilation Command:**
  ```powershell
  flutter build apk --release --dart-define=API_URL=https://procura-production-98b4.up.railway.app
  ```
* **Icon Optimization:** Font asset `MaterialIcons-Regular.otf` tree-shaken from 1,645,184 bytes to 5,356 bytes (99.7% reduction).

---

## 3. Automated Verification Status

### A. Static Code Analysis (`flutter analyze`)
* **Result:** **0 issues found** across the entire Flutter mobile codebase.

### B. Automated Test Suite (`flutter test`)
* **Result:** **23 / 23 tests passed** (100% pass rate).
* **Test Breakdown:**
  - `widget_test.dart`: Login form rendering, empty form validation, Manual Request UI switching.
  - `vendor_screens_test.dart`: CreateVendorScreen form fields, empty submission validation, email format checks.
  - `auth_service_test.dart`: JWT token parsing with standard claims (`sub`, `email`, `role`), Microsoft/XMLSoap URI claims (`nameidentifier`, `emailaddress`, `role`), base64Url padding, expired token rejection, malformed token handling.
  - `config_test.dart`: `apiUrl` protocol validation, non-empty host validation, trailing slash normalization.
  - `api_models_test.dart`: DTO JSON serialization/deserialization for login, procurement requests, vendors, and approval workflows.

### C. Live Backend API Integration
The deployed Railway backend was verified against live endpoints using authorized test accounts:
* `GET /health` $\rightarrow$ `200 OK` (`Healthy`)
* `POST /api/auth/login` (Employee) $\rightarrow$ `200 OK` (Valid JWT, claims: `sub`, `role: EMPLOYEE`)
* `POST /api/auth/login` (Procurement Officer) $\rightarrow$ `200 OK` (Valid JWT, claims: `sub`, `role: PROCUREMENT_OFFICER`)
* `POST /api/auth/login` (Bad credentials) $\rightarrow$ `401 Unauthorized`
* `GET /api/procurement-requests` $\rightarrow$ `200 OK`
* `POST /api/procurement-requests` (Valid requisition) $\rightarrow$ `201 Created`
* `POST /api/procurement-requests/{id}/submit` $\rightarrow$ `200 OK` (Status updated to `SUBMITTED`)
* `GET /api/vendors` $\rightarrow$ `200 OK` (11 active vendors retrieved)
* Role-based access control (RBAC) enforcement verified (`403 Forbidden` for unauthorized roles)

---

## 4. Known Testing Limitations & Operational Notes

1. **Physical Device / Emulator Execution:**
   * **Status:** **Outstanding / Unverified on physical hardware.**
   * *Detail:* No physical Android device or running emulator was connected during the build audit session (`adb devices` reported 0 attached devices). While all client-side logic and automated tests passed, physical-device installation and on-device manual smoke testing must be performed by the submission team prior to final project submission.

2. **DEF-APP-002 (Server-Side Past Date Persistence):**
   * *Detail:* The deployed backend does not enforce a server-side `[FutureDate]` model validation attribute on `RequiredByDate`. Raw HTTP requests can persist past dates.
   * *Mitigation in APK:* The mobile application uses the native Material `showDatePicker` configured with `firstDate: DateTime.now()`, preventing mobile users from selecting past dates in the UI.

3. **Empty User Object in Auth Response Body:**
   * *Detail:* The backend `/api/auth/login` endpoint returns `user: {}` (empty JSON object).
   * *Mitigation in APK:* `AuthService.decodeToken` directly parses user identity and role from the cryptographically signed JWT payload, ensuring user roles are correctly identified on the client.

4. **Cloud Container Cold Starts:**
   * *Detail:* The backend is hosted on a Railway container. After periods of inactivity, the container may take 15–30 seconds to spin up on the initial incoming request.

5. **Swagger UI Availability:**
   * *Detail:* In the production deployment, Swagger UI (`/swagger`) is disabled (`app.Environment.IsDevelopment()`). The `/health` endpoint is publicly accessible.
