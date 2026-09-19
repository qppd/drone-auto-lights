# DRONE AUTO LIGHTS

> **MDL-style high-power pan-tilt LED system for drones**  
> **Target output:** **≥3,500 lumens**, recommended 4,500–5,500 lumens  
> **Control:** ESP32 + MPU6050 auto stabilization + 2.4GHz RC manual override  
> **Status:** Design / BOM phase  
> **Last Updated:** September 2026

---

## 1. Overview

This project is a modular, MDL-style drone light head built around a **50W DC COB LED**, a constant-current boost driver, active cooling, and a two-axis pan-tilt mechanism. The ESP32 reads an MPU6050 for roll/pitch stabilization and reads a 6-channel RC receiver for manual control.

The previous NeoPixel concept is no longer the primary light source. NeoPixels may be used later for low-power indicators, but they cannot meet the required search-light output.

---

## 2. Key Specifications

| Specification | Target |
|---------------|--------|
| Primary LED | 50W white DC COB module |
| Luminous flux | **≥3,500 lumens minimum** |
| Recommended flux | 4,500–5,500 lumens |
| LED drive | Constant-current boost driver, 32–36V, 1.2–1.5A |
| Pan/tilt actuators | 2× DS3218/DS3225 metal-gear servo, 20 kg-cm |
| Stabilization sensor | MPU6050 roll/pitch IMU |
| Manual control | 2.4GHz RC transmitter + 6-channel PWM receiver |
| Main battery | 4S LiPo, 14.8V, 2200mAh, 30C+ |
| Cooling | Aluminum heatsink + 5V blower fan |
| Protection | 10A fuse, thermal sensor, firmware derating/shutdown |
| Estimated full-power runtime | 25–30 minutes, depending on battery and load |

---

## 3. Features

- MDL-style modular light head: LED board, reflector, heatsink, and fan.
- Pan and tilt movement with mechanical limits.
- MPU6050-assisted roll/pitch stabilization.
- Manual RC override for pan, tilt, brightness, mode, and master on/off.
- PWM dimming of the high-power LED driver.
- Thermal fan control and automatic LED derating.
- Battery voltage monitoring.
- RC-loss failsafe.
- Optional compass module for absolute heading hold.

---

## 4. Important Design Notes

### Why the LED changed

A WS2812B/NeoPixel strip is suitable for indicators and color effects, but it is not suitable as the main drone search light. The new design uses a 50W DC COB LED with a reflector or projector lens.

### MPU6050 limitation

The MPU6050 can estimate roll and pitch, but it cannot provide reliable absolute yaw/heading. Add a QMC5883L or HMC5883L compass if the light must maintain a compass direction.

### Payload warning

A 50W COB LED, heatsink, fan, reflector, bracket, and two metal-gear servos can weigh approximately **560–970g**. Confirm the drone’s payload rating and center-of-gravity limits before building or flying.

---

## 5. Documentation

| Document | Description |
|----------|-------------|
| [BOM](./docs/bom.md) | Complete parts list, supplier searches, cost tiers, LED specification, and validation checklist |
| [System Architecture](./docs/block-diagram.md) | Power, control, mechanical, optical, and safety architecture |
| [Firmware](./docs/firmware.md) | ESP32 pin map, RC channel map, MPU6050 processing, LED dimming, and thermal protection |
| [Assembly Guide](./docs/setup.md) | Mechanical assembly, wiring, first power-up, thermal testing, and flight preparation |

---

## 6. System Block Diagram

```text
4S LiPo
   │
   ├─ 10A fuse ── 60W+ boost constant-current driver ── 50W COB LED
   │                         │
   │                         └─ PWM/DIM from ESP32
   │
   ├─ 5V/6V 5A UBEC ── Pan servo + Tilt servo + RC receiver
   │
   └─ 5V / 3.3V regulators ── Fan + ESP32 + MPU6050

RC transmitter ── RC receiver ── PWM inputs ── ESP32
MPU6050 ── I2C ─────────────────────────────── ESP32
ESP32 ── PWM ───────────────────────────────── servos
ESP32 ── thermal sensor + fan + battery ADC ── safety control
```

---

## 7. Quick Start

1. Confirm the drone can carry the estimated light-head payload.
2. Order the parts from `docs/bom.md`.
3. Build and thermally test the light head on a stand.
4. Upload and bench-test the ESP32 firmware.
5. Calibrate the MPU6050 and RC channels.
6. Test failsafe, thermal shutdown, and low-battery behavior.
7. Conduct a tethered or low-altitude test flight in a safe open area.

---

## 8. Version History

| Date | Version | Changes |
|------|---------|---------|
| September 2026 | 1.1 | Replaced NeoPixel concept with MDL-style 50W COB LED head and added MPU6050, RC override, thermal protection, and ≥3,500-lumen requirement |
