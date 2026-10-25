<div align="center">

# 🔐 KEYSTONE

**A lightweight, offline password manager for Android.**

![Platform](https://img.shields.io/badge/platform-Android%207.0%2B-3DDC84?logo=android&logoColor=white)
![Language](https://img.shields.io/badge/Kotlin-WebView-7F52FF?logo=kotlin&logoColor=white)
![Version](https://img.shields.io/badge/version-1.0-blue)
![Offline](https://img.shields.io/badge/network-100%25%20offline-success)
![Status](https://img.shields.io/badge/status-early%20release-orange)

[⬇️ Download APK](../../releases/latest) · [🐞 Report a bug](../../issues) · [💡 Request a feature](../../issues)

</div>

---

> ⚠️ **Early release (v1.0).** KEYSTONE is a learning project. The vault is **not yet encrypted at rest** (see [Known limitations](#-known-limitations)). Please don't store high-value passwords (banking, primary email) until encryption lands in v2.

---

## 📖 Table of contents

- [About](#-about)
- [Screenshots](#-screenshots)
- [Features](#-features)
- [Download & install](#-download--install)
- [How to use](#-how-to-use)
- [How it works](#-how-it-works)
- [Tech stack](#-tech-stack)
- [Project structure](#-project-structure)
- [Build from source](#-build-from-source)
- [Privacy](#-privacy)
- [Known limitations](#-known-limitations)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [Contact](#-contact)
- [License](#-license)

---

## 🧭 About

KEYSTONE is a simple password vault that lives entirely on your phone. There is no account, no cloud sync, no ads, and the app has **no internet permission**, so nothing it stores can leave your device through the app.

It was built to learn how a real Android app comes together end to end: a native Kotlin shell hosting a web-based UI, local storage, file import/export, theming, and app-icon design.

## 📸 Screenshots

<!-- Add your screenshots to a /screenshots folder, then uncomment: -->
<!--
| Lock screen | Vault | Add password | Dark mode |
|:-:|:-:|:-:|:-:|
| ![lock](screenshots/lock.png) | ![vault](screenshots/vault.png) | ![add](screenshots/add.png) | ![dark](screenshots/dark.png) |
-->

*Screenshots coming soon.*

## ✨ Features

| | Feature | Details |
|---|---|---|
| 🔑 | **Master password lock** | Set a master password on first launch. The app asks for it every time you open it. |
| 🔒 | **Auto-lock** | The vault locks again when you switch away from the app. |
| ➕ | **Add / edit / delete logins** | Name, category, optional username, and password, with a show/hide toggle. |
| 🗂️ | **Categories** | App, Website, Gmail, Other, with filter chips to narrow the list. |
| 🔍 | **Search** | Instantly filter your saved entries. |
| 🌗 | **Light & dark theme** | Toggle from the top bar. Your choice is remembered. |
| 💾 | **Backup & restore** | Export your entries as a JSON file and import them back. |
| 🔄 | **Change master password** | Available from the menu (requires your current password). |
| 📴 | **Fully offline** | No internet permission, no analytics, no tracking. |

## ⬇️ Download & install

### Option 1: Download the APK (easiest)

1. Go to the **[Releases page](../../releases/latest)** and download **`keystone.apk`**.
2. Open the file on your Android phone.
3. If Android blocks it, allow **"Install unknown apps"** for your browser or file manager when prompted.
4. Tap **Install**, then open **KEYSTONE**.

**Requirements:** Android 7.0 (API 24) or newer.

### Verify your download (optional)

Compare the file's SHA-256 checksum with the one below to make sure it wasn't altered:

```
3b3aec04c3810a35914cffef5600301a59dd449838ac2771a5af607132c7ed51  keystone.apk
```

Windows (PowerShell): `Get-FileHash keystone.apk -Algorithm SHA256`
macOS / Linux: `sha256sum keystone.apk`

> This is a **debug-signed** build intended for testing and evaluation. It is not distributed through the Play Store. Only install APKs from the official Releases page of this repository.

### Option 2: Run it in an emulator

1. Install [Android Studio](https://developer.android.com/studio) and create a virtual device (Device Manager → Create device).
2. Start the emulator and drag `keystone.apk` onto its window.

Or from a terminal:

```bash
adb install keystone.apk
```

## 📱 How to use

1. **First launch:** create a master password. **There is no recovery option**, so if you forget it, the only way back in is clearing the app's data, which deletes everything in the vault.
2. **Add a login:** tap the **+** button, fill in the name, pick a category, add your username (optional) and password, then tap **Add**.
3. **Find a login:** use the search bar or tap a category chip.
4. **Back up:** open the menu → **Backup** to save your entries as a JSON file. Keep this file somewhere safe, because it is **not encrypted**.
5. **Restore:** open the menu → **Restore** and pick a previously saved backup file.
6. **Change theme:** tap the theme button in the top bar.

## ⚙️ How it works

```
┌────────────────────────────────────────────┐
│  Android app (Kotlin)                      │
│  MainActivity                              │
│   └─ WebView ──► file:///android_asset/    │
│                    index.html              │
│                    ├─ UI  (HTML + CSS)     │
│                    └─ Logic (vanilla JS)   │
│                                            │
│  Storage: WebView localStorage             │
│  Import: native file chooser (WebView)     │
└────────────────────────────────────────────┘
```

- The Kotlin `MainActivity` hosts a WebView with JavaScript and DOM storage enabled.
- The whole interface and app logic live in a single local file, `assets/index.html`. Nothing is loaded from the internet.
- The master password is stored as a **salted hash** and is used to unlock the UI.
- Vault entries are saved in the WebView's local storage on the device.
- Restore uses the native file chooser; Backup builds a JSON file from your entries.

## 🛠️ Tech stack

| Layer | Technology |
|---|---|
| Native shell | Kotlin, Jetpack Compose project template, Android WebView |
| UI & logic | HTML, CSS, vanilla JavaScript (single file, no frameworks) |
| Storage | WebView `localStorage` |
| Min / target SDK | 24 (Android 7.0) / 37 |
| Package name | `com.example.keystone` |

## 🗂️ Project structure

```
keystone/
├── app/
│   └── src/main/
│       ├── java/com/example/keystone/
│       │   └── MainActivity.kt        # WebView host, file chooser
│       ├── assets/
│       │   └── index.html             # Entire UI + app logic
│       ├── res/                       # Icons, themes, strings
│       └── AndroidManifest.xml
├── screenshots/                       # README images
├── build.gradle.kts
└── README.md
```

## 🏗️ Build from source

**Prerequisites:** [Android Studio](https://developer.android.com/studio) (latest stable) and JDK 17 or newer.

```bash
# 1. Clone
git clone https://github.com/<your-username>/keystone.git
cd keystone

# 2. Open the folder in Android Studio and let Gradle sync
#    (or build from the terminal:)
./gradlew assembleDebug        # macOS / Linux
gradlew.bat assembleDebug      # Windows
```

The debug APK is created at:

```
app/build/outputs/apk/debug/app-debug.apk
```

To run it: choose a device or emulator in Android Studio and press **Run ▶**.

## 🔏 Privacy

- **No internet permission.** The app cannot send data over the network.
- **No accounts, ads, analytics, or trackers.**
- **Data stays on your device**, in the app's private storage.
- Backup files are created only when **you** choose Backup, and are saved where **you** choose.

## ⚠️ Known limitations

Being upfront about where v1.0 stands:

- **The vault is not encrypted at rest.** Entries are saved as plain data in local storage. The master password protects access through the app's lock screen, but it does not encrypt the stored data. Someone with deep access to the device's app data could read it.
- **Backup files are unencrypted JSON.** Store them carefully and delete them when no longer needed.
- **Debug build.** The APK is debug-signed and marked debuggable, with Android backup allowed. A hardened, signed release build is planned.
- **Backup export is still being hardened.** File export from inside a WebView can behave differently across devices. If Backup does not produce a file on your phone, please [open an issue](../../issues) with your device and Android version.
- **No master password recovery.** This is by design for now, but it means a forgotten password equals a wiped vault.
- **No biometric unlock yet.**

## 🗺️ Roadmap

- [ ] 🔐 Encrypt the vault using a key derived from the master password (PBKDF2 + AES-GCM via Web Crypto)
- [ ] 📦 Encrypted backup and restore
- [ ] 💾 Reliable native file save for Backup
- [ ] 👆 Biometric unlock (fingerprint / face)
- [ ] 🎲 Built-in password generator
- [ ] 📋 Clipboard auto-clear after copying a password
- [ ] 🚫 Block screenshots inside the app (`FLAG_SECURE`)
- [ ] ✍️ Signed release build (and possible Play Store listing)
- [ ] 🌍 Multi-language support

## 🤝 Contributing

Contributions are welcome, and **security reviews are especially appreciated**.

1. Fork the repository
2. Create a branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "Add my feature"`
4. Push the branch: `git push origin feature/my-feature`
5. Open a Pull Request

Found a vulnerability? Please open a private security advisory (Repository → **Security** → **Report a vulnerability**) rather than a public issue.

## 📬 Contact

Want to try the app, collaborate, give feedback, or have a similar app built? **DM me** on LinkedIn: **[your LinkedIn link]**

## 📄 License

Released under the **MIT License**. See [`LICENSE`](LICENSE) for details.
*(Add a `LICENSE` file to the repo root. On GitHub: Add file → Create new file → name it `LICENSE` → click "Choose a license template" → MIT.)*

---

<div align="center">

Made with ☕ by **kart01k**

If you find this project useful, consider giving it a ⭐

</div>
