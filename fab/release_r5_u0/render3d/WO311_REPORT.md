# WO311 — 3D 渲染返工报告（模型位姿 + BOX 标注 + 连接器朝向复核）

- 日期：2026-11-08
- 源板（不得修改）：`out/r5_u0_63p4mm_release.kicad_pcb`
  - sha256：`0d3a08dca5c17d6d8db34b523969c183148c1529c44c8c61bde2182d9201c4a6`
  - 尺寸：32.00 × 63.40 mm nominal（32.1 × 63.5 bbox 含线宽）
- 渲染副本：`key_v2/fab/release_r5_u0/render3d/r5_u0_63p4mm_3d_render.kicad_pcb`
  - sha256：`e354b9c3417cb3efa3a572073affd296e3261356b9cb118e8fe858a28969845e`
- 铜/孔/焊盘/填充指纹：源板与渲染副本完全一致
  - tracks 5231 / footprints 105 / zones 13 / nets 149
  - 几何指纹 `ff2f2c4f20e09793686fe6320cb108c5f9c214d1f4bc3890e68a99137c514505`（两侧相等）
- 本单不改板、不改网表、不 commit。

---

## 0. 结论摘要

1. **J2 M.2 座已修好**：最终 `offset (-2.9155, 1.334, 1.724) mm`、`rotate (90, -180, -90) deg`。BTM 图可见完整白色 M.2 座，两排焊脚落在金手指焊盘上；TOP 图不再出现放错位置的白色座子。
2. **TOP 的“白色模型”根因**：旧版 J2 没有 offset/rotate，模型被放在 TOP 面 U12 附近；修正后 J2 只在 BTM 出现。J2 区域的近白像素占比：TOP 1.93% vs BTM 35.58%（8-bit RGB 全部 >200 的像素），定量确认座体只在背面。
3. **J1 USB-C 朝向已复核**：采用 KiCad 官方 footprint/model 的零变换（`offset 0 / rotate 0`），开口面中心在板坐标 **y = 0.97 mm**（< 3 mm），朝板边 y=0；之前 Z180 翻转会把开口转向板内，已撤销。
4. **J3 UART 排针**：本体朝上（+Z），模型 pin1 与 footprint pin1 同侧（0.02 mm 级一致）。
5. **四张交付图**：TOP/BTM 原始 + TOP/BTM 标注，均 4000×2667，sha256 见 §4。
6. **标注**：13 颗 owner 点名器件全部有型号文字；大 BOX 直接叠在顶面中心，U13 因本体仅 1.4×1.8 mm 采用“贴边标签 + 引线”，不压焊盘/1 脚点。

---

## 1. 模型位姿修正（渲染副本，仅改 3D model 字段）

| ref | 修正前 offset / rotate | 修正后 offset / rotate | 依据 |
|---|---|---|---|
| **J2** | `(0,0,0)` / `(0,0,0)` | `(-2.9155, 1.334, 1.724)` / `(90, -180, -90)` | 同一 STEP 在 KiCad demo `CM5_MINIMA_3` 的 J701 用 `(0,1.8,1.3)/(90,-180,-90)`；本板 footprint 是 demo 焊盘包络的 180° 面内变体（中心 y=+1.5 vs demo −1.5），对 demo 变换做 Rz(180) 后按“模型 bbox 中心 = 焊盘包络中心、模型 z_min = 0”解出 offset。 |
| **J1** | `(0,-0.895,0)` / `(0,0,180)`（翻转后开口朝内，错） | `(0,0,0)` / `(0,0,0)` | KiCad 官方 footprint 自带零变换；模型 bbox（10.0×10.5）与 footprint 本体/丝印包络对齐，开口面 native y_min → 板坐标 y=0.97 mm。 |
| **J3** | `(0,0,0)` / `(0,0,0)` | `(0,1.27,0)` / `(0,0,0)` | STEP native bbox y 中心 = −1.27 mm；+1.27 后模型 pin 行与 footprint 1.27 mm 间距焊盘重合；z_max=+4.0667 mm（本体朝上）。 |
| **D12** | `(0,0,0)` / `(0,0,0)` | `(-0.175,0,0)` / `(0,0,0)` | LED 模型 x 中心比焊盘包络偏 +0.175 mm；平移后 dCenter=0。 |

