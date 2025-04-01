# Volume, Input Sensor

```cpp title="volumeSerial.ino" linenums="1" hl_lines="9"
const int vol_pin = A0;

void setup() {
  Serial.begin(115200);
  Serial.println("===== Program Start =====");
}

void loop() {
  int value = analogRead(vol_pin);
  Serial.println(value);
  delay(10);
}
```

```cpp title="volumeGraph.ino" linenums="1" hl_lines="11-14"
const int vol_pin = A0;

void setup() {
  Serial.begin(115200);
  Serial.println("===== Program Start =====");
}

void loop() {
  int value = analogRead(vol_pin);
  
  Serial.print("0, 1023, ");
  Serial.print(value);
  Serial.print(", ");
  Serial.println(led_value);
  
  delay(10);
}
```

```cpp title="volumeLed.ino" linenums="1" hl_lines="12"
const int vol_pin = A0;
const int led_pin = 13;

void setup() {
  pinMode(led_pin, OUTPUT);
  Serial.begin(115200);
  Serial.println("===== Program Start =====");
}

void loop() {
  int value = analogRead(vol_pin);
  int led_value = value / 4;
  
  analogWrite(led_pin, led_value);
  
  Serial.print("0, 1023, ");
  Serial.print(value);
  Serial.print(", ");
  Serial.println(led_value);
  
  delay(10);
}
```

```cpp title="volumeServo.ino" linenums="1" hl_lines="17"
#include <Servo.h>

const int vol_pin = A0;
const int servo_pin

Servo sv;

void setup() {
  sv.attach(servo_pin, 544, 2400);

  Serial.begin(115200);
  Serial.println("===== Program Start =====");
}

void loop() {
  int value = analogRead(vol_pin);
  int sv_value = map(value, 0, 1023, 0, 180);
  
  sv.write(sv_value);
  
  Serial.print("0, 1023, ");
  Serial.print(value);
  Serial.print(", ");
  Serial.println(sv_value);
  
  delay(10);
}
```
