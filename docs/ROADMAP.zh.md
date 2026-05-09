# 路线图

Manual Calibration Tools 从现有 FAST-Calib offline GUI 出发，逐步扩展成更完整的标定工作台。路线图采用分阶段方式，保证公开说明和已实现行为一致。

## v1：FAST-Calib Offline GUI

状态：私有 beta / 文档预览。

范围：

- 离线 MID360 image/PCD sample workflow；
- 已整理好的 `dataset_root` 输入；
- 图像 ROI 和基于 ArUco 的标定板位姿；
- LiDAR ROI、平面和孔心检查；
- sample status 和人工接受；
- 只读 batch impact evaluation；
- 用刚体 SVD 求解 `T_cam_lidar`；
- 导出外参、残差 CSV、可视化诊断、manifest 和报告。

v1 公开文档不做这些承诺：

- 不宣称直接读取 rosbag；
- 不宣称源码已经公开；
- 不宣称支持所有标定板方案；
- 不宣称已经是通用多传感器标定套件。

## v1.x：打包与示例

计划：

- 准备好后发布源码包；
- 提供可复现的环境文件；
- 增加最小公开示例数据或数据准备指南；
- 优化报告模板；
- 增加 import、dataset scan 和 docs link 的 CI 检查；
- 在安全可公开时增加截图或演示视频。

## v2：标定板方案扩展

计划：

- 在当前 FAST-Calib board 之外增加明确的 target profiles；
- 支持可配置的标定板几何和 marker layout；
- 更清楚地区分 detector profile、solver profile 和 export profile；
- 增强人工标注持久化。

每个新的标定板工作流都应作为命名 profile 加入，而不是静默改变 v1 FAST-Calib 约定。

## v3：更多外参标定任务

计划：

- 增加更多 camera-LiDAR 标定变体；
- 在数据模型支持后扩展到多相机或多 LiDAR；
- 改进 project/session 管理；
- 增加跨运行对比和质量看板；
- 为常见机器人栈提供更多下游导出 profile。

产品原则保持不变：不仅给出最终矩阵，还要暴露使标定结果可信的证据。

