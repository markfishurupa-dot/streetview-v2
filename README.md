# StreetView 2.0

**Turn road survey videos and GPS tracks into interactive maps, georeferenced photos, and GIS-ready exports.**

Capture on Android → Upload → Validate → Process → Explore → Export.

![StreetView workspace showing a processed survey](site/public/images/screenshots/viewer.png)

*Actual StreetView portal screenshot captured with synthetic survey data. It is not a photograph of a real survey.*

StreetView is an **open-source, self-hosted street-imagery workflow** for road surveys, infrastructure documentation, project planning, and construction supervision. Survey data stays in your own Docker installation unless you deliberately expose a published map or share exports. This project is not affiliated with Google Street View.

**[Download Android APK](https://github.com/markfishurupa-dot/streetview-v2/releases/download/v2.0.0-alpha.6/StreetView.apk)** · **[Try the interactive demo](https://markfishurupa-dot.github.io/streetview-v2/demo/)** · **[Read the documentation](https://markfishurupa-dot.github.io/streetview-v2/)**

The demo is read-only. To upload videos and run processing, install the portal locally.

## What you can do

- **Capture on Android:** Record MP4 video and a GPS log, then share the matching files individually or together as a ZIP.
- **Import and validate:** Upload a ZIP or separate MP4/LOG files; inspect media integrity and GPS quality warnings.
- **Process:** Extract photos by GPS fix, time, or distance; preserve the video-to-location relationship.
- **Review and correct:** Navigate route photographs, adjust point coordinates and bearings, rotate images, and remove unwanted photo points.
- **Export:** Download photographs and GIS data including GeoPackage, GeoJSON, CSV, and a portable survey ZIP.
- **Publish a read-only map:** Select route/photo layers and control whether the published view requires administrator authentication.

Multiple recordings can be combined into one survey while retaining separate route segments. The portal uses FastAPI, a background worker, and PostGIS with persistent Docker volumes.

## Install the portal

**Tested deployment target:** Linux `amd64` containers through Docker Desktop. The project currently supports **one administrator and one processing worker**. A clean native-Linux installation and full backup/restore remain release verification items.

You need Git and Docker Desktop running **Linux containers** on Windows. From PowerShell:

```powershell
git clone https://github.com/markfishurupa-dot/streetview-v2.git
cd streetview-v2
.\setup.ps1
```

Open **http://127.0.0.1:18080/**. Sign in as `admin` using the `SV_ADMIN_PASSWORD` generated in your local, untracked `deploy/.env` file. Keep this file private and preserve it across restarts.

On Linux, the repository also includes `bash ./setup.sh`; native-Linux clean-install validation is still pending.

**[Full Windows/Linux installation guide](https://markfishurupa-dot.github.io/streetview-v2/start/install/)** includes troubleshooting, storage, and stop/restart instructions. The default installation binds to the host loopback interface, not the public Internet.

## Capture and process your first survey

1. **Android app:** Follow the [Android capture guide](https://markfishurupa-dot.github.io/streetview-v2/start/android/). The Android 1.3.0 **source** is available in [`apps/android`](apps/android). The signed Android 1.3.0 APK is available in the [v2.0.0-alpha.6 GitHub prerelease](https://github.com/markfishurupa-dot/streetview-v2/releases/tag/v2.0.0-alpha.6). **[Download StreetView.apk](https://github.com/markfishurupa-dot/streetview-v2/releases/download/v2.0.0-alpha.6/StreetView.apk)**, then follow the [Android installation and capture guide](https://markfishurupa-dot.github.io/streetview-v2/start/android/). Check the [verified SHA-256 digest](docs/apk-v1.3.0-verification.md) before installing.
2. **Record and transfer:** Capture a survey and share its ZIP, or the matching MP4 and LOG with identical basenames.
3. **Create and upload:** In the portal, create a survey, then upload the source files. The two files can also be uploaded at different times.
4. **Validate and process:** Review the GPS report; choose GPS-fix, time, or distance sampling; start processing.
5. **Explore and export:** Navigate georeferenced photos, correct points if necessary, and download `survey.zip` or individual GIS files.
6. **Publish optionally:** A published map is read-only. **Publicly available** means anyone who can reach your server can view the selected publication without signing in; it does *not* automatically expose localhost to the Internet.

Follow the [illustrated first-survey walkthrough](https://markfishurupa-dot.github.io/streetview-v2/guide/first-survey/) for the full process.

## Documentation and project status

| Resource | Purpose |
| --- | --- |
| [Live demo](https://markfishurupa-dot.github.io/streetview-v2/demo/) | Explore a sample route and photos without installation |
| [Installation](https://markfishurupa-dot.github.io/streetview-v2/start/install/) | Windows, Linux, credentials, Docker troubleshooting |
| [Android APK - 1.3.0](https://github.com/markfishurupa-dot/streetview-v2/releases/download/v2.0.0-alpha.6/StreetView.apk) | Download the signed Android capture app |
| [Android guide](https://markfishurupa-dot.github.io/streetview-v2/start/android/) | Install, record and export a survey |
| [Publishing](https://markfishurupa-dot.github.io/streetview-v2/guide/publishing/) | Read-only access, authentication, and network exposure |
| [Development guide](https://markfishurupa-dot.github.io/streetview-v2/reference/development/) | Local tests, API and architecture |
| [Release checklist](docs/public-release-checklist.md) | Verified features and remaining acceptance items |

**Current maturity:** The [v2.0.0-alpha.6 prerelease](https://github.com/markfishurupa-dot/streetview-v2/releases/tag/v2.0.0-alpha.6) includes a signed Android 1.3.0 APK; the asset checksum matches the independently inspected APK. It remains an alpha, not a stable release. No prebuilt GHCR portal image has been verified. A future optional ML/YOLO workflow is not part of this release.

**Privacy:** Survey photographs and GPS tracks can contain personal and sensitive location information. Obtain appropriate permission before recording and sharing. The [public demo dataset](site/public/demo/DATASET_LICENSE.md) has separate data-use terms from the software license.

## Contribute and license

See [CONTRIBUTING.md](CONTRIBUTING.md), [SECURITY.md](SECURITY.md), and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

Copyright © 2026 Mark Urupa. StreetView source is licensed under **[GNU AGPL-3.0-only](LICENSE)**. Third-party libraries, container components, and survey imagery retain their own terms.
