# OLO Keystroke Injector User Manual

Welcome to the **OLO Keystroke Injector**! This manual describes how to operate your hardware device, understand its animations/states, toggle features, customize settings, and write custom keystroke payloads.

---

## 🎨 Interactive Navigation Model

The device is controlled with a **single capacitive touch sensor** (TTP223 connected to Pin 1). The interface responds to single taps, double taps, and long presses:

### 1. In Dashboard Screens (Face, Clocks & Status Pages)
- **Single Tap (Click)**:
  - *Idle Face*: Plays a random looking animation (Look Left, Look Right, Look Down).
  - *Clock Screen Faces*: Cycles through the 4 available faces:
    1. **Digital Clock Face**: Displays large hours, minutes, seconds, and dates.
    2. **Analog Clock Face**: Visual clock dial with moving vector hands.
    3. **Bluetooth Status Page**: Displays BLE advertising/connection state.
    4. **WiFi Status Page**: Displays Wi-Fi connection SSID, IP address, and status.
  - *Main Category Menu*: Cycles down to highlight the next category option.
- **Double Tap (Double Click)**:
  - *Idle Face*: Enters the first Time/Status Clock screen.
  - *Clock Screens (Digital & Analog)*: Enters the Keystroke Injector Category Menu.
  - *Bluetooth Status Page*: Toggles the Bluetooth radio ON/OFF dynamically. **This state is persistent across reboots (saved to NVS config store).**
  - *WiFi Status Page*: Toggles the Wi-Fi radio ON/OFF dynamically to make the device offline/online. Note: This state is temporary, defaults to ON on boot, and does not save to NVS to avoid flash memory wear.
  - *Main Category Menu*: Selects or enters the highlighted category.
- **Long Press (Hold for >0.8s)**:
  - *Any Menu / Clock Screen*: Moves backwards up one level in the menu tree.

### 2. In Keystroke Action Submenus (Interactive Selection Model)
- **Single Tap (Click)**: Cycles down to highlight the next option/keystroke in the list.
- **Double Tap (Double Click)**: **Instantly triggers the highlighted action/keystroke injection** (and displays a full-screen typing animation!).
- **Long Press (Hold for >0.8s)**: Exits the submenu and returns to the Main Category Menu.

*Note: Double-tap timing is tuned to a reliable `350ms` window for touch consistency. Interaction beeps are set to a very short `5ms` duration during scrolling.*

---

## 🛌 Screensaver, Sleep & Bluetooth Auto-Disable Modes

The device features built-in inactivity management to save power and protect hardware resources:
1. **Screensaver Stage**: If there is no touch action or activity for the **Screensaver Delay** duration (default `30s`, configurable from `5s` to `90s`), the display transitions from the active page to a pair of drifting blinking/sleeping eyes.
2. **Sleep Stage**: If the device remains idle in the screensaver stage for the **Sleep Delay** duration (default `120s / 2 mins`, configurable from `120s` to `600s`), it transitions to a full sleep state displaying a sleep animation.
   - *Waking up*: Simply tap the touch sensor once. The device wakes up instantly and returns to the Idle Face screen.
3. **Bluetooth Auto-Disable**: If Bluetooth is enabled but remains disconnected (no host device connects) and the device is idle for **2 minutes (120,000ms)**, Bluetooth will automatically toggle OFF and save its state to NVS to save power and improve security. A dual falling-tone beep sounds to notify you.

---

## 🚨 Status Indicators & Audio Feedback

### NeoPixel LED Status (Pin 6)
- **Normal Connection Mode**:
  - 🟢 **Solid Green**: Connected to host device via Bluetooth.
  - 🔴 **Pulsing Red**: Bluetooth advertising (ready to pair).
  - 🟣 **Solid Purple**: Bluetooth radio disabled.
- **Custom LED Modes**:
  - 🎨 **Static Solid**: Constant custom RGB color.
  - 🌈 **Rainbow Cycle**: Cycles through the RGB color wheel at a customizable speed.
  - 💓 **Breathing Glow**: Fades the custom RGB color in and out.
  - ❌ **LED OFF**: Disables the indicator NeoPixel completely.
- **Keystroke Signaling**:
  - ⚪ **White Flash**: Blinks white for 700ms when a keystroke payload is injected.

