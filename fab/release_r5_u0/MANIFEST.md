# WO307/WO311/WO312/WO313/WO314 — r5_u0 63.4mm 终板 MANIFEST

- 日期：2026-11-08
- 终板：`/home/fila/jqdDev_2025/ic_hw/key_v2/out/r5_u0_63p4mm_release.kicad_pcb`
- 终板 sha256：`0d3a08dca5c17d6d8db34b523969c183148c1529c44c8c61bde2182d9201c4a6`
- 板框：**32.000 × 63.400 mm** nominal（Edge.Cuts bbox 32.1 × 63.5 含 0.05 mm 线宽）
- DRC：**0 violations / 0 unconnected**；布线 **u = 0**；fill parity **0 mismatch**（13 zones，deterministic）
- BOM：35 组 / 101 器件；有建议 MPN 25 组（77 器件），待定 10 组（24 器件）；C11 DNP=Y
- KiCad CLI：`/home/fila/jqdDev_2025/ic_hw/AppDir/bin/kicad-cli` 10.0.5
- WO311 追加：3D 模型位姿返工 + 四张 4000×2667 渲染 + 连接器朝向复核。
- WO312 追加：清理 out/ 过程版本；render3d 删除会话副产物 `.kicad_prl`。
- WO313 追加：J1 加 rotate Z180 翻正；J2 弃用旧变换，按渲染可见轮廓实测重解；重出四图 + J1/J2 特写。
- WO314 追加：四图按同一 mm 窗口 `x=-2…34 / y=-2…65.4` 裁切为 **2136×4000**；板框四边实测 118 px = **1.987 mm**（≤2 mm）；渲染副本未动。
- 本目录文件数（不含 MANIFEST.md）：**78**；合计字节：**21292446**。
  - gerber 14 / drill 30 / pos 2 / bom 3 / render3d 28 / root 1

## 生成命令

### Gerber（终板原生 mask/silk）
```bash
kicad-cli pcb export gerbers -o key_v2/fab/release_r5_u0/gerber \
  -l 'F.Cu,In1.Cu,In2.Cu,In3.Cu,In4.Cu,In5.Cu,In6.Cu,B.Cu,F.Mask,B.Mask,F.Silkscreen,B.Silkscreen,Edge.Cuts' \
  --subtract-soldermask out/r5_u0_63p4mm_release.kicad_pcb
```

### Drill
```bash
kicad-cli pcb export drill -o key_v2/fab/release_r5_u0/drill \
  --format excellon --excellon-units mm --generate-map out/r5_u0_63p4mm_release.kicad_pcb
```

### CPL
```bash
kicad-cli pcb export pos --format csv --units mm --side front --output /tmp/pos_top.csv out/r5_u0_63p4mm_release.kicad_pcb
kicad-cli pcb export pos --format csv --units mm --side back  --output /tmp/pos_bottom.csv out/r5_u0_63p4mm_release.kicad_pcb
# 过滤纯 THT J3、Val 换行替换为空格、追加 DNP 列（C11=Y）后落 pos/
```

### 3D 渲染（WO314 裁切版）
```bash
export KICAD10_3DMODEL_DIR=/mnt/sdb5/opt/kicad/AppDir/share/kicad/3dmodels
kicad-cli pcb render --quality high --floor --perspective --background opaque --width 4000 --height 3000 --zoom 0.92 --side top \
  -o /tmp/top_wide.png render3d/r5_u0_63p4mm_3d_render.kicad_pcb
kicad-cli pcb render --quality high --floor --perspective --background opaque --width 4000 --height 3000 --zoom 0.92 --side bottom \
  -o /tmp/bottom_wide.png render3d/r5_u0_63p4mm_3d_render.kicad_pcb
# 同一 mm 窗口 crop 1519x2845+1228+62 -> resize 2136x4000!
# 标注版/特写为纯位图后处理（PIL），不写 PCB
```

## 文件清单（SHA-256，全量）

