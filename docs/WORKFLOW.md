# GUI Workflow

This workflow documents the current FAST-Calib offline GUI behavior. It is written from the existing implementation, not from a new design.

## 1. Prepare The Offline Dataset

Convert flat image/PCD pairs into the offline dataset layout:

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset
```

Open `/path/to/offline_dataset` in the GUI. Do not open the flat source folder, the `samples/` subfolder, or a single sample folder.

## 2. Open Dataset

Click `Open Dataset` and select `dataset_root`.

The loader scans:

- `samples/<sample_id>/image.*`;
- `samples/<sample_id>/cloud.pcd`;
- optional `samples/<sample_id>/meta.json`;
- `camera_intrinsics.yaml`;
- `board_config.yaml`.

Invalid samples stay visible but cannot be optimized until missing files or configuration issues are fixed.

## 3. Review A Sample

Select one sample from the sample rail. The GUI presents the image view, point-cloud view, projection/reprojection diagnostics, correspondence table, batch-impact tab, and run log.

Important sample-level state is stored in `SampleAnnotation`, including:

- image board ROI;
- detected ArUco markers;
- camera-side 3D hole centers;
- image-space hole centers;
- LiDAR ROI;
- LiDAR plane and hole centers;
- provisional transform;
- pixel and metric residuals;
- included/locked/status flags.

## 4. Detect Camera Features

Use the image view to draw the board ROI. Then click `Detect Camera Features`.

The current detector:

- masks detection to the image ROI when provided;
- detects ArUco markers;
- estimates board pose with camera intrinsics;
- projects the four known board hole centers;
- records camera-frame hole centers and image-space centers;
- draws marker borders, marker IDs, hole circles, and center indices.

If too few board markers are detected, the sample needs review before it can become `Ready`.

## 5. Detect LiDAR Plane And Holes

Use the point-cloud view to draw or refine the LiDAR ROI. Then run:

1. `Detect Plane` to confirm the target plane inside the active ROI.
2. `Detect Holes` to extract four LiDAR-side hole centers.

The GUI crops the point cloud to the active ROI before plane/circle extraction. Background points outside the crop should not participate in the feature solve.

## 6. Estimate And Review Projection

Once a sample has four camera centers and four LiDAR centers, estimate the sample pose or use the primary next-action button. Review:

- center numbering;
- center-to-center correspondence;
- projection overlay;
- per-point pixel residuals;
- metric residuals;
- sample message and status.

Projection source priority:

1. latest batch optimized `T_cam_lidar`;
2. current sample provisional pose;
3. loaded or manually provided initial pose;
4. unavailable.

Use direction checks when the projection is empty or completely wrong. The expected transform convention is `T_cam_lidar`.

## 7. Mark Ready Samples

A complete sample needs:

- valid image and PCD files;
- four camera-side hole centers;
- four LiDAR-side hole centers;
- confirmed numbering/correspondence;
- status manually accepted as `Ready`;
- `included` enabled if it should participate in batch optimization.

`Optimize Included` is blocked until at least two checked samples are `Ready`, included, and complete.

## 8. Evaluate Batch

`Evaluate Batch` is read-only. It helps decide which ready samples should stay included before final optimization.

It reports:

- current included-set mean/RMSE/max reprojection error;
- each sample's single-sample pose error when available;
- the batch error if an unchecked ready sample is added;
- the batch error if a checked ready sample is removed;
- a simple recommendation such as `Keep`, `Review`, `Try Include`, `Exclude`, or `Need more samples`.

It does not export files and does not change sample status.

## 9. Optimize Included

Click `Optimize Included` after the included ready set is clear.

The current optimizer:

- validates sample count and status;
- requires at least two ready samples;
- requires four image centers and four LiDAR centers per selected sample;
- stacks all selected center correspondences;
- solves a rigid transform with SVD;
- writes `T_cam_lidar`, residuals, diagnostics, and report artifacts to the default result directory.

## 10. Export Result

`Optimize Included` writes a default result tree. `Export Result` copies the latest result tree to a user-selected destination.

Run `Export Result` only after a successful optimization. If no current result exists, the GUI reports:

```text
Run Optimize Included before exporting a result.
```

## Blocking Rules

Common blocking messages map to concrete fixes:

| Message | Meaning |
|---|---|
| `Select at least 2 ready samples` | Include at least two accepted samples |
| `sample status is ...; mark it Ready before optimization` | Finish review and explicitly mark the sample ready |
| `expected 4 image hole centers` | Redetect or manually fix camera-side features |
| `expected 4 LiDAR hole centers` | Refine ROI and redetect LiDAR holes |
| `invalid sample` | Fix missing image/PCD files or dataset layout |

