# Manual Calibration Tools

<p align="center">
  <a href="README.md"><strong>English</strong></a> | <a href="README.zh.md">中文</a>
</p>

<p align="center">
  <img src="assets/hero.svg" alt="Manual Calibration Tools hero" width="100%" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/status-docs--preview-orange.svg" alt="Docs preview" />
  <img src="https://img.shields.io/badge/source-private--beta-lightgrey.svg" alt="Private beta source" />
  <img src="https://img.shields.io/badge/python-3.10%2B-blue.svg" alt="Python 3.10+" />
  <img src="https://img.shields.io/badge/GUI-PySide6-2E8B57.svg" alt="PySide6 GUI" />
  <img src="https://img.shields.io/badge/vision-OpenCV-5C3EE8.svg" alt="OpenCV" />
  <img src="https://img.shields.io/badge/point%20cloud-Open3D-0B7285.svg" alt="Open3D" />
  <img src="https://img.shields.io/badge/future%20source-GPLv3-blue.svg" alt="Future GPLv3 source" />
</p>

<p align="center">
  <strong>Author</strong>:
  <a href="https://github.com/JokerJohn">Xiangcheng HU</a><br/>
  <strong>Contact</strong>: <a href="mailto:xhubd@connect.ust.hk">xhubd@connect.ust.hk</a>
</p>

Python-first calibration GUI for camera-LiDAR and future multi-sensor extrinsic calibration workflows.

> **Documentation preview.** The public source code is not released yet. This repository currently documents the product workflow, dataset format, environment plan, FAST-Calib v1 behavior, and release roadmap. The v1 workflow is based on the existing private implementation in `fast_calib_offline_gui`; the public repository should not be interpreted as a runnable source release at this stage.

## Overview

Manual Calibration Tools is designed as a practical calibration workbench instead of a one-click black box. The first release path productizes the existing MID360 FAST-Calib offline GUI workflow:

- offline image/PCD sample organization instead of direct rosbag loading;
- PySide6 desktop GUI for sample review, ROI editing, feature detection, projection checking, and batch optimization;
- OpenCV-based ArUco board pose and camera-side hole-center extraction;
- Open3D-assisted point-cloud loading and LiDAR-side ROI / plane / hole-center review;
- rigid SVD solve over confirmed 2D/3D-derived center correspondences;
- auditable outputs: extrinsics, residual tables, visual diagnostics, run manifest, and HTML/Markdown report.

The project is intended to grow from the FAST-Calib v1 workflow into a broader manual-assisted calibration platform for calibration-board methods and other extrinsic calibration tasks.

## Source Of Truth For V1

The v1 public documentation follows the current implementation surface directly:

| Area | Existing implementation contract |
|---|---|
| Entry point | `fast_calib_offline_gui/main.py` with `gui` and `prepare-flat` |
| Dataset preparation | `materialize_flat_mid360_dataset()` and `scan_dataset()` |
| Data model | `DatasetProject`, `CalibrationSample`, `SampleAnnotation`, `CalibrationResult` |
| GUI actions | `Open Dataset`, `Detect Camera Features`, `Detect Plane`, `Detect Holes`, `Evaluate Batch`, `Optimize Included`, `Export Result` |
| Feature detection | ArUco board pose, image centers, LiDAR ROI, target plane, LiDAR hole centers |
| Optimization | rigid SVD solve for `T_cam_lidar` over confirmed center correspondences |
| Export | extrinsic files, center record, annotations, residual CSVs, visual diagnostics, reports, manifest |

No alternate workflow, state machine, directory layout, or solver behavior is introduced by this documentation repository.

## Workflow

<p align="center">
  <img src="assets/workflow.svg" alt="FAST-Calib offline GUI workflow" width="100%" />
</p>

1. Convert flat `mid*.png + mid*.pcd` data into the offline dataset layout with `prepare-flat`.
2. Open the generated `dataset_root` in the GUI. The GUI does not read raw rosbag files.
3. For each sample, review the image, point cloud, metadata, and optional LiDAR ROI.
4. Draw the board ROI and run `Detect Camera Features` to recover camera-side hole centers.
5. Draw or refine the LiDAR ROI, then run `Detect Plane` and `Detect Holes`.
6. Check four numbered camera/LiDAR center correspondences, projection, residuals, and sample status.
7. Mark valid samples as `Ready` and keep at least two ready samples included.
8. Run `Evaluate Batch` for read-only batch-impact diagnostics.
9. Run `Optimize Included` to solve `T_cam_lidar` and write the default result directory.
10. Use `Export Result` when the latest result tree should be copied elsewhere.

## Dataset Layout

The GUI opens an already prepared offline dataset root:

```text
dataset_root/
  samples/
    mid22/
      image.png
      cloud.pcd
      meta.json
    mid33/
      image.png
      cloud.pcd
      meta.json
  camera_intrinsics.yaml
  board_config.yaml
```

`meta.json` may include a sample-specific LiDAR ROI:

```json
{
  "sample_id": "mid22",
  "source_rosbag": "mid22.bag",
  "sync_ok": true,
  "preprocess_version": "custom-export-v1",
  "lidar_roi": {
    "x_min": 2.0,
    "x_max": 5.0,
    "y_min": -4.0,
    "y_max": 1.0,
    "z_min": 0.0,
    "z_max": 2.0
  }
}
```

See [docs/DATASET.md](docs/DATASET.md) for the full data contract.

## Environment Preview

The current private beta runtime is Python-first and centers on:

- Ubuntu 20.04/22.04 style desktop environment;
- Python 3.10 or newer;
- PySide6 for the desktop GUI;
- OpenCV with ArUco support for image feature detection;
- Open3D for point cloud I/O and review;
- NumPy, SciPy-style numerical utilities, and PyYAML.

When the source release is available, the expected commands will follow this pattern:

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset

python -m fast_calib_offline_gui gui
```

For the current environment notes, see [docs/INSTALL.md](docs/INSTALL.md).

## Exported Results

A successful optimization writes:

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

See [docs/OUTPUTS.md](docs/OUTPUTS.md) for file meanings and review guidance.

## Documentation

- [Installation and environment](docs/INSTALL.md)
- [Dataset format](docs/DATASET.md)
- [GUI workflow](docs/WORKFLOW.md)
- [Outputs and reports](docs/OUTPUTS.md)
- [Roadmap](docs/ROADMAP.md)
- [Chinese README](README.zh.md)

The source pages for the GitHub Wiki are kept under [wiki/](wiki/).

## Roadmap

| Stage | Scope | Status |
|---|---|---|
| v1 | Productize the existing FAST-Calib offline MID360 GUI workflow | Private beta / docs preview |
| v1.x | Improve packaging, example dataset guidance, and report polish | Planned |
| v2 | Add calibration-board workflows beyond the current FAST-Calib target | Planned |
| v3 | Extend to other extrinsic calibration tasks and sensor combinations | Planned |

## License And Source Availability

The public source code is not included in this repository yet. The intended source-code license for the future release is GNU GPLv3. Until the source is published, implementation details remain unpublished, and this repository should be treated as documentation and project planning material.

