# hw_ic_key — KEY Secure Key Vault (B1 Security Module)

Open-source hardware — KiCad design files for the **KEY module**, the secure
key vault that gates the IOCONVERT V2.0 conversion card family. It carries
the hardware root of trust and issues the pre-authentication handshake before
any data path is enabled.

## Overview

KEY is a B1 security module that connects to the K1 master gate card over
USB Type-C. It holds the hardware vault, flash storage, and the SE050 secure
element that performs mutual authentication with the gatekeeper MCU. Only
after successful pre-authentication is the PCIe data path allowed to start.

```
KEY (B1 security module)
├─ B1 hardware vault + eMMC flash
├─ SE050 secure element (pre-auth / mutual auth over SBU I2C)
├─ M.2 M-key socket
├─ FUSB302 USB-C controller
└─ Type-C link → K1 Master Gate Card
```

## Key Features

- **Secure element**: NXP SE050 (hardware root of trust, key storage)
- **Storage**: eMMC (BGA-153) + LPDDR4 (200-FBGA) memory subsystem
- **Compute**: SOM module (B1 platform) with PMIC + 100 MHz XO
- **Connectivity**: USB Type-C receptacle, M.2 M-key socket
- **Debug**: UART debug header

## Repository Contents

| Path | Description |
|---|---|
| `key_v2.kicad_pro` | KiCad 10 project |
| `sch/` | Schematics: main, power |
| `lib/key_v2_symbols.kicad_sym` | Symbol library — required for schematics |
| `lib/footprint/` | Footprint library (13 custom footprints) |
| `fab/` | BOM (CSV) |
| `boards/` | Configuration (YAML) |

## Getting Started

1. Install [KiCad](https://www.kicad.org/) 10.x
2. Clone this repository
3. Open `key_v2.kicad_pro`
4. If symbols/footprints show as missing, add `lib/key_v2_symbols.kicad_sym`
   to the symbol library table and `lib/footprint/` to the footprint library
   table (global or project-level)

## License

AGPL-3.0 (see `LICENSE`)

---

*Part of the IOCONVERT V2.0 family: [hw_ic_k1](https://github.com/dataxcash/hw_ic_k1) (master gate card) · [hw_ic_k2](https://github.com/dataxcash/hw_ic_k2) (worker data-plane card)*
