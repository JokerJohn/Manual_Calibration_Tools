# FAST-Calib 工作流

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/FAST-Calib-Workflow) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/FAST-Calib-Workflow-zh)

v1 工作流与现有 `fast_calib_offline_gui` 实现保持一致。

## 主要操作

- `Open Dataset`：扫描整理好的 dataset root。
- `Detect Camera Features`：检测 ArUco 标定板 markers 和相机侧孔心。
- `Detect Plane`：验证 active ROI 内的 LiDAR 目标平面。
- `Detect Holes`：提取四个 LiDAR 侧孔心。
- `Evaluate Batch`：对 included sample set 做只读诊断。
- `Optimize Included`：从 ready samples 求解 `T_cam_lidar`。
- `Export Result`：复制最新结果树。

## Ready 规则

`Optimize Included` 至少需要两个满足以下条件的 samples：

- valid；
- included；
- 人工接受为 `Ready`；
- 具有四个 image centers 和四个 LiDAR centers。

## 求解器

当前求解器会汇总所有已接受的中心对应关系，并使用 SVD 求解一个刚体变换。结果约定为 `T_cam_lidar`。

## 输出

export 路径会写出外参文件、中心记录、标注状态、残差表、每个 sample 的可视化图、`report.md`、`report.html`、`run_manifest.json` 和 `run.log`。

完整说明见 [docs/WORKFLOW.zh.md](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/WORKFLOW.zh.md)。

