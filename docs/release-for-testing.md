# Steps to Release a New Version of the App to Testers

This document outlines the required steps to release a new version of the app to testers via **App Store Connect** (iOS) and **Google Play Console** (Android). The process varies depending on whether the release is for **internal testing** or **user acceptance testing (UAT)**.

---

## Case 1: Releasing a New Version for Internal Testing

Internal testing is intended for limited groups within the development team and does not require review from Apple or Google.

### **App Store Connect (iOS)**

1. **Prepare the Build:**
    - Ensure the latest build is submitted to App Store Connect via `EAS Submit` for the `main` profile.

2. **Log in to App Store Connect:**
    - Go to [App Store Connect](https://appstoreconnect.apple.com/).
    - Navigate to **My Apps** and select your app.

3. **Assign the Build to Tester Groups:**
    - Go to the **TestFlight** tab.
    - Locate the newest build and navigate to the **Groups** section.
    - Select the tester groups (e.g., Internal testers) for the build.

4. **Notify Testers:**
    - Testers will receive an email notification or can check the TestFlight app for the new version.

---

### **Google Play Console (Android)**

1. **Prepare the Build:**
    - Ensure the latest build is uploaded to the **Internal Track** in Google Play Console via `EAS Submit` for the `main` profile. By default, builds are uploaded with `releaseStatus` set to `draft` in `eas.json`.

2. **Log in to Google Play Console:**
    - Go to [Google Play Console](https://play.google.com/console/).
    - Navigate to your app’s dashboard.

3. **Release the Draft:**
    - Go to **Testing** > **Internal Testing**.
    - Locate the draft release created by `EAS Submit`.
    - Review the release details and click **Release** to make the app available to testers.

> - **Important:** Always manually release draft builds in Google Play Console to avoid delays.

4. **Notify Testers:**
    - Share the testing link with testers or notify them directly via email.

---

## Case 2: Releasing a New Version for User Acceptance Testing (UAT)

Releasing for UAT requires review from Apple or Google. It is aimed at gathering feedback from a broader audience.

### **App Store Connect (iOS)**

1. **Prepare the Build:**
    - Submit the latest build to App Store Connect via `EAS Submit` for the `uat` profile.

2. **Log in to App Store Connect:**
    - Go to [App Store Connect](https://appstoreconnect.apple.com/).
    - Navigate to **My Apps** and select your app.

3. **Assign the Build to Tester Groups:**
    - Go to the **TestFlight** tab.
    - Locate the newest build and navigate to the **Groups** section.
    - Select the tester groups (e.g., First Wave Testers) for the build.

4. **Write a Note for Testers:**
    - Select the desired build and use the **Test Information** section in App Store Connect to write a note for testers. Include:
        - What to test (e.g., specific features, bug fixes).
        - What is new in this release.

> - **Important:** Writing a note for testers is a good practice to ensure they focus on testing the intended features and provide relevant feedback.

5. **Submit for Review:**
    - Apple requires a review for external testing. Once the tester groups are selected, click **Submit for Review**.
    - Wait for Apple to approve the build. This may take a few hours or up to a day.

6. **Notify Testers:**
    - Once approved, TestFlight will notify testers via email.

---

### **Google Play Console (Android)**

1. **Prepare the Build:**
    - Submit the latest build to the **Alpha Track** in Google Play Console via `EAS Submit` for the `uat` profile. By default, builds are uploaded with     `releaseStatus` set to `draft` in `eas.json`.

2. **Log in to Google Play Console:**
    - Go to [Google Play Console](https://play.google.com/console/).
    - Navigate to your app’s dashboard.

3. **Write a Note for Testers and Release the Draft for Review:**
    - Go to **Testing** > **Closed Testing**.
    - Locate the draft release created by `EAS Submit`.
    - Use the **Release Notes** section in Google Play Console to write a note for testers. Include:
        - What to test (e.g., specific scenarios, bug fixes).
        - What is new in this release.
    - Review the release details and click **Release** to send it for Google’s review.
    - Google will review the release. This process usually takes a few hours but may take longer.

> - **Important:** Writing a note for testers is a good practice to ensure they focus on testing the intended features and provide relevant feedback.
> - **Important:** Always manually release draft builds in Google Play Console to avoid delays.

4. **Notify Testers:**
    - Once approved, notify testers via email or share the testing link.

---

## Notes
- **Draft Releases on Google Play Console:** Drafts are created by default due to `releaseStatus` settings in `eas.json`. Always ensure that drafts are manually reviewed and released in the console to avoid delays.
- **Apple Review Times:** External testing builds on TestFlight typically require approval within 24 hours. Plan accordingly.
- **Google Review Times:** Closed testing builds on Google Play Console are generally reviewed faster but can vary.

---