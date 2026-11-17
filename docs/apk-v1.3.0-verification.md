# StreetView Android 1.3.0 - Uploaded APK verification

Audit date: **2026-11-17**  
File supplied by project owner: `StreetView.apk`  
Published GitHub release asset filename: `StreetView.apk`

The APK is **not stored in the Git source tree**. It is distributed as an [official GitHub prerelease asset](https://github.com/markfishurupa-dot/streetview-v2/releases/download/v2.0.0-alpha.6/StreetView.apk). GitHub reports an SHA-256 digest matching the independently inspected binary below. The APK was checked against the Android project configuration in `apps/android`.

## Verified APK identity

| Field | Observed APK |
| --- | --- |
| SHA-256 | `e0538baafce11915f1a332180ab4adcfefb7bbb6be34921a18e34fcd0ebc3aa1` |
| File size | 13,406,645 bytes |
| Android package | `com.example.streetview` |
| Version name | `1.3.0` |
| Version code | `130` |
| Minimum Android SDK | API 26 (Android 8.0) |
| Target Android SDK | API 36 |
| Signing certificate subject | `CN=Mark URUPA` |
| Signing certificate SHA-256 | `659d32a0437090e65c6a7e0c7e749615ebfae3c84c69d4cd39843b4eb1a0a741` |
| Certificate valid from | 2026-02-26 18:58:48 UTC |
| Certificate valid until | 2051-02-20 18:58:48 UTC |
| APK signing | APK Signature Scheme v2, RSA PKCS#1 v1.5/SHA-256 |
| Cryptographic checks | Signer public key matches signing certificate; signature verifies; signed APK content digest matches |
| APK ZIP structure | ZIP integrity test passed |

Manifest metadata **agrees** with `apps/android/app/build.gradle.kts` on the package ID, version, minimum SDK and target SDK.

**Caution:** Matching version metadata and a valid signature do **not** prove byte-for-byte reproducibility from the exact GitHub source revision. A separately installed device version may also use a different signing certificate.

## Permissions reported by the APK

The decoded Android manifest declares camera, precise location, foreground services (camera/location/data sync), notifications, wake lock, network state, boot completed and a package-specific receiver permission. These are consistent in type with a camera/location recording app, but were not independently functionally tested in this audit.

The manifest's application element sets `allowBackup=true`, and the checked-in `data_extraction_rules.xml` contains a template with no explicit exclusions. **Assess backup behavior for survey files and personal location data before promising data will remain only on the device.** Avoid changing signing or package identity merely to address that issue: any code change creates a new APK that requires its own release verification and field tests.

## What this check does not establish

- No physical Android-device recording or installation was performed in this environment.
- We did not receive a device-test log proving that **this exact SHA-256 build** is the one used during field surveys.
- No verification was made against the signature of a previous installed release, so in-place update compatibility is not established.
- On 2026-11-17, the published asset was downloaded again. Android SDK Build Tools 36.0.0 `apksigner verify --verbose --print-certs` reported a valid v2 signature, one signer and certificate SHA-256 `659d32a0437090e65c6a7e0c7e749615ebfae3c84c69d4cd39843b4eb1a0a741`. `aapt dump badging` confirmed package `com.example.streetview`, version 1.3.0, version code 130, minimum SDK 26 and target SDK 36. The downloaded file's SHA-256 matched the published digest.
- No malware/AV certification or full static application security audit was performed.

## Release verification commands

After obtaining the candidate APK on a machine with Android SDK Build Tools:

```sh
apksigner verify --verbose --print-certs StreetView.apk
```

On Windows, compute the checksum:

```powershell
Get-FileHash -Algorithm SHA256 .\StreetView.apk
```

On Linux:

```sh
sha256sum StreetView.apk
```

Compare the result to `e0538baafce11915f1a332180ab4adcfefb7bbb6be34921a18e34fcd0ebc3aa1` and confirm the same signing certificate fingerprint is expected for upgrade users.

## Distribution conditions

The owner uploaded the signed APK to the **published** `v2.0.0-alpha.6` prerelease. GitHub reports its size as 13,406,645 bytes and its SHA-256 digest as an exact match to the independently inspected candidate. This verifies the uploaded asset identity, **not** which source commit produced it or whether the exact binary was tested on a physical Android device. Document field-test evidence and previously installed APK signing compatibility before making those claims.

Do **not** publish the keystore or signing passwords. Do not use the CI debug APK as a replacement for this signed candidate.

See [the Android APK release procedure](android-release-verification.md) and [the public-release checklist](public-release-checklist.md).
