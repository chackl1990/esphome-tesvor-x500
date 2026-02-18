# Tesvor X500 ESPHome (UART)

This repository contains an ESPHome configuration (`tesvor_x500.yaml`) to control and monitor a Tesvor X500 robot vacuum via UART using an ESP32.

Reference / inspiration:
- https://github.com/johkn/tesvor-x500-esphome-vacuum

## 🤖 ProjectGizmo

Our robot’s name is **Gizmo** — lovingly chosen by my girlfriend, who grew quite attached to this little guy.

When the original **Weback cloud service** was discontinued, Gizmo was replaced with a newer Tesvor model **Herbert**.  
Unfortunately, **Herbert** didn’t even survive his warranty period.  

Due to my limited patience for buying yet another robot, testing it, and getting annoyed about what doesn’t work — I decided to take a different path:

Instead of buying another robot i got the old Gizmo…  
and I gave him a **new brain**.

Thanks to excellent documentation found online, a lot of tinkering, many personal optimizations, and some help from AI, Gizmo is now smarter than ever — fully local, cloud-free, and integrated into Home Assistant.

What started as a discontinued cloud device is now:

- 💡 Fully local
- 🧠 Running on an ESP32
- 🏠 integrated into Home Assistant
- ❤️ Officially back in service

ProjectGizmo is proof that sometimes the best upgrade isn’t buying new hardware —  
it’s giving old hardware a second life.

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
- **TX = GPIO17**

If your wiring differs, adjust the `uart:` pins in `tesvor_x500.yaml`.

## Features

- Vacuum state reported via UART (e.g., cleaning, docked, returning, idle, hibernated, error)
- Battery level sensor
- Error text sensor with basic error decoding
- Buttons for common actions (start modes, stop, go charge, movement)
- Fan speed selection (Normal / Strong)

## Usage

1. Copy `tesvor_x500.yaml` into your ESPHome project.
2. Fill in your secrets (`wifi_ssid`, `wifi_password`, `wifi_fallback`, `api_key`, `ota_password`) in `secrets.yaml`.
3. Compile and flash as usual via ESPHome.
4. Add the device to Home Assistant via the ESPHome integration.

## Sensors
- `sensor.vacuum_state` — **Vacuum State** (text sensor reporting current robot state: cleaning, idle, returning, docked, hibernated, error)  
- `sensor.error_state` — **Error State** (text sensor reporting detailed error description)  
- `sensor.battery` — **Battery** (battery level in %)  
- `sensor.uptime` — **Uptime** (time since boot)  
- `sensor.wifi_signal` — **WiFi Signal** (Wi-Fi RSSI value)  

## Select / Fan Control
- `select.fan_speed` — **Fan Speed** (options: Normal / Strong)  

## Buttons (Actions / Commands)
- `button.smart_cleaning` — **Smart Cleaning**  
- `button.spot_cleaning` — **Spot Cleaning**  
- `button.edge_cleaning` — **Edge Cleaning**  
- `button.stop` — **Stop**  
- `button.go_charge` — **Go Charge**  
- `button.move_front` — **Move Front**  
- `button.move_back` — **Move Back**  
- `button.move_left` — **Move Left**  
- `button.move_right` — **Move Right**  
- `button.reboot` — **Reboot**  

## Disclaimer

This project is provided as-is. You are working with hardware modifications and low-voltage power inside a consumer device—proceed carefully and at your own risk.
