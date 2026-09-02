# 📡 LoRa Offline Communication System

## 🔐 BLE + VEGA + E220 LoRa + ESP32

An **offline communication system** that enables users to exchange messages between Android phones without relying on Wi-Fi or mobile internet.

The system combines **Bluetooth Low Energy (BLE)** for phone-to-board communication and **E220 LoRa modules** for long-range board-to-board communication.

---

## 🚀 Project Overview

This project is designed to provide **offline phone-to-phone communication** using a LoRa-based communication link.

### Communication Architecture

```text
             PHONE 1
                │
              BLE
                │
                ▼
        ┌────────────────┐
        │  VEGA ARIES    │
        │    IoT v2      │
        └────────────────┘
                │
              UART
                │
                ▼
        ┌────────────────┐
        │  E220 LoRa     │
        │   Transmitter  │
        └────────────────┘
                │
             LoRa RF
                │
                ▼
        ┌────────────────┐
        │  E220 LoRa     │
        │    Receiver    │
        └────────────────┘
                │
              UART
                │
                ▼
        ┌────────────────┐
        │     ESP32      │
        └────────────────┘
                │
              BLE
                │
                ▼
             PHONE 2
```

---

# ✨ Features

* 📱 Phone-to-phone offline communication
* 🔵 Bluetooth Low Energy communication
* 📡 Long-range LoRa communication
* 💬 Text messaging
* 📝 Multi-word and paragraph messages
* 📍 Offline location-sharing concept
* 🔐 AES-GCM encryption support
* 🔄 Packet-based data transmission
* 📊 Serial Monitor debugging
* 🎙️ Short voice-message support can be added
* 🌐 No Wi-Fi required
* 📶 No mobile network required

---

# 🧩 Hardware Components

| Component           | Purpose                           |
| ------------------- | --------------------------------- |
| VEGA Aries IoT v2   | Phone 1 BLE + LoRa controller     |
| ESP32               | Phone 2 BLE + LoRa controller     |
| E220 LoRa Module ×2 | Long-range wireless communication |
| Android Phone ×2    | User interface                    |
| Jumper Wires        | Hardware connections              |
| USB Cable           | Programming and debugging         |

---

# 🔌 Hardware Connections

## VEGA Aries IoT v2 → E220

### UART Connection

```text
VEGA Aries IoT v2       E220 LoRa
----------------------------------
J2-13 (UART1 TX)   →    RXD
J2-15 (UART1 RX)   ←    TXD
GND                →    GND
```

### Control Pins

```text
VEGA GPIO 5  → E220 M0
VEGA GPIO 6  → E220 M1
VEGA GPIO 7  → E220 AUX
```

> Make sure the E220 and VEGA use compatible logic levels and that GND is common.

---

# 🔵 BLE Communication

The Android application communicates with the boards using **Bluetooth Low Energy**.

The project uses the Nordic UART Service (NUS) UUID structure.

```text
Service UUID:
6E400001-B5A3-F393-E0A9-E50E24DCCA9E

RX UUID:
6E400002-B5A3-F393-E0A9-E50E24DCCA9E

TX UUID:
6E400003-B5A3-F393-E0A9-E50E24DCCA9E
```

### Phone 1

```text
Phone 1
   ↓ BLE
VEGA Aries IoT v2
```

### Phone 2

```text
ESP32
  ↓ BLE
Phone 2
```

---

# 📡 LoRa Communication

The two E220 modules form the long-range wireless link.

```text
VEGA
  │
  │ UART
  ▼
E220 #1
  │
  │ LoRa RF
  ▼
E220 #2
  │
  │ UART
  ▼
ESP32
```

The LoRa link transfers packetized data between the two boards.

---

# 💬 Text Message Flow

For a normal text message:

```text
Phone 1
   │
   │ BLE
   ▼
VEGA
   │
   │ UART
   ▼
E220 #1
   │
   │ LoRa
   ▼
E220 #2
   │
   ▼
ESP32
   │
   │ BLE
   ▼
Phone 2
```

Example:

```text
Phone 1:
"Hello, how are you?"

        ↓

BLE

        ↓

VEGA

        ↓

LoRa

        ↓

ESP32

        ↓

BLE

        ↓

Phone 2:
"Hello, how are you?"
```

---

# 📝 Long Messages

The system is designed to support messages longer than a single short word.

Instead of sending a complete paragraph as one small packet, the message can be divided into multiple packets.

Example:

```text
Original Message
       ↓
Split into chunks
       ↓
Packet 1
Packet 2
Packet 3
Packet 4
       ↓
Transmit through LoRa
       ↓
Receive packets
       ↓
Reassemble
       ↓
Original paragraph
```

A packet can contain information such as:

```text
Message ID
Packet Number
Total Packets
Payload Length
Payload Data
```

This allows the receiver to reconstruct the complete message.

---

# 🔐 AES-GCM Encryption

For secure communication, the application can use **AES-GCM**.

Basic flow:

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

Example:

```text
HELLO KAVIN

      ↓ Encryption

8F A3 91 C2 ...

      ↓ LoRa

8F A3 91 C2 ...

      ↓ Decryption

HELLO KAVIN
```

AES-GCM provides confidentiality and authentication when implemented with proper key and nonce management.

---

# 📍 Offline Location Communication

The project can also be extended to exchange location information without requiring internet connectivity.

