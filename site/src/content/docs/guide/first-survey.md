---
title: Process your first survey
description: A beginner's walkthrough from Android recording to an interactive map and GIS export.
---

This guide takes you from an Android recording to georeferenced photographs and downloadable GIS files. The images show the **real StreetView portal with synthetic test data**, so you can recognize the same controls on your own installation.

:::note[Before you begin]
You need a running [StreetView portal](/streetview-v2/start/install/) and a matching **MP4 video + GPS LOG**, or the **ZIP** exported by the Android app. The Android source is available, and a [signed Android 1.3.0 APK is available from the official alpha.6 prerelease](https://github.com/markfishurupa-dot/streetview-v2/releases/download/v2.0.0-alpha.6/StreetView.apk).
:::

## 1. Record and transfer the survey

Use [StreetView on Android](/streetview-v2/start/android/) to record the route. The app saves a video and GPS log with matching basenames, for example:

```text
SV_20260423_114708.mp4
SV_20260423_114708.log
```

Share the **ZIP** for the simplest import, or transfer the **MP4 and LOG separately**. The portal can accept one of the two files first and the matching file later. Do not rename just one of the pair.

## 2. Create a survey and upload your files

Open **http://127.0.0.1:18080/** and sign in as `admin` with the password in your local `deploy/.env` file.

Create a survey, choose **Recording files**, select either the ZIP or the two files, then press **Upload**.

![Portal upload controls](https://raw.githubusercontent.com/markfishurupa-dot/streetview-v2/codex/alpha-implementation/site/public/images/screenshots/upload.png)

Expected result: the portal reports that sources were staged. If only one of the two files is available, upload the matching counterpart before processing.

## 3. Validate your recording

Select **Validate sources**. The portal checks the media and GPS inputs, then shows a result with quality warnings. Use **Download report** to obtain more detail and excluded-record information.

![Portal validation results and processing controls](https://raw.githubusercontent.com/markfishurupa-dot/streetview-v2/codex/alpha-implementation/site/public/images/screenshots/validation.png)

**Warnings are not always fatal.** Poor GPS accuracy or sudden position jumps may need review, while corrupted media, unmatched filenames or missing files can prevent processing.

## 4. Choose processing settings

Set the **Sampling** mode:

- **GPS fixes:** photos associated with valid recorded GPS observations.
- **Time (seconds):** photos at a selected time interval.
- **Distance (metres):** photos sampled along the route at a selected distance interval.

Set **Interval** for time or distance sampling, and optionally set **Maximum GPS accuracy (m)** to exclude lower-quality fixes. Choose **Rotate exported photos** if the source camera orientation requires it.

Start with **GPS fixes** for your first survey. Select **Process survey** and follow the progress bar. Processing runs in the background worker and can take longer for larger videos.

![Processing progress and cancellation](https://raw.githubusercontent.com/markfishurupa-dot/streetview-v2/codex/alpha-implementation/site/public/images/screenshots/processing.png)

## 5. Explore and correct the map

Once processing completes, select a photo point on the map or use **Previous photo** and **Next photo**. The photo viewer is linked to the point locations and route.

![Processed survey viewer](https://raw.githubusercontent.com/markfishurupa-dot/streetview-v2/codex/alpha-implementation/site/public/images/screenshots/viewer.png)

In the authenticated workspace, selecting a point provides location and GPS details. Use **Edit** to correct coordinates or bearing, drag the map handle for a position adjustment, or choose **Delete point** to remove an unwanted point. The route reconnects neighboring points within the *same recording*. Corrections are preserved across matching reprocessing runs.

For more details, read [Map, photos and corrections](/streetview-v2/guide/map/).

## 6. Export GIS data

Use the download links below the map. Depending on the output, exports include:

| File | Use |
| --- | --- |
| `survey.zip` | Portable bundle with imagery, GIS results and QA information |
| `points.gpkg` | Georeferenced photo points for QGIS or other GIS software |
| `points.geojson` | GeoJSON photo points |
| `route.geojson` | Recorded or processed survey route |
| CSV | Photo locations, timing and related attributes |

The portable ZIP uses **relative image paths**, so keep the folder structure together when sharing. The geographic outputs use **WGS 84 / EPSG:4326**. Open the GeoPackage in QGIS as a vector layer and inspect its attributes and image references.

## 7. Publish a read-only map (optional)

Choose **Published map layers**, **Default basemap**, and whether the map should be **Publicly available**. Select **Publish locally**, then **Open published map**.

- Without public access enabled, readers need administrator authentication.
- With public access enabled, the selected map and photographs can be viewed **without signing in by anyone able to reach the StreetView server**.
- The default Docker installation listens only on `127.0.0.1`, so it is **not automatically accessible from the Internet**.
- A publication does not update after workspace edits until you republish it.

Read [Publishing and access control](/streetview-v2/guide/publishing/) before exposing any survey outside your computer.

## Next steps

You can [add another recording](/streetview-v2/guide/recordings/) to the same survey, explore the [interactive public demo](/streetview-v2/demo/) without uploading files, or read [troubleshooting](/streetview-v2/reference/help/) if validation or Docker startup fails.
