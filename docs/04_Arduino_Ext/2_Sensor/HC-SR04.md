# HC-SR04 초음파센서

초음파 센서를 사용해 거리를 측정한다.

```cpp title="ultrasonic.ino" linenums="1" hl_lines="5"

#define TRIG_PIN 9
#define ECHO_PIN 8

void setup() {
    Serial.begin(115200);
    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);
}

void loop() {
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);
    long dist = pulseIn(ECHO_PIN, HIGH) / 58;

    Serial.print(dist);
    Serial.println(" cm");

    delay(500);
}
```

* 초음파는 28us 마다 1cm 이동
* 초음파가 왕복 했으므로 28 * 2 해서 58, 58이면 2cm
* 측정범위 2cm ~ 4m