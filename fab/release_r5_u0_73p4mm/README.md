# r5_u0_73p4mm_release — key_v2 终板发布包

- 日期：2026-11-10
- 终板：`board/r5_u0_73p4mm_release.kicad_pcb`
- 终板 sha256：`4ad679b8064b4ae505174c1d6a5d15cc32a3e507ddbfa3cf3c37d8326b3f808d`
- DRC：0 violations / 0 unconnected（`drc.json`）
- 板框：标称 32.000 × 73.400 mm；含 Edge.Cuts 弧鼓实测 32.100 × 73.500 mm
- 层数：8；fill parity：15 zones / mismatch 0
- 关键长度：USB2_DP 91.2910 mm / 36 段 / 7 孔（dL vs DM +4.5368）；PCIE_TC_TX0_N 98.7043 mm；REFCLK_100M_P/N 40.3098 mm / dL 0
- 验收：verify key_v2 7/7 PASS；connectivity split_nets 0；3V3_AUX/U6 收口完成
- U2 首件门槛：连续 >1.5 A 或峰值 >2.5 A / >2 ms / >2% duty ⇒ 下一版 TPS22965DSGR
