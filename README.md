
# 🗑️ Touchless Automatic Dustbin using Arduino UNO

A simple sensor-based automatic dustbin developed using an **Arduino UNO, HC-SR04 ultrasonic sensor, and servo motor**.

The system detects a person's hand without physical contact and automatically opens or closes the dustbin lid based on the measured distance.

---

## 📸 Project

![Touchless Automatic Dustbin](<img width="1392" height="658" alt="smart dustbin pic " src="https://github.com/user-attachments/assets/701d10fe-614e-4a44-ad3c-63bfb1a5d7fa" />
)

---

## ⚙️ Working Principle

The **HC-SR04 ultrasonic sensor** continuously measures the distance of an object from the dustbin.

- When an object comes within **25 cm**, the Arduino commands the servo motor to rotate to **90°**, opening the lid.
- When the object moves away to **30 cm or more**, the servo returns to **0°**, closing the lid.

```text
Hand/Object
     ↓
Ultrasonic Sensor
     ↓
  Arduino UNO
     ↓
 Servo Motor
     ↓
  Dustbin Lid
````

---

## 🔥 Chattering Reduction

A common problem with sensor-based systems is **chattering**.

Chattering occurs when the sensor reading fluctuates around a single threshold, causing the servo to repeatedly switch between open and closed positions.

For example:

```text
24.9 cm → Open
25.1 cm → Close
24.8 cm → Open
25.2 cm → Close
```

### Solution: Hysteresis

To reduce this effect, two different distance thresholds are used:

```text
≤ 25 cm  → Open Lid
25–30 cm → Maintain Current State
≥ 30 cm  → Close Lid
```

This **hysteresis-based control** prevents unnecessary servo movement and makes the system more stable.

---

## 💻 Arduino UNO Code

```cpp
#include <Servo.h>

Servo lidServo;

const int trigPin = 4;
const int echoPin = 3;
const int servoPin = 12;

const int CLOSED_ANGLE = 0;
const int OPEN_ANGLE = 90;

const float OPEN_DISTANCE = 25.0;
const float CLOSE_DISTANCE = 30.0;

bool lidOpen = false;

long duration;
float distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  lidServo.attach(servoPin);
  lidServo.write(CLOSED_ANGLE);

  Serial.begin(9600);

  delay(500);
}

void loop() {

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);

  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH, 30000);

  if (duration > 0) {
    distance = duration * 0.0343 / 2.0;
  }
  else {
    distance = 999;
  }

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  if (!lidOpen && distance <= OPEN_DISTANCE) {
    lidServo.write(OPEN_ANGLE);
    lidOpen = true;

    Serial.println("Lid OPEN");
  }

  else if (lidOpen && distance >= CLOSE_DISTANCE) {
    lidServo.write(CLOSED_ANGLE);
    lidOpen = false;

    Serial.println("Lid CLOSED");
  }

  delay(100);
}
```

---

## 🧠 Key Features

* 🚫 **Touchless operation**
* 📡 **Ultrasonic distance sensing**
* ⚙️ **Automatic servo-controlled lid**
* 🔄 **Real-time sensor-based control**
* 🛡️ **Hysteresis to reduce chattering**
* 💻 **Arduino UNO based implementation**

---

## 🔲 Schematic Diagram

![Schematic Diagram](<img width="1312" height="682" alt="Screenshot 2026-08-27 213658" src="https://github.com/user-attachments/assets/f96916c9-6365-417f-82e1-99cdd64bf212" />
)

---

## 🎥 Demonstration

A short demonstration of the working prototype:

**[▶️ Watch Demo]()**

---

## 🛠️ Components

* Arduino UNO
* HC-SR04 Ultrasonic Sensor
* Servo Motor
* Dustbin
* Breadboard
* Jumper Wires

---

## 📚 Concepts Demonstrated

* Arduino Programming
* Ultrasonic Distance Measurement
* Servo Motor Control
* Sensor-Based Automation
* Embedded System Design
* State-Based Control
* Hysteresis and Chattering Reduction

---

## 👨‍💻 Author

**Lalan Kumar Das**
Roll No. **230108028**
Electrical and Electronics Engineering
Indian Institute of Technology Guwahati

```

