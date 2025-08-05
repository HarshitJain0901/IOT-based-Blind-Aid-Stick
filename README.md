# 🦯 IoT-Based Blind Aid Stick

![IoT](https://img.shields.io/badge/IoT-Project-blue)
![Blynk](https://img.shields.io/badge/Blynk-ESP8266-green)
![C++](https://img.shields.io/badge/Language-C%2B%2B-orange)
![Arduino IDE](https://img.shields.io/badge/IDE-Arduino-blue)
![Status](https://img.shields.io/badge/Status-Completed-success)

The **IoT-Based Blind Aid Stick** is a smart assistive device designed to help visually impaired individuals navigate safely.  
It uses **ultrasonic sensors** to detect obstacles, **moisture sensors** to detect wet surfaces, and **IoT integration (Blynk)** for remote monitoring and buzzer control.

---

## 📌 Features

- 🔹 **Obstacle Detection:** Alerts user if objects are within 60 cm.  
- 🔹 **Wet Surface Detection:** Warns user about wet or slippery surfaces.  
- 🔹 **IoT Remote Control:** Locate the stick with a Blynk-controlled buzzer.  
- 🔹 **Portable Design:** Lightweight PVC-based construction.  

---

## 🛠 Components Used

| Component | Description |
|----------|-------------|
| **NodeMCU (ESP8266)** | Microcontroller with Wi-Fi |
| **HC-SR04 Ultrasonic Sensor** | Measures distance to detect obstacles |
| **Soil Moisture Sensor** | Detects wet/slippery surfaces |
| **IR Remote + Receiver** | Controls the buzzer wirelessly |
| **Active & Passive Buzzers** | Provides audio alerts |
| **Breadboard & Jumper Wires** | For prototyping connections |
| **9V Battery** | Power supply |
| **PVC Pipe** | Stick armature |

---

## ⚡ Circuit Diagram

Here’s the **breadboard circuit diagram**:


<img width="719" height="473" alt="image" src="https://github.com/user-attachments/assets/0a7f6def-9be9-44d1-b2c7-4ce356f0aaaf" />


---

## 🏗 Prototype

Real working model of the Blind Aid Stick:



<img width="686" height="399" alt="image" src="https://github.com/user-attachments/assets/60253e81-832b-4b04-9400-d220db9cb3b8" />


---

## 💻 Code

Implemented using **Arduino IDE** with **ESP8266 and Blynk libraries**.

```cpp
#define BLYNK_TEMPLATE_ID "TMPL3FLr3e-l9"
#define BLYNK_TEMPLATE_NAME "Blind Aid Stick"
#define BLYNK_AUTH_TOKEN "Your_Blynk_Token"

#define trigPin D1
#define echoPin D2
#define buzzerPin D3
#define buzzer D4
#define moistureSensorPin A0

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Your_SSID";
char pass[] = "Your_Password";

int duration, distance, moistureLevel;

BLYNK_WRITE(V0) {
  bool value1 = param.asInt();
  digitalWrite(buzzer, value1 ? LOW : HIGH);
}

void setup() {
  pinMode(buzzer, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  digitalWrite(buzzer, HIGH);
  Serial.begin(9600);
  Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);
}

void loop() {
  Blynk.run();

  // Ultrasonic distance measurement
  digitalWrite(trigPin, LOW); delayMicroseconds(2);
  digitalWrite(trigPin, HIGH); delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.0344 / 2;

  // Moisture level
  moistureLevel = analogRead(moistureSensorPin);

  Serial.print("Distance: "); Serial.print(distance);
  Serial.print(" cm, Moisture: "); Serial.println(moistureLevel);

  // Alert conditions
  if (distance < 60 || moistureLevel < 1024) {
    digitalWrite(buzzerPin, HIGH);
    delay(200);
    digitalWrite(buzzerPin, LOW);
  } else {
    digitalWrite(buzzerPin, LOW);
  }

  delay(200);
}
