# 4. Servo 모터 사용하기


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


```cpp title="servo-for-loop.ino" linenums="1" hl_lines="9"
#include <Servo.h>

const int servoPin1 = 9;
const int servoPin2 = 10;

Servo sv1, sv2;

void setup() {
  sv1.attach(servoPin1, 544, 2400);
  sv2.attach(servoPin2);
}

void loop() {
  for(int i=0; i <= 180; i++) {
    sv1.write(i);
    delay(1);
    sv2.write(180-i);
    delay(10);  
  }
  for(int i=0; i <= 180; i++) {
    sv1.write(180-i);
    delay(1);
    sv2.write(i);
    delay(10);  
  }
}
```
