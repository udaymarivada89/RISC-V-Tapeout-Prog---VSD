# RISC-V-Tapeout-Prog---VSD

Welcome to my repository, where I am gonna document my weekly progress in the tasks and my learnings of the RISC-V SoC tapeout program by VSD.


### VLSI SoC Design Flow

This document provides a high-level overview of the typical workflow for designing a Very-Large-Scale Integration (VLSI) System on a Chip (SoC). It outlines the key stages from initial software development and architectural modeling to final chip fabrication and validation.

---

#### 1. Application and Processor Modeling

The process begins with a C language application.

* **Software Compilation (O0):** The application is first compiled with GCC, and its output is measured as **O0**.
* **Processor Specification Modeling (O1):** Before writing the hardware description language (RTL), the processor's specifications are modeled in a C environment. This allows for early-stage testing and verification of the architectural design. The output of this stage is **O1**. The goal is to ensure that **O0 = O1**, confirming the correctness of the processor's functional specifications.

---

#### 2. RTL Design and Verification

Once the specifications are finalized, the hardware is described using RTL languages like Verilog, Bluespec, or Chisel.

* **Hardware Simulation (O2):** The C application is run on this RTL model of the hardware. The resulting output is **O2**. The verification step is to ensure that **O2 = O1**, which validates that the hardware implementation correctly executes the application as per the frozen specifications.

---

#### 3. SoC Design and Integration

The hardware design is broken down into two main blocks: the **processor** and **peripherals/IPs** (Intellectual Property).

* **ASIC Design Flow:**
    * **Processor:** The processor's Verilog code is synthesizable, meaning it can be converted into a gate-level netlist. This is referred to as "Synth P1."
    * **Peripherals/IPs:** These can be of two types:
        * **Macros (Synthesizable RTL):** These blocks are instantiated multiple times and must be synthesizable into gates.
        * **Analog IPs (Functional RTL):** These blocks (e.g., ADCs, PLLs) are designed using transistors (NMOS/PMOS) and are not synthesizable from RTL. They are replaced by pre-designed functional blocks later in the flow.
* **SoC Integration (O3):** All the individual blocks—the synthesized processor, macros, and analog IPs—are integrated together to form the complete SoC. The output of running the C application on this integrated design is **O3**. A crucial verification step is to confirm that **O1 = O2 = O3**.

**Microprocessor vs. Microcontroller:** A **microprocessor** is the standalone processing unit. When it is combined with peripherals and other IPs on a single chip, it becomes a **microcontroller**.

---

#### 4. Physical Design and Fabrication

This stage converts the logical design into a physical layout for manufacturing.

* **RTL to GDSII:** The front-end RTL design is converted into a physical layout file called **GDSII (Graphic Data System II)**. This file contains all the information about the metal layers and geometric shapes for fabrication.
* **DRC/LVS Checks:** Due to their massive size, GDSII files are not simulated with applications. Instead, they undergo **Design Rule Checking (DRC)** and **Layout Versus Schematic (LVS)** checks to ensure the layout adheres to manufacturing rules and matches the original circuit schematic.
* **Tapeout:** Once verified, the GDSII file is sent to a foundry for fabrication. This milestone is known as **tapeout**.
* **Tape-in:** When the fabricated chips are received back from the foundry, it is called **tape-in**.

---

#### 5. Final Validation

After receiving the chips, a final validation stage is performed.

* **Board and Firmware:** A Printed Circuit Board (PCB) is prepared, and firmware is written to support the new chip.
* **Final Application Test (O4):** The initial C language application is run on the physical chip. The resulting output is **O4**.
* **Success Criteria:** The entire design process is considered successful if **O1 = O2 = O3 = O4**, confirming that the final fabricated chip behaves exactly as the initial software model and subsequent simulations predicted.

**Timeline:** This entire process is lengthy, typically taking around **14 months**, with the foundry phase alone consuming **4-6 months**.
