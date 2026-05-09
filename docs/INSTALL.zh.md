# 安装与环境配置

本文档说明 Manual Calibration Tools v1 的目标运行环境。当前仓库暂未发布公开源码，因此下面的命令描述的是私有 beta 的运行形态和未来公开版本预期接口。

## 当前状态

- 当前仓库状态：文档预览。
- 当前可运行实现：私有的 `fast_calib_offline_gui` 实现。
- 公开源码发布：计划中。
- v1 范围：基于离线 image/PCD sample 的 MID360 FAST-Calib GUI。

GUI 不直接读取原始 rosbag。需要先准备离线 dataset，然后在 GUI 中打开 `dataset_root`。

## 推荐系统

| 组件 | 推荐配置 |
|---|---|
| 操作系统 | Ubuntu 20.04 或 22.04 桌面环境 |
| Python | 3.10+ |
| GUI | PySide6 |
| 图像处理 | 带 ArUco 支持的 OpenCV |
| 点云处理 | Open3D |
| 数值计算 | NumPy |
| 配置文件 | PyYAML |
| 显示环境 | 本地图形桌面或配置正确的远程显示 |

当前私有实现的依赖集合来自 FAST-Calib workspace 中已有的 offline GUI requirements。未来公开包会保持同样的 Python-first 设计。

## 未来公开版本的安装形态

源码发布后，普通本地安装预计如下：

```bash
git clone git@github.com:JokerJohn/Manual_Calibration_Tools.git
cd Manual_Calibration_Tools

python3 -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

启动 GUI：

```bash
python -m fast_calib_offline_gui gui
```

准确的 `requirements.txt` 和 package entry points 会随源码一起发布。

## 私有 beta 的运行形态

当前实现中的功能入口如下：

```bash
python -m fast_calib_offline_gui prepare-flat \
  --source-root /path/to/flat_mid360_data \
  --output-root /path/to/offline_dataset

python -m fast_calib_offline_gui gui
```

当前实现还包含用于创建 conda 环境和启动 GUI 的辅助脚本。这些脚本暂未包含在本公开文档预览仓库中。

## 启动前的数据准备

如果源数据是扁平目录：

```text
flat_mid360_data/
  mid22.png
  mid22.pcd
  mid33.png
  mid33.pcd
```

需要先转换成：

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

只有在需要自包含数据集时才使用 `--copy`。如果原始磁盘稳定挂载，默认软链接方式更适合较大的 PCD 文件。

## 启动检查清单

点击 `Open Dataset` 前请确认：

- 选择的文件夹内部有 `samples/`；
- 每个 sample 有 `image.*` 和 `cloud.pcd`；
- `camera_intrinsics.yaml` 存在，或者已有可靠 fallback config；
- `board_config.yaml` 存在，或者接受默认标定板参数；
- 当前桌面有可用显示环境；
- OpenCV 可以导入 `cv2.aruco`；
- Open3D 可以读取 PCD 文件。

依赖快速检查：

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

## 常见环境问题

| 现象 | 常见原因 | 处理方式 |
|---|---|---|
| `ModuleNotFoundError: PySide6` | GUI 包未安装 | 在当前环境安装 PySide6 |
| 缺少 `cv2.aruco` | OpenCV 包不含 contrib/ArUco | 换用带 ArUco 支持的 OpenCV |
| Qt xcb/platform plugin 报错 | 显示环境或 Qt platform 配置异常 | 使用本地图形桌面，或正确配置 X11 forwarding |
| Open3D 读不到 PCD | 软链接失效或文件格式异常 | 检查 `readlink -f`，必要时用 `--copy` 重新生成 |
| `Offline dataset must contain a samples/ directory` | 选错了文件夹 | 打开 `dataset_root`，不要打开扁平源目录 |

## v1 GUI 不需要什么

v1 GUI 路径是 Python-first。普通离线 GUI 检查不要求用户构建 ROS workspace、运行 catkin，或启动直接处理 rosbag 的节点。原始 rosbag 的导出可以作为上游数据准备步骤，但不是 GUI 的直接输入约定。

