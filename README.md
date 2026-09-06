# Basys-3 UART Communication Bridge

A complete bidirectional UART (Universal Asynchronous Receiver-Transmitter) implementation written in Verilog for the Digilent Basys-3 FPGA.

This project establishes a reliable 9600-baud serial link between a host PC and the FPGA. It processes incoming ASCII data to display on the board's 7-segment display and LEDs, and transmits hardware switch states back to the PC terminal.

---

## Table of Contents

- [Overview](#overview)
- [System Architecture](#system-architecture)
- [Core Modules](#core-modules)
- [Hardware I/O Mapping](#hardware-io-mapping)
- [How to Run](#how-to-run)

---

## Overview

- **Language:** Verilog
- **Target Hardware:** Digilent Basys-3 FPGA
- **Toolchain:** Xilinx Vivado
- **Baud Rate:** 9600

---

## System Architecture

> Add a block diagram here showing the clock, TX, RX, and data paths between the host PC and the FPGA.

---

## Core Modules

| Module | Description |
|---|---|
| `baud_rate_generator.v` | Derives the 9600 baud timing ticks from the Basys-3 100 MHz system clock |
| `receiver.v` | Samples the `rx` line, detects start/stop bits, and packages serial data into 8-bit parallel bytes |
| `transmitter.v` | Converts 8-bit parallel data from the hardware switches into a formatted serial stream sent out through the `tx` line |
| `top.v` | Instantiates the UART core, manages edge-detection for the transmit button, and includes binary-to-BCD logic for multiplexing the 7-segment display |

---

## Hardware I/O Mapping

| PC/External | Basys-3 Component | FPGA Pin | Purpose |
|---|---|---|---|
| PuTTY (PC) | USB-UART `rx` / `tx` | B18 / A18 | Serial data lines |
| User Input | Switches [7:0] | V17 – W13 | 8-bit data payload to transmit |
| User Input | Top Button (`btn`) | T18 | Triggers transmission (pulse) |
| User Input | Center Button (`rst`) | U18 | System reset / initialize TX idle |
| Output | LEDs [7:0] | U16 – V14 | Binary visualization of received ASCII |
| Output | 7-Segment Display | W7 – W4 | Decimal visualization of received ASCII |

---

## How to Run

1. Clone the repository and open Xilinx Vivado.
2. Create a new RTL project, selecting part `xc7a35tcpg236-1` (Basys-3).
3. Import the `.v` files from the `/src` directory as design sources.
4. Import the `.xdc` file from the `/constraints` directory.
5. Generate the bitstream and program the device.
6. Open PuTTY (or any serial terminal) on the host PC. Connect to the board's COM port at **9600 baud** (ensure "Local Echo" is configured as needed).
