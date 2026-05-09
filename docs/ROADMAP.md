# Roadmap

Manual Calibration Tools starts from the existing FAST-Calib offline GUI and grows toward a broader calibration workbench. The roadmap is intentionally staged so public claims stay aligned with implemented behavior.

## v1: FAST-Calib Offline GUI

Status: private beta / documentation preview.

Scope:

- offline MID360 image/PCD sample workflow;
- prepared `dataset_root` input;
- camera ROI and ArUco-based board pose;
- LiDAR ROI, plane, and hole-center review;
- sample status and manual acceptance;
- read-only batch impact evaluation;
- rigid SVD solve for `T_cam_lidar`;
- exported extrinsics, residual CSVs, visual diagnostics, manifest, and reports.

## v1.x: Packaging And Examples

Planned:

- publish the source package when ready;
- provide reproducible environment files;
- add a minimal public example dataset or dataset-preparation guide;
- polish report templates;
- add CI checks for import, dataset scan, and docs links;
- add screenshots or demo videos once public artifacts are safe to share.

## v2: Calibration-Board Expansion

Planned:

- explicit target profiles beyond the current FAST-Calib board;
- configurable board geometry and marker layouts;
- clearer separation between detector profile, solver profile, and export profile;
- richer manual annotation persistence.

Each new board workflow should be introduced as a named profile rather than changing the v1 FAST-Calib contract.

## v3: Broader Extrinsic Calibration Tasks

Planned:

- additional camera-LiDAR calibration variants;
- multi-camera or multi-LiDAR extensions where the data model supports them;
- improved project/session management;
- cross-run comparison and quality dashboards;
- downstream export profiles for common robotics stacks.

The product principle remains the same: expose the evidence that makes a calibration credible, not just the final matrix.
