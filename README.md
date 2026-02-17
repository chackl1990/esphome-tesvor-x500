# Tesvor X500 ESPHome (UART)

This repository contains an ESPHome configuration (`tesvor_x500.yaml`) to control and monitor a Tesvor X500 robot vacuum via UART using an ESP32.

Reference / inspiration:
- https://github.com/johkn/tesvor-x500-esphome-vacuum

## Hardware

- **Robot vacuum:** Tesvor X500
- **ESP32 board:** `nodemcu-32s`
- **Power:** The X500 provides **3.3V**, which can be used to power the ESP32 board (3V3 + GND).

### Notes (general)
- This setup assumes you removed the original Tesvor control board and connected the vacuum’s UART lines to an ESP32.
- A common approach is to cut the original connector cable and attach jumper terminals/clamps, then wire those to the ESP32.

## Wiring (generic)

Typical connections:

- **GND (Vacuum) → GND (ESP32)**
- **3.3V (Vacuum) → 3V3 (ESP32)**
- **UART TX (Vacuum) → UART RX (ESP32)**
- **UART RX (Vacuum) → UART TX (ESP32)**

> ⚠️ UART is 3.3V TTL. Do not use 5V UART adapters.

### ESP32 UART pins
This repo’s YAML is intended for a NodeMCU-32S and uses:
- **RX = GPIO16**
- **TX = GPIO15**

If your wiring differs, adjust the `uart:` pins in `tesvor_x500.yaml`.

## Features

- Vacuum state reported via UART (e.g., cleaning, docked, returning, idle, hibernated, error)
- Battery level sensor
- Error text sensor with basic error decoding
- Buttons for common actions (start modes, stop, go charge, movement)
- Fan speed selection (Normal / Strong)

## Usage

1. Copy `tesvor_x500.yaml` into your ESPHome project.
2. Fill in your Wi-Fi secrets (`wifi_ssid`, `wifi_password`) in `secrets.yaml`.
3. Compile and flash as usual via ESPHome.
4. Add the device to Home Assistant via the ESPHome integration.

## Disclaimer

This project is provided as-is. You are working with hardware modifications and low-voltage power inside a consumer device—proceed carefully and at your own risk.
