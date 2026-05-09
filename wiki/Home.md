# Manual Calibration Tools Wiki

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Home) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Home-zh)

Manual Calibration Tools is a Python-first calibration GUI project. The first public product direction is based on the existing FAST-Calib offline GUI workflow for prepared image/PCD samples.

**Author**: [Xiangcheng HU](https://github.com/JokerJohn)  
**Contact**: [xhubd@connect.ust.hk](mailto:xhubd@connect.ust.hk)

> Current status: documentation preview. The public source code is not released yet.

<p align="center">
  <img src="https://raw.githubusercontent.com/JokerJohn/Manual_Calibration_Tools/master/assets/hero.svg" alt="Manual Calibration Tools hero" width="100%" />
</p>

## Start Here

- [Quick Start](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Quick-Start)
- [Dataset Format](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Dataset-Format)
- [FAST-Calib Workflow](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/FAST-Calib-Workflow)
- [Troubleshooting](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Troubleshooting)
- [Roadmap](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Roadmap)

## What V1 Does

The v1 workflow is an offline calibration workbench:

- prepare image/PCD samples from flat MID360 data;
- open a structured `dataset_root`;
- review image and point cloud samples side by side;
- detect camera-side board features and LiDAR-side hole centers;
- inspect projection and residual diagnostics;
- evaluate included samples before optimization;
- solve `T_cam_lidar`;
- export extrinsics, residuals, reports, and visual artifacts.

## What V1 Does Not Claim

- It does not read raw rosbag files directly in the GUI.
- It does not publish source code yet.
- It does not claim general support for every calibration target or every multi-sensor task.

## Repository Links

- [Main README](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/README.md)
- [Chinese README](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/README.zh.md)
- [Installation](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/INSTALL.md)
- [Dataset Documentation](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/DATASET.md)
- [Workflow Documentation](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/WORKFLOW.md)
- [Outputs Documentation](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/OUTPUTS.md)

