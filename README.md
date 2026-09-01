# 📡 LoRa Mesh Communication App

A wireless offline communication system using **ESP32, E220 LoRa modules, Bluetooth Low Energy (BLE), and an Android application**.

The project is designed to enable communication between two phones without requiring Wi-Fi or mobile internet. Messages are transferred through ESP32 and LoRa modules.

---

## 🚀 Project Overview

The system provides two stages of communication.

### 🔹 Step 1 — LoRa Communication with Serial Monitor

First, two ESP32 boards are tested directly using E220 LoRa modules.

```text
ESP32-1 + E220  📡  ~~~~~ LoRa ~~~~~  📡  E220 + ESP32-2
   ↓                                      ↓
Serial Monitor                         Serial Monitor
 Transmitter                             Receiver
```

In this stage:

* ESP32-1 acts as the **Transmitter**
* ESP32-2 acts as the **Receiver**
* Messages are entered through the Arduino Serial Monitor
* ESP32-1 sends the message through E220 LoRa
* ESP32-2 receives the message
* The received message is displayed on the Serial Monitor

### Example

```text
ESP32-1 Serial Monitor

Enter message:
Hello from ESP32 1

        ↓
      E220
        ↓
     LoRa RF
        ↓
      E220
        ↓

ESP32-2 Serial Monitor

Received:
Hello from ESP32 1
```

---

# 🔹 Step 2 — Phone-to-Phone Communication

After successfully testing LoRa communication, the system is connected to an Android application using BLE.

```text
📱 Phone 1
     │
     │ BLE
     ↓
ESP32-1
     │
     │ UART
     ↓
E220 LoRa
     │
     │  LoRa Wireless Link
     ↓
E220 LoRa
     │
     │ UART
     ↓
ESP32-2
     │
     │ BLE
     ↓
📱 Phone 2
```

### Communication Flow

```text
Phone 1
   ↓
BLE
   ↓
ESP32-1
   ↓
E220 LoRa
   ↓
Wireless LoRa
   ↓
E220 LoRa
   ↓
ESP32-2
   ↓
BLE
   ↓
Phone 2
```

The same path can be used in the reverse direction:

```text
Phone 2 → BLE → ESP32-2 → LoRa → ESP32-1 → BLE → Phone 1
```

This allows **two-way communication**.

---

# ✨ Features

* 📡 Long-range LoRa communication
* 📱 Phone-to-phone communication
* 🔵 BLE connectivity
* 🔌 ESP32-based communication nodes
* 📶 E220 LoRa modules
* 💬 Text message transmission
* 📝 Support for multi-word messages
* 📄 Support for dialogue/paragraph-style messages
* 🔄 Two-way communication
* 🌐 Offline communication
* 🖥️ Serial Monitor testing
* 🔐 Can be extended with AES-GCM encryption
* 🗺️ Can be extended with offline location/map functionality

---

# 🧩 Hardware Requirements

| Component               | Quantity |
| ----------------------- | -------: |
| ESP32 Development Board |        2 |
| E220 LoRa Module        |        2 |
| Android Phones          |        2 |
| USB Cable               |        2 |
| Jumper Wires            | Required |
| Power Supply            | Required |

---

# 🔌 ESP32–E220 Connection

The project uses UART communication between the ESP32 and E220.

### Example Pin Configuration

| E220 | ESP32   |
| ---- | ------- |
| TX   | GPIO 16 |
| RX   | GPIO 17 |
| M0   | GPIO 25 |
| M1   | GPIO 26 |
| AUX  | GPIO 27 |
| VCC  | 3.3V    |
| GND  | GND     |

> **Note:** Verify the voltage and pin requirements of your specific E220 module before powering it.

---

# 🖥️ Step 1: Serial Monitor Test

The first objective is to verify that the LoRa link works correctly.

### ESP32-1 — Transmitter

```text
Serial Monitor
      ↓
Enter message
      ↓
ESP32-1
      ↓
E220
      ↓
LoRa
```

### ESP32-2 — Receiver

```text
LoRa
 ↓
E220
 ↓
ESP32-2
 ↓
Serial Monitor
```

### Test Example

```text
TRANSMITTER

> Hello

RECEIVER

Received: Hello
```

Test with longer messages:

```text
> Hello, this is a LoRa communication test between two ESP32 boards.
```

The receiver should display the complete message.

---

# 📱 Step 2: Phone-to-Phone Communication

After the Serial Monitor test is successful, BLE is added.

### Phone 1

```text
User enters message
        ↓
Android App
        ↓
BLE
        ↓
ESP32-1
```

### LoRa Link

```text
ESP32-1
   ↓
E220
   ↓
LoRa
   ↓
E220
   ↓
ESP32-2
```

### Phone 2

```text
ESP32-2
   ↓
BLE
   ↓
Android App
   ↓
Received message
```

---

# 🔵 BLE Communication

