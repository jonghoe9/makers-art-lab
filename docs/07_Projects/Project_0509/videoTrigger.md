# Code 2: 시리얼 컨트롤

* 시리얼 포트로 들어오는 문자열로 동영상 재생


## 프로세싱 코드

```cpp title="p59_videoTrigger.pde" linenums="1" hl_lines="31-33"
import processing.serial.*;
import processing.video.*;

Serial port;
Movie movNor, movTrg;

float loopPos = 0.2;
String movFile1 = "6162.mov";
String movFile2 = "5824.mov";
int scrW, scrH;

int sensorChk = 0;

void setup() {
    size(1280, 720);
    scrW = width;
    scrH = (int)(width *9/16);
    port = new Serial(this, "/dev/cu.usbmodem141011", 115200);
    movNor = new Movie(this, movFile1);
    movTrg = new Movie(this, movFile2);
    movNor.play();
    movTrg.pause();
}

void draw() {
    if(sensorChk == 1) {
        image(movTrg, 0, 0, scrW, scrH);
    } else {
        image(movNor, 0, 0, scrW, scrH);
    }
    if(movNor.duration() - movNor.time() < loopPos) {
        movNor.jump(0);
    }
}

void movieEvent(Movie m) {
    m.read();
}

void serialEvent(Serial port) {
    String str = port.readStringUntil('\n');
    if(str != null) {
        String s = trim(str);
        if(s == "DETECT") {
            sensorChk = 1;
            movTrg.jump(0);
            movTrg.play();
        } else if(s == "NOT DETECT") {
            sensorChk = 0;
            movTrg.pause();
            movTrg.jump(0);
        }
    }
}

```


## 아두이노 코드

```cpp title="sensor_sr501.ino" linenums="1" hl_lines="9"
#define SENSOR_PIN 7

void setup() {
    pinMode(SENSOR_PIN, INPUT);
    Serial.begin(115200);
}

void loop() {
    int chk = digitalRead(SENSOR_PIN);
    if(chk == HIGH) {
        Serial.println("DETECT");
    } else {
        Serial.println("NOT DETECT");
    }
    delay(200);
}
```