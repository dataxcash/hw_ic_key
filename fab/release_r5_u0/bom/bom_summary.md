# KEY v2 BOM 汇总（含建议 MPN）

> 器件 101 | 料号组 35 | 有建议MPN 25 组 / 待定 10 组

| Ref | Qty | Value | Footprint | Suggested_MPN | MPN_Note | DNP |
|---|---|---|---|---|---|---|
| C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C12,C15,C17,C19,C21,C26,C27,C28,C31,C32,C33,C34,C35,C36,C37,C38 | 26 | 100nF | Capacitor_SMD:C_0402_1005Metric | LCSC C110251 / FH 0402B104K500NT | 50V X7R, LCSC verified |  |
| C11 | 1 | SUPERCAP_1F | KeyV2_Footprints:KR_5R5V105_SMD_DNI | KR-5R5V105 (DNP) | DNI/DNP; vendor PN from value | Y |
| C13,C14,C16,C18,C20,C22,C29,C30 | 8 | 10uF | Capacitor_SMD:C_0402_1005Metric | LCSC C15525 / Samsung CL05A106MQ5NUNC | 6.3V X5R, LCSC verified |  |
| C23,C25 | 2 | 12pF | Capacitor_SMD:C_0402_1005Metric | LCSC C1547 / FH 0402CG120J500NT | 50V C0G, LCSC verified |  |
| C24 | 1 | 1uF | Capacitor_SMD:C_0402_1005Metric | LCSC C52923 / Samsung CL05A105KA5NQNC | 25V X5R, LCSC verified |  |
| D1,D2,D3,D4,D5,D6,D7,D8,D9,D10,D11 | 11 | TVS | Diode_SMD:D_SOD-523 | 待定 | BOM 未给工作电压/电容/极性，无法唯一选型 |  |
| D12 | 1 | LED_DUAL | LED_SMD:LED_LiteOn_LTST-S326 | LiteOn LTST-S326KGJRKT | from footprint descr |  |
| D13 | 1 | BAT54C | Diode_SMD:D_SOT-23 | BAT54C | generic PN |  |
| J1 | 1 | USB_C_RECEPT_IND | Connector_USB:USB_C_Receptacle_Amphenol_12401610E4-2A | Amphenol 12401610E4-2A | from footprint name |  |
| J2 | 1 | M2_M_SOCKET_2230_ONLY | KeyV2_Footprints:M2_M_SOCKET_2230_ONLY | MTSSD03-67MSW337 | from 3D model |  |
| J3 | 1 | UART_DEBUG | KeyV2_Footprints:PinHeader_1x03_P1.25mm_Vertical | 待定 | 仅 1x03 1.25mm 排针，未给安装方向/镀层/品牌 |  |
| L1,L2,L3,L4,L5 | 5 | 1uH | Inductor_SMD:L_0805_2012Metric | 待定 | 未给额定电流/饱和电流/直流电阻 |  |
| L6 | 1 | 4.7uH | Inductor_SMD:L_0805_2012Metric | 待定 | 未给额定电流/饱和电流/直流电阻 |  |
| R1,R2 | 2 | 5.1K | Resistor_SMD:R_0402_1005Metric | LCSC C25905 / UNI-ROYAL 0402WGF5101TCE | 1% 0402, LCSC verified |  |
| R3,R8,R10,R11,R15,R16,R17,R18,R20,R21,R22,R23 | 12 | 10K | Resistor_SMD:R_0402_1005Metric | LCSC C25744 / UNI-ROYAL 0402WGF1002TCE | 1% 0402, LCSC verified |  |
| R4,R19 | 2 | 100K | Resistor_SMD:R_0402_1005Metric | LCSC C25741 / UNI-ROYAL 0402WGF1003TCE | 1% 0402, LCSC verified |  |
| R5,R6,R9,R12 | 4 | 4.7K | Resistor_SMD:R_0402_1005Metric | LCSC C25900 / UNI-ROYAL 0402WGF4701TCE | 1% 0402, LCSC verified |  |
| R7 | 1 | 1M | Resistor_SMD:R_0402_1005Metric | LCSC C26083 / UNI-ROYAL 0402WGF1004TCE | 1% 0402, LCSC verified |  |
| R13,R24,R25 | 3 | 100k | Resistor_SMD:R_0402_1005Metric | LCSC C25741 / UNI-ROYAL 0402WGF1003TCE | 1% 0402, LCSC verified |  |
| R14 | 1 | 11k | Resistor_SMD:R_0402_1005Metric | LCSC C25749 / UNI-ROYAL 0402WGF1102TCE | 1% 0402, LCSC verified |  |
| U1 | 1 | PMIC_P1 | Package_DFN_QFN:QFN-60-1EP_7x7mm_P0.4mm_EP3.4x3.4mm | SpacemiT SPM8821 | from footprint descr |  |
| U2 | 1 | TPS22919 | Package_TO_SOT_SMD:SOT-363_SC-70-6 | TI TPS22919DCKR | value+package |  |
| U3 | 1 | CLK_BUF_1TO2 | Package_SO:MSOP-10_3x3mm_P0.5mm | 待定 | 仅功能描述，无厂商/型号 |  |
| U4 | 1 | SE050C2 | KeyV2_Footprints:HX2QFN20_3x3mm_P0.4mm | NXP SE050C2 | value; LCSC not verified |  |
| U5 | 1 | EMMC_16G | KeyV2_Footprints:EMMC_BGA153 | 待定 | 仅容量/封装，无厂商/料号 |  |
| U6 | 1 | FUSB302 | KeyV2_Footprints:FUSB302 | ON Semi FUSB302MPX | from footprint descr |  |
| U7 | 1 | DCDC_12V_3V3 | Package_TO_SOT_SMD:SOT-23-6 | 待定 | 仅功能描述，无厂商/型号 |  |
| U8,U14 | 2 | PI3DBS16412 | KeyV2_Footprints:PI3DBS16412 | Diodes PI3DBS16412 | value |  |
| U9 | 1 | SN74LVC2T45 | Package_SO:VSSOP-8_3x3mm_P0.65mm | TI SN74LVC2T45DCUR | value+package |  |
| U10 | 1 | LDO_5V_3V3 | Package_TO_SOT_SMD:SOT-23-5 | 待定 | 仅功能描述，无厂商/型号 |  |
| U11 | 1 | SN74LVC1T45 | Package_TO_SOT_SMD:SOT-23-6 | TI SN74LVC1T45DBVR | value+package |  |
| U12 | 1 | SOM_B1 | KeyV2_Footprints:B1_PsP_568 | SpacemiT B1 | module value |  |
| U13 | 1 | TS3USB221A | KeyV2_Footprints:TS3USB221A | TI TS3USB221ARSER | value+package |  |
| Y1 | 1 | 24MHz_9pF | Crystal:Crystal_SMD_3225-4Pin_3.2x2.5mm | 待定 | 未给稳定性/负载电容/品牌 |  |
| Y2 | 1 | XO_100M | KeyV2_Footprints:XO_3225 | 待定 | 未给输出类型/稳定性/品牌 |  |
