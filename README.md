# smart-house-cleaning-bot

The Smart House Cleaning Bot is an intelligent robot that automates brooming and vacuuming tasks. It navigates spaces, avoids obstacles and edges, and efficiently cleans floors using built-in sweeping and suction mechanisms, offering a practical solution for daily household cleaning.

# Components used

- Arduino Uno
- BLDC Motor
- Impeller
- ESC Speed Controller
- Ultrasonic sensors
- Gear Motors
- BO Motors
- L298N Motor driver
- Li-ion Battery pack

# Features

- Automated Cleaning: Performs essential floor-cleaning tasks such as brooming and vacuuming with minimal human intervention.
- Dual-Mode Operation: Supports both autonomous cleaning and manual control for flexibility and convenience.
- Smart Navigation: Detects and avoids obstacles, furniture, and walls for smooth, uninterrupted cleaning.
- Time-Saving Solution: Reduces the time and effort required for daily household cleaning tasks.
- Learning-Oriented Design: Integrates key concepts from robotics, embedded systems, sensor technology, and wireless communication.

# Circuit Diagram
<img width="1200" height="693" alt="image" src="https://github.com/user-attachments/assets/31edf723-45e2-44ac-8866-08d20f3af3ab" />

# Code

#include <motor.h>
int front, left, right;
Motor Motor2(9, 8, 10);
Motor Motor1(12, 13, 11);
#define trig1 2  // Front
#define echo1 3
#define trig2 4  // Left
#define echo2 5
#define trig3 6  // Right
#define echo3 7


#define IN1 8
#define IN2 9
#define IN3 12
#define IN4 13

long readUltrasonicDistance(int trigPin, int echoPin) {
  long duration, distance;

  // First ping
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH, 30000); // timeout after 30ms
  distance = duration * 0.034 / 2;

  // Retry if invalid reading
  if (distance <= 0 || distance > 400) {
    delay(10);
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    duration = pulseIn(echoPin, HIGH, 30000);
    distance = duration * 0.034 / 2;
  }

  // Return 400 cm if still invalid
  if (distance <= 0 || distance > 400) {
    return 400;
  }

  return distance;
}




void setup() {
 pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  // Set ultrasonic pins
  pinMode(trig1, OUTPUT);
  pinMode(echo1, INPUT);
  pinMode(trig2, OUTPUT);
  pinMode(echo2, INPUT);
  pinMode(trig3, OUTPUT);
  pinMode(echo3, INPUT);

Serial.begin(9600);
  delay(100);
 
}

void loop() {
	long frontDist = readUltrasonicDistance(trig1, echo1);
  long leftDist = readUltrasonicDistance(trig2, echo2);
  long rightDist = readUltrasonicDistance(trig3, echo3);


Serial.print("Front: "); Serial.print(frontDist);
  Serial.print(" | Left: "); Serial.print(leftDist);
  Serial.print(" | Right: "); Serial.println(rightDist);
	
if (frontDist > 20)  {
		Motor2.moveMotor(255);
		Motor1.moveMotor(255);
	}
	else {
    delay(300);
    if (leftDist > rightDist)   {
			Serial.println("Turning Left");
      Motor1.moveMotor(-255);
			Motor2.moveMotor(255);
			delay(5000);
		}
		else {
			Serial.println("Turning Right");
      Motor1.moveMotor(255);
			Motor2.moveMotor(-255);
			delay(5000);
		}
    delay(200);
}

}

# How to use

1. Upload the coded to Arduino UNO using ArduinoIDE
2. Connect the components as per the circuit diagram
3. Turn the switch on
