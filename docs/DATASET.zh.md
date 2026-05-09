# 数据格式

Manual Calibration Tools v1 沿用现有 FAST-Calib offline GUI 的数据约定。GUI 打开的是已经整理好的 `dataset_root`；它不直接打开原始 rosbag 目录，也不直接打开扁平的 `mid22.png + mid22.pcd` 源目录。

## 必需目录结构

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

允许的图像文件名包括 `image.png`、`image.jpg`、`image.jpeg`、`image.bmp`、`image.tif` 或 `image.tiff`。点云文件必须命名为 `cloud.pcd`。

## Sample 目录

每个 sample 表示一帧同步图像和对应点云：

```text
samples/
  mid22/
    image.png
    cloud.pcd
    meta.json
```

如果 `meta.json` 中提供了 `sample_id`，优先使用该字段；否则使用文件夹名。

## `meta.json`

`meta.json` 不是强制文件，但建议提供：

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

如果存在 `lidar_roi`，GUI 会把它作为该 sample 的初始 LiDAR crop box。用户仍然可以在界面中继续细化 ROI。

## `camera_intrinsics.yaml`

相机配置需要提供 pinhole 内参和可选畸变参数：

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

当前实现也接受 `camera`、`intrinsics` 或 `camera_intrinsics` 这样的嵌套字段，并在读取前展开。

## `board_config.yaml`

标定板配置描述当前 FAST-Calib target 的几何参数：

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

这是现有 offline GUI 使用的 FAST-Calib 风格标定板。未来如果扩展其他标定板方案，应作为明确的新 profile 加入，而不是静默改变 v1 数据约定。

## 从扁平 MID360 数据生成 dataset

当前实现提供 `prepare-flat` 命令，将扁平 `mid*.png` 和 `mid*.pcd` 数据整理成离线目录：

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset
```

如果生成的数据集需要脱离原始文件独立保存，使用 `--copy`：

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset \
  --copy
```

## 正确选择文件夹

应该选择：

```text
/path/to/offline_dataset
```

不要选择：

```text
/path/to/flat_mid360_data
/path/to/offline_dataset/samples
/path/to/offline_dataset/samples/mid22
```

选错目录时常见错误是：

```text
Offline dataset must contain a samples/ directory: <selected path>
```

## 校验规则

dataset scanner 只有在下面条件满足时才把 sample 标记为 valid：

- sample 文件夹位于 `samples/` 下；
- 存在允许命名的 `image.*` 文件；
- 存在 `cloud.pcd`；
- 如果存在 `meta.json`，其内容必须是 JSON object。

缺文件的 sample 仍会显示，但会被标记为 invalid，并出现类似提示：

```text
Missing image.*
Missing cloud.pcd
```

