# 1016. 아두이노 센서 입력

## Code 1. 동작확인

```cpp title="led-blink.ino" linenums="1" hl_lines="4"
const int led = 13;

void setup() {
  pinMode(led, OUTPUT);
}

void loop() {
  digitalWrite(led, HIGH);
  delay(500);

  digitalWrite(led, LOW);
  delay(500);
}
```

## Code 2. 시리얼 포트 글자 출력
```cpp title="led-serial1.ino" linenums="1" hl_lines="5"
const int led = 13;

void setup() {
  pinMode(led, OUTPUT);
  Serial.begin(115200);
}

void loop() {
  Serial.println("LED ON");
  digitalWrite(led, HIGH);
  delay(500);

  Serial.println("LED OFF");
  digitalWrite(led, LOW);
  delay(500);
}
```

## Code 3. 볼륨값 입력 받기
```cpp title="led-serial2.ino" linenums="1" hl_lines="4"
const int led = 13;

void setup() {
  pinMode(led, OUTPUT);
  Serial.begin(115200);
}

void loop() {
  int val = analogRead(A0);
  Serial.print("Volume : ");
  Serial.println(val);
  
  Serial.println("LED ON");
  digitalWrite(led, HIGH);
  delay(500);
  
  Serial.println("LED OFF");
  digitalWrite(led, LOW);
  delay(500);
}
```

## Code 4. 볼륨값 그래프로 보기
```cpp title="led-serial3.ino" linenums="1" hl_lines="4"
const int led = 13;

void setup() {
  pinMode(led, OUTPUT);
  Serial.begin(115200);
}

void loop() {
  int val = analogRead(A0);
  Serial.print("0, 1023, ");
  Serial.println(val);
  
  Serial.println("LED ON");
  digitalWrite(led, HIGH);
  delay(500);
  
  Serial.println("LED OFF");
  digitalWrite(led, LOW);
  delay(500);
}
```

## Code 5. 볼륨값이 특정 구간에 들어오면 LED 켜기
```cpp title="led-serial4.ino" linenums="1" hl_lines="4"
const int led = 13;
const int trigL = 500;

void setup() {
  pinMode(led, OUTPUT);
  Serial.begin(115200);
}

void loop() {
  int val = analogRead(A0);
  Serial.print("0, 1023, ");
  Serial.println(val);

  if(val > trigL) {
    digitalWrite(led, HIGH);
    delay(10);
  } else {
    digitalWrite(led, LOW);
    delay(10);
  }
}
```

## Code 6. 서보모터 움직이기
```cpp title="servo.ino" linenums="1" hl_lines="1"
#include <Servo.h>

const int servoPin = 9;

Servo sv;

void setup() {
  sv.attach(servoPin);
}

void loop() {
  sv.write(0);
  delay(1000);

  sv.write(90);
  delay(1000);
  
  sv.write(180);
  delay(1000);
  
  sv.write(90);
  delay(1000);
}

```

## Code 7. 볼륨으로 서보 모터 움직이기
```cpp title="serial-servo1.ino" linenums="1" hl_lines="4"
#include <Servo.h>
#define SERVO_PIN 9

const int led = 13;

Servo sv;

void setup() {
  pinMode(led, OUTPUT);
  Serial.begin(115200);
  sv.attach(SERVO_PIN);
}

void loop() {
  int val = analogRead(A0);
  Serial.print("0, 1023, ");
  Serial.println(val);
  sv.write(val);
  delay(10);
}
```

## Code 8. 볼륨으로 서보 모터 움직이기, 범위제한
```cpp title="serial-servo2.ino" linenums="1" hl_lines="15"
#include <Servo.h>
const int led = 13;
const int servo_pin = 9;

void setup() {
  pinMode(led, OUTPUT);
  Serial.begin(115200);
  sv.attach(servo_pin);
}

void loop() {
  int val = analogRead(A0);
  Serial.print("0, 1023, ");
  Serial.println(val);
  val = map(val, 0, 1023, 0, 180);
  sv.write(val);
  delay(10);
}
```