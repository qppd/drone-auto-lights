# Bill of Materials (BOM) — DRONE AUTO LIGHTS

> **System:** MDL-style modular high-power LED head with ESP32 + MPU6050 auto stabilization and RC manual override  
> **Minimum optical target:** **≥3,500 lumens** at rated LED current; recommended target **4,500–5,500 lumens**  
> **Supplier priority:** Makerlab Electronics for sensors/MCUs → Lazada/Shopee for high-power LED, servo, battery, and RC parts  
> **Prices:** Estimated in Philippine Peso (₱), September 2026

---

## 1. Core Components

| # | Item | Qty | Unit (₱) | Total (₱) | Link |
|---|------|-----|----------|-----------|------|
| 1 | ESP32 Dev Module, 38-pin | 1 | ₱250 | ₱250 | [Makerlab search](https://shopee.ph/search?keyword=esp32%2038pin%20makerlab) |
| 2 | MPU6050 6-axis IMU | 1 | ₱120 | ₱120 | [Makerlab search](https://shopee.ph/search?keyword=mpu6050%20makerlab) |
| 3 | DS3218 / DS3225 metal-gear servo, 20 kg-cm | 2 | ₱300 | ₱600 | [Lazada search](https://www.lazada.com.ph/catalog/?q=DS3218%2020kg%20servo) |
| 4 | Aluminum pan-tilt bracket for 20 kg-cm servos | 1 | ₱350 | ₱350 | [Lazada search](https://www.lazada.com.ph/catalog/?q=aluminum%20pan%20tilt%20bracket%20servo) |
| 5 | 50W white COB LED module, 32–36V, 1.2–1.5A | 1 | ₱180 | ₱180 | [Lazada search](https://www.lazada.com.ph/catalog/?q=50W%20COB%20LED%20module%2032V%2036V%20white) |
| 6 | DC-DC boost constant-current LED driver, 60W+ | 1 | ₱300 | ₱300 | [Lazada search](https://www.lazada.com.ph/catalog/?q=DC%20DC%20boost%20constant%20current%20LED%20driver%2060W) |
| 7 | 4S LiPo battery, 14.8V 2200mAh, 30C+ | 1 | ₱650 | ₱650 | [Lazada search](https://www.lazada.com.ph/catalog/?q=4S%20LiPo%2014.8V%202200mAh%2030C) |
| 8 | LiPo balance charger for 4S | 1 | ₱550 | ₱550 | [Lazada search](https://www.lazada.com.ph/catalog/?q=4S%20LiPo%20balance%20charger) |
| 9 | 5V/6V 5A UBEC / servo power regulator | 1 | ₱150 | ₱150 | [Lazada search](https://www.lazada.com.ph/catalog/?q=5V%206V%205A%20UBEC%20servo) |
| 10 | 5V to 3.3V buck regulator for ESP32 | 1 | ₱50 | ₱50 | [Lazada search](https://www.lazada.com.ph/catalog/?q=5V%203.3V%20buck%20converter%20ESP32) |
| 11 | 2.4GHz RC transmitter + 6-channel PWM receiver | 1 | ₱1,200 | ₱1,200 | [Lazada search](https://www.lazada.com.ph/catalog/?q=2.4GHz%20RC%20transmitter%206%20channel%20receiver%20PWM) |

**Core Subtotal:** **~₱4,400**

---

## 2. MDL-Style Light Head

| # | Item | Qty | Unit (₱) | Total (₱) | Link |
|---|------|-----|----------|-----------|------|
| 12 | Large aluminum heatsink for 50W COB LED | 1 | ₱150 | ₱150 | [Lazada search](https://www.lazada.com.ph/catalog/?q=aluminum%20heatsink%2050W%20LED) |
| 13 | 5V blower fan or radial cooling fan | 1 | ₱100 | ₱100 | [Lazada search](https://www.lazada.com.ph/catalog/?q=5V%20blower%20fan%20cooling) |
| 14 | COB LED reflector or projector lens, 15–30° | 1 | ₱150 | ₱150 | [Lazada search](https://www.lazada.com.ph/catalog/?q=COB%20LED%20reflector%20lens%2050W) |
| 15 | DS18B20 waterproof temperature sensor or NTC thermistor | 1 | ₱50 | ₱50 | [Lazada search](https://www.lazada.com.ph/catalog/?q=DS18B20%20temperature%20sensor) |
| 16 | Thermal paste + thermal pads | 1 | ₱80 | ₱80 | [Lazada search](https://www.lazada.com.ph/catalog/?q=thermal%20paste%20thermal%20pad) |

**Light-Head Subtotal:** **~₱530**

---

## 3. Prototyping, Wiring & Protection

| # | Item | Qty | Unit (₱) | Total (₱) | Link |
|---|------|-----|----------|-----------|------|
| 17 | Perf board or small custom PCB | 1 | ₱50 | ₱50 | [Makerlab search](https://shopee.ph/search?keyword=perf%20board%20makerlab) |
| 18 | 18AWG silicone wire, red/black pair | 2m | ₱80 | ₱160 | [Lazada search](https://www.lazada.com.ph/catalog/?q=18AWG%20silicone%20wire%20red%20black) |
| 19 | 22AWG signal wire / jumper wires | 1 lot | ₱50 | ₱50 | [Makerlab search](https://shopee.ph/search?keyword=jumper%20wire%20makerlab) |
| 20 | XT60 connectors, male/female pairs | 2 | ₱40 | ₱80 | [Lazada search](https://www.lazada.com.ph/catalog/?q=XT60%20connector%20pair) |
| 21 | JST-XH 2-pin and 3-pin connectors | 1 lot | ₱60 | ₱60 | [Makerlab search](https://shopee.ph/search?keyword=JST-XH%20connector%20makerlab) |
| 22 | 10A automotive blade fuse + holder | 1 | ₱50 | ₱50 | [Lazada search](https://www.lazada.com.ph/catalog/?q=10A%20fuse%20holder%20automotive) |
| 23 | Logic-level MOSFET module or LED-driver PWM dimming input | 1 | ₱80 | ₱80 | [Lazada search](https://www.lazada.com.ph/catalog/?q=logic%20level%20MOSFET%20module) |
| 24 | 1000µF/50V electrolytic capacitor | 1 | ₱40 | ₱40 | [Lazada search](https://www.lazada.com.ph/catalog/?q=1000uF%2050V%20capacitor) |
| 25 | 100nF ceramic capacitors | 5 | ₱5 | ₱25 | [Lazada search](https://www.lazada.com.ph/catalog/?q=100nF%20capacitor) |
| 26 | 300Ω resistor for signal-line protection | 1 | ₱10 | ₱10 | [Makerlab search](https://shopee.ph/search?keyword=300%20ohm%20resistor%20makerlab) |

**Wiring Subtotal:** **~₱605**

---

## 4. Enclosure & Mounting

| # | Item | Qty | Unit (₱) | Total (₱) | Link |
|---|------|-----|----------|-----------|------|
| 27 | Lightweight waterproof enclosure, 120×80×50mm or larger | 1 | ₱180 | ₱180 | [Lazada search](https://www.lazada.com.ph/catalog/?q=waterproof%20project%20enclosure%20120x80x50mm) |
| 28 | Aluminum standoffs, M3/M4 | 1 set | ₱80 | ₱80 | [Lazada search](https://www.lazada.com.ph/catalog/?q=M3%20M4%20aluminum%20standoffs) |
| 29 | M3/M4 screws, nuts, and lock washers | 1 set | ₱80 | ₱80 | [Lazada search](https://www.lazada.com.ph/catalog/?q=M3%20M4%20screws%20nuts) |
| 30 | Vibration isolation pads / foam tape | 1 | ₱60 | ₱60 | [Lazada search](https://www.lazada.com.ph/catalog/?q=vibration%20damping%20foam%20pad) |
| 31 | Cable glands / strain relief | 2 | ₱40 | ₱80 | [Lazada search](https://www.lazada.com.ph/catalog/?q=cable%20gland%20strain%20relief) |

**Enclosure Subtotal:** **~₱480**

---

## 5. Optional Heading Sensor

> MPU6050 provides roll and pitch stabilization, but **does not provide absolute yaw/heading**. Add this if the light must maintain a compass direction.

| # | Item | Qty | Unit (₱) | Total (₱) | Link |
|---|------|-----|----------|-----------|------|
| 32 | QMC5883L / HMC5883L digital compass module | 1 | ₱100 | ₱100 | [Lazada search](https://www.lazada.com.ph/catalog/?q=QMC5883L%20compass%20module) |

**Optional Subtotal:** **~₱100**

---

## 6. One-Time Tools

| # | Item | Qty | Unit (₱) | Total (₱) | Note |
|---|------|-----|----------|-----------|------|
| 33 | Soldering iron kit, 60W | 1 | ₱300 | ₱300 | High-current joints |
| 34 | Wire stripper/cutter | 1 | ₱120 | ₱120 | 18–22AWG wire |
| 35 | Digital multimeter | 1 | ₱250 | ₱250 | Verify voltage/current |
| 36 | Precision screwdriver set | 1 | ₱100 | ₱100 | Servo/bracket assembly |
| 37 | Heat gun / lighter for heat shrink | 1 | ₱100 | ₱100 | Insulation |

**Tools Subtotal:** **~₱870** *(one-time, reusable)*

---

## 7. Cost Summary

| Tier | Included | Estimated Cost | Notes |
|------|----------|----------------|-------|
| **MVP / Bench Build** | ESP32, MPU6050, servos, 50W COB, driver, battery, charger, cooling, basic wiring | **~₱4,130** | Auto stabilization and brightness control; no RC transmitter |
| **Standard Drone Build** | MVP + RC transmitter/receiver + enclosure + protection + light head | **~₱5,330** | Recommended build |
| **Complete Build** | Standard + compass + spare battery + better reflector/lens | **~₱6,200–₱6,800** | Heading hold and longer field operation |
| **Tools** | One-time tools | **~₱870** | Excluded from build totals |

> **Price note:** These are planning estimates. Verify actual seller price, shipping, battery discharge rating, and LED datasheet before ordering.

---

## 8. Required LED Specification

The primary LED must be a **high-power white COB module**, not a NeoPixel strip.

| Parameter | Minimum Requirement | Recommended |
|-----------|--------------------|-------------|
| Rated power | 50W | 50W |
| Rated voltage | 32–36V DC | 32–36V DC |
| Rated current | 1.2–1.5A | 1.5A |
| Luminous flux | **≥3,500 lm** | **4,500–5,500 lm** |
| Color temperature | 5000–6500K | 6000–6500K for maximum lumens |
| CRI | ≥70 | ≥80 if color accuracy matters |
| Beam control | Reflector/lens required | 15–30° projector optics |
| Cooling | Heatsink + active fan | Heatsink + blower + thermal cutoff |

### Purchase Rule

Do **not** buy a generic “50W COB LED” unless the listing or datasheet states luminous flux. Ask the seller for:

1. Lumens at rated current.
2. Forward voltage range.
3. Recommended drive current.
4. Thermal resistance or maximum junction temperature.
5. Whether the module is DC or AC. **Use DC only for this drone design.**

---

## 9. Power Architecture

```text
4S LiPo 14.8V 2200mAh
        │
        ├── 10A fuse ── 60W+ boost constant-current driver ── 50W COB LED
        │                         │
        │                         └── PWM/DIM input from ESP32
        │
        ├── 5V/6V 5A UBEC ── Pan servo + Tilt servo + RC receiver
        │
        ├── 5V buck/regulator ── Cooling fan
        │
        └── 5V→3.3V regulator ── ESP32 + MPU6050 + optional compass
```

All grounds must be common: battery, LED driver, UBEC, ESP32, receiver, and servos.

### Estimated Runtime

| Mode | Approx. Load | Estimated Runtime on 4S 2200mAh |
|------|--------------|---------------------------------|
| LED at 100%, servos moving | 55–60W | 25–30 minutes |
| LED at 50%, occasional servo movement | 30–35W | 40–50 minutes |
| LED off, control electronics only | 3–6W | 2–4 hours |

---

## 10. Already Purchased

| Item | Qty | Notes |
|------|-----|-------|
| None recorded | — | Mark any owned parts before purchasing |

---

## 11. Supplier Notes

### Electronics / Sensors
- [Makerlab Electronics on Shopee](https://shopee.ph/makerlabelectronics)
- [e-Gizmo on Shopee](https://shopee.ph/e-gizmo)
- [Cytron Technologies on Shopee](https://shopee.ph/cytrontechnologies)

### High-Power / RC / Battery Parts
- [Lazada Philippines](https://www.lazada.com.ph/)
- Search terms are provided in each table because stock and sellers change frequently.

---

## 12. Validation Checklist

- [ ] LED datasheet/listing confirms **≥3,500 lumens** at rated current.
- [ ] LED module is **DC**, not AC-direct.
- [ ] LED driver is constant-current and rated for at least 60W.
- [ ] Driver output voltage/current matches the LED module.
- [ ] 4S LiPo has a 30C or higher discharge rating.
- [ ] 10A fuse and XT60 connectors are installed on the main battery lead.
- [ ] Servos are metal-gear 20 kg-cm class, not MG90S.
- [ ] Pan-tilt bracket is rated for the complete light-head weight.
- [ ] Heatsink and fan keep the COB LED below its maximum temperature.
- [ ] Thermal sensor and firmware cutoff are installed.
- [ ] MPU6050 is powered from 3.3V, not 5V.
- [ ] RC receiver PWM signals and all grounds are tested before flight.
- [ ] Payload and center of gravity are tested on the actual drone.
- [ ] Full-brightness ground test runs for at least 10 minutes before flight.

---

## Version History

| Date | Version | Changes |
|------|---------|---------|
| September 2026 | 1.1 | Replaced NeoPixel design with MDL-style 50W COB LED head, MPU6050, 20 kg-cm servos, RC override, thermal protection, and ≥3,500-lumen requirement |
