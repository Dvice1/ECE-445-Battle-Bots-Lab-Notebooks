## ALL DESIGN CONSIDERATIONS:

10/5/24:
- Started using a PS4 controller for Bluetooth connection to the ESP32. However, there are some concerns about latency. We expect the latency to be around 1s, which is right on the border of our high-level requirement. If possible, we might pivot to using an Xbox controller since it uses BLE (Bluetooth low energy), which would be better for power and significantly reduce latency. But it is also more difficult to code. We referenced this document: https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/bluetooth/index.html 
- We also discussed what materials we should finally use. We saw suggestions about using PLA+ throughout, but need to research the use of PETG.

10/15/24:
- We decided that using the Xbox controller and incorporating BLE would be the right decision. The proposed data packet format would be 8-bit left-wheel intensity, 8-bit right-wheel intensity, 8-bit weapon motor intensity, and 8-bit system command that we would use for our kill switch. We developed some preliminary code and showed the following printing on the terminal
![Alt text for the image](IMG_3448.jpg.png)

10/21/24:
- We modeled our bot through CAD and this is what we got:
![Alt text for the image](CADbot.png)

10/28/24:
I performed a tolerance analysis on our weapon. Here are the equations used:
- Moment of inertia (I): I = (1/2) × m × r² = 0.5 × 0.15 kg × (0.09 m)² = 0.000608 kg·m²
- Angular velocity (ω): 750 r/min = 78.54 rad/s
- Impact force capacity: F = m × r × ω² = 0.15 kg × 0.09 m × (78.54 rad/s) ² = 69.5 N
- Rotational kinetic energy: E = (1/2) × I × ω² = 1.87 J

11/31/24:
Finalized our BLE code snippet. Everything works with this:
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

12/1/24:
- 3D printed our parts using the 2070 lab 3D printer. Here is the final bot, ready to demo, with everything inside:
![Alt text for the image](Finalbot.png)


## MEETINGS RECAP, NOTES AND MORE:
9/18/24:
- Met with Gregg in the machine shop. 

9/25/24:
- checked Github notebooks

- opinions on the proposal
  - improve the introduction by describing the background of battle bots and the competition
  - label the visual aid
  - label data transmission lines in the block diagram
  - add voltage value to the voltage regulator in the block diagram
  - include the use of the PWM with the motors
  - requirements need hard metrics. Under each subsystem requirement, what should it satisfy to know that it's working

- discussed design document

9/30/24:
- Submitted the design document. Discussed proposal regrade.

10/8/24:
- Attended the design review. Got suggestions on ESCs. Also talked about how we will be implementing our blade. Should it be
  in the middle of the bot or the top? We talked a little about things on our PCB but this is still quite uncertain. 

10/16/24:
- Submitted our PCB design for the first iteration. There may be potential issues with the resistors/capacitors we are using.
- Reminder to submit the team evaluation form due today.
- Already met with machine shop people so that's nice.

10/23/24:
- Submitted our second round of PCB orders after modifying the first one. But it turns out the first one is too small.
- Going to work on a better, larger design (10x10cm) to be able to solder for our third-week order.

10/30/24:
- Design document suggestions/missing things:
  - Lost 2 points for PCB schematic
  - Missing citations
  - Everything else looks good
 
11/6/24:
- Met over Zoom today, which was nice
- Submitted our new board today, let's see how it goes.
- Reminder for the individual progress report due today.
- Also a reminder for the design doc that needs to be updated and submitted by the day after tomorrow.
- Here is what our 4th round board looks like:
![Alt text for the image](SchematicWeek3.png)
![Alt text for the image](PCBWeek3.png)

11/11/24:
- Placed an order for all parts for our final PCB. We also picked some stuff up from the machine shop:
  - Resistors: 5.1k, 1k and 10k
  - Capacitors 0805: 0.1uF (needed 1205 so this was wrong and reordered with other parts.
 
![Alt text for the image](myeceparts.png)

11/13/24:
- Sign up for presentation and demo times.
- Make sure to wear business casuals for the presentation.
- We missed an order deadline so order the final PCB ourselves. Here is the PCB and Schemtic:
![Alt text for the image](FinalSchematic.png)
![Alt text for the image](PCBFinal.png)

11/23/24:
- Placed an order for parts through myECE:
![Alt text for the image](myecepinheaders.png)

12/12/24:
- Looking back at this class today with tremendous joy. Chi was an amazing TA, we got our project fully working and had a great demo and presentation. Thoroughly enjoyed it!


## References/Citations
- "How to use L293D motor driver with ESP32," YouTube, uploaded by Arnov Sharma, 6 Jul 2020. [Online]. Available: https://www.youtube.com/watch?v=aDBF9Yj04MU [Accessed: Dec. 11 2024].
- "ESP32 and 6 motor driver PCB board," YouTube, uploaded by KOKENSHA TECH, 5 Sept 2024. [Online]. Available: https://www.youtube.com/watch?v=TynKvJ_xZA8 [Accessed: Dec. 11 2024].
- Espressif Systems, "ESP32-S3-DEVKITM-1 V1 Schematics," Mar. 10, 2021. [Online]. Available: https://dl.espressif.com/dl/schematics/SCH_ESP32-S3-DEVKITM-1_V1_20210310A.pdf [Accessed: Dec. 11 2024].
- Espressif Systems, "Espressif KiCad Libraries - Footprints," GitHub. [Online]. Available: https://github.com/espressif/kicad-libraries/tree/main/footprints/Espressif.pretty [Accessed: Dec. 11 2024].
- Advanced Monolithic Systems, "AMS1117-3.3 Datasheet," [Online]. Available: https://www.alldatasheet.com/datasheet-pdf/pdf/205691/ADMOS/AMS1117-3.3.html [Accessed: Dec. 11 2024].
- ON Semiconductor, "SRV05-4 Datasheet," [Online]. Available: https://www.onsemi.com/pdf/datasheet/srv05-4-d.pdf [Accessed: Dec. 11 2024].
- Texas Instruments, "L293D Datasheet," [Online]. Available: https://www.ti.com/lit/ds/symlink/l293.pdf [Accessed: Dec. 11 2024].
- Rachel De Barros, "Connect Your Game Controller to an ESP32: The Complete Guide," Electronics, Robotics & Animatronics Engineering Artist, Aug. 30, 2024. [Online]. Available: https://racheldebarros.com/esp32-projects/connect-your-game-controller-to-an-esp32/ [Accessed: Nov. 07, 2024].
- "ESP32 BLE Server and Client (Bluetooth Low Energy) | Random Nerd Tutorials," Nov. 11, 2021. [Online]. Available: https://randomnerdtutorials.com/esp32-ble-server-client/ [Accessed: Dec. 11 2024].
- "In-Depth: Getting Started with the ESP32 Bluetooth Low Energy (BLE)," Last Minute Engineers, Sep. 10, 2023. [Online]. Available: https://lastminuteengineers.com/esp32-ble-tutorial/ [Accessed: Dec. 11 2024].









