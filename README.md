🔧 Arduino Fan Mode Switch Project
📋 Description
This project allows the user to switch between three modes of a fan (or motor) using a push button:

Mode 1: Fan ON (Normal fan mode)

Mode 2: Fan ON (Exhaust mode – reversed direction)

Mode 3: Fan OFF

The motor direction is controlled through an L298N motor driver, and the input is given via a push button. The fan used is a DC motor powered by 9V lithium batteries.

🧰 Components
Arduino UNO

L298N Motor Driver

DC Motor

Push Button

9V Lithium Battery (or Power Supply)

Jumper Wires

Breadboard

⚡ Circuit Connections
Arduino Pin	Component	Notes
D3	IN3 (L298N)	Motor driver input
D4	IN4 (L298N)	Motor driver input
D5	Push Button	With internal pull-up
GND	GND (L298N + Button)	Common ground
5V	VCC for Button (if needed)	Can be omitted if pull-up used

The motor is connected to OUT3 and OUT4 on the L298N, which is powered by the 9V battery.

🧠 Code Logic Summary
Each button press changes the mode in a loop: Mode 1 → Mode 2 → Mode 3 → Mode 1 ...

The motor spins forward in Mode 1, reverse in Mode 2, and stops in Mode 3.

💻 Arduino Code

#include <LiquidCrystal_I2C.h>

const int buttonPin = 2;
const int motorPin1 = 3;
const int motorPin2 = 4;

bool lastButtonState = HIGH;
bool currentButtonState;
int mode = 0;

LiquidCrystal_I2C lcd(0x27, 16, 2); 
void setup() {
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(motorPin1, OUTPUT);
  pinMode(motorPin2, OUTPUT);

  lcd.begin(16,2);
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("System Starting...");
  delay(1000);
  lcd.clear();
}

void loop() {
  currentButtonState = digitalRead(buttonPin);

  if (lastButtonState == HIGH && currentButtonState == LOW) {
    mode++;
    if (mode > 2) mode = 0;
    updateDisplay();
    delay(300);
  }

  lastButtonState = currentButtonState;

  switch (mode) {
    case 0: // OFF
      digitalWrite(motorPin1, LOW);
      digitalWrite(motorPin2, LOW);
      break;

    case 1: // Fan
      digitalWrite(motorPin1, HIGH);
      digitalWrite(motorPin2, LOW);
      break;

    case 2: // Suction
      digitalWrite(motorPin1, LOW);
      digitalWrite(motorPin2, HIGH);
      break;
  }
}

void updateDisplay() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Mode: ");
  switch (mode) {
    case 0: lcd.print("OFF"); break;
    case 1: lcd.print("Fan"); break;
    case 2: lcd.print("Suction"); break;
  }
}

📦 How to Use
Connect the components as shown.

Upload the code to your Arduino.

Power the circuit.

Press the button to cycle between modes.


