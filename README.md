# radar
radar project for school and college using arduino uno micro controller and ultra sonic sensor 
for Arduino IDE:
#include <Servo.h>

Servo myServo;

const int trigPin = 10;
const int echoPin = 11;

long duration;
int distance;

void setup() {
  myServo.attach(9);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  // Sweep 0° → 180°
  for (int angle = 0; angle <= 180; angle++) {
    myServo.write(angle);
    distance = getDistance();
    Serial.print(angle);
    Serial.print(",");
    Serial.println(distance);
    delay(40);
  }

  // Sweep 180° → 0°
  for (int angle = 180; angle >= 0; angle--) {
    myServo.write(angle);
    distance = getDistance();
    Serial.print(angle);
    Serial.print(",");
    Serial.println(distance);
    delay(40);
  }
}

int getDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  int dist = duration * 0.034 / 2;
  if (dist > 200) dist = 200;
  return dist;
  }
  Code for visual output for processing 4:
  import processing.serial.*;
import java.awt.event.KeyEvent;
import java.io.IOException;

Serial myPort;
String data = "";
int angle, distance;

void setup() {
  size(900, 900);
  smooth();
  myPort = new Serial(this, "COM3", 9600);  // ← Change COM3 to your actual port!
  myPort.bufferUntil('\n');
}

void draw() {
  background(0);
  fill(0, 255, 0);
  noStroke();
  textSize(20);
  text("ARDUINO RADAR", width / 2 - 100, 30);
  
  // Draw radar circles
  stroke(0, 255, 0);
  noFill();
  ellipse(width / 2, height / 2, 600, 600);
  ellipse(width / 2, height / 2, 400, 400);
  ellipse(width / 2, height / 2, 200, 200);

  // Draw radar lines
  line(width / 2, height / 2, width / 2 + 300 * cos(radians(angle)), height / 2 - 300 * sin(radians(angle)));
  
  // Show detected object point
  int pixDist = distance * 3; // scale for display
  int x = width / 2 + int(pixDist * cos(radians(angle)));
  int y = height / 2 - int(pixDist * sin(radians(angle)));
  
  fill(255, 0, 0);
  ellipse(x, y, 10, 10);
}

void serialEvent(Serial myPort) {
  data = myPort.readStringUntil('\n');
  if (data != null) {
    data = trim(data);
    String[] values = split(data, ',');
    if (values.length == 2) {
      angle = int(values[0]);
      distance = int(values[1]);
    }
  }
}

}