The Android application communicates with ESP32 using **Bluetooth Low Energy**.

The project can use the Nordic UART Service structure:

```text
BLE Service
     │
     ├── RX Characteristic
     │      ↓
     │   Phone → ESP32
     │
     └── TX Characteristic
            ↓
         ESP32 → Phone
```

---

# 📦 Message Structure

The system is designed to support messages larger than a single word.

Example:

```text
Hello
```

and:

```text
Hello, how are you?
```

and longer dialogue:

```text
Hello, how are you? I am testing the LoRa communication system.
The message should travel from Phone 1 to ESP32-1,
then through LoRa to ESP32-2,
and finally reach Phone 2.
```

For larger messages, the application can divide the data into packets and reconstruct the complete message at the receiver.

```text
Large Message
      ↓
Packet 1
Packet 2
Packet 3
Packet 4
      ↓
     LoRa
      ↓
Packet 1
Packet 2
Packet 3
Packet 4
      ↓
Reconstruct Message
      ↓
Complete Message
```

---

# 🔐 Future Security

The project can use **AES-GCM encryption** to protect messages.

```text
Original Message
      ↓
   AES-GCM
      ↓
Encrypted Data
      ↓
    LoRa
      ↓
Encrypted Data
      ↓
   AES-GCM
      ↓
Original Message
```

This can provide:

* Confidentiality
* Message integrity
* Authentication

---

# 🗺️ Future Offline Location Feature

An offline location-sharing feature can be integrated into the application.

```text
Phone 1
  ↓
Location Data
  ↓
ESP32
  ↓
LoRa
  ↓
ESP32
  ↓
Phone 2
  ↓
Offline Map
```

The goal is to allow users to exchange location information without relying on mobile internet.

---

# 📂 Project Structure

```text
LoRa-Mesh-App/
│
├── README.md
│
├── ESP32/
│   ├── ESP32_Transmitter/
│   │   └── transmitter.ino
│   │
│   └── ESP32_Receiver/
│       └── receiver.ino
│
├── BLE/
│   └── BLE_Communication/
│
├── Android-App/
│   └── LoRaMeshApp/
│
└── Documentation/
    ├── Circuit_Diagram
    ├── Block_Diagram
    └── Project_Report
```

---

# 🛠️ Development Stages

### Stage 1

```text
ESP32 → E220 → E220 → ESP32
```

Test using Serial Monitor.

### Stage 2

```text
Phone → BLE → ESP32
```

Test BLE communication.

### Stage 3

Combine both:

```text
Phone 1
 ↓ BLE
ESP32-1
 ↓
E220
 ↓
LoRa
 ↓
E220
 ↓
ESP32-2
 ↓ BLE
Phone 2
```

### Stage 4

Add:

* Long-message support
* Packet fragmentation
* Packet reassembly
* Message IDs
* Error checking
* AES-GCM encryption
* Offline location sharing
* Mesh/network expansion

---

# 🎯 Project Objective

The main objective of this project is to develop an **offline communication platform** that allows users to exchange messages over long distances using **BLE and LoRa**, without depending on Wi-Fi or cellular internet.

---

# 📌 Technologies Used

* **ESP32**
* **E220 LoRa**
* **LoRa RF Communication**
* **Bluetooth Low Energy (BLE)**
* **Android**
* **UART**
* **AES-GCM** *(planned security feature)*
* **Offline Maps** *(planned feature)*

---

# 🔄 Complete System

```text
                 OFFLINE COMMUNICATION

📱 PHONE 1
    │
    │ BLE
    ▼
┌──────────┐
│ ESP32-1  │
└──────────┘
    │
    │ UART
    ▼
┌──────────┐
│ E220 #1  │
└──────────┘
    │
    │  LoRa Wireless
    │  Communication
    ▼
┌──────────┐
│ E220 #2  │
└──────────┘
    │
    │ UART
    ▼
┌──────────┐
│ ESP32-2  │
└──────────┘
    │
    │ BLE
    ▼
📱 PHONE 2
```

---

# 🚀 Future Improvements

* Multi-node LoRa mesh networking
* Multiple users
* Message acknowledgement
* Automatic packet retransmission
* Message encryption
* Offline live-location sharing
* Offline map support
* Emergency communication mode
* Battery-powered portable nodes
* Improved range and reliability

---

# 👨‍💻 Project Status

**Current Development Path:**

```text
✅ ESP32 + E220 Hardware Setup
        ↓
✅ Serial Monitor Communication
        ↓
🔄 BLE Communication
        ↓
🔄 Phone-to-Phone Communication
        ↓
🔜 Long Message / Packet Handling
        ↓
🔜 AES-GCM Security
        ↓
🔜 Offline Location Sharing
        ↓
🔜 Multi-Node LoRa Mesh
```

---

## ⭐ Project Goal

> **Build a reliable offline communication system that connects smartphones through BLE and long-range LoRa technology without depending on the Internet or cellular network.**
