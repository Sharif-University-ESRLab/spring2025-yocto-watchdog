# Yocto-Based I2C Watchdog for Embedded Systems

This repository contains the source code for the "Error Detection in Communication Buses" project. The goal is to implement a watchdog system using a Raspberry Pi to monitor an I2C communication bus in real-time, detect anomalies, and report errors. The entire system is built upon a custom Linux distribution generated with **Yocto**.

## Project Overview

In many embedded systems, components like sensors and microcontrollers communicate over a shared bus such as I2C. This communication is vulnerable to errors caused by software bugs, environmental factors, or even security attacks.

This project implements a **guardian (or watchdog) system** that passively monitors all data exchanged on an I2C bus. It identifies anomalies that could indicate a fault or attack, ensuring the integrity of the communication channel.


[cite_start]The system architecture consists of three main components[cite: 165, 166]:
1.  [cite_start]**Main Embedded System**: A Raspberry Pi responsible for communicating with a sensor over the I2C bus[cite: 165].
2.  [cite_start]**I2C Sensor**: An I2C-based sensor that communicates with the main system[cite: 161, 165].
3.  **Watchdog System**: A second Raspberry Pi that connects to the same I2C bus in a monitoring (or "sniffer") role. [cite_start]It analyzes the traffic to detect and report anomalies[cite: 166].

[cite_start]Anomalies detected can include[cite: 166]:
* Unauthorized data exchange
* Inconsistent data or incorrect addressing
* Delayed or missing responses from devices

## Hardware & Software Requirements

### Hardware
* [cite_start]**Raspberry Pi**: 2 units [cite: 161]
* [cite_start]**I2C-based Sensor**: 1 unit (e.g., a temperature sensor, accelerometer, etc.) [cite: 161]
* MicroSD cards for the Raspberry Pis
* Necessary wiring to connect the devices to the same I2C bus (SDA, SCL, GND)

### Software
* [cite_start]**Yocto Project**: Used to build the custom Linux OS for both Raspberry Pi boards[cite: 163, 167].

## Code Structure

The code in this repository is organized into the following directories:

* **/Main_System**: Contains the source code for the primary Raspberry Pi. This code is responsible for initiating communication with the I2C sensor.
* **/I2C_Sniffer**: Contains the source code for the watchdog Raspberry Pi. This application actively monitors the I2C bus, parses the traffic, and implements the logic for anomaly detection.
* **/Test_Code**: Includes supplementary test scripts and code used for debugging or simulating specific error conditions on the bus.

## Getting Started

### 1. Hardware Setup
1.  Identify the I2C pins (SDA, SCL, GND) on both Raspberry Pis and the sensor.
2.  Connect all three devices to a common I2C bus. Ensure that the SDA, SCL, and GND lines are properly shared among them.

### 2. Operating System
[cite_start]Build and flash a custom Linux image generated with the **Yocto Project** onto the microSD cards for both Raspberry Pi units[cite: 175].

### 3. Build and Run
1.  Clone this repository onto both Raspberry Pi systems.
    ```bash
    git clone [https://github.com/Sharif-University-ESRLab/spring2025-yocto-watchdog.git](https://github.com/Sharif-University-ESRLab/spring2025-yocto-watchdog.git)
    cd spring2025-yocto-watchdog/Code
    ```
2.  **On the Main System Raspberry Pi**:
    * Navigate to the `Main_System` directory.
    * Compile the code (update the command based on your build system, e.g., `make`).
    * [cite_start]Run the compiled executable to start communication with the sensor[cite: 175].

3.  **On the Watchdog Raspberry Pi**:
    * Navigate to the `I2C_Sniffer` directory.
    * Compile the code.
    * [cite_start]Run the sniffer executable to begin monitoring the I2C bus for anomalies[cite: 176].

Once both systems are running, the watchdog will print any detected errors or anomalies to the console. You can use the code in the `Test_Code` directory to inject faults and verify the watchdog's detection capabilities.
