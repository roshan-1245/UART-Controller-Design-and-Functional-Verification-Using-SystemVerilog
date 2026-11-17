# UART Controller Design and Functional Verification Using SystemVerilog

### Modular RTL Architecture | FIFO-Based Data Buffering | Xilinx Vivado

---

## 1. Repository Structure

The project repository is organized into separate directories to maintain a modular and systematic development workflow. The structure includes RTL source files, testbench, architecture schematics, simulation waveforms, Vivado documentation, results, and technical references.

```text
UART-Controller-SystemVerilog/
│
├── README.md
│
├── RTL/
│   ├── uart_tx_top.sv
│   ├── uart_rx_top.sv
│   ├── regs_uart.sv
│   ├── fifo_top.sv
│   └── all_mod.sv
│
├── Testbench/
│   └── all_mod_tb.sv
│
├── Documentation/
│   ├── Architecture/
│   │   ├── uart_top_level_schematic.png
│   │   └── uart_elaborated_schematic.png
│   │
│   ├── Simulation_Waveforms/
│   │   ├── simulation_overview.png
│   │   ├── register_operations.png
│   │   ├── tx_fifo_waveform.png
│   │   ├── rx_fifo_waveform.png
│   │   ├── baud_rate_waveform.png
│   │   └── internal_signals.png
│   │
│   └── Design_Notes/
│       ├── system_architecture.md
│       └── module_description.md
│
├── Vivado_Project/
│   ├── Project_Setup.md
│   ├── Simulation_Guide.md
│   └── Source_File_Configuration.md
│
├── Constraints/
│   └── README.md
│
├── Results/
│   ├── Simulation_Results.md
│   ├── Verification_Summary.md
│   └── Waveform_Analysis.md
│
├── References/
│   ├── REFERENCES.md
│   ├── Research_Papers/
│   ├── Datasheets/
│   └── Technical_Documentation/
│
├── LICENSE
├── CONTRIBUTING.md
└── .gitignore
```

### Directory Description

| Directory | Description |
|---|---|
| `RTL/` | SystemVerilog design source files |
| `Testbench/` | Functional simulation testbench |
| `Documentation/` | Architecture diagrams and simulation waveforms |
| `Vivado_Project/` | Vivado setup and simulation instructions |
| `Constraints/` | FPGA pin and timing constraints |
| `Results/` | Simulation observations and verification reports |
| `References/` | Research papers, datasheets, and technical documentation |

---

## 2. Project Overview

This project presents the design and functional verification of a modular UART (Universal Asynchronous Receiver Transmitter) controller using SystemVerilog and Xilinx Vivado.

UART is a widely used asynchronous serial communication protocol that enables data exchange between digital systems without requiring a shared clock signal between communicating devices.

The proposed design implements a modular UART architecture consisting of a UART Transmitter (TX), UART Receiver (RX), programmable Register Interface, and separate Transmit and Receive FIFO buffers.

The transmitter and receiver handle serial data communication, while the register interface manages configuration, control, and data-access operations. FIFO-based buffering is incorporated to temporarily store parallel data during transmission and reception, providing an organized data path between the register interface and UART communication blocks.

The complete architecture is integrated through a top-level SystemVerilog module. The design is analyzed using RTL schematic visualization and evaluated through behavioral simulation in Xilinx Vivado.

**The primary objective is to develop a structured UART communication architecture and gain practical experience in RTL design, SystemVerilog implementation, module integration, and functional verification.**

## 3. Project Objectives

The major objectives of this project are:

- Design and implementation of a modular UART communication controller using SystemVerilog.
- Development of independent UART transmitter and receiver modules.
- Implementation of a register-based interface for UART configuration and control.
- Integration of separate FIFO buffers for transmit and receive data paths.
- Development of baud-rate-related timing and control logic.
- Integration of individual RTL modules into a unified architecture.
- Analysis of module hierarchy and internal signal connectivity using Vivado schematic visualization.
- Functional evaluation of the integrated design through behavioral simulation.
- Monitoring and analysis of internal data, control, and status signals.

