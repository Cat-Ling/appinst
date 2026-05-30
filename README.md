# appinst (App Installer & Uninstaller)
###### A command-line utility to install IPA packages and uninstall apps for iOS 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, and 18.

`appinst` is a lightweight, low-level command-line utility designed for iOS devices to install decrypted `.ipa` packages and cleanly uninstall existing applications by their bundle identifier. It supports both **rootful** (`iphoneos-arm`) and modern **rootless** (`iphoneos-arm64`) jailbreak architectures.

---

## Features
- **App Installation**: Easily install ad-hoc signed, fakesigned, unsigned, or expired apps directly from the terminal (AppSync Unified or rootless hooks required to bypass signatures).
- **App Uninstallation**: Perform clean uninstallation of user applications (including group containers, sandbox caches, and plugin extensions) matching the native system behavior.
- **Multi-Firmware Support**: Leverages private system APIs (`LSApplicationWorkspace` on iOS 8+ and `MobileInstallation` on iOS 5–7) to execute tasks safely.
- **Fully Rootless Compliant**: Supports dynamic jailbreak directory structures using `rootless.h` and the modern `iphoneos-arm64` package format.

---

## Usage

### 1. Install an App
Pass the path to any local `.ipa` file:
```shell
appinst <path to IPA file>
```
*Example:* `appinst /var/mobile/Downloads/Terminus.ipa`

### 2. Uninstall an App
Pass the `-u` or `--uninstall` flag followed by the target application's bundle identifier:
```shell
appinst -u <bundle identifier>
# OR
appinst --uninstall <bundle identifier>
```
*Example:* `appinst -u com.example.myawesomeapp`

### 3. Help Screen
Show usage information:
```shell
appinst -h
# OR
appinst --help
```

---

## How to Build

First, make sure you have [Theos](https://github.com/theos/theos) installed. If you don't, [please refer to the official documentation](https://theos.dev/docs/installation) on how to set up Theos on your operating system of choice.

Once you have Theos installed, clone this repository and acquire a copy of `libzip.a` to statically link against:

1. Download the `libzip` package (e.g. version 0.11.2 from the BigBoss repository, or build it yourself).
2. Locate the static library file `libzip.a` and copy it to `$THEOS/libs/`.
3. Open a terminal in the root of the project directory and build the package.

### Build for Rootful Jailbreaks:
```shell
make clean package
```
*Outputs: `debs/ai.akemi.appinst_<version>_iphoneos-arm.deb`*

### Build for Rootless Jailbreaks (iOS 15+):
```shell
make clean package THEOS_PACKAGE_SCHEME=rootless FINALPACKAGE=1
```
*Outputs: `debs/ai.akemi.appinst_<version>_iphoneos-arm64.deb`*

---

## Automated GitHub Workflows (CI)
This repository includes a pre-configured GitHub Actions CI workflow under `.github/workflows/build.yml` which automatically:
- Compiles the tool for rootless devices on every commit to `master`/`main`.
- Automatically publishes the generated rootless `.deb` package to a new GitHub Release whenever a release tag (`v*`) is pushed.

---

## License
Licensed under [GPLv3](http://www.gnu.org/copyleft/gpl.html).
