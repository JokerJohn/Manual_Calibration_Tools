# Manual Calibration Tools

<p align="center">
  <a href="README.md">English</a> | <a href="README.zh.md"><strong>中文</strong></a>
</p>

<p align="center">
  <img src="assets/hero.svg" alt="Manual Calibration Tools hero" width="100%" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/status-docs--preview-orange.svg" alt="Docs preview" />
  <img src="https://img.shields.io/badge/source-private--beta-lightgrey.svg" alt="Private beta source" />
  <img src="https://img.shields.io/badge/python-3.10%2B-blue.svg" alt="Python 3.10+" />
  <img src="https://img.shields.io/badge/GUI-PySide6-2E8B57.svg" alt="PySide6 GUI" />
  <img src="https://img.shields.io/badge/vision-OpenCV-5C3EE8.svg" alt="OpenCV" />
  <img src="https://img.shields.io/badge/point%20cloud-Open3D-0B7285.svg" alt="Open3D" />
  <img src="https://img.shields.io/badge/future%20source-GPLv3-blue.svg" alt="Future GPLv3 source" />
</p>

<p align="center">
  <strong>作者</strong>：
  <a href="https://github.com/JokerJohn">Xiangcheng HU</a>、
  <a href="https://github.com/zarathustr">Jin Wu</a> 和
  <a href="https://github.com/Chen-Xieyuanli">Xieyuanli Chen</a><br/>
  <strong>联系邮箱</strong>：<a href="mailto:xhubd@connect.ust.hk">xhubd@connect.ust.hk</a>
</p>

<p align="center">
  <strong>相关链接</strong>：
  <a href="https://github.com/hku-mars/FAST-Calib?tab=readme-ov-file">FAST-Calib</a>
  ·
  <a href="https://github.com/ethz-asl/kalibr">Kalibr</a>
</p>

面向相机-激光雷达以及后续多传感器外参标定任务的 Python 优先标定 GUI。

> **文档预览仓库。** 当前暂未公开源码。本仓库用于介绍私有 `fast_calib_offline_gui` 实现的产品工作流、效果展示、文档和路线图。

## 项目目标

标定精度往往不只取决于最后的优化器，更取决于进入优化器之前的观测质量：准确的 2D 像素特征点、准确的 3D 点，以及可靠的对应关系。

Manual Calibration Tools 聚焦在这个观测层：在接受最终外参前，让特征关联过程可见、可编辑、可量化。

## 效果预览

<p align="center">
  <img src="assets/screenshots/gui_workbench.png" alt="Manual Calibration Tools GUI workbench" width="100%" />
</p>

## 热点功能

<p align="center">
  <img src="assets/demos/edit_circle.gif" alt="Editing image circle centers with live reprojection error review" width="100%" />
</p>

核心交互是手动细调图像圆心：选中某个圆心，用键盘精细移动像素位置，并立即查看重投影误差变化，类似测绘和摄影测量软件中的特征点选取流程。

## 主要功能

| 功能 | 说明 |
|---|---|
| FAST-Calib v1 工作流 | 产品化当前离线 MID360 image/PCD sample workflow |
| 交互式特征检查 | 检查图像特征、LiDAR ROI、目标平面、孔心和对应关系 |
| 可编辑 2D 中心点 | 手动修正像素中心点，并直接观察重投影误差变化 |
| 投影诊断 | 在接受样本前检查密集投影 overlay 和中心残差 |
| Batch sample evaluation | 最终优化前比较 included samples |
| 可审查导出 | 保存外参、残差表、可视化诊断、manifest 和报告 |

## 结果展示

<p align="center">
  <img src="assets/screenshots/image_features.png" alt="Detected image features" width="32%" />
  <img src="assets/screenshots/reprojection_error.png" alt="Reprojection error view" width="32%" />
  <img src="assets/screenshots/projection_overlay.png" alt="Projection overlay" width="32%" />
</p>

## 文档入口

| 主题 | 链接 |
|---|---|
| 快速开始 | [Wiki: Quick Start](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Quick-Start-zh) |
| 数据格式 | [Wiki: Dataset Format](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Dataset-Format-zh) |
| FAST-Calib 工作流 | [Wiki: FAST-Calib Workflow](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/FAST-Calib-Workflow-zh) |
| 故障排查 | [Wiki: Troubleshooting](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Troubleshooting-zh) |
| 详细文档 | [docs/](docs/) |

## 路线图

| 阶段 | 范围 | 状态 |
|---|---|---|
| v1 | FAST-Calib 离线 MID360 GUI 工作流 | 私有 beta / 文档预览 |
| v1.x | 打包、示例和报告优化 | 计划中 |
| v2 | 更多标定板 profiles | 计划中 |
| v3 | 更多外参标定任务 | 计划中 |

## 许可证与源码状态

当前仓库暂不包含公开源码。未来源码发布计划采用 GNU GPLv3。
