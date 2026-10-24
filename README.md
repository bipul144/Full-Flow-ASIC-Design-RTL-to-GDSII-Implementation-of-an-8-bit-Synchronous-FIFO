# Full-Flow ASIC Design: RTL-to-GDSII Implementation of an 8-bit Synchronous FIFO

This repository contains the complete RTL-to-GDSII implementation flow for an 8-bit, 100 MHz Synchronous FIFO using **SCL 180 nm 4-Metal Layer (4LM) CMOS technology**. The project spans architectural design, logic synthesis, physical design (floorplanning, power planning, placement, CTS, routing), sign-off DRC/LVS, and post-layout parasitic extraction and simulation.

---

## ðŸ› ï¸ Tools & Technologies

| Stage | Tool / Technology |
| :--- | :--- |
| **PDK / Process Node** | SCL 180 nm CMOS Technology (4 Metal Layers - TS18SL) |
| **Hardware Description Language** | Verilog / SystemVerilog |
| **Logic Synthesis** | Cadence Genus Synthesis Solution |
| **Place & Route (P&R)** | Cadence Innovus Implementation System |
| **Physical Verification** | Siemens (Mentor) Calibre nmDRC / nmLVS |
| **Layout Editing / Schematic View**| Cadence Virtuoso |
| **Post-Layout Simulation** | Synopsys HSPICE |
| **Constraints / Formats** | SDC, LEF, DEF, LIB, SPEF, GDSII |

---

## ðŸ“ Design Specifications

* **Design:** 8-Bit Synchronous FIFO (Depth: 16)
* **Target Clock Frequency:** 100 MHz ($T_{clk} = 10\text{ ns}$)
* **Operating Corners (MMMC):** 1.68V â€“ 1.98V across SS, TT, FF process corners
* **Clock Skew Target:** $< 300\text{ ps}$
* **Metal Density Target:** 25% â€“ 75% for CMP compliance

---

## âš™ï¸ ASIC Flow & Step-by-Step Implementation

### 1. RTL Design & Functional Verification
* Authored synthesizeable Verilog modules for the 8-bit synchronous FIFO with status flags (`full`, `empty`, `almost_full`, `almost_empty`).
* Developed testbenches and ran behavioral simulation to confirm write/read cycle accuracy and memory pointer boundaries.

### 2. Logic Synthesis & Timing Constraints (Cadence Genus)
* Applied **Synopsys Design Constraints (SDC)** specifying a 100 MHz clock target, input/output delays, and transition times.
* Performed technology mapping using SCL 180 nm standard cell libraries.
* Optimized gate-level netlist and evaluated early area, power, and timing reports.

### 3. Physical Design (Cadence Innovus)
* **Floorplanning & I/O Placement:** Defined core utilization, aspect ratio, and pad-ring IO positioning.
* **Power Delivery Network (PDN):**
  * Constructed 30 Âµm power/ground rings.
  * Designed mesh stripes using `TOP_M` and `M3`.
  * Executed `SRoute` for standard cell rail connection on `M1`.
* **Placement & Optimization:** Placed standard cells and inserted tie-hi/tie-lo cells and decap cells.
* **Clock Tree Synthesis (CTS):** Utilized Innovus CCOpt to build a balanced clock tree, achieving $< 300\text{ ps}$ clock skew and balanced insertion delay.
* **Routing & Timing Closure:** Executed multi-cut via insertion, nanoRoute detailed routing, and post-route setup/hold optimization across MMMC PVT corners. Zero WNS/TNS violations achieved.

### 4. Physical Verification & Sign-Off (Siemens Calibre)
* Inserted pad-ring IO fillers, end-cap cells, and standard cell core fillers to maintain N-well/substrate continuity.
* Automated **Calibre SVRF scripts** for dummy metal fill generation to meet the 25%â€“75% CMP density requirement.
* Ran **Calibre nmDRC** and **nmLVS** to ensure 100% design rule and layout versus schematic clean status.

### 5. Parasitic Extraction & SPICE Sign-Off (Cadence Virtuoso / Synopsys HSPICE)
* Extracted RC parasitics to generate the sign-off **SPEF** and **PEX** netlists.
* Back-annotated SPEF into HSPICE for transistor-level timing and power validation.


## ðŸš€ Key Results & Highlights

* **Timing:** Zero WNS / TNS setup & hold violations at 100 MHz sign-off.
* **Clock Tree:** Balanced skew $< 300\text{ ps}$ across all MMMC corners.
* **Physical Verification:** 100% DRC/LVS clean tapeout database.
* **Density:** Fully compliant CMP metal density (25%â€“75%).