## 4. System Architecture

The proposed UART controller consists of five major functional instances integrated through a common top-level module.

The architecture is organized into the following blocks:

1. UART Transmitter (TX)
2. UART Receiver (RX)
3. UART Register Interface
4. Transmit FIFO (TX FIFO)
5. Receive FIFO (RX FIFO)

Each block performs a dedicated operation and communicates with other modules through internal data buses and control signals.

### 4.1 Top-Level Module Integration

The top-level module `all_mod.sv` integrates the transmitter, receiver, register interface, and FIFO instances.

It establishes the communication paths between the individual functional blocks and provides the external interface for serial communication and register access.

**External Input Signals:**

| Signal | Description |
|---|---|
| `clk` | System clock input |
| `rst` | Reset input |
| `rx` | UART serial receive input |
| `addr[2:0]` | Register address input |
| `din[7:0]` | Register write-data input |
| `rd` | Register read control |
| `wr` | Register write control |

**External Output Signals:**

| Signal | Description |
|---|---|
| `dout[7:0]` | Register read-data output |
| `tx` | UART serial transmit output |

### 4.2 UART Transmitter (TX)

The UART transmitter converts parallel data into a serial bit stream for asynchronous communication.

It receives data from the transmit FIFO and generates the corresponding UART transmission sequence on the TX output line.

**Key Functional Operations:**

- Acquires parallel data from the transmit FIFO.
- Generates the UART start bit.
- Performs parallel-to-serial data conversion.
- Transmits data bits sequentially.
- Handles parity and stop-bit-related control signals.
- Supports break and sticky-parity-related control inputs.
- Uses baud-rate timing pulses for transmission control.
- Generates FIFO read and transmitter control signals.

**RTL File:** `uart_tx_top.sv`

### 4.3 UART Receiver (RX)

The UART receiver captures incoming serial data and reconstructs the original parallel data.

It monitors the RX input line, detects incoming UART frames, and processes the received information using baud-rate-related timing control.

**Key Functional Operations:**

- Monitors the incoming serial RX signal.
- Detects incoming data activity.
- Performs serial-to-parallel data conversion.
- Generates received parallel data.
- Monitors parity and framing-related error conditions.
- Generates receive status and control signals.
- Controls receive FIFO data insertion.

**RTL File:** `uart_rx_top.sv`

### 4.4 UART Register Interface

The register interface acts as the central configuration and control block of the UART architecture.

It communicates with external logic through register address, input data, and read/write control signals.

**Key Functional Operations:**

- Register address decoding.
- Register read and write operations.
- UART configuration and control.
- Baud-rate divisor-related register access.
- Line-control configuration.
- Transmitter and receiver control.
- FIFO-related control and status management.

**RTL File:** `regs_uart.sv`

### 4.5 Transmit FIFO (TX FIFO)

The transmit FIFO provides temporary storage for parallel data before transmission.

Data is transferred from the register interface to the transmit FIFO. The transmitter retrieves the stored data using FIFO read-control signals.

**Key Functional Operations:**

- Stores parallel transmit data.
- Supports FIFO write and read operations.
- Generates FIFO status information.
- Provides data to the UART transmitter.
- Coordinates FIFO push and pop operations.

**RTL Instance:** `tx_fifo_inst`

### 4.6 Receive FIFO (RX FIFO)

The receive FIFO stores parallel data generated by the UART receiver after serial data reception.

It provides buffering between the receiver and register interface, allowing received data to be accessed through register read operations.

**Key Functional Operations:**

- Stores received parallel data.
- Supports FIFO write and read operations.
- Provides stored data to the register interface.
- Generates FIFO status information.
- Coordinates receive-side data movement.

**RTL Instance:** `rx_fifo_inst`

## 5. Data Flow and Module Interconnection

The UART architecture contains two independent communication paths: the transmit path and the receive path.

### 5.1 Transmit Data Path

Register Interface → UART Registers → TX FIFO → UART Transmitter → Serial TX Output

The register interface manages the data access operation, the TX FIFO temporarily stores the parallel data, and the transmitter converts the stored data into a serial bit stream.

