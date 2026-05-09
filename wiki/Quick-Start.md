# Quick Start

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Quick-Start) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Quick-Start-zh)

This page describes the intended v1 workflow. The public source code is not released yet, so treat commands as the private beta interface and future public shape.

## 1. Prepare The Environment

The GUI is Python-first:

- Python 3.10+;
- PySide6;
- OpenCV with ArUco;
- Open3D;
- NumPy;
- PyYAML.

See [Installation](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/INSTALL.md) for details.

## 2. Prepare Data

The GUI opens a prepared `dataset_root`:

```text
dataset_root/
  samples/
    mid22/
      image.png
      cloud.pcd
      meta.json
  camera_intrinsics.yaml
  board_config.yaml
```

If your source data is flat `mid*.png + mid*.pcd`, use the private beta command shape:

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset
```

## 3. Launch The GUI

```bash
python -m fast_calib_offline_gui gui
```

Click `Open Dataset` and select `/path/to/offline_dataset`.

## 4. Process Samples

For each sample:

1. draw the image board ROI;
2. click `Detect Camera Features`;
3. draw or refine the LiDAR ROI;
4. click `Detect Plane`;
5. click `Detect Holes`;
6. review projection and residuals;
7. mark the sample `Ready` only after checking all four correspondences.

## 5. Batch And Export

- Keep at least two ready samples included.
- Click `Evaluate Batch` for read-only diagnostics.
- Click `Optimize Included` to solve `T_cam_lidar`.
- Click `Export Result` if the latest result tree should be copied elsewhere.

