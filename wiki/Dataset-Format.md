# Dataset Format

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Dataset-Format) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Dataset-Format-zh)

The v1 GUI opens a prepared offline dataset root.

```text
dataset_root/
  samples/
    sample_id/
      image.png
      cloud.pcd
      meta.json
  camera_intrinsics.yaml
  board_config.yaml
```

## Required Files

| File | Purpose |
|---|---|
| `image.*` | image frame for board feature detection |
| `cloud.pcd` | corresponding LiDAR point cloud |
| `meta.json` | optional sample metadata and LiDAR ROI |
| `camera_intrinsics.yaml` | pinhole intrinsics and distortion |
| `board_config.yaml` | FAST-Calib target geometry |

## Folder Selection

Select the dataset root itself:

```text
/path/to/offline_dataset
```

Do not select:

```text
/path/to/flat_mid360_data
/path/to/offline_dataset/samples
/path/to/offline_dataset/samples/mid22
```

The common error is:

```text
Offline dataset must contain a samples/ directory
```

Full details are in [docs/DATASET.md](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/DATASET.md).