### 5.2 Receive Data Path

Serial RX Input → UART Receiver → RX FIFO → UART Registers → External Register Read Interface

The receiver reconstructs the incoming serial data, the RX FIFO stores the received parallel data, and the register interface provides access to the stored information.

### 5.3 Baud-Rate and Timing Control

Baud-rate-related timing logic provides the control pulses required for UART communication.

Important internal signals observed during simulation include:

- `baud_cnt`
- `baud_pulse`
- `update_baud`

These signals are useful for analyzing counter progression, timing updates, and baud-rate-related control activity.

## 6. RTL Architecture and Schematic Visualization

The complete design hierarchy is analyzed using the schematic visualization facilities of Xilinx Vivado.

The RTL schematic illustrates the interconnections between the five major functional instances and their associated internal signals.

### 6.1 Top-Level RTL Schematic
<img width="1906" height="873" alt="Screenshot 2026-11-17 125056" src="https://github.com/user-attachments/assets/48671221-f74f-4d42-833f-4a9e4999fe46" />

<img width="1590" height="847" alt="Screenshot 2026-11-17 125806" src="https://github.com/user-attachments/assets/c6c8921f-481e-482b-a88e-cc00b9b49639" />

### 6.2 Elaborated Design Schematic

<img width="1587" height="826" alt="Screenshot 2026-11-17 125855" src="https://github.com/user-attachments/assets/25d4d187-3ae9-48e7-94c7-e78084f8642f" />


The schematic analysis provides visibility into:

- Top-level module hierarchy.
- UART transmitter and receiver instances.
- Register interface connectivity.
- Separate transmit and receive FIFO instances.
- Internal data buses and control signals.
- External input and output connections.
- Interconnections between different functional blocks.

## 7. RTL Design Methodology

The project follows a modular hardware design methodology to simplify development, integration, and debugging.

| Design Stage | Description |
|---|---|
| Architecture Design | Definition of UART functional blocks |
| RTL Development | Individual SystemVerilog module implementation |
| Module Integration | Interconnection of TX, RX, registers, and FIFO blocks |
| RTL Elaboration | Analysis of module hierarchy and connectivity |
| Schematic Analysis | Visualization of the integrated architecture |
| Testbench Development | Creation of simulation stimulus |
| Behavioral Simulation | Functional evaluation using Vivado Simulator |
| Waveform Analysis | Monitoring internal data and control signals |

## 8. Functional Verification and Simulation

The integrated UART architecture is evaluated using behavioral simulation in Xilinx Vivado.

A dedicated SystemVerilog testbench named `all_mod_tb.sv` provides input stimulus to the top-level design and allows monitoring of internal signals during simulation.

### 8.1 Clock and Reset Signals

- `clk`
- `rst`

These signals are monitored to examine initialization and sequential operation.

### 8.2 Register Interface Signals

- `addr[2:0]`
- `din[7:0]`
- `dout[7:0]`
- `wr`
- `rd`

These signals are used to analyze register addressing, write operations, and read-data behavior.

### 8.3 FIFO Control Signals

- `tx_fifo_push`
- `tx_fifo_pop`
- `rx_fifo_push`
- `rx_fifo_pop`

These signals are monitored to understand FIFO data transfer and control operations.

### 8.4 Baud-Rate Control Signals

- `baud_cnt`
- `baud_pulse`
- `update_baud`

These signals are observed to analyze baud-rate-related counter and timing behavior.

### 8.5 Internal Data Path Signals

Internal data buses, FIFO outputs, status signals, and communication control signals are monitored to understand the interaction between the different RTL modules.

## 9. Simulation Results and Waveform Analysis

The supplied Vivado behavioral simulation captures illustrate the activity of different functional blocks within the UART architecture.

The captured waveforms include:

- Clock and reset transitions.
- Register address and input data variations.
- Register write-control activity.
- Transmit FIFO push and pop operations.
- FIFO data output transitions.
- Baud-rate counter progression.
- Baud pulse and update control signals.
- Internal register and control signal transitions.

