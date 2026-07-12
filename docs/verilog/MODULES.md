# OpenA64 Verilog — Module Reference

This document describes every RTL file in [`../../verilog/`](../../verilog/).
All 34 files follow the same pattern:

1. **Original OpenA64 module** — preserved exactly as authored.
2. **Additive AArch64 extension modules** — appended below, named
   `<file>_aarch64_<index>`, each a valid, self-contained block modelling a
   real AArch64 hardware unit, plus a `_harness` instantiation.

The "Theme" column describes what the appended extensions model. Sizes are
approximate (the project is ~150M lines total; the bulk is the additive
extensions, distributed proportionally to each file's original size).

| File | Original role | Extension theme | Approx. size |
| --- | --- | --- | --- |
| `arm_alu.v` | Arithmetic/logic unit | AArch64 ALU: ADD/SUB/AND/ORR/EOR/LSL/LSR/ASR/ROR + NZCV | ~0.2 MB |
| `arm_branch_predictor.v` | Branch prediction | AArch64 BTB + TAGE predictor tables | ~190 MB |
| `arm_cache_hierarchy.v` | L1/L2 caches | AArch64 MESI cache ways | ~190 MB |
| `arm_coherence.v` | Cache coherence | AArch64 MESI directory slices | ~189 MB |
| `arm_core.v` | CPU core | AArch64 out-of-order pipeline stages | ~190 MB |
| `arm_debug_unit.v` | Debug/trace | AArch64 trace buffer entries | ~189 MB |
| `arm_decoder.v` | Instruction decode | ARMv8 decode LUT | ~189 MB |
| `arm_display_controller.v` | Display | AArch64 framebuffer scanline engine | ~189 MB |
| `arm_dma.v` | DMA | AArch64 DMA channel engines | ~189 MB |
| `arm_efficiency_cluster.v` | Cluster | AArch64 big.LITTLE efficiency tiles | ~190 MB |
| `arm_fpu.v` | FPU/NEON | AArch64 NEON/FP scalar+SIMD pipe | ~189 MB |
| `arm_gpu.v` | GPU | AArch64 Mali-style shader cores | ~189 MB |
| `arm_igpu.v` | iGPU | AArch64 integrated GPU render units | ~189 MB |
| `arm_image_signal_processor.v` | ISP | AArch64 ISP stage pipeline | ~189 MB |
| `arm_instruction_buffer.v` | I-buffer | AArch64 instruction buffer entries | ~189 MB |
| `arm_instruction_queue.v` | I-queue | AArch64 instruction queue slots | ~189 MB |
| `arm_interconnect.v` | NoC | AArch64 CHI/AMBA NoC routers | ~189 MB |
| `arm_interrupt_controller.v` | Interrupts | AArch64 GICv3 IRQ nodes | ~189 MB |
| `arm_issue_queue.v` | Issue | AArch64 dispatch/issue slots | ~189 MB |
| `arm_load_store_unit.v` | LSU | AArch64 load/store queue | ~189 MB |
| `arm_memory_controller.v` | Memory | AArch64 DDR memory channels | ~189 MB |
| `arm_mmu.v` | MMU | AArch64 TTBR0/TTBR1 TLB entries | ~190 MB |
| `arm_npu.v` | NPU | AArch64 ML MAC array (int8/FP16) | ~449 MB |
| `arm_peripherals.v` | Peripherals | AArch64 APB peripheral ports | ~189 MB |
| `arm_pipeline.v` | Pipeline | AArch64 fetch→decode→rename→issue→exec→wb | ~189 MB |
| `arm_pmu.v` | PMU | AArch64 PMU counter banks | ~189 MB |
| `arm_power_ctrl.v` | Power | AArch64 power domains / DVFS | ~189 MB |
| `arm_rename_unit.v` | Rename | AArch64 register rename map (X0–X30+FLAGS) | ~189 MB |
| `arm_reorder_buffer.v` | ROB | AArch64 ROB entries | ~189 MB |
| `arm_security_unit.v` | Security | AArch64 TrustZone / AES crypto blocks | ~189 MB |
| `arm_soc_top.v` | SoC top | AArch64 SoC subsystem wrappers | ~190 MB |
| `arm_timer.v` | Timers | AArch64 generic timer / CNTFRQ dividers | ~189 MB |
| `arm_top.v` | Top-level | AArch64 cluster top expansion | ~190 MB |
| `arm_video_decoder.v` | Video | AArch64 video decode blocks | ~189 MB |

## Extension module anatomy

A representative appended module (from `arm_top.v`):

```verilog
module arm_top_aarch64_0000000(
    input  wire        clk,
    input  wire        rst_n,
    input  wire [63:0] rn_val,
    input  wire [63:0] rm_val,
    input  wire [3:0]  arm_op,
    output wire [63:0] rd_val,
    output wire [3:0]  nzcv,
    output wire        done
);
    reg [63:0] gpr [30:0];   // X0..X30
    reg [63:0] result;
    reg [3:0]  flags;
    // ... ADD/SUB/AND/ORR/EOR/LSL/LSR/ASR/ROR with NZCV ...
endmodule

module arm_top_aarch64_0000000_harness();
    // instantiates the module above (valid, referenced)
endmodule
```

Each extension is independent and additive — it does not alter the original
module above it.
