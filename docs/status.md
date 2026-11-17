# Implementation status

## Final release audit in progress

On 17 November 2026, the owner reported testing all components, including the mobile app, and that everything worked as expected. Device details and the tested APK digest were not specified.

On 17 November 2026, the signed alpha.6 APK was downloaded from GitHub and its SHA-256 matched the release asset. Android SDK Build Tools 36.0.0 `apksigner` verified its v2 signature and signer certificate; `aapt` confirmed the package, version and SDK values. See [APK verification](apk-v1.3.0-verification.md). The current Python suite passed 11 tests with the private-sample test skipped; JavaScript syntax checks, the documentation build and six desktop/mobile Playwright checks passed. The [final audit](release-final-audit.md) records remaining privacy, device, installation, backup and distribution work. The next release should remain an alpha until those gates are met.

## Baseline, 2026-11-16

Private GitHub repository created and visibility verified. Local Windows isolated clone; production not touched. Python 3.14, FFmpeg and ffprobe available; Docker CLI present but Linux engine unavailable. Local ports 18080, 18081, 15432 and 16379 had no listeners when checked; recheck before use.

The original two-file Android archive passed CRC and prototype full-decode preflight: H.264 1920x1080, 237 frames, duration 7.8996 s, seven GPS fixes, two low-accuracy fixes and one jump warning. Another same-named archive contains two source pairs and is ambiguous. Validator is a prototype, not a production service. Nothing has been processed, packaged or deployed by this project.

Phase 0 gate incomplete: test harness and scanning not yet established. Phases 1–7 not implemented. Docker clean-host, browser, Android device and backup tests not run. Direct SSH to existing deployment denied.

## Preflight prototype baseline

Adapted the user-supplied standalone validator into `services/processor/preflight.py`; corrected its rejection of v1.3 pre-recording rows whose start anchor is -1. It remains a CLI prototype, not a secure upload service. `python -m unittest discover -s tests -v`: 3 passed (incomplete input, traversal, mismatched stems). Real original ZIP and loose files both returned READY with full decode and identical MP4/LOG SHA-256; 2 low-accuracy fixes and 1 jump were warnings. This does not establish output equivalence, processing, or a complete security test suite. Prototype rejects archives with extra candidate pairs; support for incomplete archives, staged uploads, resource isolation, detailed error contracts and CRC/bomb test coverage remains incomplete.

## GPS-fix processing slice

Added local CLI `services/processor/process.py` that validates a loose pair, reads decoded frame PTS, selects the nearest frame within 100 ms, verifies JPEG decode, and writes CSV, GeoJSON points/route, a minimal GeoPackage, QA/manifest and portable ZIP into a new run directory. Previous run directories are not overwritten. GPS pre-recording rows are skipped. It currently decodes once per photo and supports only GPS-fix sampling. This is not an upload service or restart-safe job worker. It does not yet preserve raw fixes in a separate artifact or validate nonzero video start-time alignment. GeoPackage interoperability beyond SQLite structure has not yet been tested.

Opt-in `tests/test_real_sample.py` safely extracts the original private sample, compares ZIP-derived and loose inputs and outputs, checks seven references/images and GeoPackage row count, verifies archive CRC and the final 7.8662 s PTS. With private sample environment variables set, `python -m unittest discover -s tests -v`: 4 tests passed. ZIP extraction in this test is internal; no upload path exists yet. No map, Docker install or publication test passed.

Docker Desktop was stopped; started the installed desktop process and `docker info` now reports server 29.9.2. No Compose stack has been deployed.

## Preflight robustness

Added regression checks for legitimate pre-recording rows and extreme timestamp values. The latter now returns a validation error instead of raising an uncaught exception. `python -m unittest discover -s tests -v` passed 5 synthetic tests, with the private sample test skipped when its environment variables were absent. Full-history limited-pattern scan found no matching secrets or prohibited file extensions; it is not a complete security or license audit. The remote remains private and the branch has not been pushed.

## Integrated private alpha

The Compose stack runs on Linux amd64 through Docker Desktop at http://127.0.0.1:18080/. The API serves the Leaflet frontend and authenticated media. PostGIS stores surveys, staged source metadata, durable jobs, geometries and artifact metadata. A single worker claims PostgreSQL jobs, writes to private media storage and can requeue interrupted runs. Uploads stream to disk and use SHA-256 for duplicate recognition. The two source modes share preflight and processing logic.

Verification performed:

