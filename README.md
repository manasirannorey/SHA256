# SHA256
RTL design and functional verification of the SHA-256 cryptographic hash algorithm using Verilog/SystemVerilog.

# SHA-256 Cryptographic Hash Engine – RTL Design and Verification

A SystemVerilog-based hardware implementation of the SHA-256 cryptographic hashing algorithm, featuring an iterative processing architecture and a dedicated verification environment with standard test vectors.

## Project Overview

SHA-256 (Secure Hash Algorithm 256-bit) is a cryptographic hash function belonging to the SHA-2 family. It operates on 512-bit message blocks and generates a fixed-length 256-bit hash value.

This project focuses on implementing the SHA-256 algorithm at the RTL level using SystemVerilog. The design follows an iterative architecture in which the same computational datapath is reused to execute all 64 rounds of the hashing process. This approach reduces hardware duplication while maintaining a structured control and processing sequence.

## Key Features

- RTL implementation of the SHA-256 hashing algorithm using SystemVerilog.
- Processing of 512-bit padded message blocks with a 256-bit digest output.
- Iterative architecture with eight 32-bit working registers.
- Dedicated modules for round constants and message-schedule generation.
- Implementation of SHA-256 logical functions, rotations, and modular arithmetic.
- Finite State Machine (FSM) for controlling initialization and round execution.
- Verification using directed test cases and standard SHA-256 known-answer vectors.
- Testbench environment incorporating stimulus generation, monitoring, scoreboard checking, assertions, and functional coverage.
- Logic synthesis and design analysis using Cadence Genus.

## Repository Organization

```text
sha_256/
│
├── src/
│   ├── sha256_core.sv
│   ├── sha256_k_constants.sv
│   └── sha256_w_mem.sv
│
└── tb/
    └── sha256_tb.sv
```

## RTL Design Architecture

### 1. SHA-256 Core (`sha256_core.sv`)

The core module serves as the primary processing unit and manages the complete 64-round hashing operation for an input message block.

It consists of eight 32-bit working registers (`a` through `h`), which are updated during each round based on the SHA-256 compression function.

The datapath incorporates:

- Choose (Ch) and Majority (Maj) logical functions.
- Uppercase sigma functions (Σ0 and Σ1) implemented using bitwise rotations.
- Intermediate calculations involving T1 and T2.
- Working-register updates across successive rounds.
- Final accumulation of the computed state into the hash registers.

The control logic coordinates the processing sequence through initialization, round execution, and digest generation.

### 2. Round Constant Generator (`sha256_k_constants.sv`)

This module provides the predefined set of 64 SHA-256 round constants. A six-bit round counter selects the corresponding 32-bit constant required during each compression round.

### 3. Message Schedule Generator (`sha256_w_mem.sv`)

The message-schedule module generates the 64-word sequence required by the SHA-256 compression algorithm.

It initially loads sixteen 32-bit words from the input message block. The remaining schedule words are generated using the lower-case sigma functions (σ0 and σ1), together with modular additions.

A 16-word sliding-window arrangement is used to manage the schedule data throughout the processing sequence.

## Verification Environment

### Testbench (`sha256_tb.sv`)

A SystemVerilog-based testbench was developed to verify the functional behavior of the SHA-256 core.

The verification environment includes:

- Transaction generation for supplying input message blocks.
- Driver logic for applying stimulus to the DUT.
- Monitor functionality for observing design outputs.
- Scoreboard-based comparison of calculated and expected digests.
- Assertions to check important control-signal conditions.
- Functional coverage to track verification scenarios.

The testbench also checks that the `init` and `next` control signals are not asserted simultaneously.

### Functional Test Vectors

The implementation is evaluated using three standard known-answer test cases:

| Test Input | Expected SHA-256 Digest |
|---|---|
| `abc` | `ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad` |
| Empty message | `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` |
| `test` | `9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08` |

The testbench supplies the corresponding padded 512-bit message blocks directly to the core and compares the resulting digest against the expected reference values.

**Note:** These test cases exercise single-block processing. Message padding and automatic handling of multi-block messages are outside the scope of the current top-level implementation.

## Module Interface

| Signal | Direction | Width | Function |
|---|---|---:|---|
| `clk` | Input | 1 bit | Clock signal |
| `reset_n` | Input | 1 bit | Active-low reset |
| `init` | Input | 1 bit | Initializes the hashing operation |
| `next` | Input | 1 bit | Controls subsequent block processing |
| `mode` | Input | 1 bit | Selects SHA-256 or SHA-224 initial state |
| `block` | Input | 512 bits | Padded message block |
| `ready` | Output | 1 bit | Indicates core readiness |
| `digest` | Output | 256 bits | Hash-state output |
| `digest_valid` | Output | 1 bit | Indicates digest validity |

## Simulation and Verification

The RTL and verification environment are written in SystemVerilog. The testbench uses advanced verification constructs, including classes, mailboxes, virtual interfaces, clocking blocks, assertions, and covergroups.

The RTL source files and testbench can be compiled using a SystemVerilog-compatible simulator. The testbench should be selected as the simulation top module.

Source compilation order:

```text
src/sha256_k_constants.sv
src/sha256_w_mem.sv
src/sha256_core.sv
tb/sha256_tb.sv
```

Simulation results should be evaluated using scoreboard comparisons, assertion outcomes, and the final testbench summary.

## Synthesis and Design Analysis

Logic synthesis of the SHA-256 RTL design was carried out using **Cadence Genus**.

The design was examined at different implementation stages to understand its functional and hardware characteristics.

The analysis includes:

- **Pre-synthesis waveforms:** Examination of RTL-level signal transitions and functional behavior.
- **Post-synthesis waveforms:** Observation of the synthesized design.
- **Area analysis:** Evaluation of the hardware area reported by the synthesis tool.
- **Timing analysis:** Assessment of the design's timing characteristics.
- **Power analysis:** Evaluation of estimated power consumption.

These results help assess the implementation from functional and physical-design perspectives.

## Tools and Technologies

| Category | Tools / Technologies |
|---|---|
| Hardware Description Language | SystemVerilog |
| RTL Design | SystemVerilog |
| Functional Verification | SystemVerilog Testbench |
| Simulation | EDA Playground |
| Logic Synthesis | Cadence Genus |
| Design Analysis | Waveform, Area, Timing and Power Reports |

## Project Scope

The current implementation focuses on the SHA-256 compression engine and its RTL verification. It accepts a pre-padded 512-bit message block and produces a 256-bit digest.

Automatic padding, complete message preprocessing, and top-level multi-block message handling are not included.

## Learning Outcomes

- Understanding the architecture and computational stages of the SHA-256 algorithm.
- Translating a cryptographic algorithm into synthesizable RTL.
- Designing iterative datapaths and control logic using SystemVerilog.
- Developing a structured testbench for functional verification.

## Author

**Manasi Kishore**  
B.Tech – Electronics and Communication Engineering  
PES University, Bengaluru, India

**Project:** SHA-256 Cryptographic Hash Engine – RTL Design and Verification

**Technologies:** SystemVerilog, Cadence Genus, EDA Playground

**GitHub:** [Your GitHub Profile URL]

---

*This project was developed as part of academic and technical learning in digital design, RTL development, and functional verification.*
