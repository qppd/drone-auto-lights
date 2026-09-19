# BOM Validation Report - DRONE AUTO LIGHTS

## Validation Date: October 2024
## Project: MDL-style high-power LED drone light system
## Target: ≥3,500 lumens, ESP32 + MPU6050 + RC control

## 1. Validation Methodology

### 1.1. Price Verification Strategy
- **Search Links**: All BOM links lead to search pages, not specific products
- **Availability Check**: Search links should be updated to specific product pages when possible
- **Price Accuracy**: Current ₱ prices are estimates from September 2026 - need current verification
- **Profitability**: Added analysis for solo engineer (3D printing, assembly, coding, soldering)

### 1.2. Component Categories
1. Core Electronics & Power Components
2. MDL-Style Light Head Components  
3. Prototyping & Wiring Components
4. Enclosure & Mounting Components
5. Optional Components

## 2. Critical Components Analysis

### 2.1. 50W COB LED Module - MOST CRITICAL
**Current BOM:** ₱180, 32-36V, 1.2-1.5A
**Requirements:** ≥3,500 lumens minimum, 4,500-5,500 recommended
**Validation Status:** ❌ Search link only - needs specific product link

**Issues Identified:**
- Price seems low for high-quality 50W COB LED
- No lumens specification in search link
- Must be DC, not AC type
- Needs thermal management verification

### 2.2. ESP32 Dev Module
**Current BOM:** ₱250, Makerlab search link
**Validation Status:** ⚠️ Search link - reasonable price range

### 2.3. MPU6050 IMU
**Current BOM:** ₱120, Makerlab search link
**Validation Status:** ⚠️ Search link - reasonable price

### 2.4. DS3218/DS3225 Metal Gear Servos (2x)
**Current BOM:** ₱300 each, ₱600 total
**Validation Status:** ❌ Search link only - needs verification

**Issues:**
- Critical for pan-tilt mechanism
- Must be 20 kg-cm torque rating
- Price seems reasonable for quality servos

### 2.5. 4S LiPo Battery
**Current BOM:** ₱650, 14.8V 2200mAh 30C+
**Validation Status:** ❌ Search link only - needs verification

**Safety Considerations:**
- Must have proper discharge rating (30C+)
- Needs balance charger (₱550 in BOM)
- Requires XT60 connectors and fuse protection

## 3. Price Verification Findings

### 3.1. Core Components Price Assessment
| Component | BOM Price (₱) | Market Range (Estimated) | Status |
|-----------|---------------|--------------------------|--------|
| ESP32 Dev Module | 250 | 200-350 | ✅ Reasonable |
| MPU6050 | 120 | 100-150 | ✅ Reasonable |
| Servos (2x) | 600 | 500-800 | ⚠️ Needs verification |
| 50W COB LED | 180 | 150-400 | ⚠️ Low estimate - verify specs |
| LED Driver | 300 | 250-500 | ⚠️ Needs verification |
| 4S LiPo | 650 | 500-800 | ✅ Reasonable |
| RC Transmitter+Receiver | 1,200 | 800-1,500 | ✅ Reasonable |

### 3.2. Potential Price Adjustments
- **50W COB LED**: Likely ₱250-₱400 for quality unit with heat sink
- **LED Driver**: Quality constant-current boost driver may be ₱350-₱500
- **Servos**: DS3218/DS3225 could be ₱350-₱450 each
- **Total Adjustments**: +₱300 to +₱700 for core components

## 4. Availability & Supply Chain

### 4.1. Local Availability (Philippines)
- **Lazada/Shopee**: Most components available
- **Specialist Stores**: Makerlab, e-Gizmo, Cytron for electronics
- **RC Components**: Available from multiple RC hobby stores
- **High-power LEDs**: Need verified sellers with proper specifications

### 4.2. Stock Status Strategy
1. **Prioritize verified sellers** with good ratings
2. **Check stock indicators** on product pages
3. **Have alternative components** identified
4. **Consider wholesale options** for multiple builds

## 5. Profitability Analysis for Solo Engineer

### 5.1. Cost Components
**Hardware Costs (Current BOM):** ~₱5,330
**Potential Adjusted Cost:** ~₱5,800-₱6,000

**Labor Components:**
1. **3D Printing**: Design + printing time
2. **Electronics Assembly**: Soldering, wiring, testing
3. **Firmware Development**: ESP32 coding, RC integration
4. **Mechanical Assembly**: Pan-tilt, cooling system
5. **Testing & Calibration**: Thermal, stabilization, flight testing

### 5.2. Time Investment Estimates
| Task | Hours | Notes |
|------|-------|-------|
| 3D Design & Printing | 15-25 | Custom brackets, enclosures |
| Electronics Assembly | 10-15 | Soldering, wiring, safety checks |
| Firmware Development | 20-30 | ESP32 code, MPU6050, RC control |
| Mechanical Assembly | 8-12 | Pan-tilt, cooling, mounting |
| Testing & Calibration | 10-15 | Thermal, stabilization, flight |
| **Total** | **63-97 hours** | |

### 5.3. Pricing Strategy
**Cost-Plus Pricing Model:**
- **Hardware Cost:** ₱6,000
- **Labor Cost:** 80 hours × ₱150/hour = ₱12,000
- **Overhead (20%):** ₱3,600
- **Total Cost:** ₱21,600

**Market Positioning:**
- **Entry Price:** ₱15,000-₱20,000 (competitive)
- **Premium Price:** ₱25,000-₱30,000 (with features)
- **Solo Engineer Profit:** ₱5,000-₱10,000 per unit

## 6. Action Items for BOM Improvement

### 6.1. Immediate Updates Needed
1. **Replace search links** with specific product pages
2. **Add alternative components** for critical parts
3. **Include specification verification** checklist for each component
4. **Add stock availability indicators**

### 6.2. Long-term Improvements
1. **Create supplier database** with verified links
2. **Develop component validation spreadsheet**
3. **Establish pricing update schedule** (quarterly)
4. **Add batch pricing** for multiple units

## 7. Recommendations

### 7.1. Procurement Strategy
1. **Start with core electronics** from verified suppliers
2. **Validate LED specifications** before purchase
3. **Order samples** of critical components first
4. **Build MVP first** then expand features

### 7.2. Validation Priority
1. **50W COB LED** - verify lumens and thermal specs
2. **LED Driver** - verify constant-current capability
3. **Servos** - verify torque and quality
4. **Battery** - verify discharge rating and safety

### 7.3. Profitability Enhancement
1. **Optimize 3D printing** for efficient production
2. **Develop modular design** for easier assembly
3. **Create firmware templates** for reuse
4. **Establish testing procedures** for quality control

## 8. Conclusion

The current BOM provides a good starting point but requires significant validation before procurement. The most critical issues are:

1. **Missing specific product links** - all components use search links
2. **LED specifications unverified** - lumens and thermal performance crucial
3. **Price estimates may be low** - especially for power components

**Next Steps:**
1. Create specific product links for each component
2. Verify LED specifications with suppliers
3. Update prices based on current market research
4. Develop profitability model for production

---
*Report generated for DRONE AUTO LIGHTS project validation*