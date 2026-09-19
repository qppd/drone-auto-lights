# Firmware Documentation — DRONE AUTO LIGHTS

> **Platform:** ESP32 Dev Module  
> **Control inputs:** MPU6050 I2C + 6-channel RC PWM receiver  
> **Outputs:** 2× metal-gear servos + PWM-dimmable 50W COB LED driver + cooling fan  
> **Last Updated:** September 2026

---

## 1. Firmware Goals

- Read MPU6050 roll/pitch data for auto stabilization.
- Read RC PWM channels for manual pan, tilt, brightness, mode, and light enable.
- Drive two 20 kg-cm servos within mechanical limits.
- PWM-dim a 50W constant-current LED driver.
- Run fan control and thermal shutdown.
- Monitor battery voltage through a resistor divider.
- Keep all safety limits enforced in firmware, even when RC input is lost.

---

## 2. Pin Definitions

```cpp
// I2C IMU
#define MPU_SDA_PIN        21
#define MPU_SCL_PIN        22

// Servo outputs
#define PAN_SERVO_PIN      26
#define TILT_SERVO_PIN     25

// High-power LED and cooling
#define LED_DIM_PIN        27
#define FAN_PIN            33
#define THERMAL_PIN        32

// Battery monitor
#define BATTERY_ADC_PIN    34

// RC PWM inputs
#define RC_PAN_PIN         18
#define RC_TILT_PIN        19
#define RC_BRIGHT_PIN      23
#define RC_MODE_PIN        5
#define RC_AUTO_PIN        17
#define RC_ENABLE_PIN      16

// Status
#define STATUS_LED_PIN     2
```

### Servo Limits

```cpp
#define PAN_MIN_US         1000
#define PAN_MAX_US         2000
#define TILT_MIN_US        1000
#define TILT_MAX_US        2000

#define PAN_MIN_ANGLE      10
#define PAN_MAX_ANGLE      170
#define TILT_MIN_ANGLE     5
#define TILT_MAX_ANGLE     85
```

### Thermal Limits

| Event | Heatsink Temperature |
|-------|----------------------|
| Fan on | 45°C |
| LED dim to 50% | 60°C |
| LED dim to 20% | 70°C |
| LED off / shutdown | 75°C |

---

## 3. Libraries

| Library | Purpose |
|---------|---------|
| `Wire.h` | MPU6050 I2C communication |
| `ESP32Servo.h` | Servo PWM generation |
| `ledc` / Arduino LEDC API | LED driver PWM dimming |
| `WiFi.h` | Optional diagnostics or future ground-station telemetry |
| MPU6050 library | Register access, calibration, and filtered roll/pitch |

Use a maintained MPU6050 library that supports ESP32 and exposes raw accelerometer/gyroscope data. Do not use a library that blocks for long periods inside the control loop.

---

## 4. Control Loop

```text
setup()
  ├─ initialize Serial
  ├─ initialize I2C and MPU6050
  ├─ attach pan and tilt servos
  ├─ configure LED PWM and fan output
  ├─ configure RC input pins
  ├─ move servos to safe neutral position
  └─ start control loop

loop()
  ├─ read MPU6050
  ├─ read RC PWM channels
  ├─ detect RC loss / invalid pulse width
  ├─ select manual, auto-stabilize, or auto-scan mode
  ├─ calculate safe pan/tilt targets
  ├─ write servo pulse widths
  ├─ calculate LED brightness from RC + thermal limits
  ├─ update fan and thermal protection
  ├─ monitor battery voltage
  └─ update status LED / optional telemetry
```

---

## 5. RC Channel Map

| Channel | RC Input | Firmware Use |
|---------|----------|--------------|
| CH1 | Pan stick | Manual pan target |
| CH2 | Tilt stick | Manual tilt target |
| CH3 | Knob or stick | LED brightness 0–100% |
| CH4 | Switch | Scan pattern or fixed mode |
| CH5 | Switch | Auto/manual selection |
| CH6 | Switch | Master LED enable |

### RC Failsafe

If a pulse is missing, outside 900–2100µs, or unchanged for the configured timeout:

