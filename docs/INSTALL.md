# Installation And Environment

This page documents the intended runtime environment for Manual Calibration Tools v1. The public source code is not released yet, so the commands below describe the private beta shape and the expected public release interface.

## Status

- Current repository state: documentation preview.
- Current runnable implementation: private `fast_calib_offline_gui` implementation.
- Public source release: planned.
- v1 scope: offline MID360 FAST-Calib GUI using prepared image/PCD samples.

The GUI does not read raw rosbag files directly. Prepare the offline dataset first, then open `dataset_root` in the GUI.

## Recommended System

| Component | Recommended value |
|---|---|
| OS | Ubuntu 20.04 or 22.04 desktop |
| Python | 3.10+ |
| GUI | PySide6 |
| Image processing | OpenCV with ArUco support |
| Point cloud | Open3D |
| Numerics | NumPy |
| Config | PyYAML |
| Display | Local X11/Wayland desktop or properly forwarded remote display |

For the current private implementation, the dependency set follows the existing offline GUI requirements under the FAST-Calib workspace. The public package will keep the same Python-first design.

## Expected Public Install Shape

After source release, a normal local workflow should look like:

```bash
git clone git@github.com:JokerJohn/Manual_Calibration_Tools.git
cd Manual_Calibration_Tools

python3 -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

Then launch:

```bash
python -m fast_calib_offline_gui gui
```

The exact `requirements.txt` and package entry points will be published with the source code.

## Private Beta Runtime Shape

The existing implementation uses the following functional entry points:

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset

python -m fast_calib_offline_gui gui
```

Equivalent helper scripts in the current implementation prepare a dedicated conda environment and start the GUI. These scripts are not included in this documentation-preview repository yet.

## Data Preparation Before Launch

If the source data is a flat directory:

```text
flat_mid360_data/
  mid22.png
  mid22.pcd
  mid33.png
  mid33.pcd
```

convert it into:

```text
offline_dataset/
  samples/
    mid22/
      image.png
      cloud.pcd
      meta.json
    mid33/
      image.png
      cloud.pcd
      meta.json
  camera_intrinsics.yaml
  board_config.yaml
```

Use `--copy` only when the offline dataset must be self-contained. The default symlink strategy is better for large PCD files when the source disk remains mounted.

## Launch Checklist

Before clicking `Open Dataset`, confirm:

- the selected folder contains `samples/`;
- each sample has `image.*` and `cloud.pcd`;
- `camera_intrinsics.yaml` exists or a known fallback config is available;
- `board_config.yaml` exists or default board values are acceptable;
- the desktop has a valid display environment;
- OpenCV can import `cv2.aruco`;
- Open3D can read the PCD files.

Quick dependency probe:

```bash
python - <<'PY'
import cv2
import numpy
import open3d
import PySide6
import yaml
print("cv2", cv2.__version__)
print("open3d", open3d.__version__)
print("PySide6", PySide6.__version__)
print("ok")
PY
```

## Common Environment Issues

| Symptom | Likely cause | Fix |
|---|---|---|
| `ModuleNotFoundError: PySide6` | GUI package missing | Install PySide6 in the active environment |
| `cv2.aruco` missing | OpenCV build lacks contrib modules | Use an OpenCV package with ArUco support |
| Qt xcb/platform plugin error | Display or Qt platform setup problem | Start from a desktop session, or configure X11 forwarding correctly |
| Open3D cannot read PCD | broken symlink or unsupported file | verify `readlink -f`, regenerate with `--copy` if needed |
| `Offline dataset must contain a samples/ directory` | wrong folder selected | open `dataset_root`, not the flat source folder |

## What Is Not Required For V1 GUI Use

The v1 GUI path is Python-first. It should not require users to build a ROS workspace, run catkin, or launch a rosbag-processing node for normal offline GUI review. Raw rosbag extraction can be part of an upstream data-preparation process, but it is not the GUI's direct input contract.

