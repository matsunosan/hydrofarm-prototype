# Automated Hydroponic Farm

## Description of the Project
The Automated Hydroponic Farm is a system designed to manage water levels and automate key processes in a hydroponic farming environment. Utilizing servo motors, a liquid level sensor, and a relay, the system monitors the water reservoir's level, controls water flow, and provides real-time feedback. This project is ideal for educational purposes and small-scale hydroponic setups.

Note: In TinkerCAD, a water level sensor is not available. Instead, a temperature sensor is used to simulate water level readings for demonstration purposes.

## Components Used
- **Arduino Uno R3**: Microcontroller to manage the system.
- **3 Servo Motors**: Control valves or other mechanisms for automation.
- **Liquid Level Sensor (eTape)**: Monitors the water level in the reservoir.
- **Relay Module**: Controls external devices like pumps or water flow.
- **5V Power Supply**: Powers the components.
- **Breadboard and Jumper Wires**: Facilitate connections between components.
- **LEDs and Resistors (optional)**: For status indication (not shown in this setup).

## Setup Instructions
1. **Circuit Assembly**:
   - Connect the servo motors to pins 9, 10, and 11 on the Arduino.
   - Attach the liquid level sensor to pin A0.
   - Connect the relay module to pin 2.
   - Use a 5V power supply to power the servo motors through the breadboard.
   - Refer to the wiring (screenshot below) for detailed connections.

2. **Code Upload**:
   - Install the Arduino IDE if not already done.
   - Copy the provided code into the Arduino IDE.
   - Select the correct board (`Arduino Uno`) and port from the Tools menu.
   - Upload the code to the Arduino board.

## Code Snippet
```cpp
#include <Servo.h>
Servo myServoA;
Servo myServoB;
Servo myServoC;

int servoSpeed = 120; 
unsigned long rotationTime = 3.3 * 1000; 

const byte servoPinA = 11;
const byte servoPinB = 10;
const byte servoPinC = 9;

int rawLiquidReading;
float liquidLevel;
const long lowEtapeValue = 20;   
const long highEtapeValue = 358; 

const byte liquidLevelPin = A0; 
const byte relayPin = 2;

void setup() {
  myServoA.attach(servoPinA);
  myServoB.attach(servoPinB);
  myServoC.attach(servoPinC);
  
  pinMode(liquidLevelPin, INPUT);
  pinMode(relayPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  myServoA.write(servoSpeed);
  myServoB.write(servoSpeed);
  myServoC.write(servoSpeed);

  delay(rotationTime);

  myServoA.write(90);
  myServoB.write(90);
  myServoC.write(90);

  delay(5000);
  
  rawLiquidReading = analogRead(liquidLevelPin);
  Serial.print("Raw Liquid Level: ");
  Serial.print(rawLiquidReading);
  liquidLevel = map(rawLiquidReading, lowEtapeValue, highEtapeValue, 0, 100);
  Serial.print(" / Liquid Remaining: ");
  Serial.print(liquidLevel);
  Serial.println("%.");

  if (liquidLevel > 95) {
    digitalWrite(relayPin, HIGH);
  } else if (liquidLevel < 50) {
    digitalWrite(relayPin, LOW);
  }

  delay(1000);
}
```

## Screenshots
1. **Circuit Diagram on TinkerCAD**:
![image](https://github.com/user-attachments/assets/5304d6e0-042e-4727-8870-9b203a2cb286)

## Usage Instructions
1. Ensure the system is connected to a 5V power supply.
2. Open the Arduino IDE's Serial Monitor to observe real-time data from the liquid level sensor.
3. The servos will adjust based on the programmed rotation time, and the relay will activate when the reservoir is over 95% full or below 50%.

## Future Improvements or Features
- Add an LCD display for real-time data visualization.
- Integrate IoT features for remote monitoring and control.
- Include additional sensors for temperature, humidity, and nutrient monitoring.
- Design a user-friendly GUI for managing and visualizing data.

## License
This project is open-source and available under the MIT License. Feel free to use, modify, and distribute as needed.


