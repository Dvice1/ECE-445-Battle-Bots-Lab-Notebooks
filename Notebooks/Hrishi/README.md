# Lab Notebook 2024

## **Lab Notebook Entry #1**  
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

---![Screenshot 2024-11-17 175043](https://github.com/user-attachments/assets/673a1479-8a25-4c4b-bdc5-369b2a370b0b)

## **Lab Notebook Entry #2**  
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

![Screenshot 2024-12-12 133447](https://github.com/user-attachments/assets/b21e1473-e41b-406d-949d-880b2ef111a2)

---

## **Lab Notebook Entry #3**  
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
  
![Screenshot 2024-10-01 165152](https://github.com/user-attachments/assets/378a3e4a-035d-437f-9961-adc526775663)
![Screenshot 2024-10-01 172913](https://github.com/user-attachments/assets/d8951db8-fac0-4f10-b728-cb2567acc61b)
![Screenshot 2024-10-01 173120](https://github.com/user-attachments/assets/4d2d9efa-f61e-4440-92f7-9e95d3c4f1f8)

---

## **Lab Notebook Entry #4**  
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

![Screenshot 2024-11-06 224822](https://github.com/user-attachments/assets/8738763e-c5a7-48c0-b4a3-255d013d0afb)

---

## **Lab Notebook Entry #5**  
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
  
![Screenshot 2024-12-12 132104](https://github.com/user-attachments/assets/4c2677f2-65fa-4318-8283-7d357b9b78c5)

---

## **Lab Notebook Entry #5**  
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
  
![Screenshot 2024-12-06 233611](https://github.com/user-attachments/assets/aa0bc03e-3bf6-4f1d-97dc-8fa41fbf0818)

---

## **Lab Notebook Entry #6**  
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

![Screenshot 2024-12-12 132510](https://github.com/user-attachments/assets/1aa0f1c5-b0b2-4ddd-b09e-5118149f9495)

---

#### Code Snippet: BLE Packet Transmission  

```c
#include <Bluepad32.h>

ControllerPtr myControllers[BP32_MAX_GAMEPADS];

// Motor control pins (GPIOs)
const int EN1 = 13, IN1 = 18, IN2 = 32; // Motor 1
const int EN2 = 26, IN3 = 25, IN4 = 33; // Motor 2
const int EN3 = 27, IN5 = 12, IN6 = 14; // Motor 3

// Deadzone value
const int DEADZONE = 50;

// Initialize motor pins
void setupMotorPins() {
    pinMode(EN1, OUTPUT);
    pinMode(IN1, OUTPUT);
    pinMode(IN2, OUTPUT);
    pinMode(EN2, OUTPUT);
    pinMode(IN3, OUTPUT);
    pinMode(IN4, OUTPUT);
    pinMode(EN3, OUTPUT);
    pinMode(IN5, OUTPUT);
    pinMode(IN6, OUTPUT);

    // Start all motors off (low PWM signal)
    analogWrite(EN1, 0);
    analogWrite(EN2, 0);
    analogWrite(EN3, 0);
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, LOW);
    digitalWrite(IN3, LOW);
    digitalWrite(IN4, LOW);
    digitalWrite(IN5, LOW);
    digitalWrite(IN6, LOW);
}

// Function to apply deadzone
int applyDeadzone(int value) {
    if (abs(value) < DEADZONE) {
        return 0;  // Inside the deadzone, return 0
    }
    return value;
}

void onConnectedController(ControllerPtr ctl) {
    bool foundEmptySlot = false;
    for (int i = 0; i < BP32_MAX_GAMEPADS; i++) {
        if (myControllers[i] == nullptr) {
            Serial.printf("CALLBACK: Controller is connected, index=%d\n", i);
            myControllers[i] = ctl;
            foundEmptySlot = true;
            break;
        }
    }
    if (!foundEmptySlot) {
        Serial.println("CALLBACK: Controller connected, but could not found empty slot");
    }
}

void onDisconnectedController(ControllerPtr ctl) {
    bool foundController = false;
    for (int i = 0; i < BP32_MAX_GAMEPADS; i++) {
        if (myControllers[i] == ctl) {
            Serial.printf("CALLBACK: Controller disconnected from index=%d\n", i);
            myControllers[i] = nullptr;
            foundController = true;
            break;
        }
    }
    if (!foundController) {
        Serial.println("CALLBACK: Controller disconnected, but not found in myControllers");
    }
}

// Control motors based on the input from the gamepad
void controlMotors(ControllerPtr ctl) {
    // Read the Y-axis of the left stick (motor 1 control)
    int motor1Speed = applyDeadzone(ctl->axisY());  // Range is from -511 to 512
    int pwm1 = map(motor1Speed, -511, 512, -255, 255);
    
    // Set motor 1 direction and speed
    if (pwm1 > 0) {
        digitalWrite(IN1, HIGH);
        digitalWrite(IN2, LOW);
    } else if (pwm1 < 0) {
        digitalWrite(IN1, LOW);
        digitalWrite(IN2, HIGH);
    } else {
        digitalWrite(IN1, LOW);
        digitalWrite(IN2, LOW);
    }
    analogWrite(EN1, abs(pwm1));  // Control the speed (PWM)

    // Read the Y-axis of the right stick (motor 2 control)
    int motor2Speed = applyDeadzone(ctl->axisRY());  // Range is from -511 to 512
    int pwm2 = map(motor2Speed, -511, 512, -255, 255);
    
    // Set motor 2 direction and speed
    if (pwm2 > 0) {
        digitalWrite(IN3, HIGH);
        digitalWrite(IN4, LOW);
    } else if (pwm2 < 0) {
        digitalWrite(IN3, LOW);
        digitalWrite(IN4, HIGH);
    } else {
        digitalWrite(IN3, LOW);
        digitalWrite(IN4, LOW);
    }
    analogWrite(EN2, abs(pwm2));  // Control the speed (PWM)

    // Read the throttle (right trigger) for motor 3 control
    int motor3Speed = applyDeadzone(ctl->throttle());  // Range is from 0 to 1023
    int pwm3 = map(motor3Speed, 0, 1023, 0, 255);

    // Set motor 3 direction and speed
    if (motor3Speed > 0) {
        digitalWrite(IN5, HIGH);
        digitalWrite(IN6, LOW);
    } else {
        digitalWrite(IN5, LOW);
        digitalWrite(IN6, LOW);
    }
    analogWrite(EN3, pwm3);  // Control the speed (PWM)
}

void processGamepad(ControllerPtr ctl) {
    controlMotors(ctl);  // Control motors based on gamepad input
}

void processControllers() {
    for (auto myController : myControllers) {
        if (myController && myController->isConnected() && myController->hasData()) {
            if (myController->isGamepad()) {
                processGamepad(myController);
            }
        }
    }
}

// Arduino setup function. Runs in CPU 1
void setup() {
    Serial.begin(115200);
    Serial.printf("Firmware: %s\n", BP32.firmwareVersion());
    const uint8_t* addr = BP32.localBdAddress();
    Serial.printf("BD Addr: %2X:%2X:%2X:%2X:%2X:%2X\n", addr[0], addr[1], addr[2], addr[3], addr[4], addr[5]);

    // Setup the Bluepad32 callbacks
    BP32.setup(&onConnectedController, &onDisconnectedController);

    // Forget Bluetooth keys (optional)
    BP32.forgetBluetoothKeys();

    // Setup motor control pins
    setupMotorPins();

    // Enable virtual devices (optional)
    BP32.enableVirtualDevice(false);
}

// Arduino loop function. Runs in CPU 1.
void loop() {
    bool dataUpdated = BP32.update();
    if (dataUpdated)
        processControllers();

    delay(150);  // Yield to lower priority tasks
}
```
*Caption: Code snippet for transmitting BLE data packet to control motors and weapon.*  


## References  

1. "How to use L293D motor driver with ESP32," YouTube, uploaded by Arnov Sharma, 6 Jul 2020. [Online]. Available: https://www.youtube.com/watch?v=aDBF9Yj04MU [Accessed: Dec. 11 2024].  
2. "ESP32 and 6 motor driver PCB board," YouTube, uploaded by KOKENSHA TECH, 5 Sept 2024. [Online]. Available: https://www.youtube.com/watch?v=TynKvJ_xZA8 [Accessed: Dec. 11 2024].  
3. Espressif Systems, "ESP32-S3-DEVKITM-1 V1 Schematics," Mar. 10, 2021. [Online]. Available: https://dl.espressif.com/dl/schematics/SCH_ESP32-S3-DEVKITM-1_V1_20210310A.pdf [Accessed: Dec. 11 2024].  
4. Espressif Systems, "Espressif KiCad Libraries - Footprints," GitHub. [Online]. Available: https://github.com/espressif/kicad-libraries/tree/main/footprints/Espressif.pretty [Accessed: Dec. 11 2024].  
5. Advanced Monolithic Systems, "AMS1117-3.3 Datasheet," [Online]. Available: https://www.alldatasheet.com/datasheet-pdf/pdf/205691/ADMOS/AMS1117-3.3.html [Accessed: Dec. 11 2024].  
6. ON Semiconductor, "SRV05-4 Datasheet," [Online]. Available: https://www.onsemi.com/pdf/datasheet/srv05-4-d.pdf [Accessed: Dec. 11 2024].  
7. Texas Instruments, "L293D Datasheet," [Online]. Available: https://www.ti.com/lit/ds/symlink/l293.pdf [Accessed: Dec. 11 2024].  
8. Rachel De Barros, "Connect Your Game Controller to an ESP32: The Complete Guide," Electronics, Robotics & Animatronics Engineering Artist, Aug. 30, 2024. [Online]. Available: https://racheldebarros.com/esp32-projects/connect-your-game-controller-to-an-esp32/ [Accessed: Nov. 07, 2024].  
9. "ESP32 BLE Server and Client (Bluetooth Low Energy) | Random Nerd Tutorials," Nov. 11, 2021. [Online]. Available: https://randomnerdtutorials.com/esp32-ble-server-client/ [Accessed: Dec. 11 2024].  
10. "In-Depth: Getting Started with the ESP32 Bluetooth Low Energy (BLE)," Last Minute Engineers, Sep. 10, 2023. [Online]. Available: https://lastminuteengineers.com/esp32-ble-tutorial/ [Accessed: Dec. 11 2024].  
11. Instructables, "Boxxer the Battle Bot (Esp32)," May 19, 2023. [Online]. Available: https://www.instructables.com/Boxxer-the-Battle-Bot-Esp32/ [Accessed: Dec. 11 2024].  
12. Espressif Systems, "Establish Serial Connection with ESP32 - ESP-IDF Programming Guide," docs.espressif.com. [Online]. Available: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/establish-serial-connection.html [Accessed: Dec. 11 2024].  


---
