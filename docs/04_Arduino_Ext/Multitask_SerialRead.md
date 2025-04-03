# Multi Task & Serial Read (예정)

```cpp title="serialWrite.ino" linenums="1" hl_lines="5"

unsigned long t = 0;

void setup() {
    Serial.begin(115200);
}

void loop() {
    while(Serial.available() > 0) {
        int val1 = Serial.parseInt();
        if(Serial.read() == '\n') {
            // Do Something
        }
    }
    // Multi Task, 100ms 마다 체크
    if(millis() -t > 100) {
        t = millis();
        // Do Something
    }
}
```

