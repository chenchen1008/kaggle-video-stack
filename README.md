# kaggle-video-stack

把 `kaggle-minimax` 的引擎脚本和它依赖的 `Wan2GP` 底座打包进同一个仓库，用来固定版本、离线复用、或者二次分发。

## 目录

| 路径 | 内容 | 版本 |
|---|---|---|
| `engine/` | kaggle-minimax 全部脚本（H3、LTX-2.3、LTX-2.5、下载/环境工具） | `5660a6b`（2026-11-17 `update`） |
| `vendor/wan2gp/` | Wan2GP 运行时（已剔除 `.git`、`docs/`、`icons/`、`installation/`、Dockerfile） | `b8b18f8`（2026-10-01） |
| `notebooks/` | 三册 Kaggle notebook（H3 / LTX-2.3 / LTX-2.5） | 与本仓库同步 |

每个子项目的版本信息也写在各自的 `COMMIT` 文件里。

## 版本约束（很重要）

| 引擎 | 需要锁定的 Wan2GP commit | 本仓库 |
|---|---|---|
| `engine/frontend.py` + `engine/backend.py`（H3） | `b8b18f8` | ✅ 已内置 |
| `engine/run_ltx25.py`（LTX-2.5） | `b8b18f8` | ✅ 已内置 |
| `engine/run_ltx23.py`（LTX-2.3） | `b8880bd`（2026-05-13） | ❌ 需另取 |

LTX-2.3 换到 `b8b18f8` 会在 FP16 下溢出、出黑视频，所以它的目录必须单独取：

```bash
git clone --depth 1 https://github.com/DeepBeepMeep/Wan2GP.git Wan2GP-2.3
cd Wan2GP-2.3 && git fetch --depth 1 origin b8880bd5d292edfbe2384f5c951d95757ad59ea0 \
  && git reset --hard FETCH_HEAD
```

## 用法

引擎按相对路径找底座（H3 用 `../Wan2GP`，LTX 用 `./Wan2GP`），所以在仓库根建一个软链即可两种布局都成立：

```bash
ln -s vendor/wan2gp Wan2GP

# H3
cd engine && python frontend.py

# LTX（回到仓库根）
python engine/run_ltx23.py     # 需要把 Wan2GP 换成 b8880bd
python engine/run_ltx25.py
```

依赖不是随仓库分发的，按对应 notebook 的 Step 1 装即可（`mmgp==3.8.2`、`comfy-kitchen==0.2.35`、`transformers==4.54.0`、`gradio==5.29.0` …）。

## 和 notebooks 的关系

`notebooks/` 里的三册**默认仍然 clone 原仓库并单独固定 Wan2GP**，不依赖本仓库，可以直接跑。

想把它们切到本仓库（离线/固定版本），需要改三处：

```python
# Step 0
REPO_URL = "https://github.com/chenchen1008/kaggle-video-stack.git"
REPO_DIR = os.path.join(WORK, "kaggle-video-stack")

# Step 1b：不再单独拉 Wan2GP，改成软链仓库内的 vendor 副本
if not os.path.exists(WAN2GP_DIR):
    os.symlink(os.path.join(REPO_DIR, "vendor", "wan2gp"), WAN2GP_DIR)
```

```python
# Step 3
ENGINE = os.path.join(REPO_DIR, "engine", "frontend.py")        # H3
# ENGINE = os.path.join(REPO_DIR, "engine", "run_ltx25.py")     # LTX-2.5
```

LTX 那两册的 cwd 仍设 `WORK`，软链 `WAN2GP_DIR` 后 `./Wan2GP` 能正常解析；H3 的 `../Wan2GP` 也指向同一个软链。

## 许可

- `vendor/wan2gp/` 采用 **WanGP Community License 2.0**（见 `vendor/wan2gp/LICENSE.txt`）：可自用、可修改、可分发源码，但不能售卖 Wan2GP 本身或提供付费 API/SaaS；分发需保留许可证与声明。
- `engine/` 来自 `milleniummike-del/kaggle-minimax`，**原仓库没有声明许可证**。公开或商用前请先确认授权。
- 模型权重（MiniMax H3 / Qwen3-VL / LTX-2.x / Gemma）不在本仓库，各自遵循上游模型许可，详见 `THIRD_PARTY_NOTICES.md`。
