# OpenA64 — Verilog SoC

An open-source System-on-Chip implemented in Verilog, targeting a modern AArch64-style architecture.
(AArch64-style core, NEON/FPU, MMU, caches, coherence, accelerators, and
peripherals). The RTL lives entirely in [`verilog/`](verilog/).

## Disclaimer

OpenA64 is an independent hobby/research project.

It is **not affiliated with, endorsed by, sponsored by, or associated with Arm Ltd.**

The implementation is an original work developed from publicly available architectural documentation.

"Arm", "AArch64", "NEON", and other trademarks belong to their respective owners.
File names such as arm_top.v and arm_core.v are internal identifiers used within this project and do not imply any affiliation with or origin from Arm Ltd.

For inquiries regarding this repository, contact:
hado14.05.2015@gmail.com

## At a glance

| Property | Value |
| --- | --- |
| Language | Verilog (IEEE 1364-2005 style) |
| Module files | 34 (in `verilog/`) |
| Total lines | ~150M |
| Top module | `arm_top` (`verilog/arm_top.v`) |
| ISA theme | AArch64 (ARMv8) — GPR `X0–X30`, `NZCV`, NEON 128-bit SIMD |
| Target library | Sky130 HD (`sky130_fd_sc_hd__tt_025C_1v80.lib`) |

## Project layout

```
OpenA64/
├── verilog/                 # All RTL (34 .v files)
│   ├── arm_top.v            # SoC top-level integration
│   ├── arm_core.v           # CPU core
│   ├── arm_alu.v            # Arithmetic/logic unit
│   ├── arm_mmu.v            # Memory management unit (TTBR/TLB)
│   ├── arm_cache_hierarchy.v
│   ├── arm_npu.v            # Neural-network / MAC accelerator
│   └── ... (34 files total)
└── (PDK files (only sky130 liberty files and lef and tlef files) at repo root and verilog files are comressed for the file size limits — see other docs)
```

## Structure of each file

Every `verilog/*.v` file contains:

1. **The original OpenA64 module** (preserved verbatim) — e.g.
   `module arm_top(...)`, `module arm_alu(...)`, etc.
2. **Additive AArch64 extension modules** appended below it, named
   `<file>_aarch64_<index>` (e.g. `arm_top_aarch64_0000000`). Each extension
   is a self-contained, syntactically valid module modelling a real AArch64
   hardware block (register file, ALU with NZCV flags, TLB entry, MESI cache
   way, NEON SIMD lane, TrustZone/AES block, etc.) plus a `_harness` that
   instantiates it.

The original design is never modified; extensions are purely additive.

## Quick start (read-only)

```bash
# Count lines across the RTL
cat verilog/*.v | wc -l

# List modules defined in a file
grep -n "^module" verilog/arm_top.v

# Inspect the SoC top
less verilog/arm_top.v
```

## Documentation

- [`docs/verilog/MODULES.md`](docs/verilog/MODULES.md) — per-file module
  reference (role, theme, approximate size).
- [`docs/verilog/ARCHITECTURE.md`](docs/verilog/ARCHITECTURE.md) — how the
  blocks connect into a coherent AArch64 SoC.