### Buzzer Sounds (Pin 2)
- 🎵 **Startup Melody**: An ascending chime on successful device booting.
- 🔊 **Quick Click (2200 Hz)**: Tactile click feedback when the touch sensor registers a press.
- 🔊 **Long Press Chime (1400 Hz)**: Low-pitched chime confirming long press registration.
- 🎵 **Success Chirps (2000 Hz + 2500 Hz)**: Ascending double tone indicating keystroke injection completion.
- 🔊 **Error / Warning Tones (800 Hz + 600 Hz)**: Descending double tone indicating BLE keyboard is disconnected when trying to send a payload.
- 🔊 **Bluetooth Auto-Disable Warning**: A dual falling-tone beep sounding at 1200 Hz for 150ms, followed by a 180ms delay, and ending with 800 Hz for 250ms when Bluetooth automatically shuts off.

---

## 📂 System Menu Hierarchy

The display flow consists of five levels:

```
[ Level 0: Animated Idle Face ]
           │ (Double Tap)
           ▼
[ Level 1: Clocks & Status ] (Single Tap cycles: Digital -> Analog -> BLE Status -> WiFi Status)
           │ (Double Tap on clocks to enter Menu, or on status pages to toggle radio)
           ▼
[ Level 2: Keystroke Category Menu ]
     ├── 1. OS Shortcuts  ──────► [ Level 3: Submenu (Lock, Close, Minimize, etc.) ]
     ├── 2. Presenter Mode ─────► [ Level 3: Submenu (Next/Prev Slide, Black/White Screen) ]
     ├── 3. Custom Payload ─────► [ Level 3: Submenu (Run Payload) ]
     ├── 4. Pen-Test Payloads ──► [ Level 3: Submenu (Notepad Demo, Add Admin, PowerShell Exec, etc.) ]
     ├── 5. Settings ──────────► [ Level 3: Submenu (LED, Sound, BLE, Time, Chime, OS, Reset) ]
     ├── 6. FIFA 2026 ─────────► [ Level 3: Submenu (Easter Egg Facts) ]
     └── 7. Exit (Back to Clock)
```

---

## 🔌 Web-Based Management (Hosted Control Center)

When the device Wi-Fi is active, connect to the same network and navigate to **`http://olokeys.local/`** in your browser. 

1. **Dynamic OTP Security Authentication**: Upon loading or refreshing the page, the ESP32 screen automatically transitions to the **Web OTP Login** view. This view displays a temporary **6-digit PIN** and a **30-second countdown progress bar**. You must type this active OTP into the browser login dialog. If the timer expires (30 seconds), the screen generates a new OTP and resets the countdown. Tapping the touch sensor on the hardware dismisses the OTP screen.
2. **Premium Redesigned Layout**: Styled in accordance with OLO Aero dark glassmorphic styling, featuring outfit typography, dark blurred panels, and neon orange borders.
3. **Tab 1: Keystroke Injection (Payloads)**:
   - Contains a quick-filter pill header for category presets (Apps, Settings, Web, Diag, Panels, Admin, Folders, Pranks).
   - Includes a text field for entering dynamic custom keystroke payloads.
   - Buttons: **Trigger Keystroke** (sends payload immediately) and **Save Offline Payload** (saves custom payload to device NVS for local triggers).
4. **Tab 2: NeoPixel LED Options (LED Glow)**:
   - Configure LED Mode (Static, Rainbow, Breathing, Off).
   - Use the custom styled color swatch picker or quick color preset dots to apply customized colors.
5. **Tab 3: Device Settings (Settings)**:
   - Set **Screensaver Delay** slider (clamped at a maximum of `90s` to prevent burn-in).
   - Set **Sleep Delay** slider (clamped at a minimum of `120s`).
   - Customize **Boot Welcome Greeting** text.
   - Customize **BLE Device Name** (defaults to `"OLO Bit"`) and **BLE Manufacturer Name** (defaults to `"Micromaker Labs"`). *Note: Renaming the BLE Device name dynamically overrides the base MAC address on boot to prevent host device name caching issues.*
   - Configure clock details: **Time Format** (12h/24h), **Hourly Chime Beep**, **Timezone Region**, or **Custom POSIX timezone string**.
   - Click **Update Settings** to save configurations to NVS.
6. **Sidebar / Device Controls (Mobile Stacked)**:
   - Monitor real-time system states (uptime, RSSI strength, heap memory) via the **Live Monitor console**.
   - Manage LED Brightness slider (saves to NVS and applies instantly).
   - **Disconnect Dashboard**: Force logging session termination.
   - **Reboot / Reset**: Reboot hardware or wipe memory settings to default values.
   - On mobile viewports, this sidebar section is positioned at the bottom of the page to prioritize primary tabs at the top.
   - Toggles Bluetooth state dynamically with NVS storage saving.