- Nine Python tests passed with the private fixture configured, including ambiguity, path traversal, symlink, archive expansion and ZIP-extracted/loose processing.
- The Playwright Edge test drove ZIP and separate LOG-first uploads through the browser, confirmed validation warnings and all seven navigable map photos, and downloaded both packages. GeoJSON points and routes and individual JPEG hashes matched. The final 7.872 s fix selected the 7.8662 s frame.
- Pyogrio/GDAL opened the downloaded GeoPackages and reported seven EPSG:4326 point features in each. The bundle checks covered CSV coordinate agreement, image references and ZIP CRC.
- Integration checks passed for anonymous access rejection, local publication and unpublication, traversal, damaged archive rejection, incomplete pair gating, Compose restart persistence, identical settings reuse, queued cancellation and a time-interval retry.
- Inspected the built Python package license metadata and Leaflet license. Scanned the working source tree, Git index and reachable history for sensitive file types, private paths, GPS rows, credentials and configured secret values. The runtime environment and survey files remain outside tracked source.

Limitations: local single-admin use, one worker, offline map without a street basemap, and soft deletion with source retention. Cancellation is checked between processing steps. Source files are preserved separately from interpolated exports. Large-survey load, clean restore to a second stack and ARM64 were not tested. See THIRD_PARTY_NOTICES.md for distribution review items.

- Stopped the worker during an active distance-sampling run, started it again, and verified that the same job recovered and completed.

## Survey viewer feedback

The shared viewer offers an optional OpenStreetMap layer. Red dots switch to bearing arrows at zoom 19, and high-zoom markers are limited to the viewport. The existing route remains visible. Published surveys have an authenticated read-only page at the survey slug. Unpublishing revokes the page and its artifact routes. The migration assigns slugs to existing surveys without regenerating outputs.

Rotation is baked into exported JPEGs when a new processing run completes. Manual points supply coordinates and a source-video timestamp, with an optional bearing. They appear in the photo sequence and spatial exports, while the route follows the sampled GPS observations.

The Docker/Edge test verified the layer toggle and attribution with intercepted tile requests, low/high zoom markers, viewport culling, a 90-degree JPEG rotation, addition of an eighth point from the private sample, and authenticated publication and revocation. Its source and generated exports remain outside Git. The existing published survey page was checked separately.

## Multiple recordings and publication layers

Version 2.0.0-alpha.3 lets users append a ZIP or an MP4/LOG pair to a completed survey. Each recording validates and extracts against its own video timeline. The combined run renumbers image paths, preserves recording provenance, and keeps routes separate. The migration preserves prior survey URLs and outputs. The publication choice persists as route only, points only, or both. The sidebar About dialog credits Mark Urupa and links to LinkedIn.

The Edge test appended a privately derived second MP4/LOG recording to the original ZIP survey, verified 14 readable photo references and two independent route segments, and opened the 14-point GeoPackage with GDAL. It confirmed that the previous run's image still matched, tested all three layer choices on the same published URL, and checked the About dialog and credit link. Nine earlier catalog entries created by test runs were removed. Browser tests now use temporary names and clean up their entries.

Processing rebuilds the combined run from recordings using the current survey settings. Manual photo points still reference the first recording.

## Validation and coordinate editing

Version 2.0.0-alpha.4 uses readable English status messages, shows a spinner during validation, and exposes a ZIP containing report.csv and excluded_records.csv. During processing, the cancel control takes the place of the start control and a progress bar shows the percentage. Deletion shares the publication action row. The workspace map point popup shows coordinates, altitude, speed, bearing and accuracy when available, and supports numeric or draggable position corrections.

Corrections create a new revision, reuse existing photos, and update the point geometries, corresponding route vertices and portable GIS export. Observed coordinates remain in the revision metadata. Reprocessing reapplies saved edits to matching observations. Existing processed runs can read missing telemetry from their matching GPS log. Published maps are view-only.

The Docker/Edge test verified validation and the two CSV files, processing progress and cancellation, both editing methods, revised point and route exports and image bytes, correction persistence on reprocessing, and the absence of editing on the published page. The existing published survey and legacy telemetry were also checked. Eleven Python tests passed with the private sample configured, including independent route segments and exclusion record numbering.

## Publication indicators and batch extraction

Version 2.0.0-alpha.5 pins each published URL to the run explicitly selected for publication. It uses a green dot for the current publication and yellow when sources or completed results change. Unpublished surveys have no dot. Existing publications were backfilled to their current completed run. Upload information and survey deletion now use in-page dialogs.

