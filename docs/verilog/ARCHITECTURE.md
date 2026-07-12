# OpenA64 Verilog — Architecture

How the blocks in [`../../verilog/`](../../verilog/) fit together into an
AArch64-compatible SoC. This is a documentation view of the RTL; the original
modules are preserved and the AArch64 extension modules are appended
additively below them.

## Top-level integration

`arm_top` (`verilog/arm_top.v`) is the SoC root. It instantiates the major
subsystems and wires their status/request busses:

```
arm_top
└── arm_efficiency_cluster      (big.LITTLE tiles)
    └── arm_core                (CPU pipeline)
        ├── arm_pipeline        (fetch→decode→rename→issue→exec→wb)
        ├── arm_decoder         (ARMv8 decode)
        ├── arm_rename_unit     (X0–X30 + FLAGS rename map)
        ├── arm_issue_queue     (dispatch/issue slots)
        ├── arm_reorder_buffer  (ROB entries)
        ├── arm_alu             (ADD/SUB/.../NZCV)
        ├── arm_fpu             (NEON/FP 128-bit SIMD)
        ├── arm_branch_predictor(BTB + TAGE)
        ├── arm_load_store_unit (LSU queue)
        └── arm_mmu             (TTBR0/TTBR1 TLB)
├── arm_cache_hierarchy         (L1I/L1D/L2, MESI)
├── arm_coherence               (MESI directory)
├── arm_interconnect            (CHI/AMBA NoC)
├── arm_memory_controller       (DDR channels)
├── arm_interrupt_controller    (GICv3)
├── arm_debug_unit              (trace buffer)
├── arm_security_unit           (TrustZone / AES)
├── arm_pmu                     (PMU counters)
├── arm_power_ctrl              (power domains / DVFS)
├── arm_dma                     (DMA channels)
├── arm_timer                   (generic timers)
├── arm_peripherals             (APB ports)
├── arm_soc_top                 (subsystem wrappers)
└── accelerators
    ├── arm_npu                 (ML MAC array)
    ├── arm_gpu / arm_igpu      (shader / render units)
    ├── arm_image_signal_processor
    ├── arm_video_decoder
    └── arm_display_controller
```

## Data flow (simplified)

1. **Fetch/Decode** — `arm_pipeline` + `arm_instruction_buffer`/`queue` feed
   `arm_decoder` (ARMv8 decode LUT).
2. **Rename/Issue** — `arm_rename_unit` maps architectural `X0–X30` to
   physical registers; `arm_issue_queue` dispatches to `arm_reorder_buffer`.
3. **Execute** — `arm_alu` (scalar, with `NZCV`), `arm_fpu` (NEON 128-bit
   SIMD), `arm_branch_predictor` (BTB+TAGE).
4. **Memory** — `arm_load_store_unit` → `arm_mmu` (TLB) →
   `arm_cache_hierarchy` → `arm_coherence` (MESI) → `arm_interconnect` →
   `arm_memory_controller` (DDR).
5. **Commit** — `arm_reorder_buffer` retires in order; `arm_pmu` counts events.
6. **System** — `arm_interrupt_controller` (GICv3), `arm_timer`,
   `arm_security_unit` (TrustZone/AES), `arm_power_ctrl` (DVFS),
   `arm_debug_unit` (trace).

## AArch64 modelling notes

- **Registers:** extensions use `gpr[X0..X30]` (64-bit) and 128-bit vectors
  for NEON/FPU/NPU.
- **Flags:** ALU extensions compute real `NZCV` (negative, zero, carry,
  overflow) from `ADD/SUB/...`.
- **Memory:** `arm_mmu` extensions model `TTBR0`/`TTBR1` TLB entries; caches
  use MESI coherence via `arm_coherence`.
- **Accelerators:** `arm_npu` MAC arrays (int8/FP16), `arm_gpu`/`arm_igpu`
  shader/render units, ISP/video/decode blocks.

## Notes on the RTL scale

The repository is ~150M lines. The original OpenA64 modules are preserved at
the top of each file; the remaining ~83M lines are additive AArch64 extension
modules appended below. This keeps the design self-consistent and the original
IP intact while scaling the codebase.
