# 🎨 OLO Keystroke Injector

🚀 **An Offline-First HID Payload Companion & IoT Device Powered by ESP32-C3 SuperMini**

[![Launch Controller](https://img.shields.io/badge/OLO_Dashboard-Launch_Live_Web_App-FF6600?style=for-the-badge&logo=googlechrome&logoColor=white)](https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/)
[![Hardware](https://img.shields.io/badge/Hardware-ESP32--C3_SuperMini-blue?style=for-the-badge)](https://github.com/YOUR-USERNAME/YOUR-REPO-NAME)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

OLO Keystroke Injector is a pocket-sized, offline-focused hardware automation tool and desktop companion. It leverages the native Bluetooth Low Energy (BLE) HID capabilities of the **ESP32-C3** to act as a wireless programmable keyboard companion. 

Featuring an intuitive on-device UI via an OLED screen and a capacitive touch sensor, it allows you to fire custom macros, system shortcuts, presenter slide commands, or pen-testing payloads instantly. It syncs fully offline using a browser-based **Web Serial (USB)** controller dashboard or hosts its own wireless local dashboard.

---

## ✨ Key Features

*   **🎮 Snappy One-Button UI:** Entirely managed through a single debounced touch sensor supporting Single-Tap, Double-Tap, and Long-Press controls.
*   **🔌 Web Serial Sync:** Program payloads instantly via a browser-based GUI over USB—no developer environments or flashing software required.
*   **🌐 Wireless Local Hub:** Hosts its own standalone HTTP dashboard straight from memory (`PROGMEM`) at `http://olokeys.local/` when Wi-Fi is active.
*   **🛌 OLED Screen Protection:** Dual-stage safety exhaustion timer system (Drifting Eye Screensaver $\rightarrow$ Deep Sleep State) to prevent display burn-in.
*   **🔊 Audio-Visual Feedback:** Fully integrated NeoPixel RGB status indicators synced with multi-frequency buzzer tone notifications.
*   **🔒 Secured Isolation:** Dynamic text pushes flow instantly straight to volatile RAM to protect persistent flash cycles, backed by a dashboard security administration gate.

---

## 🔌 Hardware Connections & Pinout

Wire your components to the **ESP32-C3 SuperMini** following this physical routing layout:

| Component | Pin Name | ESP32-C3 GPIO | Power / Ground Rail | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **OLED Display** | SDA | `GPIO 20` | `3.3V` / `GND` | I2C Data Line (Fixed) |
| **OLED Display** | SCL | `GPIO 21` | `3.3V` / `GND` | I2C Clock Line (Fixed) |
| **Touch Sensor** | I/O | `GPIO 1` | `3.3V` or `5V` / `GND` | TTP223 Input Channel |
| **Buzzer** | Signal | `GPIO 2` | `GND` | PWM Frequency Audio Out |
| **NeoPixel LED** | DIN | `GPIO 6` | `5V` / `GND` | WS2812B RGB Control Line |

> [!WARNING]
> Double-check your hardware lines before applying power. Swapping `SCL/SDA` or selecting incorrect pin definitions will prevent the OLED system from initializing properly.

---

## 📂 System Menu Hierarchy

The physical display navigation maps across five structural device layers:

```text
[ Level 0: Animated Idle Face ]
           │ (Double Tap)
           ▼
[ Level 1: Clocks & Status ] (Single Tap cycles: Digital -> Analog -> BLE Status -> WiFi Status)
           │ (Double Tap on clocks to enter Menu, or on status pages to toggle radio)
           ▼
[ Level 2: Keystroke Category Menu ]
     ├── 1. Music Control ──────► [ Level 3: Submenu (Play, Next, Vol Up, etc.) ]
     ├── 2. OS Shortcuts  ──────► [ Level 3: Submenu (Lock, Close, Task Manager) ]
     ├── 3. Presenter Mode ─────► [ Level 3: Submenu (Next/Prev Slide, Blank Screen) ]
     ├── 4. Custom Payload ─────► [ Level 3: Submenu (Run Payload) ]
     ├── 5. Pen-Test Payloads ──► [ Level 3: Submenu (Notepad Demo, Add Admin, etc.) ]
     ├── 6. Settings ──────────► [ Level 3: Submenu (Toggle BLE, Toggle WiFi) ]
     ├── 7. FIFA 2026 ─────────► [ Level 3: Submenu (Easter Egg Facts) ]
     └── 8. Exit (Back to Clock)
