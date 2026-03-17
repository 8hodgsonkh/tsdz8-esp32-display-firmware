# TSDZ8 ESP32 Display Firmware

Custom touchscreen display firmware for the **TSDZ8 mid-drive e-bike motor** running VESC firmware, built for the **Waveshare ESP32-S3-Touch-LCD-7** (800×480, 7" capacitive touch).

Connects to a VESC controller (MakerX GO-FOC S100 D100S) via BLE through the onboard NRF52.

## Features

### Display Modes
- **Dashboard** — Speed, battery, motor current, duty cycle, trip info
- **Diagnostics** — Real-time charts, performance metrics, analytics panel
- **Configuration** — Motor/app config editing via touchscreen
- **Navigation** — GPS navigation display (WIP)
- **Torque Test** — Motor torque measurement mode (WIP)

### LED Strip Control (WS2812B)
22 addressable LED effects for underglow:
- Off, Solid Color, Current Reactive, Duty Reactive
- Rainbow, Rainbow Wave, Pulse, Comet, Police, Fire
- Sparkle, Larson Scanner, Breathing, VU Meter, Gradient Fill
- Vibe Mode, Perlin Flow, Plasma, Particles, Matrix
- **Brake Light** — Automatic red on deceleration
- **Accel G-Force** — Color intensity from acceleration

5 color schemes: Cool, Warm, Rainbow, Ocean, Fire

### Security
- PIN-protected bike lock screen

### Communication
- BLE telemetry from VESC (selective values via bitmask)
- Custom commands for assist level control, LED settings

## Hardware Requirements

| Component | Details |
|---|---|
| Display | [Waveshare ESP32-S3-Touch-LCD-7](https://www.waveshare.com/esp32-s3-touch-lcd-7.htm) |
| Controller | MakerX GO-FOC S100 D100S (or any VESC 6.x with NRF52 BLE) |
| Motor | TSDZ8 mid-drive (or any VESC-compatible motor) |
| LED Strip | WS2812B 144 LEDs/m, connected to GPIO 6 |

## Flashing

### Prerequisites
- [esptool.py](https://github.com/espressif/esptool) (`pip install esptool`)
- USB-C cable connected to the ESP32-S3

### Flash Command
```bash
esptool.py --chip esp32s3 -b 460800 \
  --before default-reset --after hard-reset \
  write_flash --flash_mode dio --flash_size 16MB --flash_freq 80m \
  0x0 firmware/bootloader.bin \
  0x8000 firmware/partition-table.bin \
  0x10000 firmware/tsdz8protodisplay.bin
```

### Flash Addresses
| File | Address |
|---|---|
| `bootloader.bin` | `0x0` |
| `partition-table.bin` | `0x8000` |
| `tsdz8protodisplay.bin` | `0x10000` |

## Build From Source

If you want to build from source instead:

```bash
# Clone the source repo
git clone https://github.com/8hodgsonkh/VESC-ESP32-Prototype-Display-TSDZ8.git

# Install ESP-IDF v6.0
# See: https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/get-started/

# Build
source ~/esp/esp-idf/export.sh
export IDF_TARGET=esp32s3
idf.py build

# Flash
idf.py flash
```

## Technical Details

- **Framework**: ESP-IDF 6.0 + LVGL 9.0
- **LVGL heap**: Routed through PSRAM (8MB octal @ 80MHz) for stability
- **Internal SRAM**: ~144KB used / 342KB available (198KB free)
- **Binary size**: ~1MB (93% flash free)
- **BLE stack**: NimBLE

## License

This project is provided as-is for personal/educational use with TSDZ8 e-bike builds.

## Source Code

Full source: [VESC-ESP32-Prototype-Display-TSDZ8](https://github.com/8hodgsonkh/VESC-ESP32-Prototype-Display-TSDZ8)