The processor selects the same numbered frames and encodes them at the same JPEG quality and rotation in a single FFmpeg pass per recording. It verifies the output JPEG sequence in a separate pass and skips repeating full preflight media decoding when input digests still match an already fully validated recording. It writes spatial records in database batches.

Local Docker benchmark, with the same recordings, rotation settings, and direct processor invocation for both builds:
- Short recording, seven photos: 13.959 s before and 6.228 s after. All JPEG, GeoJSON and CSV hashes matched.
- Long recording, 128 photos: 90.958 s after. The old run was stopped without completing after at least 288.14 s of extraction. All 57 finished JPEGs captured before stopping matched their counterparts byte for byte. This comparison is against the previous portal processor.
- Twelve Python tests passed with the private sample configured. A synthetic smoke test compared batch and single-frame extraction, including a repeated source frame and a rotated last frame.
- Edge verified the upload information dialog, absent/green/yellow indicators, preservation of the published run until republication, and both cancel and confirm in the deletion dialog.

## Public map access and default basemap

Version 2.0.0-alpha.6 saves access and basemap choices for each publication. Existing maps require admin credentials and open with Survey only. Public maps serve their selected layers and photos without credentials. The workspace, survey management, and export bundles remain admin only.

The isolated Compose stack applied the migration. A temporary synthetic survey verified anonymous access to map pages, selected layers and photos, the saved OpenStreetMap preference, access denial for admin endpoints and export bundles, and revocation after switching to private or unpublishing.

## Public release preparation in progress

The Android 1.3.0 project was copied from the owner's local Android Studio project without its keystore, APK or local configuration. `assembleDebug` succeeded locally with Android Studio's JDK and SDK. Source inspection found video recording, GPS log fields matching the portal contract, ZIP export, and sharing of paired files. Physical-device recording and sharing have not been verified, nor has signed release provenance or update compatibility.

The initial Starlight documentation builds locally. Starlight 0.43.1 and Astro 7.4.12 replaced an older version after an npm audit reported vulnerabilities; the resulting npm audit reports zero known vulnerabilities at the time of review. Candidate comparison: withastro/starlight (MIT, default branch commit 2026-11-17) provides the requested documentation integration; squidfunk/mkdocs-material (MIT, default branch commit 2026-11-07) is an alternative Python-based documentation engine. No external project code was copied.

The Python suite passed 11 tests with the private-sample test skipped. Android and documentation GitHub Actions are prepared but have not yet been verified on GitHub. The release checklist records outstanding launch gates, including the demo, screenshots, installation validation, privacy and distribution review. This branch is not yet a complete public release candidate.

## Public launch finalization in progress

The PR #1 Android, core and documentation checks passed. The static demo uses the existing viewer and twelve synthetic images derived from one generated scene with fictional coordinates. A disposable Docker installation processed a synthetic MP4 and LOG to capture actual interface screenshots. Documentation and demo Playwright checks passed at desktop and mobile sizes, including navigation and no backend requests.

The Windows installer was run in isolated Compose projects, including a fresh generated-credentials run and a repeat run that preserved the credentials. The Linux installer, backup and restore, physical Android use, complete privacy audit and image redistribution review still require verification. The Pages and container publication templates are disabled. See the release checklist for remaining work.

Subsequent verification: the Bash installer succeeded twice inside a Linux CLI container connected to Docker Desktop, with credentials preserved. This is not a clean native Linux host test. An initial fresh stack exposed an API/PostGIS startup race; the API now restarts on failure and a second fresh stack reached healthy status. Gitleaks 8.30.1 scanned 21 reachable commits without reported secrets. GitHub reported no stored Actions artifacts or releases at inspection. The PR's Android, core, and documentation checks passed before these follow-up changes.

Owner review: the processed `SV_20260423_114721` survey was copied into ignored local preview storage and overlaid onto the built static site. All 183 points and images remain outside Git; a browser check opened the first image and navigated to the second. Public distribution requires imagery and rights review. The Android guide now documents the owner-described workflow and includes supplied app screenshots, with APK distribution and device checks still pending.

Mobile review found that the old development preview served the synthetic sample. The private review now has a dedicated build command and an ignored output directory. The demo route works in Astro development and preview. Mobile Chromium loaded live OpenStreetMap tiles on both local review ports and received distinct JPEGs through Next and a touch on a map point. Six desktop and mobile Playwright checks passed on the regular synthetic build.