### Simulation Waveforms
<img width="1906" height="873" alt="Screenshot 2026-11-17 125056" src="https://github.com/user-attachments/assets/7632df83-a7e9-448a-bb3c-2fcaba6cbe2c" />

<img width="1617" height="861" alt="Screenshot 2026-11-17 123903" src="https://github.com/user-attachments/assets/61fded85-8652-4e81-a083-cef12020ad6a" />

<img width="1617" height="861" alt="Screenshot 2026-11-17 123903" src="https://github.com/user-attachments/assets/5f25db04-37a3-447c-bcf6-848344238985" />

<img width="1595" height="836" alt="Screenshot 2026-11-17 124638" src="https://github.com/user-attachments/assets/1d68a90c-2cce-4d15-a32a-86e553f1b2ff" />


<img width="1592" height="841" alt="Screenshot 2026-11-17 124616" src="https://github.com/user-attachments/assets/13a929ad-34f2-42cd-bd7a-16ce28ff0291" />

<img width="1592" height="841" alt="Screenshot 2026-11-17 124616" src="https://github.com/user-attachments/assets/185d399c-bd93-4ecf-9f0f-91f7375d9642" />

<img width="1603" height="842" alt="Screenshot 2026-11-17 124154" src="https://github.com/user-attachments/assets/9e736d5e-bb9d-4ff7-a406-f98f62feb750" />


These waveform captures provide a visual understanding of internal control signals, register operations, FIFO activity, and timing behavior.

**Note:** Waveform observations are not equivalent to complete functional verification. Successful end-to-end TX/RX communication and correct data reception should be established through appropriate self-checking testbench results.

## 10. Tools and Technologies

| Category | Technology |
|---|---|
| Hardware Description Language | SystemVerilog |
| FPGA Development Environment | Xilinx Vivado |
| Simulation Tool | Vivado Simulator |
| Design Methodology | Modular RTL Design |
| Architecture Analysis | RTL Schematic Visualization |
| Verification Approach | Behavioral Simulation |
| Communication Protocol | UART |

## 11. Applications

UART communication controllers are commonly used in:

- FPGA-based embedded systems.
- Microcontroller and processor communication.
- Serial data communication between digital devices.
- Sensor interfacing and data acquisition.
- Peripheral communication interfaces.
- Hardware debugging and monitoring systems.
- FPGA-based digital control applications.

## 12. Future Scope

The current RTL architecture provides a foundation for further development and verification.

Possible improvements include:

- Development of comprehensive self-checking SystemVerilog testbenches.
- SystemVerilog Assertions (SVA) for protocol and control verification.
- Functional coverage analysis.
- UART TX/RX loopback testing.
- Extended baud-rate configuration and verification.
- FIFO overflow and underflow testing.
- Detailed parity, framing, and break-condition verification.
- FPGA hardware implementation and validation.
- Resource utilization and timing analysis.
- Power, performance, and area (PPA) evaluation.

## 13. Conclusion

This project demonstrates the design and functional simulation of a modular UART communication controller using SystemVerilog and Xilinx Vivado.

The architecture integrates independent transmitter and receiver modules, a programmable register interface, and separate FIFO buffers to establish organized transmit and receive datapaths.

The modular RTL implementation provides a structured approach to digital hardware development, while Vivado schematic visualization helps analyze the design hierarchy and internal connectivity.

Behavioral simulation provides visibility into register operations, FIFO control signals, baud-rate-related timing behavior, and internal data movement.

Overall, this project provides practical experience in RTL development, digital communication architecture, SystemVerilog coding, module integration, schematic analysis, and functional verification.

## 14. References

[The following technical resources provide background for UART communication, RTL development, and FPGA implementation.
](https://www.udemy.com/course/communication-series-p1-uart-spi-and-i2c-in-verilog/learn/practice/1513152#overview)


---



**Technical Interests:** RTL Design | Digital VLSI | FPGA Architecture | SystemVerilog | Functional Verification

**Tools:** Xilinx Vivado | SystemVerilog | Vivado Simulator
