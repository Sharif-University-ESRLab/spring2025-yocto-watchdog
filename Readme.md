# Hardware Lab Project - Spring 2025
### Error Detection in Communication Buses

[![Sharif University of Technology Logo](https://img.shields.io/badge/Sharif_University_of_Technology-blue)](https://www.sharif.edu/)
[![ESRLab Logo](https://img.shields.io/badge/Internet_Of_Things_Lab-red)](https://esrlab.ce.sharif.edu/)

---

## 📖 Table of Contents
* [Project Description](#-project-description)
* [Hardware Requirements](#-hardware-requirements)
* [System Architecture](#-system-architecture)
* [Preliminary Setups](#-preliminary-setups)
* [How to Run](#-how-to-run)
* [Results](#-results)
* [Project Report](#-project-report)
* [Developers](#-developers)

---

## 📝 Project Description

This project aims to enhance the reliability of embedded systems by detecting errors on communication buses. We implement a **watchdog system** using a dedicated Raspberry Pi to monitor an **I2C bus** in real-time. This watchdog passively "sniffs" the data being exchanged between a primary embedded system (another Raspberry Pi) and an I2C sensor.

The primary goal is to identify and report various anomalies, which could be caused by software bugs, environmental interference, or potential security attacks. The watchdog is designed to detect issues such as unauthorized data transfers, incorrect device addressing, inconsistent data, and response delays. The entire system runs on a custom Linux distribution built using the **Yocto Project**.

---

## 💻 Hardware Requirements

To replicate this project, you will need the following hardware:
* **Raspberry Pi Boards**: 2 units
* **I2C-based Sensor**: 1 unit
* MicroSD Cards (one for each Raspberry Pi)
* Power supplies and necessary wiring for the I2C bus (SDA, SCL, GND)

---

## 🏗️ System Architecture

The physical setup consists of three devices sharing a common I2C communication bus. This is achieved by connecting the SDA (Serial Data), SCL (Serial Clock), and GND (Ground) pins of all three components together.

The roles within this architecture are clearly defined:

1.  **Main Embedded System (Raspberry Pi)**: This device acts as the I2C bus master. It is responsible for initiating communication with and reading data from the sensor.
2.  **Watchdog System (Raspberry Pi)**: This is the core of our project. It is connected to the same I2C bus but acts as a passive listener or "sniffer". It does not initiate transactions or interfere with the primary communication. Its sole purpose is to capture and analyze all traffic to identify and report anomalies.
3.  **Sensor (I2C Device)**: This device acts as the I2C bus slave. It responds to requests from the Main Embedded System.

This separation of concerns allows the primary system to perform its tasks without the overhead of self-monitoring, while the dedicated watchdog ensures high-integrity, non-intrusive supervision of the communication channel.

---

## 🛠️ Preliminary Setups

Both Raspberry Pi boards must be running a custom Linux image built with Yocto. Follow these steps to create the image.

### 1. Set Up the Build Environment
First, clone the Yocto Project's core, Poky.

```bash
git clone git://git.yoctoproject.org/poky
cd poky
```

### 2. Configure the Build
Source the environment setup script to create the build directory.

```bash
source oe-init-build-env
```
This will place you inside the `build/conf` directory.

### 3. Add Necessary Layers
Clone the required meta-layers for Raspberry Pi support.

```bash
git clone git://git.openembedded.org/meta-openembedded
git clone git://git.yoctoproject.org/meta-raspberrypi
```

Next, add these layers to your configuration by editing `bblayers.conf`:

```bash
# Add these lines to your build/conf/bblayers.conf file
BBLAYERS += " ${TOPDIR}/../meta-openembedded/meta-oe "
BBLAYERS += " ${TOPDIR}/../meta-openembedded/meta-python "
BBLAYERS += " ${TOPDIR}/../meta-openembedded/meta-multimedia "
BBLAYERS += " ${TOPDIR}/../meta-openembedded/meta-networking "
BBLAYERS += " ${TOPDIR}/../meta-raspberrypi "
```

### 4. Configure `local.conf`
Edit the `build/conf/local.conf` file to set the machine type and add necessary features like SSH and I2C tools.

```bash
# Set the machine to Raspberry Pi 3 (or your model)
MACHINE ?= "raspberrypi3-64"

# Enable UART and SSH for remote access
ENABLE_UART = "1"
ENABLE_SSH_SERVER = "1"

# Add i2c-tools and other packages
EXTRA_IMAGE_FEATURES:append = " tools-debug"
IMAGE_INSTALL:append = " i2c-tools"
```

### 5. Build the Image
Start the build process. This can take a significant amount of time.

```bash
bitbake core-image-base
```

### 6. Flash the Image
Once the build is complete, the image will be located at `build/tmp/deploy/images/raspberrypi...`. Flash the `.wic.gz` image file to your microSD cards using a tool like Raspberry Pi Imager or `dd`.

---

## 🚀 How to Run

1.  **Connect Hardware**: Wire the two Raspberry Pis and the I2C sensor to the same I2C bus by connecting their respective SDA, SCL, and GND pins.
2.  **Clone Repository**: On both flashed Raspberry Pis, clone this project's repository.
    ```bash
    git clone [https://github.com/Sharif-University-ESRLab/spring2025-yocto-watchdog.git](https://github.com/Sharif-University-ESRLab/spring2025-yocto-watchdog.git)
    cd spring2025-yocto-watchdog
    ```
3.  **Run the Main System**: On the primary Raspberry Pi, navigate to the `Code/Main_System` directory, compile the code, and run the executable. This will start the communication with the sensor.
4.  **Run the Watchdog**: On the watchdog Raspberry Pi, navigate to the `Code/I2C_Sniffer` directory, compile the code, and run the executable. The watchdog will now begin monitoring the bus and reporting its findings to the console.

---

## 📊 Results

The expected outcome is for the **Watchdog System** to successfully monitor the I2C bus. When an error or anomaly is detected (either naturally or by using the test scripts in `Code/Test_Code`), the watchdog will print a descriptive message to its console, detailing the nature of the fault.

---

## 📜 Project Report

The final detailed report for this project, including design choices, implementation details, and test results, can be found here:

**[Project Report](./Report.pdf)**

---

## 🧑‍💻 Developers

* Ali Shahheidar
* Alireza Rezaeimoghadam
