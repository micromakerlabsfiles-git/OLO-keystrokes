# OLO Keystroke Injector: BLE & WiFi Robot Companion

A smart, interactive, and customizable keystroke injector built for the **ESP32-C3 Super Mini** micro-controller. This device emulates a standard Bluetooth Low Energy (BLE) Keyboard using the **ESP32-BLE-Keyboard** library wrapped in the lightweight **NimBLE** stack, and hosts an on-chip Web Control Center served over local Wi-Fi.

The device is disguised as a small robot-like character displaying animations on a dynamic OLED screen. It supports dual radio capabilities (BLE and Wi-Fi), which can be managed independently to enable completely offline modes.

---

## 🛠️ Pin Configuration & Hardware Connection

The default wiring assignments are as follows. These pins can be customized dynamically using the **Web Serial Controller** interface (`index.html`) without recompiling.

| Component | Pin | Description |
|---|---|---|
| **BUTTON_PIN** | `GPIO 1` | TTP223 Capacitive Touch Sensor (Active HIGH) |
| **BUZZER_PIN** | `GPIO 2` | Passive Buzzer for acoustic and menu feedback |
| **LED_PIN** | `GPIO 6` | WS2812B Single NeoPixel (RGB State indicator) |
| **I2C_SDA** | `GPIO 20` | Dynamic I2C Data line for SSD1306 / SH1106 |
| **I2C_SCL** | `GPIO 21` | Dynamic I2C Clock line for SSD1306 / SH1106 |

---

## 🎯 Enhanced Features

1. **U8g2 Graphics Engine**: Built using `olikraus/U8g2` display bindings, replacing basic Adafruit drivers with high-fidelity monospace/serif typography fonts and native hardware I2C rendering.
2. **Dynamic Clock & Status Faces**:
   - **Digital Face**: Large hour/minute/second counters displaying active timezone calendars.
   - **Analog Face**: Visual watch dial calculating hour, minute, and second vector ticks dynamically.
   - **Bluetooth Status Face**: Shows active pairing state and connection info. Double-tap to toggle Bluetooth.
   - **WiFi Status Face**: New 4th face showing SSID and connection info. Double-tap to toggle WiFi radio dynamically (defaults ON at boot, does not write state to NVS to prevent wear).
3. **Dual-Stage Inactivity Timers**:
   - **Screensaver Delay**: Activates after idle time (default `30s`, max `90s`) to display a pair of blinking/drifting eyes (preventing OLED screen burn-in).
   - **Sleep Delay**: Activates after continued idle time (default `120s`, min `120s`) transitioning to a sleeping face animation.
   - Tap the capacitive touch sensor once to instantly wake up from any idle/screensaver state.
4. **Instant Touch Feedback**:
   - Dynamic corner dot rendering when the capacitive touch pin is physically HIGH.
   - **Quick-Action Submenus**: Single tapping instantly executes highlighted script injections, while double tapping cycles selection down.
   - **Optimized Latency**: Double-tap detection window reduced to `150ms` for faster single-click response times.
   - **Short Scroll Beeps**: Buzzer scroll feed tones shortened to `5ms` for a snappier, responsive menu feel.
5. **Pre-Loaded Pentest Audit Scripts**:
   - Notepad proof-of-concept typer.
   - Administrative local user addition.
   - PowerShell payload execute command sequence.
   - Windows Defender Firewall disable utility.
6. **Customizable NeoPixel LED Modes**: 
   - Static color pickers, breathing glow, rainbow color cycle, normal BLE status signaling, or complete LED shutdown. Includes dynamic brightness scaling in C++ saved directly in NVS.
7. **Separate Web Interfaces**:
   - **Local Setup Companion (`index.html`)**: Run locally in the user's browser for flashing firmware over Web Serial and configuring basic display I2C pins. Forces web controller disconnection to ensure smooth flashing.
   - **Lightweight Hosted Dashboard (`dashboard.html`)**: Hosted directly on the ESP32 chip (accessible via `http://olokeys.local/` on Wi-Fi). Highly optimized and mobile-responsive (main control tabs at the top, compact mobile buttons, larger preset dots, styled color swatches). Contains security login popup on refresh.

---

## 💻 Compilation and Installation Guide

### Prerequisites
- Install **VS Code** with the **PlatformIO** extension.
- Connect the ESP32-C3 Super Mini directly to your computer using a USB-C data cable.

### Build & Upload Firmware
1. Open the folder in VS Code.
2. PlatformIO will automatically read `platformio.ini` and download the dependencies (`NimBLE-Arduino`, `U8g2`, `Adafruit NeoPixel`).
3. Press **Build** (`Ctrl+Alt+B` or checkmark icon in the footer).
4. Press **Upload** (arrow icon in the footer) to flash the binary over USB.

*Alternatively, compile via the PlatformIO Core CLI:*
```bash
# Build binary
pio run

# Upload binary
pio run --target upload
```
