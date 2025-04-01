# 5. Serial 포트 사용하기

시리얼 포트를 사용해 보자.


## 코드 1. LED 밝기 값을 Serial로 출력

```cpp title="serialWrite.ino" linenums="1" hl_lines="5"
const int led = 13;

void setup() {
    pinMode(red, OUTPUT);
    Serial.begin(115200);
    Serial.println("===== Program Start =====");
}

void loop() {
    for(int i = 0; i <= 255; i++) {
        Serial.println(i);
        analogWrite(red, i);
        delay(50);
    }
    delay(2000);
}
```


## 코드 2. Serial로 LED 2개 밝기 조절

```cpp title="serialRead.ino" linenums="1" hl_lines="12-15"
const int led1 = 9;
const int led2 = 10;

void setup() {
    pinMode(led1, OUTPUT);
    pinMode(led2, OUTPUT);
    Serial.begin(115200);
    Serial.println("===== Program Start =====");
}

void loop() {
    while(Serial.available() > 0) {
        int val1 = Serial.parseInt();
        int val2 = Serial.parseInt();
        if(Serial.read() == '\n') {
            Serial.print(val1);
            Serial.print(", ");
            Serial.println(val2);
            analogWrite(led1, val1);
            analogWrite(led2, val2);
        }
    }
}
```

* 시리얼 모니터로 값을 2개 입력해 보자 (예: 120, 200)
* 입력 값은 쉼표로 구분하여 숫자 2개를 입력한다.


## 코드 3. 시리얼로 서보모터 제어

```cpp title="serialReadServo.ino" linenums="1" hl_lines="20-21"
#include <Servo.h>

const int SV1_PIN = 6;
const int SV2_PIN = 5;

Servo sv1, sv2;

void setup() {
    sv1.attach(SV1_PIN, 544, 2400);
    sv2.attach(SV2_PIN);
    Serial.begin(115200);
    Serial.println("===== Serial Port Ready =====");
}

void loop() {
    while(Serial.available() > 0) {
        val1 = Serial.parseInt();
        val2 = Serial.parseInt();
        
        val1 = constrain(val1, 0, 180);
        val2 = constrain(val2, 0, 180);

        if(Serial.read() == '\n') {
            sv1.write(val1);
            sv2.write(val2);
            Serial.print(val1);
            Serial.print(", ");
            Serial.println(val2);
        }
    }
}
```