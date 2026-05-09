# 路线图

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Roadmap) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Roadmap-zh)

## v1：FAST-Calib Offline GUI

状态：私有 beta / 文档预览。

已明确方向：

- 准备好的 image/PCD sample 输入；
- 相机和 LiDAR 特征检查；
- batch diagnostics；
- SVD 求解 `T_cam_lidar`；
- 可审查的报告和可视化输出。

## v1.x：打包

计划：

- 公开源码包；
- 可复现环境文件；
- 示例数据说明；
- 在安全可公开时加入截图或演示视频；
- import 和文档检查。

## v2：标定板 Profiles

计划：

- 在当前 FAST-Calib board 之外增加命名 target profiles；
- 可配置标定板几何；
- 更清楚地区分 detector/solver/export profile。

## v3：更多标定任务

计划：

- 更多 camera-LiDAR 标定变体；
- 未来多相机或多 LiDAR 工作流；
- project/session 管理；
- 下游导出 profiles。

