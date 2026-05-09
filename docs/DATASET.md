# Dataset Format

Manual Calibration Tools v1 follows the existing FAST-Calib offline GUI dataset contract. The GUI opens a prepared `dataset_root`; it does not open raw rosbag folders and it does not open a flat `mid22.png + mid22.pcd` source directory directly.

## Required Layout

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

Allowed image names are `image.png`, `image.jpg`, `image.jpeg`, `image.bmp`, `image.tif`, or `image.tiff`. The point cloud file must be named `cloud.pcd`.

## Sample Directory

Each sample is one synchronized image and one corresponding point cloud:

```text
samples/
  mid22/
    image.png
    cloud.pcd
    meta.json
```

The sample id is read from `meta.json` when present; otherwise the folder name is used.

## `meta.json`

`meta.json` is optional but recommended:

```json
{
  "sample_id": "mid22",
  "source_rosbag": "mid22.bag",
  "image_timestamp": null,
  "pointcloud_timestamp": null,
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

When `lidar_roi` exists, the GUI uses it as the initial crop box for that sample. Users can still refine the ROI interactively.

## `camera_intrinsics.yaml`

The camera config must provide the pinhole intrinsics and optional distortion coefficients:

```yaml
fx: 913.2
fy: 912.8
cx: 640.0
cy: 360.0
k1: 0.0
k2: 0.0
p1: 0.0
p2: 0.0
k3: 0.0
```

The current implementation also accepts nested mappings such as `camera`, `intrinsics`, or `camera_intrinsics` and flattens them before reading fields.

## `board_config.yaml`

The board config describes the current FAST-Calib target geometry:

```yaml
marker_size: 0.20
delta_width_qr_center: 0.55
delta_height_qr_center: 0.35
delta_width_circles: 0.50
delta_height_circles: 0.40
circle_radius: 0.12
min_detected_markers: 3
lidar_roi:
  x_min: 2.0
  x_max: 5.0
  y_min: -4.0
  y_max: 4.0
  z_min: 0.0
  z_max: 2.0
```

This v1 target is the FAST-Calib-style board used by the existing offline GUI. Future calibration-board workflows should be added as explicit new profiles instead of changing this v1 contract silently.

## Preparing Flat MID360 Data

The current implementation exposes a `prepare-flat` command that materializes the offline layout from flat `mid*.png` and `mid*.pcd` pairs:

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset
```

Use `--copy` when the generated dataset must not depend on symlinks:

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset \
  --copy
```

## Correct Folder Selection

Select:

```text
/path/to/offline_dataset
```

Do not select:

```text
/path/to/flat_mid360_data
/path/to/offline_dataset/samples
/path/to/offline_dataset/samples/mid22
```

The common error for selecting the wrong folder is:

```text
Offline dataset must contain a samples/ directory: <selected path>
```

## Validation Rules

The dataset scanner marks each sample valid only when:

- the sample folder exists under `samples/`;
- an allowed `image.*` file exists;
- `cloud.pcd` exists;
- `meta.json`, when present, contains a JSON object.

Missing files keep the sample visible but invalid, with messages such as:

```text
Missing image.*
Missing cloud.pcd
```

