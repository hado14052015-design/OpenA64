# OpenA64 Roadmap

> **Project status:** Experimental research project.

This roadmap tracks the major milestones planned for OpenA64. Items may
change as the architecture evolves.

------------------------------------------------------------------------

## Phase 1 -- Core RTL

-   [x] Initial project structure
-   [x] Core Verilog modules
-   [x] Cache hierarchy
-   [x] MMU framework
-   [x] NPU module
-   [x] Project documentation
-   [x] MIT License

------------------------------------------------------------------------

## Phase 2 -- Verification

-   [ ] Unit test major RTL blocks
-   [ ] Add regression testbench
-   [ ] Verify exception handling
-   [ ] Verify cache behavior
-   [ ] Verify MMU operation
-   [ ] Verify interrupt controller

------------------------------------------------------------------------

## Phase 3 -- Synthesis

-   [ ] Clean synthesis with Yosys
-   [ ] Resolve synthesis warnings
-   [ ] Generate timing reports
-   [ ] Generate area reports

------------------------------------------------------------------------

## Phase 4 -- Physical Design

-   [ ] OpenLane flow
-   [ ] Floorplanning
-   [ ] Placement
-   [ ] Clock Tree Synthesis
-   [ ] Routing
-   [ ] DRC clean
-   [ ] LVS clean
-   [ ] GDSII generation

------------------------------------------------------------------------

## Phase 5 -- Bring-up

-   [ ] Execute first instruction in simulation
-   [ ] Run bare-metal test program
-   [ ] Verify exception vectors
-   [ ] Validate memory subsystem

------------------------------------------------------------------------

## Phase 6 -- FPGA

-   [ ] FPGA prototype
-   [ ] UART output
-   [ ] Memory controller
-   [ ] Demonstration programs

------------------------------------------------------------------------

## Phase 7 -- Long-Term Goals

-   [ ] Improve performance
-   [ ] Expand documentation
-   [ ] Additional peripherals
-   [ ] Continuous verification
-   [ ] Community contributions

------------------------------------------------------------------------

## Notes

OpenA64 is an experimental hobby/research project. Dates are
intentionally omitted because development follows learning and
experimentation rather than a fixed release schedule.