Possible architecture:

```text
Phone 1
   ↓
Location Data
   ↓
BLE
   ↓
VEGA
   ↓
LoRa
   ↓
ESP32
   ↓
BLE
   ↓
Phone 2
```

The application can display received coordinates on an offline map.

---

# 🎙️ Voice Message Extension

Short voice messages can be added as a future feature.

### Voice Message Flow

```text
Phone 1
   ↓
Record Voice
   ↓
Compress Audio
   ↓
Split into Packets
   ↓
BLE
   ↓
VEGA
   ↓
E220 LoRa
   ↓
E220 LoRa
   ↓
ESP32
   ↓
BLE
   ↓
Phone 2
   ↓
Reassemble Audio
   ↓
Play Voice
```

### Important

The E220 LoRa link is better suited to **short recorded voice messages** than continuous live voice calls because LoRa has limited data throughput.

---

# 🖥️ Serial Monitor Testing

Before connecting the Android application, the hardware can be tested through the Serial Monitor.

### Transmitter

```text
================================
      VEGA LoRa TRANSMITTER
================================

VEGA READY
LoRa READY

Enter message:
Hello from VEGA

Message transmitted successfully
```

### Receiver

```text
================================
       ESP32 LoRa RECEIVER
================================

ESP32 READY
LoRa READY

Received:
Hello from VEGA
```

---

# 🧪 Development Stages

## Stage 1 — LoRa Hardware Test

```text
VEGA → E220 → E220 → ESP32
```

Verify that messages are correctly transmitted and received through the Serial Monitor.

---

## Stage 2 — BLE Test

```text
Phone → BLE → Board
```

Verify that the Android application can discover, connect and communicate with the board.

---

## Stage 3 — BLE + LoRa

```text
Phone 1
   ↓ BLE
VEGA
   ↓ LoRa
ESP32
   ↓ BLE
Phone 2
```

Test normal text messages.

---

## Stage 4 — Long Messages

Implement packet fragmentation and reassembly.

```text
Long Message
     ↓
Packetization
     ↓
LoRa
     ↓
Reassembly
     ↓
Complete Message
```

---

## Stage 5 — Security

Add AES-GCM encryption and authentication.

```text
Message
 ↓
AES-GCM
 ↓
Packetization
 ↓
LoRa
 ↓
Reassembly
 ↓
AES-GCM Verification
 ↓
Message
```

---

## Stage 6 — Offline Location

Add location data exchange and offline map functionality.

---

## Stage 7 — Voice Messages

Add short recorded voice messages using audio compression and packetized transmission.

---

# 📂 Suggested GitHub Repository Structure

```text
LoRa-Offline-Communication/
│
├── README.md
│
├── VEGA/
│   └── vega_lora_ble.ino
│
├── ESP32/
│   └── esp32_lora_ble.ino
│
├── Android-App/
│   ├── BLE Communication
│   ├── Chat
│   ├── Location
│   └── Voice Message
│
├── LoRa/
│   ├── Transmitter
│   └── Receiver
│
├── Encryption/
│   └── AES-GCM
│
└── Documentation/
    ├── Circuit Diagram
    ├── Block Diagram
    └── Project Photos
```

---

# 🛠️ Technologies Used

* **VEGA Aries IoT v2**
* **ESP32**
* **E220 LoRa**
* **Bluetooth Low Energy (BLE)**
* **UART**
* **Arduino IDE**
* **Android**
* **AES-GCM**
* **Offline Maps**
* **Packet-based communication**

---

# 🎯 Project Objective

The main objective of this project is to develop an **offline communication platform** that can exchange information between smartphones using BLE and LoRa, without depending on conventional internet or cellular networks.

The project demonstrates how embedded systems, wireless communication, encryption and mobile applications can be integrated into a single communication system.

---

# 🔮 Future Enhancements

* 🎙️ Improved voice-message transmission
* 📷 Image/file transfer
* 👥 Multi-node LoRa mesh networking
* 🔐 End-to-end encrypted communication
* 📍 Improved offline maps
* 🔋 Low-power operation
* 📶 Automatic node discovery
* 🔄 Message acknowledgement and retransmission
* 📊 Signal-strength monitoring
* 🧩 More LoRa nodes for mesh communication

---

# 👨‍💻 Project Status

| Module                  | Status                 |
| ----------------------- | ---------------------- |
| E220 LoRa communication | ✅                      |
| Serial Monitor testing  | ✅                      |
| ESP32 communication     | ✅                      |
| VEGA integration        | ✅                      |
| BLE communication       | 🔧 In development      |
| Phone-to-phone text     | 🔧 In development      |
| Long messages           | 🔧 In development      |
| AES-GCM                 | 🔧 Planned/Integration |
| Offline location        | 🔧 Planned/Integration |
| Voice messages          | 🔮 Future enhancement  |

---

# 📜 License

This project is intended for educational, research and development purposes.

---

## ⭐ Project Concept

**"Communicate anywhere, even without the Internet."**

```text
📱 PHONE
   ↓
🔵 BLE
   ↓
🟢 VEGA
   ↓
📡 E220 LoRa
   ↓
📡 E220 LoRa
   ↓
🔵 ESP32 BLE
   ↓
📱 PHONE
```

**Offline • Long Range • Secure • Embedded • Communication**
