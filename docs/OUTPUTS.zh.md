# 输出与报告

本文档说明当前 FAST-Calib offline GUI export 路径写出的结果树。

## 结果目录

一次成功的 `Optimize Included` 会写出带时间戳的结果目录。`Export Result` 用于把最新结果树复制到用户选择的位置。

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

## 外参文件

### `extrinsic.yaml`

主要的机器可读标定结果，包含：

- `T_cam_lidar`；
- 展平后的 `Rcl`；
- `Pcl`；
- `rmse_m`；
- 参与优化的 `sample_ids`；
- `camera_intrinsics`。

### `extrinsic_fastlivo2.txt`

FAST-LIVO2 风格导出文件，包含 camera model、畸变参数、RMSE、`Rcl` 和 `Pcl`。

当下游程序需要 FAST-LIVO2 标定文本格式时使用该文件。

## 中心记录

### `circle_center_record.txt`

兼容 FAST-Calib center-record workflow 的文件。每个优化样本会记录：

- 作为 `time` 的 sample id；
- 四个 LiDAR centers；
- 四个 camera-side QR/board centers。

该文件适合用来把 GUI 检查后的对应关系和命令行 center-record 工作流对照。

## 标注和 Manifest

### `manual_annotations.json`

保存检查状态和人工编辑内容，便于复现：

- image ROI；
- markers；
- image centers；
- LiDAR ROI；
- LiDAR centers；
- provisional transforms；
- status 和 inclusion flags。

### `run_manifest.json`

顶层运行摘要：

- 输入 dataset root；
- 输出 result root；
- 优化样本 id；
- RMSE 和残差；
- `T_cam_lidar`；
- 相机内参；
- 标定板配置；
- 残差表和报告路径；
- 每个 sample 的可视化路径。

审查一个结果目录时，建议先看这个文件。

## 残差表

### `residual_summary.csv`

按 sample 汇总的 residual table，包含重投影误差统计。它适合快速判断某个 sample 是否主导最终误差。

### `per_point_residuals.csv`

逐对应点 residual table。它适合定位某个中心编号的投影或米制残差是否明显异常。

## 可视化诊断

`samples/` 目录包含每个优化样本的审查 artifacts：

| 文件 | 作用 |
|---|---|
| `*_image_features.png` | 图像 marker、board ROI、检测到的孔圆和中心编号 |
| `*_reprojection.png` | 使用优化后 transform 的密集投影 overlay |
| `*_reprojection_error.png` | 带残差线和误差标注的中心重投影视图 |
| `*_colored_cloud.pcd` | 可用时导出的投影/审查着色点云 |

这些文件用于在不重新打开 GUI 的情况下审查结果。

## 报告

### `report.md`

Markdown 摘要，适合版本管理、issue 讨论和纯文本审查。

### `report.html`

HTML 摘要，适合本地浏览器查看。它链接同一批 artifacts，并展示最终外参和残差诊断。

## 运行日志

### `run.log`

记录 dataset 路径、优化样本 id、RMSE，以及关键运行说明：GUI 使用离线 image/PCD samples，不读取 rosbag。

## 结果审查清单

接受一个标定结果前，请检查：

- `sample_ids` 是否是预期的 ready samples；
- `rmse_m` 和每个 sample 的像素误差是否合理；
- 打开每个 optimized sample 的 `*_reprojection_error.png`；
- 确认 transform 方向是 `T_cam_lidar`；
- 只有在确认下游约定后，再对比 `extrinsic.yaml` 和 `extrinsic_fastlivo2.txt`。

