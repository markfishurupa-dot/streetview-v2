# Public release audit

Audit date: 17 November 2026. This report tracks the public alpha and outstanding release work; it is not a stable-release approval.

| Severity | Location | Finding and action | Evidence |
| --- | --- | --- | --- |
| High | `site/public/demo/` | The public demo contains real locations, people and vehicle details. Obtain confirmation of dataset rights and complete a frame-by-frame privacy review. | The published dataset has 183 photos; its license does not grant general reuse. |
| High | `docs/public-release-checklist.md` | Associate the owner's successful mobile-app test with the exact published APK and verify compatibility with earlier signing keys. | The owner reports that all components, including the mobile app, worked as expected; the tested APK digest and device details were not specified. |
| High | `deploy/`, `THIRD_PARTY_NOTICES.md` | Complete container redistribution, dependency and backup/restore review before a new stable release or GHCR publication. | Existing notices identify unresolved FFmpeg, PostGIS and transitive obligations. |
| Medium | `docs/release-notes-alpha.md` | Historical text incorrectly stated that no public release existed and the demo was synthetic. Corrected. | GitHub alpha.6 release and public Pages demo. |
| Medium | `docs/apk-v1.3.0-verification.md` | Official Android SDK signature verification was missing. Completed and recorded. | Build Tools 36.0.0 `apksigner` and `aapt` checked the downloaded asset. |
| Medium | `site/src/content/docs/guide/first-survey.md` | Screenshot paths did not render when viewed as Markdown on GitHub. Corrected. | The GitHub Markdown route does not serve the Pages image paths. |
| Medium | `README.md` | Android APK download was buried below the opening description. Added to the primary links. | Published alpha.6 contains `StreetView.apk`. |
| Medium | `site/src/content/docs/` | Tutorials need further review for screenshot annotations, accessibility and local asset links. | Screenshots use branch-specific raw GitHub URLs to support both GitHub Markdown and Pages. |
| Medium | `docs/android-release-signing.md` | Android backup configuration permits backup and requires a separate privacy assessment. | APK verification records `allowBackup=true`; no change to the released APK. |
| Medium | `setup.sh` | A clean native Linux host and complete recovery/upgrade path remain unverified. | Earlier Bash checks used a Linux CLI container with Docker Desktop. |
| Medium | `docs/releases/v2.0.0-alpha.6.md` | Release copy is lengthy and should point to the illustrated guides. Prepare a concise replacement and verify links before editing the public release description. | Current public alpha.6 release body. |
| Low | Repository settings | Default branch uses `codex/alpha-implementation`. Evaluate links, Pages, workflows and developer checkouts before proposing a migration to `main`. | Current GitHub default branch and branch-specific documentation links. |

Next release recommendation: continue the alpha until device, recovery, distribution, privacy and branding gates have evidence. The existing alpha.6 tag and signed APK remain in place.
