# Circle Arrow with PCA9685

* PCA9684를 이용해 서보모터 2개를 여러개 연결하고 제어한다.
* 시리얼 포트로 값을 입력하면(쉼표로 구분 2개 값)

```cpp title="pca9685_servo.ino" linenums="1" hl_lines="29-30"
//
// PCA96885 Wiring
// SCL -> A4
// SDA -> A5
//
#include <Adafruit_PWMServoDriver.h>

#define sv1 0
#define sv2 1

#define SERVOMIN  125
#define SERVOMAX  625

Adafruit_PWMServoDriver sv_brd = Adafruit_PWMServoDriver(0x40);

void setup() {
    Serial.begin(115200);
    Serial.println("===== Serial Port Ready =====");

    sv_brd.begin();
    sv_brd.setPWMFreq(60);
}

void loop() {
    while(Serial.available() > 0) {
        int val1 = Serial.parseInt();
        int val2 = Serial.parseInt();

        if(Serial.read() == '\n') {
            sv_brd.setPWM(sv1, 0, servoPulse(val1));
            sv_brd.setPWM(sv2, 0, servoPulse(val2));

            Serial.print(val1);
            Serial.print(", ");
            Serial.println(val2);
        }
    }
}

int servoPulse(int angle) {
    int pulse = constrain(angle, 0, 180);
    return map(pulse, 0, 180, SERVO_MIN, SERVO_MAX);
}
```