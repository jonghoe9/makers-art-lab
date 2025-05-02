# 1. 아두이노 프로그래밍 1

* 초음파 센서를 읽고 시리얼로 출력한다.
* [초음파 센서에 대한 내용 보기](/04_Arduino_Ext/2_Sensor/HC-SR04/)

```cpp title="p51_arduino1.ino" linenums="1" hl_lines="14"
#define TRIG_PIN 9
#define ECHO_PIN 8

void setup() {
    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);
}

void loop() {
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);
    long dist = pulseIn(ECHO_PIN, HIGH) / 58;

    print(dist);   // cm 단위
    delay(200);
}
```

---
[Project 0501](../)

