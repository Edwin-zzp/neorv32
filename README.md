# [The NEORV32 Processor](https://github.com/stnolting/neorv32) (RISC-V)

[![Build Status](https://travis-ci.com/stnolting/neorv32.svg?branch=master)](https://travis-ci.com/stnolting/neorv32)
[![license](https://img.shields.io/github/license/stnolting/neorv32)](https://github.com/stnolting/neorv32/blob/master/LICENSE)
[![release](https://img.shields.io/github/v/release/stnolting/neorv32)](https://github.com/stnolting/neorv32/releases)


## Table of Content

* [Overview](#Overview)
* [Project Status](#Status)
* [Features](#Features)
* [FPGA Implementation Results](#FPGA-Implementation-Results)
* [Performance](#Performance)
* [Top Entities](#Top-Entities)
* [**Getting Started**](#Getting-Started)
* [Contribute](#Contribute)
* [Legal](#Legal)



## Overview

The NEORV32 Processor is a customizable microcontroller-like system on chip (SoC) that is based
on the RISC-V-compliant NEORV32 CPU. The project consists of two main parts:


### [NEORV32 CPU](#CPU-Features)

The CPU implements an `rv32i RISC-V` core with optional `C`, `E`, `M`, `U`, `Zicsr`, `Zifencei` and
`PMP` (physical memory protection) extensions. It passes the official [RISC-V compliance tests](https://github.com/stnolting/neorv32_riscv_compliance)
and is compliant to the *Unprivileged ISA Specification [Version 2.2](https://github.com/stnolting/neorv32/blob/master/docs/riscv-privileged.pdf)*
and a subset of the *Privileged Architecture Specification [Version 1.12-draft](https://github.com/stnolting/neorv32/blob/master/docs/riscv-spec.pdf)*.

If you do not want to use the NEORV32 Processor setup, you can also use the CPU in
stand-alone mode and build your own SoC around it.


### [NEORV32 Processor](#Processor-Features)

Based on the NEORV32 CPU, the NEORV32 Processor is a full-scale RISC-V microcontroller system (**SoC**)
that already provides common peripherals like GPIO, serial interfaces, timers, embedded
memories and an external bus interface for connectivity and custom extension.
All optional features and modules beyond the base CPU can be enabled and configured via
[VHDL generics](#Top-Entities).

The processor is intended as ready-to-use auxiliary processor within a larger SoC
designs or as stand-alone custom microcontroller. Its top entity can be directly
synthesized for any target technology without modifications.

This project comes with a complete software ecosystem that features core
libraries for high-level usage of the provided functions and peripherals,
makefiles, a runtime environment, several example programs to start with - including a free RTOS demo - and
even a builtin bootloader for easy program upload via UART.
All software source files provide a doxygen-based documentary (available on [GitHub pages](https://stnolting.github.io/neorv32/files.html)).


### [How to get started?](#Getting-Started)

The processor is intended to work "out of the box". Just synthesize the
[test setup](#Create-a-new-Hardware-Project), upload it to your FPGA board of choice and start playing
with the NEORV32. If you do not want to [compile the GCC toolchains](https://github.com/riscv/riscv-gnu-toolchain) by yourself, you can also
download [pre-compiled toolchains](https://github.com/stnolting/riscv_gcc_prebuilt) for Linux.

For more information take a look at the [![NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/figures/PDF_32.png) NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/NEORV32.pdf).


### Key Features

* RISC-V-compliant `rv32i` CPU with optional `C`, `E`, `M`, `U`, `Zicsr`, `Zifencei` and `PMP` (physical memory protection) extensions
* GCC-based toolchain ([pre-compiled rv32i and rv32e toolchains available](https://github.com/stnolting/riscv_gcc_prebuilt))
* Application compilation based on [GNU makefiles](https://github.com/stnolting/neorv32/blob/master/sw/example/blink_led/makefile)
* [Doxygen-based](https://github.com/stnolting/neorv32/blob/master/docs/doxygen_makefile_sw) documentation of the software framework: available on [GitHub pages](https://stnolting.github.io/neorv32/files.html)
* [**Detailed data sheet**](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/NEORV32.pdf) (pdf)
* Completely described in behavioral, platform-independent VHDL - no primitives, macros, etc.
* Fully synchronous design, no latches, no gated clocks
* Small hardware footprint and high operating frequency
* Highly configurable CPU and processor setup
* [FreeRTOS port](https://github.com/stnolting/neorv32/blob/master/sw/example/demo_freeRTOS) available


### Design Principles

 * From zero to `main()`: Completely open source and documented.
 * Plain VHDL without technology-specific parts like attributes, macros or primitives.
 * Easy to use – working out of the box.
 * Clean synchronous design, no wacky combinatorial interfaces.
 * Be as small as possible – but with a reasonable size-performance tradeoff.
 * The processor has to fit in a Lattice iCE40 UltraPlus 5k FPGA running at 20+ MHz.


## Status

The processor is [synthesizable](#FPGA-Implementation-Results) (tested on *real hardware* using Intel Quartus Prime, Xilinx Vivado and Lattice Radiant/Synplify Pro) and can successfully execute
all the [provided example programs](https://github.com/stnolting/neorv32/tree/master/sw/example) including the [CoreMark benchmark](#CoreMark-Benchmark).

The processor passes the official `rv32i`, `rv32im`, `rv32imc`, `rv32Zicsr` and `rv32Zifencei` [RISC-V compliance tests](https://github.com/riscv/riscv-compliance). 

The project’s change log is available in the [CHANGELOG.md](https://github.com/stnolting/neorv32/blob/master/CHANGELOG.md) file in the root directory of this repository.

| Project component                                                               | CI status | Note     |
|:--------------------------------------------------------------------------------|:----------|:---------|
| [NEORV32 processor](https://github.com/stnolting/neorv32)                       | [![Build Status](https://travis-ci.com/stnolting/neorv32.svg?branch=master)](https://travis-ci.com/stnolting/neorv32) | [![sw doc](https://img.shields.io/badge/SW%20documentation-gh--pages-blue)](https://stnolting.github.io/neorv32/files.html) |
| [Pre-built toolchain](https://github.com/stnolting/riscv_gcc_prebuilt)          | [![Build Status](https://travis-ci.com/stnolting/riscv_gcc_prebuilt.svg?branch=master)](https://travis-ci.com/stnolting/riscv_gcc_prebuilt) | |
| [RISC-V compliance test](https://github.com/stnolting/neorv32_riscv_compliance) | [![Build Status](https://travis-ci.com/stnolting/neorv32_riscv_compliance.svg?branch=master)](https://travis-ci.com/stnolting/neorv32_riscv_compliance) | |


### To-Do / Wish List

* Add AXI(-Lite) bridges
* Synthesis results (+ wrappers?) for more platforms
* Maybe port additional RTOSs (like [Zephyr](https://github.com/zephyrproject-rtos/zephyr) or [RIOT](https://www.riot-os.org))
* Implement further CPU extensions:
  * Atomic operations (`A`)
  * Bitmanipulation operations (`B`), when they are "official"
  * Floating-point instructions (`F`)
  * ...


## Features

The full-blown data sheet of the NEORV32 Processor/CPU is available as pdf file:
[![NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/figures/PDF_32.png) NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/NEORV32.pdf).

### Processor Features

![neorv32 Overview](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/figures/neorv32_processor.png)

The NEORV32 Processor provides a full-scale microcontroller-like SoC based on the NEORV32 CPU. The setup
is highly customizable via the processor's top generics.

* Optional processor-internal data and instruction memories (**DMEM** / **IMEM**)
* Optional internal **Bootloader** with UART console and automatic SPI flash boot option
* Optional machine system timer (**MTIME**), RISC-V-compliant
* Optional universal asynchronous receiver and transmitter (**UART**) with simulation output option via text.io
* Optional 8/16/24/32-bit serial peripheral interface controller (**SPI**) with 8 dedicated chip select lines
* Optional two wire serial interface controller (**TWI**), compatible to the I²C standard
* Optional general purpose parallel IO port (**GPIO**), 32xOut & 32xIn, with pin-change interrupt
* Optional 32-bit external bus interface, Wishbone b4 compliant (**WISHBONE**), *standard* or *pipelined* handshake/transactions mode
* Optional watchdog timer (**WDT**)
* Optional PWM controller with 4 channels and 8-bit duty cycle resolution (**PWM**)
* Optional GARO-based true random number generator (**TRNG**)
* Optional custom functions unit (**CFU**) for tightly-coupled custom co-processors
* System configuration information memory to check hardware configuration by software (**SYSINFO**)

### CPU Features

![neorv32 Overview](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/figures/neorv32_cpu.png)

The CPU is [compliant](https://github.com/stnolting/neorv32_riscv_compliance) to the
[official RISC-V specifications (2.2)](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/riscv-spec.pdf) including a subset of the 
[RISC-V privileged architecture specifications (1.12-draft)](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/riscv-spec.pdf).

More information regarding the CPU including a detailed list of the instruction set and the available CSRs can be found in
the [NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/NEORV32.pdf).


**General**:
  * Modified Harvard architecture (separate CPU interfaces for data and instructions; NEORV32 processor: Single processor-internal bus via I/D mux)
  * Two stages in-order pipeline (FETCH, EXECUTE); each stage uses a multi-cycle processing scheme
  * No hardware support of unaligned accesses - they will trigger an exception
  * Little-endian byte order
  * All reserved or unimplemented instructions will raise an illegal instruction exception
  * Privilege levels: `machine` mode, `user` mode (if enabled via `U` extension)


**RV32I base instruction set** (`I` extension):
  * ALU instructions: `LUI` `AUIPC` `ADDI` `SLTI` `SLTIU` `XORI` `ORI` `ANDI` `SLLI` `SRLI` `SRAI` `ADD` `SUB` `SLL` `SLT` `SLTU` `XOR` `SRL` `SRA` `OR` `AND`
  * Jump and branch instructions: `JAL` `JALR` `BEQ` `BNE` `BLT` `BGE` `BLTU` `BGEU` 
  * Memory instructions: `LB` `LH` `LW` `LBU` `LHU` `SB` `SH` `SW`
  * System instructions: `ECALL` `EBREAK` `FENCE`

**Compressed instructions** (`C` extension):
  * ALU instructions: `C.ADDI4SPN` `C.ADDI` `C.ADD` `C.ADDI16SP` `C.LI` `C.LUI` `C.SLLI` `C.SRLI` `C.SRAI` `C.ANDI` `C.SUB` `C.XOR` `C.OR` `C.AND` `C.MV` `C.NOP`
  * Jump and branch instructions: `C.J` `C.JAL` `C.JR` `C.JALR` `C.BEQZ` `C.BNEZ`
  * Memory instructions: `C.LW` `C.SW` `C.LWSP` `C.SWSP`
  * System instructions: `C.EBREAK` (only with `Zicsr` extension)

**Embedded CPU version** (`E` extension):
  * Reduced register file (only the 16 lowest registers)

**Integer multiplication and division hardware** (`M` extension):
  * Multiplication instructions: `MUL` `MULH` `MULHSU` `MULHU`
  * Division instructions: `DIV` `DIVU` `REM` `REMU`
  * By default, the multiplier and divider cores use an iterative bit-serial processing scheme
  * Multiplications can be mapped to DSPs via the `FAST_MUL_EN` generic to increase performance

**Privileged architecture / CSR access** (`Zicsr` extension):
  * Privilege levels: `M-mode` (Machine mode)
  * CSR access instructions: `CSRRW` `CSRRS` `CSRRC` `CSRRWI` `CSRRSI` `CSRRCI`
  * System instructions: `MRET` `WFI`
  * Counter CSRs: `cycle` `cycleh` `instret` `instreth` `time` `timeh` `mcycle` `mcycleh` `minstret` `minstreth`
  * Machine CSRs: `mstatus` `misa`(read-only!) `mie` `mtvec` `mscratch` `mepc` `mcause` `mtval` `mip` `mvendorid` [`marchid`](https://github.com/riscv/riscv-isa-manual/blob/master/marchid.md) `mimpid` `mhartid` `mzext`(custom)
  * Supported exceptions and interrupts:
    * Misaligned instruction address
    * Instruction access fault
    * Illegal instruction
    * Breakpoint (via `ebreak` instruction)
    * Load address misaligned
    * Load access fault
    * Store address misaligned
    * Store access fault
    * Environment call from M-mode (via `ecall` instruction)
    * Machine timer interrupt `mti` (via processor's MTIME unit)
    * Machine software interrupt `msi` (via external signal)
    * Machine external interrupt `mei` (via external signal)
    * Four fast interrupt requests (custom extension)

**Privileged architecture / User mode** (`U` extension, requires `Zicsr` extension):
  * Privilege levels: `M-mode` (Machine mode) + `U-mode` (User mode)

**Privileged architecture / FENCE.I** (`Zifencei` extension):
  * System instructions: `FENCE.I`

**Privileged architecture / Physical memory protection** (`PMP`, requires `Zicsr` extension):
  * Additional machine CSRs: `pmpcfg0` `pmpcfg1` `pmpaddr0` `pmpaddr1` `pmpaddr2` `pmpaddr3` `pmpaddr4` `pmpaddr5` `pmpaddr6` `pmpaddr7`


### Non-RISC-V-Compliant Issues

* `misa` CSR is read-only - no dynamic enabling/disabling of synthesized CPU extensions during runtime; for compatibility: write accesses (in m-mode) are ignored and do not cause an exception
* The physical memory protection (**PMP**) only supports `NAPOT` mode, a minimal granularity of 8 bytes and only up to 8 regions


### NEORV32-Specific CPU Extensions

The NEORV32-specific extensions are always enabled and are indicated via the `X` bit in the `misa` CSR.

* Four *fast interrupt* request channels with according control/status bits in `mie` and `mip` and custom exception codes in `mcause`
* `mzext` CSR to check for implemented `Z*` CPU extensions (like `Zifencei`)



## FPGA Implementation Results

### NEORV32 CPU

This chapter shows exemplary implementation results of the NEORV32 CPU for an **Intel Cyclone IV EP4CE22F17C6N FPGA** on
a DE0-nano board. The design was synthesized using **Intel Quartus Prime Lite 19.1** ("balanced implementation"). The timing
information is derived from the Timing Analyzer / Slow 1200mV 0C Model. If not otherwise specified, the default configuration
of the CPU's generics is assumed (for example no PMP). No constraints were used at all.

Results generated for hardware version: `1.4.4.8`

| CPU Configuration                      | LEs        | FFs      | Memory bits | DSPs | f_max    |
|:---------------------------------------|:----------:|:--------:|:-----------:|:----:|:--------:|
| `rv32i`                                |        983 |      438 |       2048  |    0 | ~120 MHz |
| `rv32i`   + `u` + `Zicsr` + `Zifencei` |       1877 |      802 |       2048  |    0 | ~112 MHz |
| `rv32im`  + `u` + `Zicsr` + `Zifencei` |       2374 |     1048 |       2048  |    0 | ~110 MHz |
| `rv32imc` + `u` + `Zicsr` + `Zifencei` |       2650 |     1064 |       2048  |    0 | ~110 MHz |
| `rv32emc` + `u` + `Zicsr` + `Zifencei` |       2680 |     1061 |       1024  |    0 | ~110 MHz |


### NEORV32 Processor-Internal Peripherals and Memories

Results generated for hardware version: `1.4.4.8`

| Module    | Description                                          | LEs | FFs | Memory bits | DSPs |
|:----------|:-----------------------------------------------------|----:|----:|------------:|-----:|
| BOOT ROM  | Bootloader ROM (default 4kB)                         |   4 |   1 |      32 768 |    0 |
| BUSSWITCH | Mux for CPU I & D interfaces                         |  62 |   8 |           0 |    0 |
| CFU       | Custom functions unit                                |   - |   - |           - |    - |
| DMEM      | Processor-internal data memory (default 8kB)         |  13 |   2 |      65 536 |    0 |
| GPIO      | General purpose input/output ports                   |  66 |  65 |           0 |    0 |
| IMEM      | Processor-internal instruction memory (default 16kb) |   7 |   2 |     131 072 |    0 |
| MTIME     | Machine system timer                                 | 268 | 166 |           0 |    0 |
| PWM       | Pulse-width modulation controller                    |  72 |  69 |           0 |    0 |
| SPI       | Serial peripheral interface                          | 184 | 125 |           0 |    0 |
| SYSINFO   | System configuration information memory              |  11 |   9 |           0 |    0 |
| TRNG      | True random number generator                         | 132 | 105 |           0 |    0 |
| TWI       | Two-wire interface                                   |  74 |  44 |           0 |    0 |
| UART      | Universal asynchronous receiver/transmitter          | 175 | 132 |           0 |    0 |
| WDT       | Watchdog timer                                       |  58 |  45 |           0 |    0 |
| WISHBONE  | External memory interface (`MEM_EXT_REG_STAGES` = 2) | 106 | 104 |           0 |    0 |


### NEORV32 Processor - Exemplary FPGA Setups

Exemplary processor implementation results for different FPGA platforms. The processor setup uses *the default peripheral configuration* (like no _CFU_ and no _TRNG_),
no external memory interface and only internal instruction and data memories. IMEM uses 16kB and DMEM uses 8kB memory space. The setup's top entity connects most of the
processor's [top entity](https://github.com/stnolting/neorv32/blob/master/rtl/core/neorv32_top.vhd) signals
to FPGA pins - except for the Wishbone bus and the interrupt signals.

Results generated for hardware version: `1.4.4.8`

| Vendor  | FPGA                              | Board            | Toolchain                  | Strategy | CPU Configuration                              | LUT / LE   | FF / REG   | DSP    | Memory Bits  | BRAM / EBR | SPRAM    | Frequency     |
|:--------|:----------------------------------|:-----------------|:---------------------------|:-------- |:-----------------------------------------------|:-----------|:-----------|:-------|:-------------|:-----------|:---------|--------------:|
| Intel   | Cyclone IV `EP4CE22F17C6N`        | Terasic DE0-Nano | Quartus Prime Lite 19.1    | balanced | `rv32imc` + `u` + `Zicsr` + `Zifencei` + `PMP` | 4008 (18%) | 1849  (9%) | 0 (0%) | 231424 (38%) |          - |        - |       105 MHz |
| Lattice | iCE40 UltraPlus `iCE40UP5K-SG48I` | Upduino v2.0     | Radiant 2.1 (Synplify Pro) | default  | `rv32ic`  + `u` + `Zicsr` + `Zifencei`         | 4296 (81%) | 1611 (30%) | 0 (0%) |            - |   12 (40%) | 4 (100%) |  *c* 22.5 MHz |
| Xilinx  | Artix-7 `XC7A35TICSG324-1L`       | Arty A7-35T      | Vivado 2019.2              | default  | `rv32imc` + `u` + `Zicsr` + `Zifencei` + `PMP` | 2390 (11%) | 1888  (5%) | 0 (0%) |            - |    8 (16%) |        - |   *c* 100 MHz |

**_Notes_**
* The Lattice iCE40 UltraPlus setup uses the FPGA's SPRAM memory primitives for the internal IMEM and DMEM (each 64kb).
The FPGA-specific memory components can be found in [`rtl/fpga_specific`](https://github.com/stnolting/neorv32/blob/master/rtl/fpga_specific/lattice_ice40up).
* The clock frequencies marked with a "c" are constrained clocks. The remaining ones are _f_max_ results from the place and route timing reports.
* The Upduino and the Arty board have on-board SPI flash memories for storing the FPGA configuration. These device can also be used by the default NEORV32
bootloader to store and automatically boot an application program after reset (both tested successfully).
* The setups with `PMP` implement 2 regions with a minimal granularity of 32kB.



## Performance

### CoreMark Benchmark

The [CoreMark CPU benchmark](https://www.eembc.org/coremark) was executed on the NEORV32 and is available in the
[sw/example/coremark](https://github.com/stnolting/neorv32/blob/master/sw/example/coremark) project folder. This benchmark
tests the capabilities of a CPU itself rather than the functions provided by the whole system / SoC.

Results generated for hardware version: `1.4.4.8`

~~~
**Configuration**
Hardware:    32kB IMEM, 16kB DMEM, 100MHz clock
CoreMark:    2000 iterations, MEM_METHOD is MEM_STACK
Compiler:    RISCV32-GCC 10.1.0 (rv32i toolchain)
Flags:       default, see makefile
Peripherals: UART for printing the results
~~~

| CPU                       | Executable Size | Optimization | CoreMark Score | CoreMarks/MHz |
|:--------------------------|:---------------:|:------------:|:--------------:|:-------------:|
| `rv32i`                   |    26 940 bytes |        `-O3` |          33.89 |    **0.3389** |
| `rv32im`                  |    25 772 bytes |        `-O3` |          64.51 |    **0.6451** |
| `rv32imc`                 |    20 524 bytes |        `-O3` |          64.51 |    **0.6451** |
| `rv32imc` + `FAST_MUL_EN` |    20 524 bytes |        `-O3` |          80.00 |    **0.8000** |

The `FAST_MUL_EN` configuration uses DSPs for the multiplier of the `M` extension (enabled via the `FAST_MUL_EN` generic).

When the `C` extension is enabled, branches to an unaligned uncompressed instruction require additional instruction fetch cycles.

### Instruction Cycles

The NEORV32 CPU is based on a two-stages pipelined architecutre. Each stage uses a multi-cycle processing scheme. Hence,
each instruction requires several clock cycles to execute (2 cycles for ALU operations, ..., 40 cycles for divisions).
The average CPI (cycles per instruction) depends on the instruction mix of a specific applications and also on the available
CPU extensions.

Please note that the CPU-internal shifter (e.g. for the `SLL` instruction) as well as the multiplier and divider of the
`M` extension use a bit-serial approach and require several cycles for completion.

The following table shows the performance results for successfully running 2000 CoreMark
iterations, which reflects a pretty good "real-life" work load. The average CPI is computed by
dividing the total number of required clock cycles (only the timed core to avoid distortion due to IO wait cycles; sampled via the `cycle[h]` CSRs)
by the number of executed instructions (`instret[h]` CSRs). The executables were generated using optimization `-O3`.

Results generated for hardware version: `1.4.4.8`

| CPU                     | Required Clock Cycles | Executed Instructions | Average CPI |
|:------------------------|----------------------:|----------------------:|:-----------:|
| `rv32i`                 |         5 945 938 586 |         1 469 587 406 |    **4.05** |
| `rv32im`                |         3 110 282 586 |           602 225 760 |    **5.16** |
| `rv32imc`               |         3 172 969 968 |           615 388 924 |    **5.16** |
| `rv32imc` `FAST_MUL_EN` |         2 590 417 968 |           615 388 890 |    **4.21** |

The `FAST_MUL_EN` configuration uses DSPs for the multiplier of the `M` extension (enabled via the `FAST_MUL_EN` generic).

When the `C` extension is enabled, branches to an unaligned uncompressed instruction require additional instruction fetch cycles.



## Top Entities

The top entity of the **NEORV32 Processor** is [**neorv32_top.vhd**](https://github.com/stnolting/neorv32/blob/master/rtl/core/neorv32_top.vhd) (from `rtl/core`).
Just instantiate this file in your project and you are ready to go! All signals of this top entity are of type *std_ulogic* or *std_ulogic_vector*, respectively
(except for the TWI signals, which are of type *std_logic*).

The top entity of the **NEORV32 CPU** is [**neorv32_cpu.vhd**](https://github.com/stnolting/neorv32/blob/master/rtl/core/neorv32_cpu.vhd) (from `rtl/core`).
All signals of this top entity are of type *std_ulogic* or *std_ulogic_vector*, respectively.

Use the generics to configure the processor/CPU according to your needs. Each generic is initilized with the default configuration.
Detailed information regarding the signals and configuration generics can be found in
the [NEORV32 documentary](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/NEORV32.pdf).

Alternative top entities, like the simplified ["hello world" test setup](#Create-a-new-Hardware-Project) or CPU/Processor
wrappers with resolved port signal types (i.e. *std_logic*), can be found in [`rtl/top_templates`](https://github.com/stnolting/neorv32/blob/master/rtl/top_templates).


### NEORV32 CPU

```vhdl
entity neorv32_cpu is
  generic (
    -- General --
    HW_THREAD_ID                 : std_ulogic_vector(31 downto 0):= (others => '0'); -- hardware thread id
    CPU_BOOT_ADDR                : std_ulogic_vector(31 downto 0):= (others => '0'); -- cpu boot address
    -- RISC-V CPU Extensions --
    CPU_EXTENSION_RISCV_C        : boolean := false; -- implement compressed extension?
    CPU_EXTENSION_RISCV_E        : boolean := false; -- implement embedded RF extension?
    CPU_EXTENSION_RISCV_M        : boolean := false; -- implement muld/div extension?
    CPU_EXTENSION_RISCV_U        : boolean := false; -- implement user mode extension?
    CPU_EXTENSION_RISCV_Zicsr    : boolean := true;  -- implement CSR system?
    CPU_EXTENSION_RISCV_Zifencei : boolean := true;  -- implement instruction stream sync.?
    -- Extension Options --
    FAST_MUL_EN                  : boolean := false; -- use DSPs for M extension's multiplier
    -- Physical Memory Protection (PMP) --
    PMP_USE                      : boolean := false; -- implement PMP?
    PMP_NUM_REGIONS              : natural := 4;     -- number of regions (max 8)
    PMP_GRANULARITY              : natural := 14     -- minimal region granularity (1=8B, 2=16B, 3=32B, ...) default is 64k
  );
  port (
    -- global control --
    clk_i          : in  std_ulogic := '0'; -- global clock, rising edge
    rstn_i         : in  std_ulogic := '0'; -- global reset, low-active, async
    -- instruction bus interface --
    i_bus_addr_o   : out std_ulogic_vector(data_width_c-1 downto 0); -- bus access address
    i_bus_rdata_i  : in  std_ulogic_vector(data_width_c-1 downto 0) := (others => '0'); -- bus read data
    i_bus_wdata_o  : out std_ulogic_vector(data_width_c-1 downto 0); -- bus write data
    i_bus_ben_o    : out std_ulogic_vector(03 downto 0); -- byte enable
    i_bus_we_o     : out std_ulogic; -- write enable
    i_bus_re_o     : out std_ulogic; -- read enable
    i_bus_cancel_o : out std_ulogic; -- cancel current bus transaction
    i_bus_ack_i    : in  std_ulogic := '0'; -- bus transfer acknowledge
    i_bus_err_i    : in  std_ulogic := '0'; -- bus transfer error
    i_bus_fence_o  : out std_ulogic; -- executed FENCEI operation
    -- data bus interface --
    d_bus_addr_o   : out std_ulogic_vector(data_width_c-1 downto 0); -- bus access address
    d_bus_rdata_i  : in  std_ulogic_vector(data_width_c-1 downto 0) := (others => '0'); -- bus read data
    d_bus_wdata_o  : out std_ulogic_vector(data_width_c-1 downto 0); -- bus write data
    d_bus_ben_o    : out std_ulogic_vector(03 downto 0); -- byte enable
    d_bus_we_o     : out std_ulogic; -- write enable
    d_bus_re_o     : out std_ulogic; -- read enable
    d_bus_cancel_o : out std_ulogic; -- cancel current bus transaction
    d_bus_ack_i    : in  std_ulogic := '0'; -- bus transfer acknowledge
    d_bus_err_i    : in  std_ulogic := '0'; -- bus transfer error
    d_bus_fence_o  : out std_ulogic; -- executed FENCE operation
    -- system time input from MTIME --
    time_i         : in  std_ulogic_vector(63 downto 0) := (others => '0'); -- current system time
    -- interrupts (risc-v compliant) --
    msw_irq_i      : in  std_ulogic := '0'; -- machine software interrupt
    mext_irq_i     : in  std_ulogic := '0'; -- machine external interrupt
    mtime_irq_i    : in  std_ulogic := '0'; -- machine timer interrupt
    -- fast interrupts (custom) --
    firq_i         : in  std_ulogic_vector(3 downto 0) := (others => '0')
  );
end neorv32_cpu;
```


### NEORV32 Processor

```vhdl
entity neorv32_top is
  generic (
    -- General --
    CLOCK_FREQUENCY              : natural := 0;      -- clock frequency of clk_i in Hz
    BOOTLOADER_USE               : boolean := true;   -- implement processor-internal bootloader?
    USER_CODE                    : std_ulogic_vector(31 downto 0) := x"00000000"; -- custom user code
    -- RISC-V CPU Extensions --
    CPU_EXTENSION_RISCV_C        : boolean := false;  -- implement compressed extension?
    CPU_EXTENSION_RISCV_E        : boolean := false;  -- implement embedded RF extension?
    CPU_EXTENSION_RISCV_M        : boolean := false;  -- implement muld/div extension?
    CPU_EXTENSION_RISCV_U        : boolean := false;  -- implement user mode extension?
    CPU_EXTENSION_RISCV_Zicsr    : boolean := true;   -- implement CSR system?
    CPU_EXTENSION_RISCV_Zifencei : boolean := true;   -- implement instruction stream sync.?
    -- Extension Options --
    FAST_MUL_EN                  : boolean := false; -- use DSPs for M extension's multiplier
    -- Physical Memory Protection (PMP) --
    PMP_USE                      : boolean := false; -- implement PMP?
    PMP_NUM_REGIONS              : natural := 4;     -- number of regions (max 8)
    PMP_GRANULARITY              : natural := 14;    -- minimal region granularity (1=8B, 2=16B, 3=32B, ...) default is 64kB
    -- Internal Instruction memory --
    MEM_INT_IMEM_USE             : boolean := true;   -- implement processor-internal instruction memory
    MEM_INT_IMEM_SIZE            : natural := 16*1024; -- size of processor-internal instruction memory in bytes
    MEM_INT_IMEM_ROM             : boolean := false;  -- implement processor-internal instruction memory as ROM
    -- Internal Data memory --
    MEM_INT_DMEM_USE             : boolean := true;   -- implement processor-internal data memory
    MEM_INT_DMEM_SIZE            : natural := 8*1024; -- size of processor-internal data memory in bytes
    -- External memory interface --
    MEM_EXT_USE                  : boolean := false;  -- implement external memory bus interface?
    MEM_EXT_REG_STAGES           : natural := 2;      -- number of interface register stages (0,1,2)
    -- Processor peripherals --
    IO_GPIO_USE                  : boolean := true;   -- implement general purpose input/output port unit (GPIO)?
    IO_MTIME_USE                 : boolean := true;   -- implement machine system timer (MTIME)?
    IO_UART_USE                  : boolean := true;   -- implement universal asynchronous receiver/transmitter (UART)?
    IO_SPI_USE                   : boolean := true;   -- implement serial peripheral interface (SPI)?
    IO_TWI_USE                   : boolean := true;   -- implement two-wire interface (TWI)?
    IO_PWM_USE                   : boolean := true;   -- implement pulse-width modulation unit (PWM)?
    IO_WDT_USE                   : boolean := true;   -- implement watch dog timer (WDT)?
    IO_TRNG_USE                  : boolean := false;  -- implement true random number generator (TRNG)?
    IO_CFU_USE                   : boolean := false   -- implement custom functions unit (CFU)?
  );
  port (
    -- Global control --
    clk_i      : in  std_ulogic := '0'; -- global clock, rising edge
    rstn_i     : in  std_ulogic := '0'; -- global reset, low-active, async
    -- Wishbone bus interface (available if MEM_EXT_USE = true) --
    wb_adr_o   : out std_ulogic_vector(31 downto 0); -- address
    wb_dat_i   : in  std_ulogic_vector(31 downto 0) := (others => '0'); -- read data
    wb_dat_o   : out std_ulogic_vector(31 downto 0); -- write data
    wb_we_o    : out std_ulogic; -- read/write
    wb_sel_o   : out std_ulogic_vector(03 downto 0); -- byte enable
    wb_stb_o   : out std_ulogic; -- strobe
    wb_cyc_o   : out std_ulogic; -- valid cycle
    wb_ack_i   : in  std_ulogic := '0'; -- transfer acknowledge
    wb_err_i   : in  std_ulogic := '0'; -- transfer error
    -- Advanced memory control signals (available if MEM_EXT_USE = true) --
    fence_o    : out std_ulogic; -- indicates an executed FENCE operation
    fencei_o   : out std_ulogic; -- indicates an executed FENCEI operation
    -- GPIO (available if IO_GPIO_USE = true) --
    gpio_o     : out std_ulogic_vector(31 downto 0); -- parallel output
    gpio_i     : in  std_ulogic_vector(31 downto 0) := (others => '0'); -- parallel input
    -- UART (available if IO_UART_USE = true) --
    uart_txd_o : out std_ulogic; -- UART send data
    uart_rxd_i : in  std_ulogic := '0'; -- UART receive data
    -- SPI (available if IO_SPI_USE = true) --
    spi_sck_o  : out std_ulogic; -- SPI serial clock
    spi_sdo_o  : out std_ulogic; -- controller data out, peripheral data in
    spi_sdi_i  : in  std_ulogic := '0'; -- controller data in, peripheral data out
    spi_csn_o  : out std_ulogic_vector(07 downto 0); -- SPI CS
    -- TWI (available if IO_TWI_USE = true) --
    twi_sda_io : inout std_logic := 'H'; -- twi serial data line
    twi_scl_io : inout std_logic := 'H'; -- twi serial clock line
    -- PWM (available if IO_PWM_USE = true) --
    pwm_o      : out std_ulogic_vector(03 downto 0); -- pwm channels
    -- Interrupts --
    msw_irq_i  : in  std_ulogic := '0'; -- machine software interrupt
    mext_irq_i : in  std_ulogic := '0'  -- machine external interrupt
  );
end neorv32_top;
```



## Getting Started

This overview is just a short excerpt from the *Let's Get It Started* section of the NEORV32 documentary:

[![NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/figures/PDF_32.png) NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/NEORV32.pdf)


### Toolchain

At first you need the **RISC-V GCC toolchain**. You can either [download the sources](https://github.com/riscv/riscv-gnu-toolchain)
and build the toolchain by yourself, or you can download a prebuilt one and install it.

:warning: Keep in mind that – for instance – a `rv32imc` toolchain only provides library code compiled with compressed and
`mul`/`div` instructions! Hence, this code cannot be executed (without emulation) on an architecture without these extensions!

To build the toolchain by yourself, follow the official [build instructions](https://github.com/riscv/riscv-gnu-toolchain).
Make sure to use the `ilp32` or `ilp32e` ABI.

**Alternatively**, you can download a prebuilt toolchain. I have uploaded the toolchains I am using to GitHub. These toolchains
were compiled on a 64-bit x86 Ubuntu 20.04 LTS (Ubuntu on Windows, actually). Download the toolchain of choice:

[https://github.com/stnolting/riscv_gcc_prebuilt](https://github.com/stnolting/riscv_gcc_prebuilt)


### Dowload the NEORV32 Project

Get the sources of the NEORV32 Processor project. The simplest way is using `git clone` (suggested for easy project updates via `git pull`):

    $ git clone https://github.com/stnolting/neorv32.git

Alternatively, you can either download a specific [release](https://github.com/stnolting/neorv32/releases) or get the most recent version
of this project as [`*.zip` file](https://github.com/stnolting/neorv32/archive/master.zip).


### Create a new Hardware Project

Create a new project with your FPGA design tool of choice. Add all the `*.vhd` files from the [`rtl/core`](https://github.com/stnolting/neorv32/blob/master/rtl)
folder to this project. Make sure to add these files to a **new design library** called `neorv32`.

You can either instantiate the [processor's top entity](https://github.com/stnolting/neorv32#top-entity) in your own project or you
can use a simple [test setup](https://github.com/stnolting/neorv32/blob/master/rtl/top_templates/neorv32_test_setup.vhd) (from the project's
[`rtl/top_templates`](https://github.com/stnolting/neorv32/blob/master/rtl/top_templates) folder) as top entity.

This test setup instantiates the processor and implements most of the peripherals and some ISA extensions. Only the UART lines, clock, reset and some GPIO output sginals are
propagated as actual entity signals. Basically, it is a FPGA "hello world" example:

```vhdl
  entity neorv32_test_setup is
    port (
      -- Global control --
      clk_i      : in  std_ulogic := '0'; -- global clock, rising edge
      rstn_i     : in  std_ulogic := '0'; -- global reset, low-active, async
      -- GPIO --
      gpio_o     : out std_ulogic_vector(7 downto 0); -- parallel output
      -- UART --
      uart_txd_o : out std_ulogic; -- UART send data
      uart_rxd_i : in  std_ulogic := '0' -- UART receive data
    );
  end neorv32_test_setup;
```


### Check the Toolchain

Make sure `GNU Make` and a native `GCC` compiler are installed. To test the installation of the RISC-V toolchain navigate to an example project like
`sw/example/blink_led` and run:

    neorv32/sw/example/blink_led$ make check


### Compiling an Example Program

The NEORV32 project includes some [example programs](https://github.com/stnolting/neorv32/tree/master/sw/example) from
which you can start your own application. Simply compile one of these projects. This will create a NEORV32
*executable* `neorv32_exe.bin` in the same folder:

    neorv32/sw/example/blink_led$ make clean_all exe


### Upload the Executable via the Bootloader

Connect your FPGA board via UART to your computer and open the according port to interface with the NEORV32 bootloader. The bootloader
uses the following default UART configuration:

* 19200 Baud
* 8 data bits
* 1 stop bit
* No parity bits
* No transmission / flow control protocol (raw bytes only)
* Newline on `\r\n` (carriage return & newline) - also for sent data

Use the bootloader console to upload the `neorv32_exe.bin` executable and run your application image.

```
  << NEORV32 Bootloader >>
  
  BLDV: Jul  6 2020
  HWV:  1.0.1.0
  CLK:  0x0134FD90 Hz
  USER: 0x0001CE40
  MISA: 0x42801104
  PROC: 0x03FF0035
  IMEM: 0x00010000 bytes @ 0x00000000
  DMEM: 0x00010000 bytes @ 0x80000000
  
  Autoboot in 8s. Press key to abort.
  Aborted.
  
  Available CMDs:
   h: Help
   r: Restart
   u: Upload
   s: Store to flash
   l: Load from flash
   e: Execute
  CMD:> u
  Awaiting neorv32_exe.bin... OK
  CMD:> e
  Booting...
  
  Blinking LED demo program
```

Going further: Take a look at the _Let's Get It Started!_ chapter of the [![NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/figures/PDF_32.png) NEORV32 data sheet](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/NEORV32.pdf).



## Contribute

I'm always thankful for help! So if you have any questions, bug reports, ideas or if you want to give some kind of feedback, feel free
to [open a new issue](https://github.com/stnolting/neorv32/issues) or directly [drop me a line](mailto:stnolting@gmail.com).

If you'd like to contribute:

1. [Fork](https://github.com/stnolting/neorv32/fork) this repository and clone the fork
2. Create a feature branch in your fork: `git checkout -b awesome_new_feature_branch`
3. Create a new remote for the upstream repo: `git remote add https://github.com/stnolting/neorv32`
3. Commit your modifications: `git commit -m "Awesome new feature!"`
4. Push to the branch: `git push origin awesome_new_feature_branch`
5. Create a new [pull request](https://github.com/stnolting/neorv32/pulls)

Please also check out the project's [code of conduct](https://github.com/stnolting/neorv32/tree/master/CODE_OF_CONDUCT.md).



## Legal

This project is released under the BSD 3-Clause license. No copyright infringement intended.
Other implied or used projects might have different licensing - see their documentation to get more information.

#### Citation

If you are using the NEORV32 Processor/CPU in some kind of publication, please cite it as follows:

> S. Nolting, "The NEORV32 Processor/CPU", github.com/stnolting/neorv32

#### BSD 3-Clause License

Copyright (c) 2020, Stephan Nolting. All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are
permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this list of
conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice, this list of
conditions and the following disclaimer in the documentation and/or other materials
provided with the distribution.
3. Neither the name of the copyright holder nor the names of its contributors may be used to
endorse or promote products derived from this software without specific prior written
permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS
OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE
GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED
OF THE POSSIBILITY OF SUCH DAMAGE.


#### Limitation of Liability for External Links

Our website contains links to the websites of third parties („external links“). As the
content of these websites is not under our control, we cannot assume any liability for
such external content. In all cases, the provider of information of the linked websites
is liable for the content and accuracy of the information provided. At the point in time
when the links were placed, no infringements of the law were recognisable to us. As soon
as an infringement of the law becomes known to us, we will immediately remove the
link in question.


#### Proprietary  Notice

"Artix" and "Vivado" are trademarks of Xilinx Inc.

"Cyclone", "Quartus Prime", "Quartus Prime Lite" and "Avalon Bus" are trademarks of Intel Corporation.

"Artix" and "Vivado" are trademarks of Xilinx, Inc.

"iCE40", "UltraPlus" and "Lattice Radiant" are trademarks of Lattice Semiconductor Corporation.

"AXI" and "AXI-Lite" are trademarks of Arm Holdings plc.



## Acknowledgements

[![RISC-V](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/figures/riscv_logo.png)](https://riscv.org/)

[RISC-V](https://riscv.org/) - Instruction Sets Want To Be Free!

[![Continous Integration provided by Travis CI](https://travis-ci.com/images/logos/TravisCI-Full-Color.png)](https://travis-ci.com/stnolting/neorv32)

Continous integration provided by [Travis CI](https://travis-ci.com/stnolting/neorv32) and powered by [GHDL](https://github.com/ghdl/ghdl).


![Open Source Hardware Logo https://www.oshwa.org](https://raw.githubusercontent.com/stnolting/neorv32/master/docs/figures/oshw_logo.png)

This project is not affiliated with or endorsed by the Open Source Initiative (https://www.oshwa.org / https://opensource.org).

--------

This repository was created on June 23th, 2020.

Made with :coffee: in Hannover, Germany.
