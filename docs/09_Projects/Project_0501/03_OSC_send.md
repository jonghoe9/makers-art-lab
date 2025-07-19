# 3. OSC Send

* 시리얼로 받은 숫자 데이터를 OSC 형식으로 보낸다.
* 보낼 곳의 IP 주소, 포트 번호가 필요하다.
* 여기서는 프로세싱과 같은 컴퓨터에 있는 Mad Mapper를 대상으로 한다.
* [**프로세싱 프로그램1**](/07_Projects/Project_0501/02_Processing/)에 [**OSC 보내기 기능**](/03_Processing_Ext/31_OSC_send/)을 추가했다.

```cpp title="p51_processing2.pde" linenums="1" hl_lines="19-20"
import processing.serial.*;
import oscP5.*;
import netP5.*;

Serial port;
int sData = 0;

OscP5 osc;
NetAddress dest;

int portNo = 9001;
int portDest = 8010;
String ipDest = "localhost";
String oscAddr = "/dist";

void setup() {
    size(255, 255);
    port = new Serial(this, "/dev/cu.usbmodem141011", 115200);
    osc = new OscP5(this, portNo);
    dest = new NetAddress(ipDest, portDest);
}

void draw() {
    background(255);
    println(sData);
    oscDistSend();
}

void serialEvent(Serial port) {
    String str = port.readStringUntil('\n');
    if(str != null) {
        String s = trim(str);
        sData = int(s);
    }
}

void oscDistSend() {
    OscMessage msg = new OscMessage(oscAddr);
    msg.add((int) sData);
    osc.send(msg, dest);
}

```

---
[Project 0501](../)
