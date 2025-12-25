# Flutter Demo App - GitHub Actions AAB Build

A simple Flutter application configured to build a signed Android App Bundle (AAB) automatically using GitHub Actions.

## 🚀 Features

- Simple counter app with beautiful Material 3 design
- Automatic signed AAB build on every push
- GitHub Actions workflow ready to use

## 📁 Project Structure

```
├── .github/
│   └── workflows/
│       └── build-aab.yml    # GitHub Actions workflow
├── android/                  # Android configuration
├── lib/
│   └── main.dart            # Flutter app code
├── pubspec.yaml             # Flutter dependencies
└── README.md
```

## 🔐 Setup GitHub Secrets

### Step 1: Generate a Keystore (if you don't have one)

```bash
keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
```

### Step 2: Encode Your Keystore to Base64

**On Linux/macOS:**
```bash
base64 -i my-release-key.jks | tr -d '\n' > keystore_base64.txt
```

**On Windows (PowerShell):**
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("my-release-key.jks")) | Out-File -FilePath keystore_base64.txt -NoNewline
```

### Step 3: Add Secrets to GitHub

Go to your repository on GitHub:
1. Click **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Add these 4 secrets:

| Secret Name | Description |
|-------------|-------------|
| `KEYSTORE_BASE64` | Contents of `keystore_base64.txt` |
| `KEYSTORE_PASSWORD` | The password you set for the keystore |
| `KEY_ALIAS` | The alias you used (e.g., `my-key-alias`) |
| `KEY_PASSWORD` | The password for the key (often same as keystore password) |

## 🏗️ Available Workflows

### 1. Debug APK (for testing)
**File:** `.github/workflows/build-debug-apk.yml`

- ✅ No signing required
- ✅ Quick build for testing on your phone
- ✅ Download `debug-apk` artifact

### 2. Signed AAB (for Play Store)
**File:** `.github/workflows/build-aab.yml`

- ✅ Production-ready signed bundle
- ✅ Requires GitHub Secrets setup
- ✅ Download `signed-aab` artifact

### Build Triggers
Both workflows run when:
- ✅ Push to `main` or `master` branch
- ✅ Pull request to `main` or `master` branch
- ✅ Manual trigger from **Actions** tab

## 📦 Download Your AAB

1. Go to the **Actions** tab in your repository
2. Click on the latest workflow run
3. Scroll down to **Artifacts**
4. Download `signed-aab`

## 🛠️ Local Development

### Prerequisites
- Flutter SDK 3.24.0 or later
- Android Studio / VS Code with Flutter extension
- JDK 17

### First-time Setup
After cloning, run this command to generate required native files (icons, etc.):
```bash
flutter create . --org com.example
flutter pub get
```

### Run the app
```bash
flutter run
```

### Build AAB locally
```bash
flutter build appbundle --release
```

## 🎨 App Preview

The demo app features:
- Modern Material 3 design
- Light and dark theme support
- Smooth animations
- Counter functionality

## ⚙️ Customization

### Change App Name
Edit `android/app/src/main/AndroidManifest.xml`:
```xml
android:label="Your App Name"
```

### Change Package Name
1. Update `android/app/build.gradle`:
   ```gradle
   applicationId "com.yourcompany.yourapp"
   namespace "com.yourcompany.yourapp"
   ```
2. Rename the directory `android/app/src/main/kotlin/com/example/flutter_demo_app/` to match your package structure

### Change Flutter Version
Edit `.github/workflows/build-aab.yml`:
```yaml
flutter-version: '3.24.0'  # Change to desired version
```

## 🔒 Security Notes

⚠️ **Important:**
- Never commit your keystore (`.jks`) file to the repository
- Never commit `key.properties` with real passwords
- Always use GitHub Secrets for sensitive data
- The `.gitignore` is configured to exclude sensitive files

## 📄 License

This project is provided as-is for demonstration purposes.
