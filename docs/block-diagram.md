# System Architecture — DRONE AUTO LIGHTS

> **System:** MDL-style high-power LED head for drones  
> **Control:** ESP32 + MPU6050 auto stabilization + 2.4GHz RC manual override  
> **Optical target:** ≥3,500 lumens, recommended 4,500–5,500 lumens  
> **Last Updated:** September 2026

---

## 1. High-Level Architecture

```text
                         ┌─────────────────────────────┐
                         │      2.4GHz RC Transmitter   │
                         │      Manual control sticks   │
                         └──────────────┬──────────────┘
                                        │ PWM / PPM
┌─────────────────────────────┐         ▼
│ Drone / Airframe            │  ┌─────────────────────────────┐
│                             │  │ 6-channel RC receiver        │
│  ┌───────────────────────┐  │  │ CH1 pan / CH2 tilt / CH3   │
│  │ MPU6050 IMU           │──┼──│ dim / CH4 mode / CH5 auto  │
│  │ roll + pitch reference│  │  │ CH6 light on/off           │
│  └───────────┬───────────┘  │  └──────────────┬──────────────┘
│              │ I2C          │                 │ PWM
│              ▼              │                 ▼
│  ┌───────────────────────┐  │  ┌─────────────────────────────┐
│  │ ESP32 Dev Module      │◄─┼──│ Servo power: 5V/6V 5A UBEC  │
│  │ control + safety      │  │  └──────────────┬──────────────┘
│  └───────┬───────┬───────┘  │                 │
│          │       │          │                 ▼
│          │       │          │  ┌─────────────────────────────┐
│          │       └─────────────► Pan servo (20 kg-cm)        │
│          │                     └──────────────┬──────────────┘
│          │                                    ▼
│          │                     ┌─────────────────────────────┐
│          └────────────────────► Tilt servo (20 kg-cm)        │
│                                └──────────────┬──────────────┘
│                                               ▼
│                                ┌─────────────────────────────┐
│                                │ MDL-style modular light head │
│                                │ 50W DC COB LED + reflector   │
│                                │ heatsink + blower + sensor   │
│                                └──────────────┬──────────────┘
│                                               │
│                                ┌─────────────────────────────┐
│                                │ 60W+ boost CC LED driver     │
│                                │ PWM/DIM input from ESP32     │
│                                └──────────────┬──────────────┘
│                                               │
│                                ┌─────────────────────────────┐
│                                │ 4S LiPo 14.8V 2200mAh 30C+  │
│                                │ fuse + XT60 + common GND     │
│                                └─────────────────────────────┘
└─────────────────────────────┘
```

---

## 2. Operating Modes

| Mode | Behavior | Input |
|------|----------|-------|
| **Manual** | RC sticks directly command pan, tilt, brightness, and on/off | RC receiver |
| **Auto Stabilize** | MPU6050 roll/pitch data compensates the beam direction | MPU6050 + RC trim |
| **Auto Scan** | ESP32 runs a programmed pan/tilt search pattern | ESP32 |
| **Thermal Protect** | Fan turns on, LED dims, then shuts off if heatsink is too hot | DS18B20/NTC |
| **Flight Safety** | LED can be disabled by RC switch; servo limits prevent mechanical binding | RC CH6 + firmware limits |

> **Important:** MPU6050 measures acceleration and angular rate. It can estimate roll and pitch, but it does **not** provide absolute yaw/heading. Add a QMC5883L/HMC5883L compass if the beam must hold a compass direction.

---

## 3. Component Responsibilities

| Component | Responsibility |
|-----------|----------------|
| ESP32 | Reads IMU and RC channels, computes servo targets, controls LED driver, manages thermal safety |
| MPU6050 | Provides roll/pitch motion reference for auto stabilization |
| RC receiver | Provides manual pan, tilt, brightness, mode, and light-enable commands |
| DS3218/DS3225 servos | Move the heavy LED head; MG90S is not suitable for this payload |
| 50W COB LED | Primary high-lumen light source |
| Boost constant-current driver | Converts 4S battery voltage to the LED’s required voltage/current |
| Heatsink + fan | Keeps the COB LED within safe temperature |
| 4S LiPo | Main energy source |
| UBEC + buck regulators | Isolated power rails for servos, receiver, ESP32, and fan |

---

## 4. Electrical Pin Map

