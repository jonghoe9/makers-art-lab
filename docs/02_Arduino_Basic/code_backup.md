 # Arduino Code 백업

```cpp title="led-blink.ino" linenums="1" hl_lines="1"
const int led = 13;

void setup() {
  pinMode(red, OUTPUT);
}

void loop() {
  analogWrite(red, 550);
  delay(500);

  analogWrite(red, 250);
  delay(500);
}
```

```cpp title="led3color.ino" linenums="1" hl_lines="1"
const int red   = 9;
const int green = 10;
const int blue  = 11;

void setup() {
  pinMode(red, OUTPUT);
  pinMode(green, OUTPUT);
  pinMode(blue, OUTPUT);
}

void loop() {
  analogWrite(red, 250);
  analoglWrite(green, 150);
  analogWrite(blue, 150);
  delay(500);

  analogWrite(red, 150);
  analogWrite(green, 250);
  analogWrite(blue, 150);
  delay(500);
}
```

```cpp title="led-for-loop.ino" linenums="1" hl_lines="1"
const int red = 9;
const int green = 10;
const int blue = 11;

void setup() {
  pinMode(red, OUTPUT);
  pinMode(green, OUTPUT);
  pinMode(blue, OUTPUT);
}

void loop() {
  for(int i=0; i <= 255; i++) {
    analogWrite(red, i);
    analogWrite(green, 255-i);
    delay(10);
  }
  for(int i=255; i >= 0; i--) {
    analogWrite(red, i);
    analogWrite(green, 255-i);
    delay(10);
  }
}
```


```cpp title="servo.ino" linenums="1" hl_lines="1"
#include <Servo.h>

const int servoPin1 = 9;
const int servoPin2 = 10;

Servo sv1, sv2;

void setup() {
  sv1.attach(servoPin1);
  sv2.attach(servoPin2);
}

void loop() {
  sv1.write(0);
  sv2.write(180);
  delay(1000);

  sv1.write(90);
  sv2.write(90);
  delay(1000);
  
  sv1.write(180);
  sv2.write(0);
  delay(1000);
  
  sv1.write(90);
  sv2.write(90);
  delay(1000);
}

```


```cpp title="servo-for-loop.ino" linenums="1" hl_lines="1"
#include <Servo.h>

const int servoPin1 = 9;
const int servoPin2 = 10;

Servo sv1, sv2;

void setup() {
  sv1.attach(servoPin1);
  sv2.attach(servoPin2);
}

void loop() {
  for(int i=0; i <= 180; i++) {
    sv1.write(i);
    sv2.write(180-i);
    delay(10);  
  }
  for(int i=0; i <= 180; i++) {
    sv1.write(180-i);
    sv2.write(i);
    delay(10);  
  }
}
```

```cpp title="servo-for-loop.ino" linenums="1" hl_lines="9-10"
#include <Servo.h>

const int servoPin1 = 9;
const int servoPin2 = 10;

Servo sv1, sv2;

void setup() {
  sv1.attach(servoPin1, 455, 2400);
  sv2.attach(servoPin2, 430, 2420);
}

void loop() {
  for(int i=0; i <= 180; i++) {
    sv1.write(i);
    sv2.write(180-i);
    delay(10);  
  }
  for(int i=0; i <= 180; i++) {
    sv1.write(180-i);
    sv2.write(i);
    delay(10);  
  }
}
```