1. Keep servo positions within limits.
2. Move to neutral if the receiver provides no valid signal.
3. Turn the LED off or hold the last safe brightness according to the selected failsafe policy.
4. Indicate fault with the status LED.

---

## 6. MPU6050 Use

### I2C Wiring

| MPU6050 | ESP32 |
|---------|-------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO 21 |
| SCL | GPIO 22 |

### Recommended Processing

1. Calibrate accelerometer and gyroscope offsets on a level surface.
2. Use a complementary filter or DMP-based solution for roll/pitch.
3. Use a low-pass filter to reduce vibration noise.
4. Do not use raw gyro integration alone; it will drift.
5. Treat MPU6050 yaw as unreliable for absolute heading.

### Auto-Stabilize Concept

```text
desired beam angle
    − estimated drone roll/pitch
    = compensated servo target
```

Clamp the result to the mechanical servo limits before writing the servo.

---

## 7. LED Driver Control

### Preferred

Use a constant-current LED driver with a dedicated `DIM` or `PWM` input. Connect ESP32 GPIO 27 to the driver’s dimming input through the driver’s specified interface.

### Alternative

If the driver has no dimming input, switch or enable it through a logic-level MOSFET and use PWM only if the driver supports external PWM dimming. Do not rapidly switch a driver that is not designed for PWM.

### Brightness Calculation

```text
finalBrightness = rcBrightness
                  × thermalDerating
                  × masterEnable
```

| Condition | Thermal Derating |
|-----------|------------------|
| Heatsink <45°C | 100% |
| 45–60°C | 100% with fan on |
| 60–70°C | 50% |
| 70–75°C | 20% |
| ≥75°C | 0% / shutdown |

---

## 8. Battery Monitoring

Use a resistor divider so the ESP32 ADC never sees more than 3.3V.

Example ratio:

```text
4S full voltage: 16.8V
Divider ratio:   1:4 or lower
ADC input:       ≤3.3V
```

Add a 100nF capacitor from ADC input to ground and average multiple readings. Calibrate the conversion factor with a multimeter.

Suggested actions:

| Battery Condition | Action |
|-------------------|--------|
| Normal | Continue operation |
| Low warning | Flash status LED / reduce LED brightness |
| Critical | Turn LED off and move servos to neutral |

---

## 9. Configuration Example

```cpp
struct Config {
  uint16_t panCenterUs = 1500;
  uint16_t tiltCenterUs = 1500;
  uint8_t maxLedPercent = 100;
  uint8_t thermalShutdownC = 75;
  uint16_t rcLossTimeoutMs = 500;
};
```

Store calibration values in NVS or a configuration file only after bench testing. Do not store flight-critical limits as user-adjustable values without bounds checking.

---

## 10. Bench Test Sequence

1. Upload firmware with LED output disabled.
2. Verify MPU6050 I2C address and stable roll/pitch readings.
3. Verify all six RC channels and failsafe behavior.
4. Test pan and tilt through the full range with the light head mounted.
5. Test LED driver at 10%, 25%, 50%, and 100% with a current meter.
6. Run the fan and verify airflow across the heatsink.
7. Heat the head at full power for 10 minutes and verify thermal derating.
8. Test low-battery behavior using a bench supply, not by over-discharging a LiPo.
9. Perform a tethered drone test before free flight.

---

## 11. Troubleshooting

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Servos jitter | Servo rail sag or shared ground issue | Use 5A UBEC, thick wires, common GND |
| ESP32 resets at full LED power | Brownout or ground bounce | Separate regulators, add capacitance, check wiring |
| MPU6050 readings noisy | Drone vibration or wrong filter | Add vibration isolation and filtering |
| LED does not turn on | Wrong driver wiring or no enable signal | Verify constant-current output and DIM interface |
| LED gets hot quickly | No thermal paste, undersized heatsink, or overcurrent | Rebuild thermal path and measure current |
| RC control reversed | Transmitter channel reversal | Reverse channel in transmitter or firmware mapping |
| No absolute heading | MPU6050 limitation | Add QMC5883L/HMC5883L compass |

---

## Version History

| Date | Version | Changes |
|------|---------|---------|
| September 2026 | 1.1 | Documented MPU6050 + RC control, 20 kg-cm servos, 50W COB driver, thermal protection, and battery monitoring |
