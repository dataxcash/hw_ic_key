# WO307 — r5_u0 63.4mm 终板 / 丝印 / 1 脚点 / BOX 对准 / BOM MPN / 重出发布件

- 日期：2026-11-08
- 终板：`/home/fila/jqdDev_2025/ic_hw/key_v2/out/r5_u0_63p4mm_release.kicad_pcb`
- 终板 sha256：`0d3a08dca5c17d6d8db34b523969c183148c1529c44c8c61bde2182d9201c4a6`
- 源板（68mm，未改）：`out/r5_u0_final.kicad_pcb` sha `594adb41…`
- 铜/孔/焊盘/填充指纹：与源板一致 `586c39c7…`
- 板长：63.4mm nominal；DRC **0** / u **0** / 全不变量 PASS

## 1. 丝印型号（0.8mm 字高；横向压缩 0.5，U12 0.32）

| Ref | 丝印文字 | 层 | 位置 |
|---|---|---|---|
| U1 | `RP2350 QFN60` | F.Silkscreen | 27.5, 19.2 |
| U5 | `EMMC BGA153` | F.Silkscreen | 11.6, 12.1 |
| U6 | `FUSB302` | B.Silkscreen | 23.5, 4.5 |
| U8 | `PI3DBS16412` | F.Silkscreen | 6.2, 13.2 |
| U12 | `SpacemiT B1 CoM`（单行旋转 90°，0.32×0.8） | F.Silkscreen | **30.0, 54.1** |
| U13 | `TS3USB221A` | B.Silkscreen | 26.2, 22.0 |
| U14 | `PI3DBS16412` | F.Silkscreen | 27.1, 14.4 |
| Y1 | `XO 100M` / `32k` | F.Silkscreen | 2.8, 34.0 |
| Y2 | `XO 100M` / `32k` | F.Silkscreen | 2.5, 22.8 |
| J1 | `USB-C` | F.Silkscreen | 18.8, 5.5 |
| J2 | `M.2 M-Key` | B.Silkscreen | 18.2, 53.4 |
| J3 | `UART` | F.Silkscreen | 30.2, 25.5 |
| D12 | `LED` | F.Silkscreen | 8.8, 2.5 |

> **U12**：`SpacemiT B1 CoM` 单行旋转 90°，字高 0.8mm、横向 0.32mm；放在 U12 右侧最近空位 **(30.0,54.1)**，距 U12 焊盘包络 **3.2mm**（文字边最近约 2.6mm）。不压焊盘、不越板边、DRC 0。

## 2. 1 脚标识（Ø0.3 圆点，焊盘外）

U1 24.15,13.7｜U5 2.55,13.7｜U6 22.3375,3.3｜U8 13.7875,22.05｜U12 5.0,28.0｜U13 30.75,25.2｜U14 21.6875,21.55｜Y1 1.7,39.55｜Y2 0.65,26.2｜J1 12.9,1.18｜J2 27.85,47.325｜J3 29.0,28.05｜D12 8.2,0.525

## 3. BOX 兜底对准（bbox vs 焊盘包络）

| Ref | 焊盘包络 w×d | BOX bbox w×d | Δw | Δd |
|---|---:|---:|---:|---:|
| U1 | 7.70×7.70 | 7.70×7.70 | 0 | 0 |
| U5 | 6.80×6.80 | 6.80×6.80 | ≤1e-6 | ≤1e-6 |
| U12 | 21.60×31.60 | 21.60×31.60 | 0 | 0 |
| U6 | 3.20×3.20 | 3.20×3.20 | ≤1e-6 | ≤1e-6 |
| U8 | 4.25×9.75 | 4.25×9.75 | ≤1e-6 | ≤1e-6 |
| U14 | 4.25×9.75 | 4.25×9.75 | ≤1e-6 | ≤1e-6 |
| D12 | 1.85×3.50 | 1.85×3.50 | ≤1e-6 | ≤1e-6 |
| C11 | 7.08×1.60 | 7.08×1.60 | ≤1e-6 | ≤1e-6 |

## 4. BOM 建议 MPN

- 文件：`bom/key_v2_bom.csv`（新增 `Suggested_MPN`、`DNP` 两列）
- 分组：**35** 组 / 器件总数 **101**
- 有建议 MPN：**25 / 35 组**（覆盖 **77 / 101** 器件）
- 待定：**10 / 35 组**（**24 / 101** 器件）
- C11：`KR-5R5V105 (DNP)`，`DNP=Y` 保留
- LCSC 仅在确认的通用阻容上给出（100nF C1525、10uF C15850）；其余拿不准全部标“待定”，未瞎填。

## 5. 任务 D — 重出发布件

目录：`/home/fila/jqdDev_2025/ic_hw/key_v2/key_v2/fab/release_r5_u0/`

- `gerber/` 14（13 Gerber + `.gbrjob`，原生 mask/silk）
- `drill/` 30（15 `.drl` + 15 map PDF）
- `pos/` 2：top 59 行 / bottom 41 行；J3 THT 排除；C11 `DNP=Y`
- `render3d/`：`r5_u0_top.png` sha `6747d2e50733af8a859af3877e9a511ad58ed3ab5b66bce479088d0594d18219`；`r5_u0_bottom.png` sha `edddeebbacb8cbd5fc2baf1937be464b1758b741d900130300192a50826bf943`
- `bom/` 3（BOM 101 器件不变，新增 MPN 列）
- `MANIFEST.md`：全文件 sha256

## 6. 缺口

1. BOM 有 10/35 组为“待定”（24/101 器件），已写原因，需采购/工程确认后补。
2. U12 型号文字距本体 3.2mm（最近空位），非紧贴；若要更近需进一步缩字或挪周边丝印。
3. 8 个 BOX 为 ENG 兜底，非真实厂商 STEP；bbox 已与焊盘包络对齐。
4. J2 真实 STEP 来自 KiCad 官方 demo（CERN-OHL-S v2），非厂商单独下载。
