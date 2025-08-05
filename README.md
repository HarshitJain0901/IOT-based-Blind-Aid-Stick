IoT-Based Blind Aid Stick
The IoT-Based Blind Aid Stick is a smart assistive device designed to help visually impaired individuals navigate their surroundings safely. It detects obstacles and wet surfaces using sensors and provides real-time feedback via a buzzer and a connected Blynk IoT application.

📌 Features
Obstacle Detection: Detects objects using the HC-SR04 ultrasonic sensor.

Wet Surface Detection: Uses a Soil Moisture Sensor to detect wet/slippery surfaces.

Remote Buzzer Activation: Can locate the stick using a Blynk-controlled buzzer.

IoT Monitoring: Displays distance and moisture readings in real-time on the Blynk App.

Portable Design: Lightweight, made using a PVC pipe armature.

🛠 Components Used
Component	Quantity	Description
NodeMCU (ESP8266)	1	Microcontroller with Wi-Fi
HC-SR04 Ultrasonic Sensor	1	Detects obstacles by measuring distance
Soil Moisture Sensor	1	Detects wet/slippery surfaces
IR Remote + Receiver	1	For wireless buzzer activation
Passive & Active Buzzers	2	Provides audio alerts
Breadboard	1	For circuit assembly
Jumper Wires (M-F / M-M / F-F)	-	For connections
9V Battery	1	Power supply
PVC Pipe	1	Stick body

⚡ Circuit Diagram
<img width="719" height="473" alt="image" src="https://github.com/user-attachments/assets/f10b1148-2d87-41b8-b2fc-c2a524ad74c9" />


💻 Implementation
The system works in three main steps:

Distance Measurement:
The ultrasonic sensor calculates the distance to nearby objects.

Surface Moisture Detection:
The soil moisture sensor detects wet or slippery surfaces.

Alert Mechanism:

If an obstacle is closer than 60 cm, the buzzer is triggered.

If moisture is detected below a threshold, the buzzer alerts the user.

Remote buzzer control via Blynk App helps locate the stick.

📜 Code
The code is implemented using Arduino IDE with ESP8266 and Blynk libraries.

cpp
Copy
Edit
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
📱 IoT Dashboard
The project uses Blynk IoT for:

Distance & Moisture Monitoring

Remote Buzzer Control

Real-time Notifications


✅ Future Improvements
Integrate GPS tracking to locate the stick if lost.

Add vibration motor for silent feedback.

Enhance battery efficiency with low-power modes.
