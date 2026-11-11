# VLSI for AI — 8×8 INT8 Systolic Accelerator

**SystemVerilog · Sky130A · Yosys · LibreLane · OpenROAD**

An ongoing RTL-to-physical-design project implementing an **8×8 INT8 systolic inference accelerator** with 64 pipelined MAC processing elements, **16 Sky130 SRAM macros**, FSM-based control, AXI-stream interfaces, and quantization. The design has passed cycle-exact functional regression (**10/10 tests; 792/792 outputs**) and completed detailed routing, but **timing, electrical, and antenna-rule closure remain open**. The 10 ns clock is a target, not achieved performance.

## Latest results

| Metric | Baseline | Registered FSM | Post-GRT timing optimization |
|---|---:|---:|---:|
| Setup WNS (ns) | −5.636 | −5.112 | −6.322 |
| Setup TNS (ns) | −6,988 | −6,305 | −3,471 |
| Setup violations | 4,014 | 4,310 | 3,514 |
| Hold violations | 1 | 11 | 17 |
| Max slew violations | 1,926 | 1,757 | 2,187 |
| Max capacitance violations | 280 | 240 | 232 |
| Max fanout violations | 135 | 128 | 264 |

*Values are from the respective routed STA reports; improvements in one metric do not imply signoff closure.*

## Current research

- **Antenna effects:** The registered-FSM run has **4 final antenna-rule net violations**. On 49 fanout-violating flip-flop outputs, **549 of 654 connected loads** are antenna diodes; none of those 49 exceeds the fanout limit when diode loads are excluded.
- **Flow experiments:** Enabling LibreLane's experimental `RUN_POST_GRT_RESIZER_TIMING` reduced setup TNS but worsened setup WNS, slew, and fanout. Investigating repair-stage ordering, diode insertion, and routing parasitics before further RTL changes.
- **Open-source goal:** Document reproducible failures and fixes, with the aim of contributing validated improvements or bug reports to LibreLane/OpenROAD. **No upstream contribution is claimed yet.**

## Next steps

1. Resolve final antenna-rule violations without disabling protection.
2. Test targeted electrical repair and compare timing, slew, capacitance, fanout, area, and congestion.
3. Publish selected configurations, reports, and experiment notes as the project evolves.

**Status:** Research prototype — not timing-closed, signoff-clean, or taped out.
