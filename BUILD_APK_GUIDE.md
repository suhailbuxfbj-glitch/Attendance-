# AttendPro - Build APK Guide

This guide will help you extract the source code and build an APK for the AttendPro Attendance Management Application.

## Prerequisites

Before starting, ensure you have installed:
- **Node.js** (v14 or higher) - [Download](https://nodejs.org/)
- **npm** or **yarn** (comes with Node.js)
- **Git** (for cloning this repository)
- **JDK** (Java Development Kit 11 or higher)
- **Android SDK** / **Android Studio** (for Android builds)

### Windows Users
1. Download and install [Android Studio](https://developer.android.com/studio)
2. During installation, ensure you select "Android SDK", "Android SDK Platform", and "Android Virtual Device"
3. Set ANDROID_HOME environment variable pointing to your SDK location

### Mac/Linux Users
```bash
# Install using Homebrew (Mac)
brew install openjdk@11
brew install android-sdk

# Set ANDROID_HOME in ~/.zshrc or ~/.bashrc
export ANDROID_HOME=~/Library/Android/sdk
```

## Step 1: Extract the Source Code

### Option A: Extract ZIP (Local Machine)
```bash
# Download the repository
git clone https://github.com/suhailbuxfbj-glitch/Attendance-.git
cd Attendance-

# Extract the ZIP file
unzip AttendPro-SourceCode.zip

# Navigate to the project
cd attendance-app
```

### Option B: Using Command Line
```bash
# Linux/Mac
unzip -q AttendPro-SourceCode.zip && cd attendance-app

# Windows (PowerShell)
Expand-Archive -Path AttendPro-SourceCode.zip -DestinationPath . ; cd attendance-app
```

## Step 2: Install Dependencies

```bash
# Install npm packages
npm install

# Verify installation
npm list
```

## Step 3: Configure Android Build

### Generate Android Project (if needed)
```bash
# Initialize Expo project for Android
npx expo prebuild --clean
```

## Step 4: Build APK

### Option A: Using Expo EAS Build (Recommended - Cloud)
```bash
# Install EAS CLI globally
npm install -g eas-cli

# Login to Expo account (or create free account)
eas login

# Build APK for Android
eas build --platform android --local

# Download the APK from the build link provided
```

### Option B: Build Locally (Without Cloud)
```bash
# Navigate to android directory
cd android

# Build release APK
./gradlew assembleRelease

# Build debug APK
./gradlew assembleDebug

# APK will be generated at:
# Release: android/app/build/outputs/apk/release/app-release.apk
# Debug: android/app/build/outputs/apk/debug/app-debug.apk
```

### Option C: Using React Native CLI
```bash
# Build and run on emulator/device
npx react-native run-android

# Build release APK
npx react-native run-android --variant=release
```

## Step 5: Install APK on Device

### Using ADB (Android Debug Bridge)
```bash
# Connect your Android device via USB (enable Developer Mode)

# List connected devices
adb devices

# Install APK
adb install path/to/app-release.apk

# Or install directly after building
adb install android/app/build/outputs/apk/release/app-release.apk
```

## Project Structure

```
attendance-app/
├── src/
│   ├── screens/          # Screen components
│   │   ├── AdminDashboard.js
│   │   ├── EmployeeDashboard.js
│   │   ├── AttendanceScreen.js
│   │   ├── ReportsScreen.js
│   │   ├── AdminControlScreen.js
│   │   ├── EmployeeListScreen.js
│   │   └── AuditLogScreen.js
│   ├── components/       # Reusable components
│   ├── utils/           # Utility functions
│   └── data/
│       └── AppContext.js # Global state management
├── App.js               # Main app entry point
├── app.json             # Expo config
├── package.json         # Dependencies
├── eas.json            # EAS Build config
└── README.md           # Project documentation
```

## Troubleshooting

### Issue: "ANDROID_HOME not set"
```bash
# Mac/Linux - Add to ~/.zshrc or ~/.bashrc
export ANDROID_HOME=~/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/platform-tools

# Windows - Set environment variable in System Settings
ANDROID_HOME = C:\Users\YourUsername\AppData\Local\Android\Sdk
```

### Issue: "Gradle sync failed"
```bash
# Clear gradle cache
cd android
./gradlew clean
cd ..
rm -rf node_modules package-lock.json
npm install
```

### Issue: "Metro bundler error"
```bash
# Clear Metro cache
npm start -- --reset-cache

# Or kill and restart
lsof -ti:8081 | xargs kill -9
npm start
```

### Issue: "Module not found"
```bash
# Reinstall dependencies
rm -rf node_modules
npm install
npm audit fix
```

## Configuration Files

### app.json (Expo Configuration)
Update before building:
```json
{
  "expo": {
    "name": "AttendPro",
    "slug": "attendpro",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "android": {
      "package": "com.attendpro.app",
      "versionCode": 1,
      "permissions": [
        "android.permission.INTERNET",
        "android.permission.CAMERA",
        "android.permission.ACCESS_FINE_LOCATION"
      ]
    }
  }
}
```

### package.json (Dependencies)
Key dependencies should include:
- react-native
- expo
- react-navigation
- async-storage
- etc.

## Next Steps

1. **Test on Emulator**: Use Android Studio's AVD Manager to create a virtual device
2. **Test on Real Device**: Enable USB debugging and connect your phone
3. **Generate Release Build**: Follow the steps above for production release
4. **Sign APK**: Create a keystore for signing release builds

## Support

For issues or questions:
- Check [Expo Documentation](https://docs.expo.dev/)
- Visit [React Native Docs](https://reactnative.dev/)
- Review Android Build [Troubleshooting Guide](https://developer.android.com/studio/troubleshoot)

---

**Happy Building! 🚀**
