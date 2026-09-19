# Assembly & Setup Guide — DRONE AUTO LIGHTS

> **Project:** MDL-style high-power pan-tilt LED head for drones  
> **Primary light:** 50W DC COB LED, ≥3,500 lumens  
> **Last Updated:** September 2026

---

## 1. Safety First

This build uses a 4S LiPo battery and a high-current LED driver.

- Use a proper LiPo balance charger.
- Install a 10A fuse close to the battery positive terminal.
- Never short the LiPo or connect the COB LED directly to the battery.
- Use a constant-current driver matched to the LED’s forward voltage and current.
- The COB LED and heatsink can become hot enough to cause burns.
- Test the light head on a non-flammable surface with airflow.
- Do not fly until the payload, center of gravity, failsafe, and thermal behavior are verified.

---

## 2. Tools and Consumables

| Tool / Consumable | Purpose |
|-------------------|---------|
| 60W soldering iron | High-current wiring and connectors |
| Solder, heat shrink, flux | Reliable insulated joints |
| Wire stripper/cutter | 18AWG power and 22AWG signal wires |
| Digital multimeter | Voltage, current, continuity, and polarity checks |
| Precision screwdriver set | Servo and bracket assembly |
| Thermal paste/pads | LED-to-heatsink heat transfer |
| Zip ties and strain relief | Vibration-resistant cable routing |
| Non-flammable test surface | Full-power LED testing |

---

## 3. Mechanical Assembly

### Step 1 — Build the MDL-Style Light Head

1. Mount the 50W DC COB LED to the aluminum heatsink.
2. Apply a thin, even layer of thermal paste or a correctly sized thermal pad.
3. Tighten the LED mounting screws evenly; do not overtighten the ceramic substrate.
4. Mount the 5V blower fan so air flows through the heatsink fins.
5. Mount the reflector or 15–30° projector lens in front of the COB LED.
6. Attach the DS18B20/NTC thermal sensor to the heatsink near the LED.
7. Secure the completed head to the tilt bracket.

### Step 2 — Install the Servos

1. Use two DS3218/DS3225 metal-gear servos, not MG90S micro servos.
2. Center both servos before installing the horns.
3. Mount the pan servo to the drone/base plate.
4. Mount the tilt servo to the pan arm.
5. Install the light head on the tilt arm.
6. Move the head slowly through the full range and check for binding.

### Step 3 — Mount the Electronics

1. Mount the ESP32, MPU6050, UBEC, buck regulator, and receiver inside the enclosure.
2. Use vibration isolation under the MPU6050 and ESP32.
3. Keep the high-current LED wires away from I2C and RC signal wires.
4. Add strain relief at every cable entry.
5. Keep the battery accessible for inspection and removal.

---

## 4. Electrical Wiring

### 4.1 Main Power

```text
4S LiPo + ── 10A fuse ── XT60 ── LED driver input
4S LiPo - ───────────── XT60 ── LED driver input / common GND
```

### 4.2 LED Driver

```text
LED driver OUT+ ── COB LED anode
LED driver OUT- ── COB LED cathode
LED driver DIM  ── ESP32 GPIO 27
LED driver GND  ── Battery negative / common GND
```

Confirm the driver’s output polarity and dimming interface from its datasheet before connecting the LED.

### 4.3 Servo and Control Power

```text
5V/6V UBEC + ── Pan servo red
             ├── Tilt servo red
             └── RC receiver VCC

UBEC GND ──── Servo brown/black wires
             └── RC receiver GND

5V regulator ── Cooling fan
3.3V regulator ── ESP32 + MPU6050
```

### 4.4 Signal Wiring

| Signal | ESP32 GPIO | Destination |
|--------|------------|-------------|
| MPU6050 SDA | GPIO 21 | MPU6050 SDA |
| MPU6050 SCL | GPIO 22 | MPU6050 SCL |
| Pan servo PWM | GPIO 26 | Pan servo signal |
| Tilt servo PWM | GPIO 25 | Tilt servo signal |
| LED DIM | GPIO 27 | Driver DIM/PWM input |
| Fan control | GPIO 33 | Fan enable/PWM |
| Thermal sensor | GPIO 32 | DS18B20/NTC |
| Battery ADC | GPIO 34 | Divider output |
| RC CH1–CH6 | GPIO 18/19/23/5/17/16 | Receiver PWM outputs |

