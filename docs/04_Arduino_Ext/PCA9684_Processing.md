# Circle Arrow with PCA9685

PCA9684를 이용해 서보모터를 여러개 연결하고 제어한다.

```cpp title="serialWrite.ino" linenums="1" hl_lines="31-37"
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

        val1 = constrain(val1, 0, 180);
        val2 = constrain(val2, 0, 180);

        int val1pulse = map(val1, 0, 180, SERVOMIN, SERVOMAX);
        int val2pulse = map(val2, 0, 180, SERVOMIN, SERVOMAX);

        if(Serial.read() == '\n') {
          
            sv_brd.setPWM(sv1, 0, val1pulse);
            sv_brd.setPWM(sv2, 0, val2pulse);

            Serial.print(val1);
            Serial.print(", ");
            Serial.println(val2);
        }
    }
}
```