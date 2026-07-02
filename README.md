# Pattern-Corr: Design and Testing of FPGA Replay Attack Detection & Mitigation

A self-protected FPGA/SoC security module that detects and mitigates **replay** and **corruption attacks** in real time, without relying on post-deployment scanning or manual intervention.

![Status](https://img.shields.io/badge/status-completed-brightgreen)
![Platform](https://img.shields.io/badge/platform-FPGA%20%7C%20CPLD-blue)
![Language](https://img.shields.io/badge/HDL-VHDL-orange)
![License](https://img.shields.io/badge/license-Academic--Project-lightgrey)

---

## 📖 Overview

Modern devices — smartphones, IoT systems, autonomous vehicles — depend on **System-on-Chip (SoC)** designs built on **FPGAs**. These systems are increasingly targeted by hardware-level attacks such as **replay attacks** and **data corruption**, which exploit design vulnerabilities to manipulate data or disrupt operation.

Most existing FPGA security solutions are **reactive** — they rely on post-deployment scanning and manual mitigation, leaving systems exposed during the detection window.

**Pattern-Corr** solves this by embedding a **self-protected module directly into the FPGA fabric**, combining:

- Dynamic **pattern-matching logic**
- A **Finite State Machine (FSM)**-based mitigation mechanism

This enables the system to detect abnormal/attack patterns in real time and automatically trigger corrective action (masking corrupted outputs, reconfiguring internal states) — with **no external intervention required**.

---

## ✨ Key Features

- 🔍 **Real-time attack detection** — compares incoming signals against predefined safe states
- 🛡️ **Autonomous mitigation** — FSM dynamically masks corrupted outputs and reconfigures internal states
- ⚡ **Low-latency response** — mitigation triggered within nanoseconds of detection
- 🧩 **Modular VHDL design** — four independent modules (attack generation, test circuit, detection, mitigation)
- 🧪 **Dual validation** — verified via ModelSim simulation *and* real hardware (oscilloscope) testing
- 📊 **Benchmarked against VALIANT**, a published EDA-based side-channel leakage evaluation framework

---

## 🏗️ System Architecture

The system is built around **four VHDL modules**:

| Module | Responsibility |
|---|---|
| **Attack Generation** | Generates attack vectors (replay, corruption, unauthorized probing, reverse engineering) based on a `config` input |
| **Test Circuit (`BENCHMARK_CKT1`)** | Simulates a functional circuit under test, producing output `ya` under normal and attacked conditions |
| **Attack Detection (`Logic_Attack_Detector`)** | Monitors circuit output against expected behavior and raises detection flags (`sk`) |
| **Attack Notification & Mitigation** | Encodes attack status into a readable flag (`VCheck`: `Normal` / `CorrAttack`) and triggers the `Mitigate` process to neutralize the threat |

**Flow:** `Initialization → Attack Generation → Test Circuit Simulation → Attack Detection → Attack Notification → Mitigation`

---

## 🛠️ Tools & Hardware Used

| Tool / Hardware | Purpose |
|---|---|
| **Xilinx XC9572XL FPGA (CPLD) Kit** | Hardware deployment and real-world validation — 72 macrocells, 34 I/O, up to 250 MHz, 3.3V core |
| **ModelSim** | HDL simulation, waveform analysis, and functional verification of VHDL modules |
| **Quartus** | Synthesis and design implementation |
| **Oscilloscope** | Real-time monitoring of output signals to visually confirm attack detection and mitigation on hardware |
| **VHDL** | Hardware description language used for all design modules |

---

## 🧪 Methodology

1. **Initialization** — System resets with control signals (`clk`, `clr`) and configuration vector (`config`)
2. **Attack Generation** — Based on `config`, the Attack Generation module produces a 7-bit attack vector (`Attack_Insert`) representing corruption, replay, probing, or reverse-engineering scenarios
3. **Test Circuit Simulation** — `BENCHMARK_CKT1` processes normal inputs (`a, b, c, d`) plus injected attack signals to produce output `ya`
4. **Attack Detection** — `Logic_Attack_Detector` compares `ya` against expected safe-state behavior and raises detection flags (`sk1`–`sk6`)
5. **Attack Notification** — Detection flags are encoded into a human-readable status string `VCheck` (`"Normal****"` or `"CorrAttack"`)
6. **Mitigation** — On detecting an attack, the `Mitigate` process activates a mitigation signal (`mit`) to neutralize the threat and restore safe operation

---

## 📊 Results

### Attack Detection & Mitigation Performance

| Attack Type | Detection Accuracy | Mitigation Response Time | Resource Utilization |
|---|---|---|---|
| Corruption Attack | **98.5%** | 12 ns | 72% |
| Replay Attack | **97.8%** | 14 ns | 74% |
| Unauthorized Probing | **96.4%** | 16 ns | 70% |
| Reverse Engineering | **96.7%** | 15 ns | 75% |

### Comparison with the VALIANT Framework

| Metric | Proposed System | VALIANT |
|---|---|---|
| Detection Accuracy | **97.35%** | 94.93% |
| Response Time | **14.25 ns** | 19.5 ns |
| Resource Utilization | **68% (avg)** | 75% (avg) |
| Scalability | **High** | Medium |

**Validation methods:**

<img width="631" height="314" alt="image" src="https://github.com/user-attachments/assets/7fcc2ed0-be6b-4492-b7d0-59492fa6c9bd" />

 ✅ ModelSim waveform simulation confirmed correct transitions from attack state → detection → mitigation → stable normal state

<img width="612" height="396" alt="image" src="https://github.com/user-attachments/assets/fb72316c-8dc9-4a97-bf2c-4879679e8469" />

 ✅ Hardware deployment on the XC9572XL FPGA Kit, with oscilloscope capture showing a clean baseline signal vs. a clearly glitched signal under injected attack, confirming real-world detection and mitigation

---

## 🎯 Applications

This self-protected FPGA module is designed for security-critical domains where data integrity and uptime cannot be compromised:

- 💳 **Financial systems** — protecting cryptographic keys and transaction data from side-channel/corruption attacks
- 🛰️ **Military & defense systems** — securing communication, radar, and control systems from corruption/replay attacks
- 🏥 **Healthcare & medical devices** — safeguarding data integrity in implantable and networked medical devices
- 🚗 **Automotive & IoT** — protecting embedded controllers in resource-constrained environments

---

## 🔮 Future Scope

- Evolving the module into a **fully self-protected SoC** (beyond FPGA-only)
- Multi-layered defense combining real-time detection, dynamic mitigation, and cryptographic protection
- Expanding coverage to **side-channel attacks, probing, and reverse engineering**
- Improved scalability for larger, more complex SoC designs
- Continued optimization for resource-constrained (IoT-class) deployments

---

## 📚 References

Key literature referenced in this project includes the VALIANT EDA framework for side-channel leakage evaluation, along with peer-reviewed studies on FPGA-based attack detection, replay-attack countermeasures, and hardware intrusion detection systems. Full citations are available in the project report.

---

## 👥 Authors

Mini Project — B.E. Electronics and Communication Engineering  
**Saveetha Engineering College, Chennai** (December 2024)

- **Swathi T** (212223060282)
.

**Guide:** Dr. S. Imran Hussain, M.Tech., Ph.D., Assistant Professor, Dept. of ECE

---

## 📄 License

This repository is provided for academic and educational purposes as part of a Bachelor of Engineering mini project submission.
