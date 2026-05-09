# FAST-Calib Workflow

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/FAST-Calib-Workflow) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/FAST-Calib-Workflow-zh)

The v1 workflow mirrors the existing `fast_calib_offline_gui` implementation.

## Main Actions

- `Open Dataset`: scan a prepared dataset root.
- `Detect Camera Features`: detect ArUco board markers and camera-side hole centers.
- `Detect Plane`: validate the LiDAR target plane in the active ROI.
- `Detect Holes`: extract four LiDAR-side hole centers.
- `Evaluate Batch`: read-only diagnostic for the included sample set.
- `Optimize Included`: solve `T_cam_lidar` from ready samples.
- `Export Result`: copy the latest result tree.

## Ready Rule

`Optimize Included` requires at least two samples that are:

- valid;
- included;
- manually accepted as `Ready`;
- complete with four image centers and four LiDAR centers.

## Solver

The current solver stacks all accepted center correspondences and solves one rigid transform with SVD. The result convention is `T_cam_lidar`.

## Output

The export path writes extrinsic files, center records, annotation state, residual tables, per-sample visualization images, `report.md`, `report.html`, `run_manifest.json`, and `run.log`.

Full details are in [docs/WORKFLOW.md](https://github.com/JokerJohn/Manual_Calibration_Tools/blob/master/docs/WORKFLOW.md).

