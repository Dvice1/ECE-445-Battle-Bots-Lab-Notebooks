# Lab Notebook 2024

## **Lab Notebook Entry**  
**Date:** 2024-05-10  
**Project:** Antweight Battle Bot - Wireless Control Subsystem  

### Objective: Investigate Bluetooth Connectivity Options for Controller Interface

#### Preliminary Research and Controller Compatibility
- Initial attempts to use PS4 controller via Bluetooth proved unsuccessful due to significant latency issues.
- Measured communication delay exceeded our target of <1000 ms, rendering the controller unsuitable for real-time battle bot control.

#### Xbox Controller BLE Investigation
Began researching ESP32 Bluetooth Low Energy (BLE) compatibility with Xbox and PS4 controllers. 
Key considerations:
- Communication protocol requirements
- Latency measurement techniques
- Signal strength and reliability

#### References:
- https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/bluetooth/index.html
- https://answers.microsoft.com/en-us/windows/forum/all/xbox-one-controller-identified-as-bluetooth-le/f6aae298-fd84-4125-8568-137b15121f20

#### Proposed Next Steps:
- Develop BLE communication protocol
- Implement signal strength testing
- Create preliminary interface code
![Alt text](/Screenshot 2024-11-17 175043.png)
---

## **Lab Notebook Entry**  
**Date:** 2024-10-15  
**Project:** Antweight Battle Bot - Wireless Communication Protocol  

### Objective: BLE Communication Protocol Development

#### Research Context
- Investigating Bluetooth Low Energy (BLE) communication protocols for minimal latency control.
- Previous PS4 Bluetooth approach proved ineffective due to high latency.

#### Communication Protocol Design
Key Requirements:
- Latency < 1000 ms
- Reliable data transmission
- Low power consumption
- Xbox controller compatibility

#### Signal Structure Development
Proposed data packet format:
- 8-bit left wheel intensity
- 8-bit right wheel intensity
- 8-bit weapon speed control
- 8-bit system command (including kill switch)

#### Preliminary Code Structure
- Implement BLE advertising
- Create characteristics for control signals
- Develop interrupt-driven response mechanism

#### Challenges Identified:
- Signal interference
- Battery power management
- Consistent signal strength

#### References:
- https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/bluetooth/bt_le.html
- https://bluepad32.readthedocs.io/en/latest/

---

## **Lab Notebook Entry**  
**Date:** 2024-10-21  
**Project:** Antweight Battle Bot - Mechanical Design and Prototyping  

### Objective: Initial CAD Modeling and 3D Printing Preparation

#### CAD Modeling Considerations
- External dimensions: 18 cm × 18 cm × 5 cm
- Wall thickness: 3.5 mm
- Material: PLA/PETG hybrid
- Target weight: <0.91 kg (2 lbs)

#### Weight Calculation
Mass estimation:
- Main chassis (PLA): 150 g
- Wheels (PETG, 2×): 40 g each
- Estimated component weight: Approximately 230 g

#### 3D Printing Preparation
- **Printer:** Carbon X1
- **Materials:**
  - PLA (Density: 1.25 g/cm³)
  - PETG (Density: 1.34 g/cm³)
- Infill density: 35%

#### Design Challenges:
- Minimize weight while maintaining structural integrity
- Create mounting points for electronics
- Ensure protection for critical components

---

## **Lab Notebook Entry**  
**Date:** 2024-11-05  
**Project:** Antweight Battle Bot - Weapon System Design  

### Objective: Horizontal Spinner Blade Optimization

#### Blade Design Calculations
Critical Parameters:
- Material: PETG
- Blade Mass: 0.15 kg ± 0.01 kg
- Blade Radius: 0.09 m ± 0.005 m

#### Kinetic Energy Calculations from Design Document

#### Material Selection Justification
- PETG Advantages:
  - 10% elongation at breaking point
  - 2-3× impact strength vs. PLA
  - Optimal weight-to-strength ratio

#### Testing Considerations:
- Verify structural integrity
- Measure vibrational characteristics
- Validate impact resistance

---

## **Lab Notebook Entry**  
**Date:** 2024-11-15  
**Project:** Antweight Battle Bot - Motor Control System  

### Objective: Motor Controller Hardware Design and Testing

#### Motor Controller Design Considerations
- **Selected Components:**
  - Microcontroller: ESP32
  - Motor Drivers: L293D H-bridge
  - Power Supply: 9V Lithium D-cell

#### Power Delivery Calculations
- **Current Requirements:**
  - Blade motor: 2.5A
  - Drive motors: 1.5A each (peak 2A)
  - Total peak current: 7A

#### Circuit Design Challenges:
- Voltage regulation (3.3V for ESP32)
- PWM signal generation
- Bidirectional motor control

#### Testing Objectives:
- Verify PWM signal generation
- Test motor speed control
- Validate power delivery stability

#### Preliminary Observations:
- Initial tests show promise for precise motor control
- Need to verify heat dissipation and thermal management

---

## **Lab Notebook Entry**  
**Date:** 2024-11-25  
**Project:** Antweight Battle Bot - Power Subsystem Optimization  

### Objective: Battery and Power Management Design

#### Power Budget Analysis
Component Current Requirements:
- Blade Motor: 2.5A
- Drive Motors: 1.5A each (peak 2A)
- ESP32 Microcontroller: 500 mA

#### Total Power Calculation
- **Peak Current Draw:**
  \[ I_{total} = 2.5 + (2 \times 2) + 0.5 = 7 \text{ A} \]
- **Power:**
  \[ P = V \times I_{total} = 9 \times 7 = 63 \text{ W} \]

#### Battery Selection Criteria
Current Solution:
- 9V Lithium D-cell
- Capacity: 1200 mAh

#### Future Optimization Paths
- Alternative Battery Options:
  - 14.8V Lithium Polymer (LiPo)
  - Higher energy density
  - Potential for extended runtime

---

## **Lab Notebook Entry**  
**Date:** 2024-12-03  
**Project:** Antweight Battle Bot - Final Integration Testing  

### Objective: Comprehensive System Validation

#### Subsystem Integration Checklist:
- Wireless Control
- Motor Control
- Power Management
- Mechanical Integrity

#### Performance Metrics Verification:
- **Latency Test Results:**
  - Measured Latency: 850 ms (Target: <1000 ms)
- **Motor Control Precision**
- **Battery Performance**
- **Structural Integrity**

#### Wireless Control Testing:
- BLE Connection Stability
- Signal Strength Measurement

#### Mechanical Stress Test Results:
- Impact Resistance Verification
- Chassis Deformation Measurements
- Component Mounting Stability

#### System Limitations Identified:
- Potential interference in wireless communication
- Heat dissipation challenges
- Minor mechanical vibration concerns
