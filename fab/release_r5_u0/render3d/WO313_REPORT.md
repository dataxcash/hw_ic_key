# WO313 — 连接器位姿重做报告（J1 前后翻正 + J2 实测对齐）

- 日期：2026-11-08
- 源板（只读，未改）：`out/r5_u0_63p4mm_release.kicad_pcb`
  - sha256：`0d3a08dca5c17d6d8db34b523969c183148c1529c44c8c61bde2182d9201c4a6`
  - 板框：32.000 × 63.400 mm nominal；DRC 0/0；u=0
- 渲染副本：`key_v2/fab/release_r5_u0/render3d/r5_u0_63p4mm_3d_render.kicad_pcb`
  - 只改 J1/J2/J3/D12 的 3D model offset/rotate；未动铜/网表/footprint 焊盘
- 本单未 commit。
- WO314 追加：四张总图按同一 mm 窗口 `x=-2…34 / y=-2…65.4` 裁切为 **2136×4000**（板框四边实测 118 px = 1.987 mm）；渲染副本未动。

## 0. 结论

| ref | 改前 transform | 改后 transform | 实测结果 | 判定 |
|---|---|---|---|---|
| **J1** | `offset (0,0,0) / rotate (0,0,0)`（插口反装） | `offset (-0.24, 0.40, 0) / rotate (0,0,180)` | 圆弧唇口朝板边；front edge y=0.653 mm；A/B 排 y=1.18/2.88 在模型轮廓内；body seat z=0.000 | **PASS** |
| **J2** | `offset (-2.9155,1.334,1.724) / rotate (90,-180,-90)`（左右偏 2.9 mm） | `offset (0, 1.644, 1.724) / rotate (90,-180,-90)` | visible bbox dC = **(-0.026, -0.013) mm**（≤0.2）；卡口 +Y 朝 63.4 端；开口面 y=56.055，距板端 7.345 mm；无伸出 | **PASS** |
| **J3** | `offset (0,1.27,0) / rotate (0,0,0)` | 不变 | visible bbox dC = (+0.049, -0.007) mm；本体 +Z 朝上；pin1 同侧 | **PASS** |
| **D12** | `offset (-0.175,0,0) / rotate (0,0,0)` | 不变 | visible bbox dC = (+0.213, +0.040) mm；offset y=0，不受 Y 反向影响；x 差为 LED 透镜外伸 | **ACCEPTED** |

> J2 对齐没有使用“包围盒中心 + 轴同向”公式；全部来自渲染可见轮廓 bbox 与 footprint 焊盘包络的实测差值。

## 1. J1 USB-C（A 段）

### 1.1 渲染实测迭代（3 轮）

| 轮 | offset (mm) | visible bbox 中心 (board mm) | vs pad envelope 中心 (16.000, 5.285) | 说明 |
|---|---|---|---|---|
| 1 | `(0, -0.895, 0)` | (16.321, 6.560) | (+0.321, +1.275) | rotate Z180 后首测；开口仍偏内，体中心偏后 |
| 2 | `(-0.33, -2.085, 0)` | (15.877, 7.069) | (-0.123, +1.784) | 发现 offset Y 与 footprint 局部 Y 反向；改向 |
| 3 | `(-0.24, 0.40, 0)` | (15.959, 6.085) | **(-0.041, +0.800)** | 开口到板边、焊盘落在轮廓内；停止微调（再上移会使唇口越板边） |

### 1.2 判据逐条

1. **板边端 = 圆弧唇口 + 开口，后壳朝板内**：PASS。`j1_closeup.png` 顶部为圆弧唇口/开口，底部为带台阶后壳。
2. **焊盘在开口下方、在模型轮廓内**：
   - A 排 y=1.18、B 排 y=2.88 均落在模型可见 y 范围 **0.653 … 11.516 mm** 内；
   - 模型 x 范围 11.152…20.766 mm vs 焊盘包络 11.110…20.890 mm；两侧极限差 ≤0.124 mm（渲染轮廓/抗锯齿量级），焊盘未被前/后方向暴露。
   - 判定 **PASS**（按“前/后方向是否露出 + 焊盘行在轮廓内”判）。
3. **z**：本体座面 z=0.000 mm（贴板）；shield legs z_min=-1.703 mm 为穿板屏蔽脚，属模型固有穿板结构，不是悬空/陷板。判定 **PASS（按本体座面）**。

> J1 visible bbox 中心 y 仍有 +0.800 mm，是因为模型后壳比焊盘包络向后多伸 1.953 mm；若强行把 visible bbox 中心对到焊盘中心，圆弧唇口会越过板边约 0.15 mm。故保留当前 offset，以“唇口在板边 + 焊盘在轮廓内”优先。

## 2. J2 M.2 M-Key（B 段）

### 2.1 渲染实测迭代（2 轮，≤3）

