# 第三方组件与许可

本仓库只做分发与版本固定，不改变任何第三方组件的许可。分发或部署时请保留以下声明。

## 1. Wan2GP（vendor/wan2gp）

- 上游：https://github.com/DeepBeepMeep/Wan2GP
- 版本：`b8b18f8114e432eea8f3d7e853a51dd91fa99571`（2026-10-01）
- 许可：**WanGP Community License 2.0**，全文见 `vendor/wan2gp/LICENSE.txt`
- 要点：允许自用、修改与源码分发；**禁止**售卖 Wan2GP 本身或基于它提供付费 API / SaaS / 托管服务；分发时必须保留许可证原文与版权声明。
- 已剔除：`.git`、`docs/`、`icons/`、`installation/`、`favicon.png`、`Dockerfile`、`.github/`。代码本身未改动。

## 2. kaggle-minimax（engine/）

- 上游：https://github.com/milleniummike-del/kaggle-minimax
- 版本：`5660a6bbe04db074d1c2ece671ad13a56849ed8e`（2026-11-17）
- 许可：**上游未声明许可证**。在获得作者授权前，仅限本地/内部研究使用，不要公开发布或商用。

## 3. pip 依赖中值得注意的

| 包 | 许可 | 说明 |
|---|---|---|
| `comfy-kitchen` | 见 PyPI 页面 | ComfyUI 拆出的 INT8/FP8 内核库，H3 与 LTX-2.5 的 W8A8 依赖它 |
| `mmgp` | MIT | Wan2GP 作者的分层卸载库 |
| `transformers` / `diffusers` | Apache-2.0 | |
| `optimum-quanto` | Apache-2.0 | LTX 2.3 权重的 quanto 量化格式 |

## 4. 模型权重（不在本仓库）

| 权重 | 来源 | 许可提示 |
|---|---|---|
| MiniMax H3 DiT（pruned + int8_convrot） | `DeepBeepMeep/MiniMax-H3` | 派生自 MiniMax H3，遵循上游模型许可 |
| Qwen3-VL-32B GGUF | 同上 | 遵循 Qwen 许可 |
| PDMD 4NFE LoRA | `pdmd2026/pdmd_4NFE_lora` | 社区 LoRA |
| LTX-2.3 / 2.5 全套 | `DeepBeepMeep/LTX-2` | 派生自 Lightricks LTX-2，遵循上游许可 |
| Gemma-3/4 12B | 同上 | 遵循 Gemma Terms of Use |

需要公开发布时，请自行核对上述模型许可对再分发的限制。