| ESP32 GPIO | Function | Connected To | Signal / Rail |
|------------|----------|--------------|---------------|
| GPIO 21 | I2C SDA | MPU6050 SDA | 3.3V logic |
| GPIO 22 | I2C SCL | MPU6050 SCL | 3.3V logic |
| GPIO 25 | Tilt servo PWM | Tilt servo signal | 5V/6V logic |
| GPIO 26 | Pan servo PWM | Pan servo signal | 5V/6V logic |
| GPIO 27 | LED PWM/DIM | LED driver DIM or MOSFET gate | 3.3V PWM |
| GPIO 32 | Thermal sensor | DS18B20 data / NTC divider | 3.3V |
| GPIO 33 | Cooling fan | Fan enable/PWM | 5V |
| GPIO 34 | Battery ADC | Voltage divider | ADC input only |
| GPIO 18 | RC CH1 | Pan command | PWM input |
| GPIO 19 | RC CH2 | Tilt command | PWM input |
| GPIO 23 | RC CH3 | Brightness command | PWM input |
| GPIO 5 | RC CH4 | Mode command | PWM input |
| GPIO 17 | RC CH5 | Auto/manual switch | PWM input |
| GPIO 16 | RC CH6 | Light enable switch | PWM input |
| GPIO 2 | Status LED | On-board LED | Status output |

### Power Rails

| Rail | Source | Loads |
|------|--------|-------|
| 14.8V nominal / 16.8V full | 4S LiPo | LED driver input |
| 32–36V constant current | Boost LED driver | 50W COB LED |
| 6V | 5V/6V UBEC | Pan/tilt servos, RC receiver |
| 5V | Regulator | Cooling fan and auxiliary modules |
| 3.3V | Buck regulator | ESP32, MPU6050, optional compass |
| GND | Common | All modules and driver |

---

## 5. Mechanical Design

### MDL-Style Modular Light Head

```text
             ┌─────────────────────────────┐
             │ Reflector / projector lens   │
             └──────────────┬──────────────┘
                            ▼
             ┌─────────────────────────────┐
             │ 50W DC COB LED module        │
             └──────────────┬──────────────┘
                            ▼
             ┌─────────────────────────────┐
             │ Thermal paste / pad          │
             └──────────────┬──────────────┘
                            ▼
             ┌─────────────────────────────┐
             │ Aluminum heatsink + 5V fan   │
             └──────────────┬──────────────┘
                            ▼
             ┌─────────────────────────────┐
             │ Tilt servo mount             │
             └──────────────┬──────────────┘
                            ▼
             ┌─────────────────────────────┐
             │ Pan servo / drone base mount │
             └─────────────────────────────┘
```

### Payload Target

| Item | Estimated Mass |
|------|----------------|
| COB LED + reflector | 80–120g |
| Heatsink + fan | 250–450g |
| Servos and bracket | 150–250g |
| Wiring and enclosure | 80–150g |
| **Total light-head payload** | **~560–970g** |

The exact drone must be checked for payload capacity and center-of-gravity limits before flight. If the drone cannot carry this mass, reduce LED power or use a larger airframe.

---

## 6. Optical Design

| Design Choice | Reason |
|---------------|--------|
| White COB LED, 5000–6500K | Highest practical lumen output for the price |
| 50W DC module | Meets the ≥3,500-lumen target when driven correctly |
| 15–30° reflector or lens | Concentrates light into a useful drone beam |
| Constant-current driver | Prevents LED current runaway and premature failure |
| Active cooling | Required for sustained full-power operation |
| PWM dimming | Allows 0–100% brightness control from ESP32 |

Lumens describe total light output. A reflector does not create more lumens, but it increases **lux** and useful throw by concentrating the beam.

---

## 7. Safety and Protection

1. Install a 10A fuse close to the LiPo positive terminal.
2. Use XT60 connectors for the main battery lead.
3. Never connect the COB LED directly to the battery.
4. Use a constant-current driver matched to the LED’s forward voltage and current.
5. Common all grounds, but keep high-current LED wiring away from I2C and servo signal wires.
6. Add thermal shutdown at approximately 75°C heatsink temperature.
7. Start with low LED current and verify temperature before full-power operation.
8. Perform a tethered or stand test before flight.
9. Verify the drone’s payload, balance, and failsafe behavior.

---

## 8. File Structure

```text
DRONE_AUTO_LIGHTS/
├── README.md
├── docs/
│   ├── bom.md
│   ├── block-diagram.md
│   ├── firmware.md
│   └── setup.md
├── firmware/
│   └── drone_auto_lights.ino
└── wiring/
    └── drone_auto_lights.ckt
```

---

## Version History

| Date | Version | Changes |
|------|---------|---------|
| September 2026 | 1.1 | Replaced low-power NeoPixel concept with MDL-style 50W COB head, 4S power system, RC override, MPU6050 stabilization, thermal protection, and ≥3,500-lumen requirement |