> J2 的详细推导：native bbox = `[-4.018874,-3.903652,-16.755515] .. [4.351126,1.724437,10.925]`；经 `(90,-180,-90)` 后局部 bbox = x `[-10.925,16.755515]`、y `[-4.018874,4.351126]`、z `[-1.724437,3.903652]`。取 offset = `(-bbox_cx, +1.5-bbox_cy, -bbox_zmin)` = `(-2.9155, 1.334, 1.724)`。

---

## 2. 模型 bbox vs 焊盘包络偏差表（101 个有模型 footprint）

完整表：
- 改前：`render3d/model_bbox_before.csv`（sha256 `7dfc45918e7ca825c3a1b7ec124e626be8126d3548a54f8011c4de1f0995774a`）
- 改后：`render3d/model_bbox_after.csv`（sha256 `b52ae25b98280f2cb4a2f98bab85041a530636d0356b777f8cfa1cb4e60b02dd`）

### 2.1 改前 >0.2 mm 的全部行

| ref | dCenter x (mm) | dCenter y (mm) | dSize w (mm) | dSize d (mm) | pad w×d (mm) | model w×d (mm) | 判定 |
|---|---:|---:|---:|---:|---|---|---|
| J2 | +0.166 | **−2.590** | −13.530 | −3.472 | 21.90×9.10 | 8.37×5.628 | REVIEW |
| J3 | 0.000 | **−1.270** | +1.100 | +0.310 | 1.00×3.50 | 2.10×3.810 | REVIEW |
| J1 | 0.000 | **+0.935** | +0.220 | +1.590 | 9.78×8.91 | 10.00×10.500 | REVIEW |

### 2.2 改后

| ref | dCenter x (mm) | dCenter y (mm) | dSize w (mm) | dSize d (mm) | pad w×d (mm) | model w×d (mm) | 判定 |
|---|---:|---:|---:|---:|---|---|---|
| J2 | 0.000 | 0.000 | +5.781 | −0.730 | 21.90×9.10 | 27.681×8.37 | PASS（中心重合；尺寸差为座体两侧卡口壁外伸） |
| J3 | 0.000 | 0.000 | +1.100 | +0.310 | 1.00×3.50 | 2.10×3.81 | PASS（塑料本体比焊盘宽，正常） |
| J1 | 0.000 | +0.935 | +0.220 | +1.590 | 9.78×8.91 | 10.00×10.50 | **ACCEPTED / 说明见下** |

**J1 唯一“超 0.2 mm”项说明（不是位姿错误）**：
- J1 焊盘包络 y = `[−5.37, +3.54]`，模型本体 y = `[−5.23, +5.27]`；模型前端与前排焊盘只差 0.14 mm，后端壳体外伸 1.73 mm。USB-C 母座的接触焊盘集中在前排，壳体向后延伸，导致“焊盘包络中心”与“壳体 bbox 中心”天然差 0.935 mm。
- 用 footprint F.Fab 本体包络（y ≈ `[−5.22,+5.23]`）复核：模型 bbox 中心偏差约 **0.015 mm**。
- 官方 footprint 的 3D model 就是零 offset/zero rotate；若强行把 bbox 中心对到焊盘包络，会把壳体向板边再推 0.935 mm，反而超出 footprint 本体。故保留零变换。
- 真正判据（owner 补充）：开口面中心 **y=0.97 mm < 3 mm**，朝板边 y=0，PASS。

---

## 3. 连接器朝向核对表（owner 追问项）