| 文件 | 字节 | SHA-256 |
|---|---:|---|

| `WO307_REPORT.md` | 3750 | `9dd12ef007e0535610dfb41986c9bec842348965c379a0ff6480e51e697fae86` |
| `bom/bom_summary.md` | 4289 | `1f592c9dc46ea27b71da25ef6281225f49b7cb2a9e06faf6ad7f0ecbbd4f2c54` |
| `bom/key_v2_bom.csv` | 3656 | `8ff9411f8b74e1da8fecd759ba7b7286564d758913482ded814617e134d97254` |
| `bom/key_v2_netlist_20261108.kicadsexpr` | 199299 | `ad588cd8cdc66cdaf3e70157ad6ab8753427785a975ef35e2fe3f1f6cd3d5527` |
| `drill/r5_u0_63p4mm_release-back-in2-drl_map.pdf` | 14672 | `5dad3723c645dc144469db13046d0c01eda8fba4b1b4350913a19effb3680cfd` |
| `drill/r5_u0_63p4mm_release-back-in2.drl` | 799 | `bb75fe7915d6a5b80692d4c5846095840cba0d9e41e30260537eedbe6fe28694` |
| `drill/r5_u0_63p4mm_release-back-in3-drl_map.pdf` | 12303 | `0f793a362e7b029ec1d26614ee225336f2dd4ba00a770860ef33011ae1bca674` |
| `drill/r5_u0_63p4mm_release-back-in3.drl` | 402 | `520ba1a7c6b2b338e6bb634c2026a029b47e147b5edba5f88ff7bd609d405d03` |
| `drill/r5_u0_63p4mm_release-back-in4-drl_map.pdf` | 13350 | `849b03fc2a50215c70a40d79c60f9d3139b84f8ef27c0e30ee4c3f77af3199a7` |
| `drill/r5_u0_63p4mm_release-back-in4.drl` | 673 | `d99c99ba77f7d97d252744ae61fdf65ed46478d7e086c7665c806e8ec14284f4` |
| `drill/r5_u0_63p4mm_release-back-in5-drl_map.pdf` | 15671 | `f859396bb1546131ed476c67c4f90d8ef4dfa30c035d0f255f087f84d49b0a8b` |
| `drill/r5_u0_63p4mm_release-back-in5.drl` | 1172 | `eb82f0a54b57508e752b14f3777f77ccf3ef2fecce98aea2153e60507745c3f2` |
| `drill/r5_u0_63p4mm_release-back-in6-drl_map.pdf` | 12396 | `ea2c4d07ad3689ca6f4f3e628bfbc30bbd2bf1127e8472e89621af5121248260` |
| `drill/r5_u0_63p4mm_release-back-in6.drl` | 414 | `7a09cdfb3df9f450279638019236ae6fefe399fdf5441e28037ce23503504917` |
| `drill/r5_u0_63p4mm_release-drl_map.pdf` | 21521 | `e19d49e74e82a84014cf6809a466fc70248105402aabe437a4b406024051aaf6` |
| `drill/r5_u0_63p4mm_release-front-in1-drl_map.pdf` | 14304 | `1adbd7cb1f517956d868c652e715513b28ab171069589e69ef55b33684bed9a0` |
| `drill/r5_u0_63p4mm_release-front-in1.drl` | 798 | `553bdc839dcc7553896a2feeaada713a73bee157ce8d606ea199806617497a4d` |
| `drill/r5_u0_63p4mm_release-front-in2-drl_map.pdf` | 17015 | `f9a781a5bf219521c5ae2b01d631c1f5ba964e456af5d7e92c45c871ab02b3a0` |
| `drill/r5_u0_63p4mm_release-front-in2.drl` | 1630 | `60c73873aa5e2eba753a84569636c68daa5373c2cae76ae4615383ba9035bdd4` |
| `drill/r5_u0_63p4mm_release-front-in3-drl_map.pdf` | 13613 | `a266b8ae2c6c3964b2555e6957f074040436ab737559f31b5c267f1f5e598ae4` |
| `drill/r5_u0_63p4mm_release-front-in3.drl` | 708 | `3328a003424a93d639cb5d6f4eeb1939609b0725f77ff6b428cd62ebef5c3652` |
| `drill/r5_u0_63p4mm_release-front-in4-drl_map.pdf` | 15770 | `5371a5c38800888b2b39933dc2cdc79e8f2c30e53f02f8b201297accfc252a8a` |
| `drill/r5_u0_63p4mm_release-front-in4.drl` | 1223 | `c8cf30beca7246a11b04a002a91df722c2e0ecb6da24a155f8e6ab1e05f7a5c6` |
| `drill/r5_u0_63p4mm_release-front-in5-drl_map.pdf` | 14907 | `63fbd81c732ca197332078575a68778575bd07c4fb144b0c8d0b42829ab08527` |
| `drill/r5_u0_63p4mm_release-front-in5.drl` | 951 | `6e7b6744d79c4cf9156928f1ff64e0ec19a335aba6d82391512b53ec6515f720` |
| `drill/r5_u0_63p4mm_release-in2-in4-drl_map.pdf` | 12399 | `6fd013fc9cd0982973c3c32bfdb05644a15e842ae73a5237fc09b9b69b05bc79` |
| `drill/r5_u0_63p4mm_release-in2-in4.drl` | 556 | `cb1091f46caa40a57ba1691c16b3489f1a6500c79220d267eb683e25cbe30bf5` |
| `drill/r5_u0_63p4mm_release-in2-in5-drl_map.pdf` | 16971 | `e63cc22ac17fbb919ec8f6435f1d4a2006fde69c6b72d7be242c21d737aa9e20` |
| `drill/r5_u0_63p4mm_release-in2-in5.drl` | 1340 | `374089e2179917cc0ec7b8bb8a9d8c89474e0dec8f1cb2f111c5d0d26b29d7f0` |
| `drill/r5_u0_63p4mm_release-in3-in4-drl_map.pdf` | 11776 | `890ea995d19ae1f2ef52bdfd8ef6e21aa6730d95811a6652cbb2d911e7cb4da3` |
| `drill/r5_u0_63p4mm_release-in3-in4.drl` | 346 | `d61bdc578296c6da8d320d238f447c46499b7ee825a1b3a9f9a3064074854f63` |
| `drill/r5_u0_63p4mm_release-in4-in5-drl_map.pdf` | 13506 | `fbc4474983c177f641a2a1196bc514d1fe7f7c8567bf50ab5352077b5343287d` |
| `drill/r5_u0_63p4mm_release-in4-in5.drl` | 768 | `04851de345cbdc77deabc36646b300b8cf501893bdede07cb83d16ff475efb14` |
| `drill/r5_u0_63p4mm_release.drl` | 2209 | `3dc119b86635a1822e4d711bdaed78e0c5b387f09c40826b8ab4793d90be8791` |
| `gerber/r5_u0_63p4mm_release-B_Cu.gbl` | 286549 | `54fbcb66501dd94daf7abf8708d949ca8964de111d0953b317bd1f07f59082ff` |
| `gerber/r5_u0_63p4mm_release-B_Mask.gbs` | 9215 | `a93536ee2efbef74f4e39af4e2279425b2cbebbdbfb98b9df42f0f1f8b5bb7b2` |
| `gerber/r5_u0_63p4mm_release-B_Silkscreen.gbo` | 95322 | `6bcd0dda307a9c122d9229c9ffee794b58f562d14d469c9dbd53423667d7b2d1` |
| `gerber/r5_u0_63p4mm_release-Edge_Cuts.gm1` | 911 | `a5d57f4336871d7b2f737f9397667b608214be938bddcb2a534c2b894466e089` |
| `gerber/r5_u0_63p4mm_release-F_Cu.gtl` | 421445 | `f63aeeb05c92238a7e234ec5e1698c3706310997ce0875e0229152a65ee91b30` |
| `gerber/r5_u0_63p4mm_release-F_Mask.gts` | 28000 | `6497be1540650ebc89c37f52e28932e040ae7bf54dde45fdff2504da40cbfb97` |
| `gerber/r5_u0_63p4mm_release-F_Silkscreen.gto` | 164913 | `0c84b6595347a37a49ce49b6d4f2582a86663847aaa0718f51fbaaa19debd15c` |
| `gerber/r5_u0_63p4mm_release-GND2.g1` | 282974 | `7895c6672233b6eb2ed683e0807d285a335502f7e47fb0ff6161179bec11c704` |
| `gerber/r5_u0_63p4mm_release-GND7.g6` | 192010 | `330ed8000cccc2de80e6e4427e5468f69d05607454a81dd77942246fe0acc58d` |
| `gerber/r5_u0_63p4mm_release-PWR4.g3` | 400093 | `ba56a3a6cf9684d82759e3b550e61aeb31823fc4b6f6f5eb51d378d8f28333ae` |
| `gerber/r5_u0_63p4mm_release-PWR5.g4` | 333723 | `eb2f70872c8b8663082679a072b4e9d4c83aa18c1ada5f09590687d1036b75d1` |
| `gerber/r5_u0_63p4mm_release-SIG3.g2` | 84353 | `584425578f0ac0627efd888aa843f6247a6e02fc3960bd8fcf1cc0255ca9ed04` |
| `gerber/r5_u0_63p4mm_release-SIG6.g5` | 65937 | `5fa92076081b37eea5dbf0f4b1753d7d12d325388c79dbf94c226792d2850b8a` |
| `gerber/r5_u0_63p4mm_release-job.gbrjob` | 4946 | `ee9f14a8f787a8f0fd6e7361df47f589d4601f400b576044458a3426494b8aa7` |
| `pos/r5_u0_cpl_bottom.csv` | 2676 | `6aa7c38fe0070fc1bd6c4fa6697e1531401498133fd26fdf234a9f3f1bf33d3a` |
| `pos/r5_u0_cpl_top.csv` | 3762 | `b4559420ae1f330cdbbcd95d469bec3e53840bb05ed4402c7127cde9c9d8c446` |
| `render3d/README.md` | 3119 | `937957788a986ddc54da8825634d7a910ebe54e494fad9f750e66ef25aef54f5` |
| `render3d/WO311_REPORT.md` | 11358 | `04ffbd369034ea5b240425e2f5f1aa9597e05eb8c5907d671b5d145542f89847` |
| `render3d/WO313_REPORT.md` | 6936 | `4536eef3f9e93bfe40200992ec6b9428358fb4e068f943576db6f59e9bf6d202` |
| `render3d/connector_orientation_audit.json` | 4550 | `27decbd2e87620b1e1f91f4d499a991c7fce4be8c0285a2fe4e8319580e6f144` |
| `render3d/j1_closeup.png` | 391219 | `840594a0a940ccf4cb7420dffc2c10450c90bb0aedd7f74fdfaeb155fa8d2d2c` |
| `render3d/j2_closeup.png` | 657802 | `88cf650f325a1d256069ba4e2d790ddc474416bcdc941f85ede79b25b01e4ce0` |
| `render3d/model_bbox_after.csv` | 6614 | `61f6560e41e990e21e6fd2719312de6814f326c70feb73fbf582f8e0396ab254` |
| `render3d/model_bbox_before.csv` | 5019 | `7dfc45918e7ca825c3a1b7ec124e626be8126d3548a54f8011c4de1f0995774a` |
| `render3d/models/fallback/C11_KR_5R5V105_SMD_DNI.wrl` | 178 | `48292282f69b1f16f332115612c645bed93e6a1f5556c2008af982cf006e87b2` |
| `render3d/models/fallback/D12_LED_LiteOn_LTST-S326.wrl` | 178 | `25a88ac4fd9fea274bf42b91941a528b6afede012c513ae478f5dc534a52fa73` |
| `render3d/models/fallback/U12_B1_PsP_568.wrl` | 179 | `f4789c2697a51d7a77f5a104fa7ff65fa6e590af65aa1bef74a129ece49d982e` |
| `render3d/models/fallback/U14_PI3DBS16412.wrl` | 178 | `4ea35283f64c34c25f7140be2c13bd26e391ff0f48fb0cef3fb2d78bd412941f` |
| `render3d/models/fallback/U1_QFN-60-1EP_7x7mm_P0.4mm_EP3.4x3.4mm.wrl` | 178 | `436eb6ed6bc6f6c3cdf7ba361f2b030bdddbde7019edc0e14b29e963af1eddd9` |
| `render3d/models/fallback/U5_EMMC_BGA153.wrl` | 178 | `f86abe2efa12f22b682bcaf06e4aa0c57e2e8ba852ed2b90aa1cb62f34dd5c6f` |
| `render3d/models/fallback/U6_FUSB302.wrl` | 178 | `65cd41d1c4dca9fa52a20c59bebfec792e4f3a5a77ef0ff19c9429d2787983cd` |
| `render3d/models/fallback/U8_PI3DBS16412.wrl` | 178 | `4ea35283f64c34c25f7140be2c13bd26e391ff0f48fb0cef3fb2d78bd412941f` |
| `render3d/models/real/J3_PinHeader_1x03_P1.27mm_Vertical.step` | 57957 | `0d86a1c61f80d706cc2e871fc03e3b5954e8bee3902afccf14819b2fa7c712be` |
| `render3d/models/real/U13_UQFN-10_1.4x1.8mm_P0.4mm.step` | 61934 | `7de579b3d78c520efca0029de5f3d27bcaaad4323a7ee8a90e74f42c56c6ceed` |
| `render3d/models/real/U4_UQFN-20-1EP_3x3mm_P0.4mm_EP1.85x1.85mm.step` | 121413 | `83811ece121b40d2fdee3de90958006fa6b7247f532c8e48ca792887a20168f5` |
| `render3d/models/real/Y2_Crystal_SMD_3225-4Pin_3.2x2.5mm.step` | 26408 | `66685d348a8eedd8d77549dc99e75d4d7835eba481a155781b24ef8c202aad53` |
| `render3d/models/vendor/J2_MTSSD03-67MSW337.STEP` | 3066125 | `bc066768fa20dcdc1415497870fd044dea478eb62ff44025da202305cb19ab22` |
| `render3d/r5_u0_63p4mm_3d_render.kicad_pcb` | 3195276 | `f971afbd520df8c3e4ebd0c191a37f531ff65e398e0f63fa91222f652c86e2c2` |
| `render3d/r5_u0_63p4mm_3d_render.kicad_pro` | 14711 | `5d9a131d4c458b2b8cb5b4d64c3141ba8ed1965816b9258986c3d33b3640f24f` |
| `render3d/r5_u0_bottom.png` | 2993744 | `1cafa2612695c154b58b4fefd0f8ea8c4cf52b96639d80db47fea4d2f90ec0a6` |
| `render3d/r5_u0_bottom_annot.png` | 2712126 | `3d6943f12fc755d4e6290698b20276c201cee902f0ba1a7cbb7eb81aa8c87c0c` |
| `render3d/r5_u0_top.png` | 2662710 | `48bfecaf2b2a18a796a7f91ad1b6acdf115ff97e2c2e84f0cae61e7ec66e5d36` |
| `render3d/r5_u0_top_annot.png` | 2468916 | `1d86edff5201a824afdf5611c31f855a68fcbb40a534fb55f5764aa410d03286` |
| `render3d/wo311_drc.json` | 1098 | `1479f261b0a52be2a79a9aff6df1ac5a9b490343fdac1d13389ca4c034a51e38` |

> `MANIFEST.md` 自身不列入上表；其 sha256 在交付回执中给出。
