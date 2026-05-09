# Outputs And Reports

This page documents the result tree written by the current FAST-Calib offline GUI export path.

## Result Directory

A successful `Optimize Included` writes a timestamped result directory. `Export Result` copies the latest result tree to a user-selected destination.

```text
result_root/
  extrinsic_fastlivo2.txt
  extrinsic.yaml
  circle_center_record.txt
  manual_annotations.json
  run_manifest.json
  residual_summary.csv
  per_point_residuals.csv
  report.md
  report.html
  samples/
    mid22_image_features.png
    mid22_reprojection.png
    mid22_reprojection_error.png
    mid22_colored_cloud.pcd
  run.log
```

## Extrinsic Files

### `extrinsic.yaml`

Primary machine-readable calibration result. It includes:

- `T_cam_lidar`;
- flattened `Rcl`;
- `Pcl`;
- `rmse_m`;
- optimized `sample_ids`;
- `camera_intrinsics`.

### `extrinsic_fastlivo2.txt`

FAST-LIVO2-style export. It writes camera model fields, distortion coefficients, RMSE, `Rcl`, and `Pcl`.

Use this file when the downstream consumer expects the FAST-LIVO2 calibration text format.

## Center Record

### `circle_center_record.txt`

Compatibility file for the FAST-Calib center-record workflow. For each optimized sample, it records:

- sample id as `time`;
- four LiDAR centers;
- four camera-side QR/board centers.

This file is useful for comparing the GUI-reviewed correspondences against command-line center-record workflows.

## Annotation And Manifest

### `manual_annotations.json`

Stores review state and manual edits for reproducibility:

- image ROI;
- markers;
- image centers;
- LiDAR ROI;
- LiDAR centers;
- provisional transforms;
- status and inclusion flags.

### `run_manifest.json`

Top-level run summary:

- input dataset root;
- output result root;
- optimized sample ids;
- RMSE and residuals;
- `T_cam_lidar`;
- camera intrinsics;
- board config;
- paths to residual tables and reports;
- per-sample visualization paths.

Use this as the first file to inspect when auditing a result directory.

## Residual Tables

### `residual_summary.csv`

Per-sample summary table with reprojection error statistics. This is the compact view for deciding whether one sample dominates the final error.

### `per_point_residuals.csv`

Per-correspondence residual table. Use it to find a specific center id whose projection or metric residual is inconsistent with the rest.

## Visual Diagnostics

The `samples/` directory contains review artifacts for each optimized sample:

| File | Purpose |
|---|---|
| `*_image_features.png` | image markers, board ROI, detected hole circles, center labels |
| `*_reprojection.png` | dense projection overlay using the optimized transform |
| `*_reprojection_error.png` | center reprojection view with residual lines and error annotations |
| `*_colored_cloud.pcd` | point cloud colored for projection/review diagnostics when available |

These files make the output auditable without reopening the GUI.

## Reports

### `report.md`

Markdown summary for version control, issue discussion, and plain-text review.

### `report.html`

HTML summary for local browser review. It links the same artifacts and presents the final extrinsic and residual diagnostics.

## Run Log

### `run.log`

Records the dataset path, optimized sample ids, RMSE, and the important runtime note that the GUI uses offline image/PCD samples and does not read rosbag files.

## Review Checklist

Before accepting a calibration result:

- check that `sample_ids` match the intended ready samples;
- inspect `rmse_m` and per-sample pixel errors;
- open `*_reprojection_error.png` for each optimized sample;
- confirm that the transform direction is `T_cam_lidar`;
- compare `extrinsic.yaml` and `extrinsic_fastlivo2.txt` only after confirming the downstream convention.

