# Manual Calibration Tools Wiki

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Home) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Home-zh)

Manual Calibration Tools 是一个 Python-first 标定 GUI 项目。第一阶段公开产品方向基于已有 FAST-Calib offline GUI，用于已经整理好的 image/PCD samples。

**作者**：[Xiangcheng HU](https://github.com/JokerJohn)、[Jin Wu](https://github.com/zarathustr)、[Xieyuanli Chen](https://github.com/Chen-Xieyuanli)  
**联系邮箱**：[xhubd@connect.ust.hk](mailto:xhubd@connect.ust.hk)

> 当前状态：文档预览。公开源码暂未发布。

<p align="center">
  <img src="https://raw.githubusercontent.com/JokerJohn/Manual_Calibration_Tools/master/assets/hero.svg" alt="Manual Calibration Tools hero" width="100%" />
</p>

## 从这里开始

- [快速开始](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Quick-Start-zh)
- [数据格式](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Dataset-Format-zh)
- [FAST-Calib 工作流](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/FAST-Calib-Workflow-zh)
- [故障排查](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Troubleshooting-zh)
- [路线图](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Roadmap-zh)

## v1 做什么

v1 是离线标定工作台：

- 从扁平 MID360 数据准备 image/PCD samples；
- 打开结构化 `dataset_root`；
- 并排检查图像和点云样本；
- 检测相机侧标定板特征和 LiDAR 侧孔心；
- 检查投影和残差诊断；
- 在优化前评估 included samples；
- 求解 `T_cam_lidar`；
- 导出外参、残差、报告和可视化 artifacts。

## 仓库链接

- [Main README](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/README.md)
- [中文 README](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/README.zh.md)
- [安装说明](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/INSTALL.zh.md)
- [数据文档](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/DATASET.zh.md)
- [工作流文档](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/WORKFLOW.zh.md)
- [输出文档](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/OUTPUTS.zh.md)