> 坐标均为板坐标（mm，y 向下；TOP 图 y=0 在上边，BTM 图 y=63.4 在下边）。
> 证据：`render3d/connector_orientation_audit.json`（sha256 `23747b18a35ed4c60f3eff519fee73f74929ac433819e4ce0c7edfb2cac54ebd`）。
> 朝向特写证据：`render3d/j1crop.png`、`render3d/j2crop.png`。

| ref | 器件 | 侧别 | footprint 旋转 | 模型 offset / rotate | 开口/本体方向 | 开口面中心 | pin1 一致性 | 判定 |
|---|---|---|---|---|---|---|---|---|
| **J1** | USB-C 沉板母座 | **F.Cu（TOP）** | 0° | `(0,0,0)` / `(0,0,0)` | 开口朝 **−Y**（板边 y≈0） | x=16.00, **y=0.97**（<3.0） | A1 局部 `(-2.75,-5.02)` → 板 `(13.25,1.18)`；官方同源零变换，USB-C 可正反插，模型无专用 pin1 特征，参考基准保持 | **PASS** |
| **J2** | M.2 (NGFF) M-Key 67pin | **B.Cu（BTM，KiCad 自动镜像）** | 180° | `(-2.9155,1.334,1.724)` / `(90,-180,-90)` | 卡口朝 **+Y**（63.4 mm 板端） | x=18.25, **y=56.085**（开口面） | pad1 局部 `(-9.25,+5.275)` → 板 `(27.5,48.125)`；模型 pin1 三角在 BTM 图左上（板 +X/−Y 角），与 pad1 同侧 | **PASS** |
| **J3** | UART 1×03 排针 1.27 mm | **F.Cu（TOP，THT）** | 0° | `(0,1.27,0)` / `(0,0,0)` | 本体朝 **+Z**（板上方） | z_max=+4.0667；pin 穿透 z_min=−2.3667 | pin1 局部 `(0,-1.25)`；模型 pin1 行局部 y=−1.27（差 0.02 mm），同侧 | **PASS** |

### 3.1 J2 视觉证据

- BTM 图：白色 M.2 座本体完整可见；上下两排焊脚与金手指焊盘逐脚对齐；卡口开口在图像下方，对应板端 y=63.4。
- TOP 图：J2 区域只看到板面/丝印，无白色座体。定量：J2 焊盘包络投影区内近白像素占比 TOP **1.93%**、BTM **35.58%**。
- J2 最终变换推导见 §1。

### 3.2 J1 视觉证据

- TOP 图：USB-C 开口面（三个缺口特征）朝图像上缘，即板边 y=0；开口面中心 y=0.97 mm。
- 标注版用红色箭头从 J1 本体指向 y=0 板边，不遮焊盘。

### 3.3 J3 视觉证据

- TOP 图：黑色排针本体竖直立在板上方，三根引脚可见；标注“UART”用引线放在左侧空位，不压焊盘。

---

## 4. 四张交付渲染（4000×2667）

| 文件 | 视角 | 像素 | sha256 |
|---|---|---|---|
| `render3d/r5_u0_top.png` | TOP 原始 | 4000×2667 | `7b2238d8487e3fe573824c9a541c607427ae87557739068069238db820718970` |
| `render3d/r5_u0_bottom.png` | BTM 原始 | 4000×2667 | `346f2d06ca3d759ac51faf07ffadbe36459b0da3f65997aaed2ae7f8b8ea9f85` |
| `render3d/r5_u0_top_annot.png` | TOP 标注 | 4000×2667 | `328d05f1607294a5c6c0feb66e5cfaaee46c724baae66f2ba8495121e79bfcbe` |
| `render3d/r5_u0_bottom_annot.png` | BTM 标注 | 4000×2667 | `9aaed12f154a9b09082deea41ab6f85ba7b0b8929f26a976853df91f5a14af59` |

渲染参数：`kicad-cli pcb render --quality high --floor --perspective --background opaque --width 4000 --height 2667 --side {top|bottom}`；KiCad 10.0.5 实际输出 3976×2632，已用 ImageMagick `-resize 4000x2667!` 落到要求尺寸。