### 4.5 Common Ground

Connect all grounds together:

- LiPo negative
- LED driver input/output reference
- UBEC negative
- 5V regulator negative
- 3.3V regulator negative
- ESP32 GND
- MPU6050 GND
- RC receiver GND
- Servo GND

---

## 5. First Power-Up

1. Remove the propellers or secure the drone on a test stand.
2. Set the RC transmitter sticks to neutral.
3. Set LED brightness to zero and master LED enable to OFF.
4. Connect the 4S LiPo.
5. Measure the UBEC output: 5V or 6V as configured.
6. Measure the ESP32 rail: 3.3V.
7. Verify the MPU6050 is detected on I2C.
8. Verify all RC channels move in the expected direction.
9. Test pan and tilt at low speed.
10. Enable the LED at 10% and measure driver current.
11. Increase to 25%, 50%, and 100% only after temperature checks.

---

## 6. Thermal Test

Run the LED on a stand with the fan installed:

| Test | Minimum Requirement |
|------|---------------------|
| 10 minutes at 25% | Stable temperature, no driver faults |
| 10 minutes at 50% | Fan running, no thermal shutdown |
| 10 minutes at 100% | Temperature below driver/LED limits |
| Thermal fault simulation | LED dims and shuts off as configured |

If the heatsink is too hot to touch, stop immediately and improve the thermal path. Do not rely on touch alone for final validation; use the installed temperature sensor and a multimeter/thermal probe.

---

## 7. Calibration

### Servo Center

1. Command 1500µs to both servos.
2. Align the horns to the mechanical center.
3. Tighten the horn screws.
4. Re-test the full range.

### MPU6050

1. Place the drone on a level surface.
2. Record accelerometer and gyroscope offsets.
3. Verify roll and pitch return near zero.
4. Add vibration isolation if readings are noisy.

### RC

1. Confirm 1000–2000µs or 1000–2100µs pulse range.
2. Set transmitter endpoints and sub-trims.
3. Configure failsafe to neutral and LED OFF.
4. Verify the auto/manual switch and master LED switch.

---

## 8. Flight Preparation

- [ ] Drone payload rating exceeds the complete light-head mass.
- [ ] Center of gravity remains within the drone’s safe range.
- [ ] All screws use thread locker or lock washers where appropriate.
- [ ] Battery is secured with a crash-retaining strap.
- [ ] Propellers are removed during electrical tests.
- [ ] LED beam does not shine into the pilot’s eyes.
- [ ] RC failsafe turns the LED off or selects a safe state.
- [ ] Full-power ground test completed for at least 10 minutes.
- [ ] First flight is low, slow, and tethered or conducted in a safe open area.

---

## 9. Maintenance

| Interval | Check |
|----------|-------|
| Before every flight | Battery, connectors, servo horns, fan, reflector, cable strain relief |
| After every flight | Heat damage, loose screws, LED discoloration, wire chafing |
| Monthly | Servo gear wear, thermal paste condition, battery capacity, RC failsafe |

---

## 10. Troubleshooting

| Problem | Check |
|---------|-------|
| LED below 3,500 lumens | Verify module datasheet, drive current, color temperature, and reflector |
| LED shuts off quickly | Check heatsink, fan airflow, thermal sensor placement, and driver current |
| Servos cannot lift head | Use 20 kg-cm metal-gear servos and reduce head weight |
| ESP32 resets | Check brownout, common ground, battery sag, and regulator capacity |
| MPU6050 unstable | Check 3.3V power, I2C pull-ups, vibration isolation, and calibration |
| RC commands reversed | Reverse channels in transmitter or firmware mapping |
| Short flight time | Reduce LED brightness, use higher-capacity battery, or reduce payload |

---

## Version History

| Date | Version | Changes |
|------|---------|---------|
| September 2026 | 1.1 | Rewritten for 50W COB MDL-style light head, 4S LiPo, RC override, MPU6050 stabilization, and thermal-safe assembly |
