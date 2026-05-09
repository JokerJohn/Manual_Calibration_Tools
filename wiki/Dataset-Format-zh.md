# 数据格式

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Dataset-Format) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Dataset-Format-zh)

v1 GUI 打开整理好的离线 dataset root。

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

## 必需文件

| 文件 | 作用 |
|---|---|
| `image.*` | 用于标定板特征检测的图像帧 |
| `cloud.pcd` | 对应的 LiDAR 点云 |
| `meta.json` | 可选 sample 元数据和 LiDAR ROI |
| `camera_intrinsics.yaml` | pinhole 内参和畸变 |
| `board_config.yaml` | FAST-Calib target 几何 |

## 文件夹选择

应选择 dataset root 本身：

```text
/path/to/offline_dataset
```

不要选择：

```text
/path/to/flat_mid360_data
/path/to/offline_dataset/samples
/path/to/offline_dataset/samples/mid22
```

常见错误是：

```text
Offline dataset must contain a samples/ directory
```

完整说明见 [docs/DATASET.zh.md](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/DATASET.zh.md)。

