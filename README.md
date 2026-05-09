# Manual Calibration Tools

<p align="center">
  <a href="README.md"><strong>English</strong></a> | <a href="README.zh.md">中文</a>
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
  <strong>Authors</strong>:
  <a href="https://github.com/JokerJohn">Xiangcheng HU</a>,
  <a href="https://github.com/zarathustr">Jin Wu</a> and
  <a href="https://github.com/Chen-Xieyuanli">Xieyuanli Chen</a><br/>
  <strong>Contact</strong>: <a href="mailto:xhubd@connect.ust.hk">xhubd@connect.ust.hk</a>
</p>

<p align="center">
  <strong>References</strong>:
  <a href="https://github.com/hku-mars/FAST-Calib?tab=readme-ov-file">FAST-Calib</a>
  ·
  <a href="https://github.com/ethz-asl/kalibr">Kalibr</a>
</p>

Python-first calibration GUI for camera-LiDAR and future multi-sensor extrinsic calibration workflows.

> **Documentation preview.** The public source code is not released yet. This repository currently introduces the product workflow, visual examples, documentation, and roadmap for the private `fast_calib_offline_gui` implementation.

## Why

Calibration accuracy is often limited less by the final optimizer than by the observations entering it: accurate 2D pixel features, accurate 3D points, and reliable correspondences.

Manual Calibration Tools focuses on that observation layer. It makes feature association visible, editable, and measurable before the final extrinsic is accepted.

## Preview

<p align="center">
  <img src="assets/screenshots/gui_workbench.png" alt="Manual Calibration Tools GUI workbench" width="100%" />
</p>

## Highlight

<p align="center">
  <img src="assets/demos/edit_circle.gif" alt="Editing image circle centers with live reprojection error review" width="100%" />
</p>

Manual image-center refinement is the key interaction. Select a circle center, nudge its pixel location with keyboard precision, and immediately evaluate the reprojection error, similar to feature-picking workflows in survey and photogrammetry software.

## Key Features

| Feature | Description |
|---|---|
| FAST-Calib v1 workflow | Productizes the current offline MID360 image/PCD sample workflow |
| Interactive feature review | Inspect image features, LiDAR ROI, target plane, hole centers, and correspondences |
| Editable 2D centers | Manually refine pixel centers and watch reprojection error change directly |
| Projection diagnostics | Review dense projection overlays and center residuals before accepting samples |
| Batch sample evaluation | Compare included samples before final optimization |
| Auditable export | Save extrinsics, residual tables, visual diagnostics, manifests, and reports |

## Result Gallery

<p align="center">
  <img src="assets/screenshots/image_features.png" alt="Detected image features" width="32%" />
  <img src="assets/screenshots/reprojection_error.png" alt="Reprojection error view" width="32%" />
  <img src="assets/screenshots/projection_overlay.png" alt="Projection overlay" width="32%" />
</p>

## Documentation

| Topic | Link |
|---|---|
| Quick start | [Wiki: Quick Start](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Quick-Start) |
| Dataset format | [Wiki: Dataset Format](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Dataset-Format) |
| FAST-Calib workflow | [Wiki: FAST-Calib Workflow](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/FAST-Calib-Workflow) |
| Troubleshooting | [Wiki: Troubleshooting](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Troubleshooting) |
| Detailed docs | [docs/](docs/) |

## Roadmap

| Stage | Scope | Status |
|---|---|---|
| v1 | FAST-Calib offline MID360 GUI workflow | Private beta / docs preview |
| v1.x | Packaging, examples, and report polish | Planned |
| v2 | Additional calibration-board profiles | Planned |
| v3 | Broader extrinsic calibration tasks | Planned |

## License And Source Availability

The public source code is not included in this repository yet. The intended source-code license for the future release is GNU GPLv3.
