# 2. 프로세싱 프로그래밍 1

* 시리얼 포트에서 문자열(String)을 읽는다.
* 입력되는 문자열은 '\n' 문자로 끝난다.
* 문자형식의 데이터를 숫자 데이터로 수정하여 저장한다.

```cpp title="p51_processing1.pde" linenums="1" hl_lines="18"
import processing.serial.*;

Serial port;

int sData = 0;

void setup() {
    size(255, 255);
    port = new Serial(this, "/dev/cu.usbmodem141011", 115200);
}

void draw() {
    background(255);
    println(sData);
}

void serialEvent(Serial port) {
    String str = port.readStringUntil('\n');
    if(str != null) {
        String s = trim(str);
        sData = int(s);
    }
}
```

---
[Project 0501](/07_Projects/Project_0501/)
