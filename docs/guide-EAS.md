# Guide to EAS Build, Submit, and Update Scripts

In this project, we use Expo Application Services (EAS) for building, submitting, and updating the app. Below are detailed instructions on how each script works and when to use them.

## Prerequisites

Before running the scripts, ensure:
- **Node.js** and **npm** are installed.
- You have an [Expo](https://expo.dev/signup) account, a [Google Play Console](https://play.google.com/console/signup) account, and an [Apple Developer Program](https://developer.apple.com/programs) account for app submission.
- Ensure that the necessary iOS and Android credentials and permissions are configured in the project's Expo developer account. [More on app credentials management with Expo](https://docs.expo.dev/app-signing/app-credentials/).

You can view and modify these scripts directly in the `package.json` file under the `scripts` section.

For further guidance on using EAS, visit the official Expo documentation here: [Expo Guide Overview](https://docs.expo.dev/guides/overview/).

> **Note:** The primary way to deploy our app is through **GitHub Actions**. All workflows should be triggered manually, ensuring that the relevant branch is selected before triggering the workflow. [More on CI/CD workflows for this project](./ci-cd-workflows.md). Below are the workflows and their respective purposes:
> - [CD-HazardHunt](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend/blob/main/.github/workflows/eas-build-submit-all.yml): Used for deploying to **internal testing** with the `main` branch.
> - [CD-HazardHunt-UAT](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend/blob/main/.github/workflows/eas-build-submit-all-uat.yml): Used for **user acceptance testing (UAT)** with the `uat` branch.
> - [CD-HazardHunt-Production](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend/blob/main/.github/workflows/eas-build-submit-all-prod.yml): Used for deploying the app to **live production** with the `production` branch.

### Environment Variables

The environment variable `EXPO_PUBLIC_EAS_PROJECT_ID` is required for both **EAS Build** and **EAS Update** jobs. Ensure this variable is correctly set in your `.env` file, as it is used in the `app.config.js` file for critical configurations.

Here’s how `EAS_PROJECT_ID` is utilized in `app.config.js`:

```javascript
import 'dotenv/config';

export default {
  updates: {
    url: `https://u.expo.dev/${process.env.EXPO_PUBLIC_EAS_PROJECT_ID}`
  },
  extra: {
    eas: {
      projectId: process.env.EXPO_PUBLIC_EAS_PROJECT_ID
    }
  }
};
```
For the full app.config.js file, see [app.config.js in the repository](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend/blob/main/TTS/app.config.js).

> **Summary:** The `EXPO_PUBLIC_EAS_PROJECT_ID` environment variable is required for:
> - **Over-the-Air Updates (OTA)**: The `updates.url` field in `app.config.js` relies on this variable to connect to the correct project in Expo.
> - **EAS Build Jobs**: The `extra.eas.projectId` ensures proper association with the Expo project during builds.

For detailed instructions on handling environment variables for this project, refer to the [Environment Variables Guide](./handling-environment-variables.md). 

---

## Build, Submit, and Update Scripts for EAS

This guide outlines how to build, submit, and update the app for different platforms (`iOS`, `Android`, or both) and environments (`main`, `uat`, `production`).

### Build vs. Submit Scripts
- **Build Scripts**: Compile the app into deployable binaries for internal testing, user acceptance testing (UAT), or production.
- **Submit Scripts**: Upload the build artifacts to app distribution platforms like the App Store or Google Play Store.

### Environments and Testing Stages
- **Main Profile:** Used for **internal testing** within the development team.
- **UAT Profile:** Used for **user acceptance testing**.
  - On Android, this is referred to as **closed testing**, which involves controlled beta testing on Google Play Console. For iOS, the term **external testing** applies and uses TestFlight.
- **Production Profile:** Used for **live releases** to the App Store and Google Play Store.

| Environment | Purpose                        | Android Term          | iOS Term              |
|-------------|--------------------------------|-----------------------|-----------------------|
| Main        | Internal testing               | Internal testing      | Internal testing      |
| UAT         | UAT                            | Closed Testing        | External Testing      |
| Production  | Live release to app stores     | Release               | Release               |

---

### Build Scripts

Use these scripts to build the app for:
- **Internal Testing** (main)
- **UAT** (uat): TestFlight External Testing (iOS), Google Play Console Closed Testing (Android)
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
> **Note:** When running a `build` script from the terminal, you might encounter the prompt:  
> _"Do you want to log in to your Apple account?"_  
> For this project, the necessary credentials for iOS builds are already preconfigured. The correct response is **"No"** to proceed without logging in manually. [Learn more about credentials management](https://docs.expo.dev/app-signing/app-credentials/).

---

### Submit Scripts

Use these scripts to submit the latest builds for:
- **Internal Testing** (main)
- **UAT** (uat): TestFlight External Testing (iOS), Google Play Console Closed Testing (Android)
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
>   - For iOS submissions:
>     - `appleId`: Use the Apple ID of the developer account responsible for submitting the app. If you are unsure which account to use, check with your team lead or the owner of the Apple Developer Program subscription for the project.
>     - `ascAppId`: Refer to the [instructions for finding `ascAppId`](#how-to-find-values-for-ascappid-and-appleteamid).
>     - `appleTeamId`: Refer to the [instructions for finding `appleTeamId`](#how-to-find-values-for-ascappid-and-appleteamid).
>   - For Android submissions, configuration is managed through the **Expo developer account**, so ensure the necessary service account keys are set up there.
> - **Avoid pushing real credentials** to GitHub; keep sensitive information secure and excluded from version control.

#### How to Find Values for `ascAppId` and `appleTeamId`

New developers can locate the values for **`ascAppId`** (App Store Connect App ID) and **`appleTeamId`** (Apple Developer Team ID) by following these steps:

##### Finding `ascAppId`
1. Log in to [App Store Connect](https://appstoreconnect.apple.com/) using the Apple Developer account credentials.
2. Navigate to the **My Apps** section.
3. Select the app associated with the project.
4. In the app details page, locate the **Apple ID** under the app's **General Information**.  
   This **Apple ID** is the value for `ascAppId`.

##### Finding `appleTeamId`
1. Log in to the [Apple Developer Portal](https://developer.apple.com/account/) using the Apple Developer account credentials.
2. Navigate to the **Membership** section.
3. Locate the **Team ID** under the **Team Information**.  
   This **Team ID** is the value for `appleTeamId`.

---

### Update Scripts (Over-the-Air Updates)

Use these scripts to push updates (e.g., UI tweaks, bug fixes) to users without requiring a full rebuild or re-submission for:
- **Internal Testing** (main)
- **UAT** (uat): TestFlight External Testing (iOS), Google Play Console Closed Testing (Android)
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
> - [More on EAS Update](https://docs.expo.dev/eas-update/how-it-works/).

---

## Profiles, Channels, and Tracks in EAS

In **EAS**, profiles, channels, and tracks are critical concepts for managing builds, updates, and submissions. Here's a breakdown of how they work in the context of our `eas.json` configuration:

### Profiles
Profiles in the `eas.json` file define the build and submission settings for different environments:
- **Main Profile:** Used for internal testing, connected to the `main` branch.
- **UAT Profile:** Used for user acceptance testing (UAT), connected to the `uat` branch.
- **Production Profile:** Used for live releases, connected to the `production` branch.

Each profile specifies:
- **Build Settings:** Defined under the `build` section, including auto-incrementing version numbers and the channel to use for updates.
- **Submission Settings:** Defined under the `submit` section, including platform-specific parameters for App Store Connect and Google Play Console.

### Channels
Channels are used in **EAS Update** to determine which builds are linked to specific over-the-air (OTA) update channels:
- **Main Channel:** Receives OTA updates from the `main` branch.
- **UAT Channel:** Receives OTA updates from the `uat` branch for testing.
- **Production Channel:** Receives OTA updates from the `production` branch for live users.

For example, in the `eas.json` configuration:
```json
"main": {
  "autoIncrement": true,
  "channel": "main"
}
```
This specifies that builds for the main profile will publish updates to the main channel.

### Tracks (Android-Specific)

Tracks define how your app is distributed in the Google Play Console:

- **Internal Track:** Used for internal testing; apps are available to a limited group of testers usually within the developer team.
- **Alpha Track:** Used for UAT; apps are distributed to a larger group of testers for user feedback.
- **Production Track:** Used for live releases; apps are made available to all users on Google Play Store.

For example:
```json
"main": {
  "android": {
    "track": "internal"
  }
}
```
This means the **`main`** profile's Android build will be uploaded to the **Internal Track** for testing.

> **Important:** Ensure alignment between `eas.json` profiles and the corresponding GitHub Actions workflows to avoid deployment mismatches. The `eas.json` file is dynamically generated during GitHub Actions workflows, utilizing **GitHub secrets** for secure configuration. 
> For iOS submissions, critical fields such as:
> - **`appleId`**
> - **`ascAppId`**
> - **`appleTeamId`**
> 
> are populated using these secrets. **New developers must ensure that the GitHub secret for `appleId` is updated with a valid value**. 
>
> ⚠️ **Note:** The current `appleId` value will be removed when the current developer group transitions the project. Without this update, iOS builds and submissions will fail.
>
> Proper alignment ensures that the correct profile, channel, and track configurations are applied to each build and submission stage. Misalignment may lead to deploying updates to unintended environments or audiences.

[More on GitHub secrets for this project](./ci-cd-workflows.md).

### Summary of Profiles, Channels, and Tracks

| Profile        | Channel       | Android Track       | Purpose                             |
|----------------|---------------|---------------------|-------------------------------------|
| **Main**       | `main`        | `internal`          | Internal testing within the team.   |
| **UAT**        | `uat`         | `alpha`             | User acceptance testing (UAT).      |
| **Production** | `production`  | `production`        | Live release to all users.          |

Properly configuring the `eas.json` file ensures streamlined workflows for building, updating, and submitting your app, targeting the right audience at every stage of development. [More on configuring eas.json](https://docs.expo.dev/eas/json/).

---