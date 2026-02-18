# Tesvor X500 ESPHome (UART)

This repository contains an ESPHome configuration (`tesvor_x500.yaml`) to control and monitor a Tesvor X500 robot vacuum via UART using an ESP32.

Reference / inspiration:
- https://github.com/johkn/tesvor-x500-esphome-vacuum

## Background

The **Tesvor X500 (late 2018 models)** was originally sold with **Weback cloud integration**.  
At some point, the Weback service was discontinued, which rendered the cloud features unusable.

As a result, many Tesvor X500 units became “cloud-less” and lost their smart functionality.

This project replaces the original Weback-based control with a **local ESPHome integration**, allowing the vacuum to be controlled via **Home Assistant** without any cloud dependency.

### Why this matters

- 100% local control  
- No external servers required  
- No account or cloud registration  
- Long-term independence from discontinued services  

This effectively gives the Tesvor X500 a second life as a fully local smart device.

## Hardware

- **Robot vacuum:** Tesvor X500
- **ESP32 board:** `nodemcu-32s`
- **Power:** The X500 provides **3.3V**, which can be used to power the ESP32 board (3V3 + GND).

### Notes
- This setup assumes you removed the original Tesvor control board and connected the vacuum’s UART lines to an ESP32.
- A common approach is to cut the original connector cable and attach jumper terminals/clamps, then wire those to the ESP32.

## Wiring

Turn your robot so that it faces to front - so that you have the connection port to the esp on the right side.

The Connection Pins from left to right (in my case - check this with an meter!!!)

**1 → Black → GND → ESP32: GND**

**2 → Yellow → UART TX (Vacuum) → ESP32: UART RX GPIO16**

**3 → Green → UART RX (Vacuum) → ESP32: UART TX GPIO17**

**4 → Red → 3,3V → ESP32: 3,3V (should not use a voltage converter)**

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
