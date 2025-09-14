UAV Telemetry data over-the-air using Lora Communication
```markdown
# LoRa Telemetry Communications Project

This repository documents the design and development of a **LoRa-based telemetry communications module** for UAVs.  
The system enables **MAVLink2 packet transmission** between a UAV flight controller (Pixhawk) and a Ground Control Station (GCS), using the **SX1262 LoRa transceiver** for long-range, efficient communication.

---

## 📡 Project Overview

- **Flight Controller**: Pixhawk (TELEM Port, DF13 6-pin)  
- **MCU**: STM32F103 (UART, SPI, GPIOs)  
- **LoRa Module**: EBYTE E22-900M30S (SX1262 + PA/LNA + IPEX Antenna)  
- **Protocol**: MAVLink2 over UART → LoRa (SPI) → UART to GCS  
- **Power**: 5V battery input, regulated to 3.3V for MCU + SX1262  
- **Debug/Config**: USB-C / FTDI / SWD programming  

This project takes inspiration from **DroneBridge** (WiFi-based MAVLink bridge), but adapts it for **LoRa SX1262 hardware** with a lightweight custom firmware and PCB.

---

## 🔧 System Block Diagram

```

+----------------------+        SPI/Control         +----------------------+
\| Pixhawk TELEM (DF13) | <---UART RX/TX (3.3V)-->   |   MCU (STM32F103)    |
\|  DF13 6-pin cable    |                            |  UART, SPI, GPIOs    |
\|  Pin2 TX -> MCU\_RX   |                            |                      |
\|  Pin3 RX <- MCU\_TX   |                            |  SPI -> SX1262 MODULE|
\|  Pin6 GND ---------- |                            |  UART debug (USB)    |
+----------------------+                            +----+----+----+----+--+
\|    |    |
NSS / SCK / MISO / MOSI | BUSY/DIO1/RESET
\|    |    |
\|    |    |
+--------v----v----v--------+
\|   EBYTE E22-900M30S      |
\|   (SX1262 + PA/LNA + IPEX)|
\|   SPI Interface, DIOs,    |
\|   ANT (IPEX)              |
+---------------------------+

Power: battery / 5V -> 3.3V regulator -> MCU + SX1262 module
Optional: USB-C or FTDI for MCU programming & config

Aux: Buttons (reset / boot), status LEDs (Tx / Rx / Link), EEPROM (config), SWD header for firmware programming

```

---

## ⚙️ Features

- ✅ **MAVLink2 support** (Pixhawk TELEM to Ground Station)  
- ✅ **STM32F103 MCU** handles UART ↔ SPI bridging and protocol handling  
- ✅ **SX1262 LoRa** for long-range, low-power telemetry  
- ✅ **EEPROM storage** for configuration persistence  
- ✅ **Status LEDs** (Tx / Rx / Link activity)  
- ✅ **SWD header & USB-C** for firmware development and debugging  

---

## 🛠️ Hardware Requirements

- **Pixhawk TELEM Cable** (DF13 6-pin)  
- **STM32F103 MCU Board / Custom PCB**  
- **EBYTE E22-900M30S (SX1262 module)**  
- **3.3V LDO Regulator** (if powering from 5V battery/USB)  
- **Optional**:  
  - USB-C / FTDI adapter for UART debugging  
  - Buttons for Reset/Boot  
  - EEPROM for configuration storage  
  - SWD header for firmware flashing  

---

## 📂 Repository Structure

```

/hardware      -> Schematics, PCB layouts, BOM
/firmware      -> STM32 code for UART-SPI bridge (MAVLink2 handling)
/docs          -> Datasheets, reference designs, wiring guides

```

---

## 🚀 Getting Started

1. Connect Pixhawk TELEM (DF13 cable) → STM32F103 UART pins.  
2. Connect STM32F103 SPI → SX1262 module (E22-900M30S).  
3. Power via 5V battery → 3.3V LDO → MCU + SX1262.  
4. Flash firmware via SWD/USB.  
5. Connect antenna to IPEX port of SX1262.  
6. Verify communication with Ground Control Station (e.g., Mission Planner, QGroundControl).  

---

## 📜 License

MIT License – free to use, modify, and distribute.

---

## 🙌 Acknowledgements

- [DroneBridge](https://github.com/DroneBridge) for inspiring efficient MAVLink communication methods  
- [EBYTE E22-900M30S](https://www.ebyte.com/en/product-view-news.aspx?id=444) (SX1262 module)  
- [MAVLink Protocol](https://mavlink.io/en/)  
```

---
