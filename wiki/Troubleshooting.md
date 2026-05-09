# Troubleshooting

[English](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Troubleshooting) | [中文](https://github.com/JokerJohn/Manual_Calibration_Tools/wiki/Troubleshooting-zh)

## Open Dataset Fails

Check that you selected `dataset_root`, not the flat source folder, not `samples/`, and not a single sample folder.

Expected:

```text
dataset_root/
  samples/
  camera_intrinsics.yaml
  board_config.yaml
```

## Sample Is Invalid

Common causes:

- no allowed `image.*` file;
- missing `cloud.pcd`;
- broken symlink;
- malformed `meta.json`.

Regenerate the dataset with `--copy` if symlinks are not stable.

## Camera Features Fail

Check:

- image ROI includes the board;
- ArUco markers are visible and not heavily blurred;
- camera intrinsics match the image;
- `min_detected_markers` is realistic for the current image.

## LiDAR Holes Fail

Check:

- LiDAR ROI contains the board and excludes most background;
- `Detect Plane` succeeds before hole detection;
- point density is enough around all four holes;
- ROI was refined in the right fixed-axis views.

## Projection Looks Wrong

Check transform direction first. The expected convention is `T_cam_lidar`. If the file stores `T_lidar_cam`, use inverse direction for review.

Also check whether the image was already undistorted while `camera_intrinsics.yaml` still contains raw distortion coefficients.

## Optimization Is Blocked

`Optimize Included` requires at least two valid, included, `Ready` samples with four image centers and four LiDAR centers each.

