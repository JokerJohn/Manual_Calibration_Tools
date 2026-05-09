# 快速开始

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Quick-Start) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Quick-Start-zh)

本文描述 v1 的目标使用流程。公开源码暂未发布，因此下面的命令应理解为私有 beta 接口和未来公开形态。

## 1. 准备环境

GUI 是 Python-first：

- Python 3.10+；
- PySide6；
- 带 ArUco 的 OpenCV；
- Open3D；
- NumPy；
- PyYAML。

详细信息见 [安装说明](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/INSTALL.zh.md)。

## 2. 准备数据

GUI 打开整理好的 `dataset_root`：

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

如果源数据是扁平的 `mid*.png + mid*.pcd`，使用私有 beta 命令形态：

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset
```

## 3. 启动 GUI

```bash
python -m fast_calib_offline_gui gui
```

点击 `Open Dataset`，选择 `/path/to/offline_dataset`。

## 4. 处理样本

对每个 sample：

1. 绘制图像 board ROI；
2. 点击 `Detect Camera Features`；
3. 绘制或细化 LiDAR ROI；
4. 点击 `Detect Plane`；
5. 点击 `Detect Holes`；
6. 检查投影和残差；
7. 确认四组对应关系后，再将 sample 标记为 `Ready`。

## 5. 批量优化与导出

- 至少保留两个 included 的 ready samples。
- 点击 `Evaluate Batch` 做只读诊断。
- 点击 `Optimize Included` 求解 `T_cam_lidar`。
- 如果需要把最新结果树复制到其他位置，点击 `Export Result`。

