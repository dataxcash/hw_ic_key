# r5_u0 63.4mm 终板双面 3D 渲染（WO313 位姿 + WO314 裁切版）

- 源板（只读，未改）：`out/r5_u0_63p4mm_release.kicad_pcb`
  - sha256 `0d3a08dca5c17d6d8db34b523969c183148c1529c44c8c61bde2182d9201c4a6`
- 渲染副本：`render3d/r5_u0_63p4mm_3d_render.kicad_pcb`
  - sha256 `f971afbd520df8c3e4ebd0c191a37f531ff65e398e0f63fa91222f652c86e2c2`
  - 只改 3D model 字段；铜/孔/网表指纹与源板一致（5231 tracks / 105 fp / 13 zones / 149 nets）
- 详细报告：`render3d/WO313_REPORT.md`（sha256 `4536eef3f9e93bfe40200992ec6b9428358fb4e068f943576db6f59e9bf6d202`）

## 交付图（2136×4000，同一 mm 窗口 x=-2…34 / y=-2…65.4）

| 文件 | 视角 | sha256 |
|---|---|---|
| `r5_u0_top.png` | TOP 原始 | `48bfecaf2b2a18a796a7f91ad1b6acdf115ff97e2c2e84f0cae61e7ec66e5d36` |
| `r5_u0_bottom.png` | BTM 原始 | `1cafa2612695c154b58b4fefd0f8ea8c4cf52b96639d80db47fea4d2f90ec0a6` |
| `r5_u0_top_annot.png` | TOP 标注 | `1d86edff5201a824afdf5611c31f855a68fcbb40a534fb55f5764aa410d03286` |
| `r5_u0_bottom_annot.png` | BTM 标注 | `3d6943f12fc755d4e6290698b20276c201cee902f0ba1a7cbb7eb81aa8c87c0c` |

## 连接器朝向（详见 WO313_REPORT）

| ref | 侧别 | 最终 transform | 开口/本体方向 | 判定 |
|---|---|---|---|---|
| J1 USB-C | F.Cu | offset `(-0.24,0.40,0)`, rotate `(0,0,180)` | 圆弧唇口/开口朝 −Y 板边；front edge y=0.653 mm | PASS |
| J2 M.2 M-Key | B.Cu (mirrored) | offset `(0,1.644,1.724)`, rotate `(90,-180,-90)` | 卡口 +Y，朝 63.4 端；dC=(-0.026,-0.013) mm | PASS |
| J3 UART | F.Cu THT | offset `(0,1.27,0)`, rotate `(0,0,0)` | 本体 +Z 朝上；pin1 同侧 | PASS |
| D12 LED | F.Cu | offset `(-0.175,0,0)`, rotate `(0,0,0)` | Y 反向不适用；x 差为镜头外伸 | ACCEPTED |

## 裁切窗口

- 四图统一窗口：`x = -2 … 34 mm`、`y = -2 … 65.4 mm`；输出 **2136×4000**。
- 板框四边到图边实测：**118 px = 1.987 mm**（TOP/BTM 相同，≤2 mm）。
- 渲染副本未动，sha 仍为 `f971afbd520df8c3e4ebd0c191a37f531ff65e398e0f63fa91222f652c86e2c2`。

## 特写与审计附件

- `j1_closeup.png` — J1 特写：红色模型轮廓框 + 蓝色焊盘包络框 + 开口箭头 + dC/front-edge/z 数字
- `j2_closeup.png` — J2 特写：红色轮廓框 + 蓝色焊盘包络框 + 开口箭头 + dC/opening-face/gap 数字
- `model_bbox_after.csv` — 101 footprint 改后偏差表（J1/J2/J3/D12 用渲染可见轮廓实测）
- `connector_orientation_audit.json` — 侧别、变换、实测 bbox、dC、开口方向、pin1、z 的机器可读审计
- `wo311_drc.json` — 源板 DRC 证据（0 violations / 0 unconnected）

## 复现

```bash
export KICAD10_3DMODEL_DIR=/mnt/sdb5/opt/kicad/AppDir/share/kicad/3dmodels
/home/fila/jqdDev_2025/ic_hw/AppDir/bin/kicad-cli pcb render \
  --quality high --floor --perspective --background opaque \
  --width 4000 --height 2667 --side top \
  -o render3d/r5_u0_top.png render3d/r5_u0_63p4mm_3d_render.kicad_pcb
```

> KiCad 10.0.5 实际输出 3976×2632；本交付已 `-resize 4000x2667!`。标注版/特写为纯位图后处理（PIL），不写 PCB。