标注版是在原始渲染上做 **纯位图后处理**（PIL），不改 PCB；板坐标→像素用 H1–H4 四个安装孔中心做单应标定。

---

## 5. 标注落位表（最终 4000×2667 像素）

| 侧 | ref | 文字 | 板上锚点 (x,y) mm | 最终像素 (x,y) | 方式 |
|---|---|---|---|---:|---|
| TOP | U1 | RP2350 / QFN60 | (27.5,16.8) | (2472.1,718.9) | 本体中心 |
| TOP | U5 | EMMC / BGA153 | (5.8,17.3) | (1579.9,737.9) | 本体中心 |
| TOP | U8 | PI3DBS / 16412 | (12.2,17.7) | (1843.3,755.0) | 本体中心 |
| TOP | U14 | PI3DBS / 16412 | (20.1,17.2) | (2168.1,734.9) | 本体中心 |
| TOP | U12 | SpacemiT / B1 CoM | (16.0,43.5) | (1999.8,1822.2) | 本体中心 |
| TOP | J1 | USB-C | (16.0,6.2) | (1999.5,279.4) | 本体中心 |
| TOP | J3 | UART | (29.0,30.0) | (2533.7,1264.5) | 引线贴边（面板 26.2,30.4） |
| TOP | Y1 | XO 100M / 32k | (2.8,39.5) | (1456.7,1656.6) | 本体中心 |
| TOP | Y2 | XO 100M / 32k | (2.5,27.0) | (1444.1,1139.3) | 本体中心 |
| TOP | D12 | LED | (8.2,2.5) | (1678.5,125.3) | 本体中心 |
| BTM | U6 | FUSB / 302 | (23.5,2.2) | (1690.7,109.9) | 本体中心 |
| BTM | U13 | TS3USB221A | (30.25,24.0) | (1412.8,1010.8) | 引线贴边（面板 20.8,23.5） |
| BTM | J2 | M.2 M-Key | (18.25,51.9) | (1907.2,2165.8) | 本体中心 |

- 13 颗 owner 点名器件全部覆盖；U13 本体仅 1.4×1.8 mm，文字无法在体上放下，已在 BTM 绿色空位放标签并用引线指到本体中心。
- 标注不覆盖焊盘/1 脚点；J1/J2 另加红色开口方向箭头（J1 指向 y=0 板边，J2 指向 y=63.4 板端）。

---

## 6. 约束/能力台账

| 项 | 结果 |
|---|---|
| 源板 sha 不变 | `0d3a08dc…` PASS |
| 铜/网表不变 | 5231 tracks / 105 fp / 13 zones / 149 nets，指纹两侧相等 PASS |
| DRC | 源板（与渲染副本同铜）`0 violations / 0 unconnected`，证据 `render3d/wo311_drc.json` |
| 新 ENG 模块 | 无；测量调用 `eda_core.deliverable_audit.model_geometry` / `expand_model_path` / `sha256`，几何算术为确定性 0/45/90 变换，无 maze/A* |
| 落板写操作 | 仅渲染副本 `r5_u0_63p4mm_3d_render.kicad_pcb` 的 model 字段；源板/网表未写 |
| commit | 无 |

## 7. 复现

```bash
cd /home/fila/jqdDev_2025/ic_hw/key_v2
export KICAD10_3DMODEL_DIR=/mnt/sdb5/opt/kicad/AppDir/share/kicad/3dmodels
B=key_v2/fab/release_r5_u0/render3d/r5_u0_63p4mm_3d_render.kicad_pcb
for side in top bottom; do
  /home/fila/jqdDev_2025/ic_hw/AppDir/bin/kicad-cli pcb render \
    --quality high --floor --perspective --background opaque \
    --width 4000 --height 2667 --side $side \
    -o render3d/r5_u0_${side}.png $B
done
```

标注后处理脚本为渲染位图叠加（PIL），不写 PCB；板坐标→像素标定用 H1–H4 安装孔中心。