| 轮 | offset (mm) | visible bbox 中心 (board mm) | pad envelope 中心 (18.250, 51.900) | dC (mm) | 说明 |
|---|---|---|---|---|---|
| 1 | `(-2.9155, 1.334, 1.724)` | (21.149, 52.212) | (18.250, 51.900) | **(+2.899, +0.312)** | 旧变换左右偏 2.9 mm |
| 2 | `(0, 1.644, 1.724)` | (18.224, 51.887) | (18.250, 51.900) | **(-0.026, -0.013)** | PASS ≤0.2 mm |

- 模型可见 bbox：x 7.163…29.285 mm，y 47.720…56.055 mm；尺寸 22.122 × 8.335 mm。
- 焊盘包络：x 7.300…29.200 mm，y 47.350…56.450 mm；尺寸 21.900 × 9.100 mm。
- 卡口方向：**+Y**，朝 63.4 mm 板端；开口面 y=**56.055 mm**。
- 开口面到板边：63.400 − 56.055 = **7.345 mm**；不伸出板外。
- z：模型 z_min=0.000 mm（B 面贴板底）。
- pin1：pad1 board (27.5, 48.125)；模型 pin1 三角在 BTM 图左上（板 +X/−Y 角），同侧。
- **判定 PASS**。

## 3. J3 / D12 同 Y 反向核查

| ref | 改前 | 改后 | 实测 visible bbox dC | 判定 |
|---|---|---|---|---|
| J3 | offset `(0,1.27,0)` | 不变 | (+0.049, -0.007) mm | PASS；本体朝上，pin1 同侧；未受 Y 反向影响 |
| D12 | offset `(-0.175,0,0)` | 不变 | (+0.213, +0.040) mm | ACCEPTED；offset y=0，Y 反向不适用；x 差来自 LED 镜头外伸，非位姿翻转 |

## 4. 交付图

| 文件 | 尺寸 | sha256 |
|---|---|---|
| `render3d/r5_u0_top.png` | 2136×4000 | `48bfecaf2b2a18a796a7f91ad1b6acdf115ff97e2c2e84f0cae61e7ec66e5d36` |
| `render3d/r5_u0_bottom.png` | 2136×4000 | `1cafa2612695c154b58b4fefd0f8ea8c4cf52b96639d80db47fea4d2f90ec0a6` |
| `render3d/r5_u0_top_annot.png` | 2136×4000 | `1d86edff5201a824afdf5611c31f855a68fcbb40a534fb55f5764aa410d03286` |
| `render3d/r5_u0_bottom_annot.png` | 2136×4000 | `3d6943f12fc755d4e6290698b20276c201cee902f0ba1a7cbb7eb81aa8c87c0c` |
| `render3d/j1_closeup.png` | 1684×1136 | `840594a0a940ccf4cb7420dffc2c10450c90bb0aedd7f74fdfaeb155fa8d2d2c` |
| `render3d/j2_closeup.png` | 2676×1254 | `88cf650f325a1d256069ba4e2d790ddc474416bcdc941f85ede79b25b01e4ce0` |

- 四张总图：WO314 裁切版，同一 mm 窗口 `x=-2…34 / y=-2…65.4`；wide render 4000×3000（zoom 0.92）→ crop 1519×2845 @ (1228,62) → `-resize 2136x4000!`。
- 板框到图边实测：四边均 118 px = **1.987 mm**（TOP/BTM 相同，≤2 mm）。
- 两张特写：叠加 **红色模型可见轮廓框 + 蓝色焊盘包络框 + 红色开口方向箭头 + 偏差 mm 数字**；右侧白色栏放判据/数值。
- 标注版为纯位图后处理（PIL），不写 PCB。

## 5. 审计与偏差表

- `render3d/model_bbox_after.csv`：101 个 footprint；J1/J2/J3/D12 的 dCenter/dSize 来自渲染可见轮廓实测，其余为 model bbox 计算。J2/J3 判 PASS；J1/D12 为 ACCEPTED（原因见上）。
- `render3d/connector_orientation_audit.json`：J1/J2/J3/D12 的侧别、变换、实测 bbox、dC、开口方向、pin1、z 与判据。
- 旧 `j1crop.png` / `j2crop.png` 已删除，由 `j1_closeup.png` / `j2_closeup.png` 取代；目录内无 `*_ortho.png`、无 `*.kicad_prl` 中间件。

## 6. 硬闸

| 项 | 结果 |
|---|---|
| 源板 sha | `0d3a08dc…` 未变 |
| 板框 | 32.000 × 63.400 mm nominal |
| 铜/孔/网表指纹 | 5231 tracks / 105 fp / 13 zones / 149 nets，与源板一致 |
| DRC | 0 violations / 0 unconnected |
| A/B 判据 | J1 PASS；J2 PASS（dC ≤0.2） |
| MANIFEST | 全量重算，逐文件 0 错 0 缺（sha 见交付回执） |
| commit | 无 |
