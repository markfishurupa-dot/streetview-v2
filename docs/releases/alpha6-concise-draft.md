# StreetView 2.0 - Release v2.0.0-alpha.6

Turn road survey videos and GPS tracks into interactive maps and GIS-ready imagery.

### Get started

1. **Android application:** [Download the signed Android 1.3.0 APK](https://github.com/markfishurupa-dot/streetview-v2/releases/download/v2.0.0-alpha.6/StreetView.apk) and follow the [installation and recording guide](https://markfishurupa-dot.github.io/streetview-v2/start/android/).
2. **Processing portal:** Follow the [Docker installation guide](https://markfishurupa-dot.github.io/streetview-v2/start/install/).
3. **First survey:** Follow the [illustrated walkthrough](https://markfishurupa-dot.github.io/streetview-v2/guide/first-survey/).
4. **Interactive demonstration:** [Explore a real survey](https://markfishurupa-dot.github.io/streetview-v2/demo/).

### What's included

Record video and GPS on Android, validate and process ZIP or MP4/LOG inputs, navigate georeferenced photographs, correct points and bearings, export GIS files, and publish read-only maps. Multiple recordings can be combined in one survey.

### System requirements

Android 8.0 or newer is required for the capture app. The portal has been exercised on Windows Docker Desktop with Linux amd64 containers. The Bash installer has been exercised in a Linux CLI container using Docker Desktop; a clean native Linux host test remains outstanding.

### Known limitations

This is an alpha with one administrator and one worker. The portal binds to localhost by default; network access requires separate configuration. Survey deletion is soft and retains source files. Exact-APK device testing, backup/restore, native Linux installation, container distribution review and real-demo data rights remain open. See the [release readiness checklist](https://github.com/markfishurupa-dot/streetview-v2/blob/codex/alpha-implementation/docs/public-release-checklist.md).

### Security and verification

The APK SHA-256 is `e0538baafce11915f1a332180ab4adcfefb7bbb6be34921a18e34fcd0ebc3aa1`. See the [signed APK verification record](https://github.com/markfishurupa-dot/streetview-v2/blob/codex/alpha-implementation/docs/apk-v1.3.0-verification.md).

### License

The software is licensed under [AGPL-3.0-only](https://github.com/markfishurupa-dot/streetview-v2/blob/codex/alpha-implementation/LICENSE). See the [third-party notices](https://github.com/markfishurupa-dot/streetview-v2/blob/codex/alpha-implementation/THIRD_PARTY_NOTICES.md). Survey media and data have separate rights.
