# 06. Arduino Serial Write
- 아두이노에서 센서 읽고 시리얼 포트에 쓰기



## Serial 포트로 RGB LED 제어하기

- 시리얼 포트로 정수값 3개가 입력된다.
- 각 숫자를 RGB 색깔에 대응해서 LED 색깔을 조절한다.

```cpp title="ew_0503.ino" linenums="1" hl_lines="17"
//
// RGB LED, 변수값 설정하여 색깔 바꾸기
//

#define PIN_R 9
#define PIN_G 10
#define PIN_B 11

void setup() {
    pinMode(PIN_R, OUTPUT);
    pinMode(PIN_G, OUTPUT);
    pinMode(PIN_B, OUTPUT);
    Serial.begin(115200);
}

void loop() {
    while(Serial.available() > 0) {
        int val_r = Serial.parseInt();
        int val_g = Serial.parseInt();
        int val_b = Serial.parseInt();
        if(Serial.read() == '\n') {
            analogWrite(PIN_R, val_r);
            analogWrite(PIN_G, val_g);
            analogWrite(PIN_B, val_b);
        }
    }    
}
```

