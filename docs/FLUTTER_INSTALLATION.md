# Procura Mobile (Android) — APK Installation & Setup Guide

**Module:** Procura Mobile Application (Flutter)  
**Academic Module / Context:** SE3090 Software Testing & Quality Evaluation / Software Engineering  
**Release Artifact:** `app-release.apk`  
**Download Location:** [GitHub Releases — Procura Flutter APK](https://github.com/AlgoDove/flutter-ku-da/releases)  
**Direct Asset Download:** [app-release.apk](https://github.com/AlgoDove/flutter-ku-da/releases/download/v1.0.0-candidate/app-release.apk)  
**Production Backend URL:** `https://procura-production-98b4.up.railway.app`  

---

## 1. Overview & Requirements

* **Application:** Procura Mobile Android Client
* **Target OS:** Android 5.0 (API Level 21) or higher
* **Artifact File:** `app-release.apk` (~51.4 MB / 53,872,838 bytes)
* **Connectivity Requirement:** **Active Internet Connection Required**  
  The application does not use mock data or an offline local database; it communicates directly over HTTPS with the live deployed ASP.NET Core backend hosted on Railway (`https://procura-production-98b4.up.railway.app`). Wi-Fi or mobile data must be active during both authentication and workflow interactions.

---

## 2. Step-by-Step Installation Instructions

### Step 1: Download the APK
Download the release APK onto your Android device:
1. Navigate to the GitHub Release page:  
   `https://github.com/AlgoDove/flutter-ku-da/releases/tag/v1.0.0-candidate`
2. Download `app-release.apk` directly to your device's **Downloads** folder.
3. *Alternative (ADB Sideload):* If connecting via USB with developer options enabled:
   ```bash
   adb install -r app-release.apk
   ```

---

### Step 2: Allow Installation from Unknown Sources (Sideloading)
Because this APK is distributed directly rather than via Google Play Store, Android requires one-time permission to install apps from external sources:

1. Open your device's **Files** or **Downloads** app and tap `app-release.apk`.
2. When prompted with *"For your security, your phone is not allowed to install unknown apps from this source"*:
   * Tap **Settings** on the prompt.
   * Toggle **Allow from this source** to **ON**.
   * Tap the **Back** arrow to return to the installer.

---

### Step 3: Handle Google Play Protect Prompt
Because the APK is signed with the project release certificate rather than a Google Play Store commercial identity, Google Play Protect may display a prompt:

1. If a dialog appears stating *"Unrecognized app"* or *"Blocked by Play Protect"*:
   * Tap **More details** (or the small dropdown arrow).
   * Tap **Install anyway**.

---

### Step 4: Complete Installation & Launch
1. On the confirmation screen, tap **Install**.
2. Once the installation is finished, tap **Open**, or locate the **Procura** app icon in your device's app drawer.

---

## 3. Account Access & Login

* Procura requires role-based authentication to access procurement workflows.
* **Test Credentials:** Please use the official test accounts provided separately through the private submission document or team evaluation channel (e.g. Employee and Procurement Officer credentials).
* *Note on Security:* In accordance with software security best practices, passwords, private keys, and JWT tokens are not hardcoded or included in source documentation.

---

## 4. Troubleshooting Guide

### A. Installation Errors

| Issue | Likely Cause | Resolution |
| :--- | :--- | :--- |
| **"App not installed"** or **Signature conflict** | A previous debug or development build of Procura is already installed on the device with a different signing key. | Fully uninstall any existing Procura app from your device, restart or clear package installer cache, and install `app-release.apk` again. |
| **"Problem parsing package"** | Incomplete download or file corrupted during transfer. | Re-download `app-release.apk`. Ensure the downloaded file size matches the verified release size (~51.4 MB / 53,872,838 bytes). Confirm device runs Android 5.0+. |
| **"Blocked by Play Protect"** | Self-signed release certificate not registered with Google Play. | Tap **More details** $\rightarrow$ **Install anyway**. |
| **Insufficient Storage** | Less than 150 MB of available device memory. | Free up storage on the device and retry installation. |

### B. Login & Runtime Network Failures

| Issue | Likely Cause | Resolution |
| :--- | :--- | :--- |
| **"Connection Error" / "Network request failed"** | Device is offline, or outbound HTTPS is blocked. | Verify Wi-Fi / mobile data is turned on. Open `https://procura-production-98b4.up.railway.app/health` in your phone's browser to confirm server accessibility (should display `Healthy`). |
| **Extended delay on initial login (15–30s)** | Cloud container cold start (Railway dormant spin-up). | Dormant cloud containers may take 15–30 seconds to wake up on the first request after idle time. Wait 30 seconds and retry. |
| **"Invalid credentials" (HTTP 401)** | Incorrect email or password. | Ensure there are no accidental spaces before or after the email or password. Confirm the account exists in the deployed database. |
| **"Forbidden" (HTTP 403)** | Role restriction (RBAC). | The mobile app is primarily designed for the `EMPLOYEE` role to submit and manage procurement requisitions. Ensure you are logged in with the appropriate role account for the intended action. |
