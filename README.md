#include <Servo.h>

// Create servo objects
Servo servoHorizontal;
Servo servoVertical;

// Servo pins
int servoPinH = 9;
int servoPinV = 10;

// LDR sensor pins
int ldrTopLeft = A0;
int ldrTopRight = A1;
int ldrBottomLeft = A2;
int ldrBottomRight = A3;

// Servo positions
int posH = 90;  // start at center
int posV = 90;

void setup() {
  servoHorizontal.attach(servoPinH);
  servoVertical.attach(servoPinV);

  servoHorizontal.write(posH);
  servoVertical.write(posV);

  delay(1000);
}

void loop() {
  // Read LDR values
  int tl = analogRead(ldrTopLeft);
  int tr = analogRead(ldrTopRight);
  int bl = analogRead(ldrBottomLeft);
  int br = analogRead(ldrBottomRight);

  // Average top and bottom
  int top = (tl + tr) / 2;
  int bottom = (bl + br) / 2;
  int left = (tl + bl) / 2;
  int right = (tr + br) / 2;

  // Vertical movement
  if (top > bottom + 50) {
    posV++;
  } else if (bottom > top + 50) {
    posV--;
  }

  // Horizontal movement
  if (left > right + 50) {
    posH++;
  } else if (right > left + 50) {
    posH--;
  }

  // Limit servo angles
  posH = constrain(posH, 0, 180);
  posV = constrain(posV, 0, 180);

  // Move servos
  servoHorizontal.write(posH);
  servoVertical.write(posV);

  delay(50);
}
