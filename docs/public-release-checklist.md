# StreetView 2.0 release readiness

**Scope:** This checklist governs a **versioned public release with installable assets**, not whether the source repository is visible.

**Current state (17 November 2026):** The repository, GitHub Pages documentation/demo, and [v2.0.0-alpha.6 GitHub prerelease](https://github.com/markfishurupa-dot/streetview-v2/releases/tag/v2.0.0-alpha.6) are **public**. The published release includes the signed Android 1.3.0 APK and a matching SHA-256 digest. Portal source builds through Docker Compose; no approved GHCR container image has been published. A published prerelease does **not** close the outstanding device, data privacy, licensing and recovery acceptance gates.

## Verified or implemented

- [x] On 17 November 2026, the project owner reported testing all components, including the mobile app, and that they worked as expected. The report did not specify device models, operating system versions or the APK digest.
- [x] Android 1.3.0 source is present in `apps/android` with release signing material excluded.
- [x] Local Android debug build and GitHub Android debug-build workflow succeeded.
- [x] Docker-based portal and processing workflow are implemented; earlier Windows/Docker Desktop browser and GIS export tests passed.
- [x] Windows installer exercised in isolated Compose projects; repeated setup preserved generated credentials.
- [x] Bash installer exercised in a Linux CLI container with Docker Desktop (not a clean native-Linux host test).
- [x] Starlight documentation is built and GitHub Pages deployment has succeeded.
- [x] Static demo is available through GitHub Pages and navigation is covered by Playwright.
- [x] Actual portal screenshots were captured using synthetic test input.
- [x] Earlier targeted scans, including Gitleaks against reachable commits at that time, reported no secrets. **This does not close the complete privacy and redistribution audit.**

## APK candidate inspection (2026-11-17)

- [x] Owner-provided Android 1.3.0 APK has been inspected: matching package/version/SDK metadata, valid v2 signing signature and verified content digest; SHA-256 `e0538baafce11915f1a332180ab4adcfefb7bbb6be34921a18e34fcd0ebc3aa1`.
- [x] The signing certificate states `CN=Mark URUPA`, certificate SHA-256 `659d32a0437090e65c6a7e0c7e749615ebfae3c84c69d4cd39843b4eb1a0a741`.
- [x] The published GitHub prerelease contains `StreetView.apk` (13,406,645 bytes); GitHub reports SHA-256 `e0538baafce11915f1a332180ab4adcfefb7bbb6be34921a18e34fcd0ebc3aa1`, an exact match to the inspected APK.
- [ ] Independently tie the **exact APK SHA-256** to field-device testing and verify old-device signing upgrade compatibility.
- [x] Android SDK Build Tools 36.0.0 `apksigner` verified the downloaded APK's v2 signature and signer certificate; its SHA-256 matched the release asset digest. See [APK evidence](apk-v1.3.0-verification.md).

See [APK candidate evidence](apk-v1.3.0-verification.md) and [published prerelease notes](releases/v2.0.0-alpha.6.md).

## Blockers for a distributable release

- [x] **Android signed APK:** owner-supplied release APK published, SHA-256 and signing certificate checked, package/version metadata compared to repository source. Exact source commit provenance still requires confirmation.
- [x] **Owner functional acceptance:** the owner reports testing all components, including the mobile app, and considers the system stable.
- [ ] **Exact Android build evidence:** associate the owner's test with the published APK digest and record device/version details and update compatibility.
- [ ] **Android upgrade path:** verify whether the signed APK upgrades existing installations without certificate or package-ID conflicts.
- [ ] **Demo imagery and route:** review the public **real** 183-photo survey for faces, plates, location sensitivity, permissions and redistribution terms. The [demo dataset license](../site/public/demo/DATASET_LICENSE.md) does **not** currently grant general reuse rights.
- [ ] **Clean install and recovery:** validate advertised platforms and demonstrate backup/restore of both PostGIS and media volumes to a separate environment.
- [ ] **Distribution license review:** audit all relevant container/base-image, FFmpeg, PostGIS, Python, Android and other transitive obligations and notices.
- [ ] **Security/privacy audit:** scan all reachable Git history, CI artifacts and container layers for credentials and private media; document findings and remedies.
- [ ] **Branding:** resolve product-name/trademark considerations before promoting a versioned release.
- [x] **Public Android release asset:** `v2.0.0-alpha.6` is published as a prerelease with an uploaded, digest-matched signed APK.
- [ ] **Docker distribution:** prebuilt versioned GHCR images remain to be reviewed, published and verified; the source-build Compose installation is available now.
- [ ] **Final quality gate:** test documentation links, mobile/desktop demo, accessibility, Docker instructions, fresh install and upgrade instructions.
- [ ] **Owner release sign-off:** approve the exact release commit, APK checksum, container digests and publishing actions.

## Important distinction: publication vs product release

- **Source code:** public now, under AGPL-3.0-only.
- **Documentation/demo:** public now; the demo uses real survey images and coordinates, so data privacy and reuse rights still require explicit confirmation.
- **Portal:** can be built and run locally using the repository's Docker Compose file. Its default API binding is loopback-only.
- **Android:** signed Android 1.3.0 APK is downloadable from the official prerelease; source and debug-build capability remain available. A device-test report tied to the exact published checksum is still outstanding.
- **Optional ML/YOLO:** not part of the current public portal release.

For the exact Android verification steps, see [Android APK release verification](android-release-verification.md).

## Documentation maintenance notes

The earlier release-preparation records describe a **synthetic** twelve-photo demo and disabled GitHub Pages workflow. Those were historical milestones; they do not describe the currently published real-photo demo or active Pages workflow. Avoid carrying their status forward as current acceptance evidence.

A clean native-Linux host test remains outstanding. The prior Windows Docker tests and Linux CLI-container tests should not be described as full cross-platform certification.

The repository's software license does not automatically license the demonstration photographs, GPS route, third-party map tiles or Android release signing materials.
