# Guide to EAS Build, Submit, and Update Scripts

In this project, we use Expo Application Services (EAS) for building, submitting, and updating the app. Below are detailed instructions on how each script works and when to use them.

### Prerequisites

Before running the scripts, ensure:
- **Node.js** and **npm** are installed.
- You have an [**Expo**](https://expo.dev/signup) account, a [**Google Play Console**](https://play.google.com/console/signup) account, and an [**Apple Developer Program**](https://developer.apple.com/programs) account for app submission.
- Ensure that the necessary iOS and Android credentials and permissions are configured in your Expo account. [**More on app credentials management with Expo**](https://docs.expo.dev/app-signing/app-credentials/).

You can view and modify these scripts directly in the `package.json` file under the `scripts` section.

For further guidance on using EAS, visit the official Expo documentation here: [Expo Guide Overview](https://docs.expo.dev/guides/overview/).

> **Note:** The primary way to deploy our app is through **GitHub Actions**. All workflows should be triggered manually, ensuring that the relevant branch is selected before triggering the workflow. Below are the workflows and their respective purposes:
> - [**CD-HazardHunt**](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend/blob/main/.github/workflows/eas-build-submit-all.yml): Used for deploying to **internal testing** with the `main` branch.
> - [**CD-HazardHunt-UAT**](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend/blob/main/.github/workflows/eas-build-submit-all-uat.yml): Used for **user acceptance testing (UAT)** with the `uat` branch.
> - [**CD-HazardHunt-Production**](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend/blob/main/.github/workflows/eas-build-submit-all-prod.yml): Used for deploying the app to **live production** with the `production` branch.

---

## Build, Submit, and Update Scripts for EAS

This guide outlines how to build, submit, and update the app for different platforms (`iOS`, `Android`, or both) and environments (`main`, `uat`, `production`).

### Build vs. Submit Scripts
- **Build Scripts**: Compile the app into deployable binaries for internal testing, user acceptance testing (UAT), or production.
- **Submit Scripts**: Upload the build artifacts to app distribution platforms like the App Store or Google Play.

### Environments and Testing Stages
- **Main Profile:** Used for **internal testing** within the development team.
- **UAT Profile:** Used for **user acceptance testing**.
  - On Android, this is referred to as **closed testing**, which involves controlled beta testing on Google Play. For iOS, the term **external testing** applies and uses TestFlight.
- **Production Profile:** Used for **live releases** to the App Store and Google Play.

| Environment | Purpose                        | Android Term          | iOS Term              |
|-------------|--------------------------------|-----------------------|-----------------------|
| Main        | Internal testing               | Internal testing      | Internal testing      |
| UAT         | UAT                            | Closed Testing        | External Testing      |
| Production  | Live release to app stores     | Release               | Release               |

---

### Build Scripts

Use these scripts to build the app for:
- **Internal Testing** (main)
- **UAT** (uat): TestFlight External Testing (iOS), Google Play Closed Testing (Android)
- **Release** (production)
  
#### iOS:
```bash
npm run build:ios:main        # Internal testing
npm run build:ios:uat         # UAT
npm run build:ios:production  # Release
```

#### Android:
```bash
npm run build:android:main        # Internal testing
npm run build:android:uat         # UAT
npm run build:android:production  # Release
```

#### All Platforms:
```bash
npm run build:all:main        # Internal testing
npm run build:all:uat         # UAT
npm run build:all:production  # Release
```

---

### Submit Scripts

Use these scripts to submit the latest builds for:
- **Internal Testing** (main)
- **UAT** (uat): TestFlight External Testing (iOS), Google Play Closed Testing (Android)
- **Release** (production)

#### iOS:
```bash
npm run submit:ios:main        # Internal testing
npm run submit:ios:uat         # UAT
npm run submit:ios:production  # Release
```

#### Android:
```bash
npm run submit:android:main        # Internal testing
npm run submit:android:uat         # UAT
npm run submit:android:production  # Release
```

#### All Platforms:
```bash
npm run submit:all:main        # Internal testing
npm run submit:all:uat         # UAT
npm run submit:all:production  # Release
```

> **Important:** When using EAS Submit locally:
> - Ensure your `eas.json` file contains valid credentials:
>   - For iOS submissions, update `appleId`, `ascAppId`, and `appleTeamId`. These fields might be set to `"intentionally_left_blank"` for security reasons and should be updated with real values manually.
>   - For Android submissions, ensure the appropriate service account keys are configured.
> - **Avoid pushing real credentials** to GitHub; keep sensitive information secure and excluded from version control.

---

### Update Scripts (Over-the-Air Updates)

Use these scripts to push updates (e.g., UI tweaks, bug fixes) to users without requiring a full rebuild or re-submission for:
- **Internal Testing** (main)
- **UAT** (uat): TestFlight External Testing (iOS), Google Play Closed Testing (Android)
- **Release** (production)

```bash
npm run update:all:main -- --message "<Your update message>"        # Internal testing
npm run update:all:uat -- --message "<Your update message>"         # UAT
npm run update:all:production -- --message "<Your update message>"  # Release
```

Example:
```bash
npm run update:all:uat -- --message "Fixed alignment issue on login screen"
```

> **Notes:**
> - The `--message` flag is **mandatory** and should clearly describe the update.
> - Updates are applied when users reopen the app.
> - [**More on EAS Update**](https://docs.expo.dev/eas-update/how-it-works/).

---