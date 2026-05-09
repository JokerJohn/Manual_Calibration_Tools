# 故障排查

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Troubleshooting) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Troubleshooting-zh)

## Open Dataset 失败

先检查是否选择了 `dataset_root`，而不是扁平源目录、`samples/` 或单个 sample 目录。

期望结构：

```text
dataset_root/
  samples/
  camera_intrinsics.yaml
  board_config.yaml
```

## Sample Invalid

常见原因：

- 没有允许命名的 `image.*` 文件；
- 缺少 `cloud.pcd`；
- 软链接失效；
- `meta.json` 格式错误。

如果软链接不稳定，可以用 `--copy` 重新生成 dataset。

## Camera Features 失败

检查：

- image ROI 是否包含标定板；
- ArUco markers 是否清晰可见；
- 相机内参是否匹配当前图像；
- `min_detected_markers` 对当前图像是否合理。

## LiDAR Holes 失败

检查：

- LiDAR ROI 是否包含标定板并排除大部分背景；
- 是否先成功执行 `Detect Plane`；
- 四个孔附近点云密度是否足够；
- 是否在正确的固定轴视图中细化 ROI。

## 投影明显错误

先检查变换方向。当前约定是 `T_cam_lidar`。如果文件保存的是 `T_lidar_cam`，审查时应使用 inverse direction。

还要检查图像是否已经离线去畸变，但 `camera_intrinsics.yaml` 中仍保留 raw distortion coefficients。

## 优化被阻塞

`Optimize Included` 至少需要两个 valid、included、`Ready` samples，并且每个 sample 都具有四个 image centers 和四个 LiDAR centers。

