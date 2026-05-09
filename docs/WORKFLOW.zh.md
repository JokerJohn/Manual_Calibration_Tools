# GUI 使用流程

本文档描述当前 FAST-Calib offline GUI 的真实行为，来源于已有实现，而不是重新设计的一套流程。

## 1. 准备离线数据集

先把扁平的图像/PCD 对转换成 offline dataset：

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset
```

在 GUI 中打开 `/path/to/offline_dataset`。不要打开扁平源目录、`samples/` 子目录或单个 sample 目录。

## 2. Open Dataset

点击 `Open Dataset`，选择 `dataset_root`。

loader 会扫描：

- `samples/<sample_id>/image.*`；
- `samples/<sample_id>/cloud.pcd`；
- 可选的 `samples/<sample_id>/meta.json`；
- `camera_intrinsics.yaml`；
- `board_config.yaml`。

invalid sample 仍会显示，但在缺失文件或配置问题修复前不能参与优化。

## 3. 检查单个 Sample

在 sample rail 中选择一个样本。GUI 会显示图像视图、点云视图、投影/重投影诊断、对应关系表、batch impact tab 和 run log。

重要的 sample 状态保存在 `SampleAnnotation` 中，包括：

- 图像 board ROI；
- 检测到的 ArUco markers；
- 相机侧 3D 孔心；
- 图像平面孔心；
- LiDAR ROI；
- LiDAR 平面和孔心；
- provisional transform；
- 像素残差和米制残差；
- included/locked/status 标记。

## 4. Detect Camera Features

先在图像视图中绘制 board ROI，然后点击 `Detect Camera Features`。

当前检测器会：

- 如果提供了 image ROI，则只在 ROI 内检测；
- 检测 ArUco markers；
- 使用相机内参估计标定板位姿；
- 投影四个已知标定板孔心；
- 记录 camera-frame 孔心和 image-space 孔心；
- 绘制 marker 边框、marker ID、孔圆和中心编号。

如果检测到的 board markers 太少，该 sample 需要人工检查，不能直接成为 `Ready`。

## 5. Detect LiDAR Plane And Holes

在点云视图中绘制或细化 LiDAR ROI，然后依次运行：

1. `Detect Plane`：确认 active ROI 内存在目标平面。
2. `Detect Holes`：提取四个 LiDAR 侧孔心。

GUI 会先按 active ROI 裁剪点云，再做平面/圆孔提取。ROI 外的背景点不应参与特征求解。

## 6. 估计并检查投影

当 sample 同时具有四个 camera centers 和四个 LiDAR centers 后，可以估计 sample pose 或使用 primary next-action button。重点检查：

- 中心编号；
- 中心对应关系；
- 投影 overlay；
- 每个点的像素残差；
- 米制残差；
- sample message 和 status。

投影来源优先级：

1. 最新 batch optimized `T_cam_lidar`；
2. 当前 sample provisional pose；
3. 用户加载或手动提供的 initial pose；
4. unavailable。

如果投影为空或完全错误，优先检查方向。当前约定是 `T_cam_lidar`。

## 7. 标记 Ready Samples

一个完整 sample 需要：

- image 和 PCD 文件有效；
- 四个 camera-side hole centers；
- 四个 LiDAR-side hole centers；
- 编号和对应关系已确认；
- 人工接受为 `Ready`；
- 如果要参与 batch optimization，则 `included` 保持开启。

`Optimize Included` 只有在至少两个 checked samples 为 `Ready`、included 且 complete 时才允许执行。

## 8. Evaluate Batch

`Evaluate Batch` 是只读诊断功能，用来决定哪些 ready samples 应该保留在最终优化集合中。

它会报告：

- 当前 included set 的 mean/RMSE/max reprojection error；
- 每个 sample 在可用时的 single-sample pose error；
- 添加某个未勾选 ready sample 后的 batch error；
- 移除某个已勾选 ready sample 后的 batch error；
- `Keep`、`Review`、`Try Include`、`Exclude` 或 `Need more samples` 等建议。

它不会导出文件，也不会改变 sample status。

## 9. Optimize Included

included ready set 明确后，点击 `Optimize Included`。

当前优化器会：

- 校验样本数量和状态；
- 要求至少两个 ready samples；
- 要求每个被选 sample 有四个 image centers 和四个 LiDAR centers；
- 汇总所有选中样本的中心对应关系；
- 使用 SVD 求解刚体变换；
- 将 `T_cam_lidar`、残差、诊断和报告写入默认结果目录。

## 10. Export Result

`Optimize Included` 会写默认结果树。`Export Result` 用于把最新结果树复制到用户选择的位置。

只有成功优化后才能执行 `Export Result`。如果当前没有结果，GUI 会提示：

```text
Run Optimize Included before exporting a result.
```

## 阻塞规则

常见阻塞信息对应的修复方向：

| 信息 | 含义 |
|---|---|
| `Select at least 2 ready samples` | 至少加入两个已接受样本 |
| `sample status is ...; mark it Ready before optimization` | 完成检查并显式标记 Ready |
| `expected 4 image hole centers` | 重新检测或手动修正图像侧特征 |
| `expected 4 LiDAR hole centers` | 细化 ROI 并重新检测 LiDAR 孔心 |
| `invalid sample` | 修复缺失图像/PCD 或数据目录结构 |

