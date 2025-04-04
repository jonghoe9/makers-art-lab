# Circle Arrow with PCA9585

```cpp title="serialWrite.ino" linenums="1" hl_lines="30-36"
#include <Adafruit_PWMServoDriver.h>
Adafruit_PWMServoDriver board1 = Adafruit_PWMServoDriver(0x40);

#define SERVOMIN  125
#define SERVOMAX  625

#define sv1 0
#define sv2 1

void setup() {
    Serial.begin(115200);
    Serial.println("===== Serial Port Ready =====");
    board1.begin();
    board1.setPWMFreq(60);
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
          
            board1.setPWM(sv1, 0, val1pulse);
            board1.setPWM(sv2, 0, val2pulse);

            Serial.print(val1);
            Serial.print(", ");
            Serial.println(val2);
        }
    }
}
```