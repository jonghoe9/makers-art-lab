# 60. Data View

* 센서값을 그래프로 출력하기

## 아두이노 프로그램
* 센서 2개를 읽고 시리얼로 출력한다.
* 데이터는 쉼표 `, `로 구분한다.
* 데이터의 끝은 줄넘김 `\n` 이다.

```cpp title="sensorRead.ino" linenums="1" hl_lines="13"
void setup() {
    pinMode(LED_BUILTIN, OUTPUT);
    Serial.begin(115200);
}

void loop() {
    digitalWrite(LED_BUILTIN, HIGH);
    int val1 = analogRead(A0);
    int val2 = analogRead(A1);
    digitalWrite(LED_BUILTIN, LOW);
    Serial.print(val1);
    Serial.print(", ");
    Serial.println(val2);
}
```

## 프로세싱 프로그램

* 시리얼로 읽고 데이터를 그래프로 표현한다.
* 그래프 그리는 클라스를 사용한다.

=== "메인 코드"
    ```java title="serialGraph.pde" linenums="1"
    SensorGraphMatrix sgm1, sgm2;

    int cntDegree = 0;

    void setup() {
        size(1000, 500);
        
        sgm1 = new SensorGraphMatrix(60, 50, 700, 150);
        sgm1.setDataMaxNColor(0, width, color(255, 0, 0));
        sgm1.setDataMaxNColor(1, height, color(0, 0, 255));
        
        sgm2 = new SensorGraphMatrix("Mouse XYZ 20ms", 60, 280, 800, 150, 3, 20);
        sgm2.setDataMaxNColor(0, width, color(255, 0, 0));
        sgm2.setDataMaxNColor(1, height, color(0, 0, 255));
        sgm2.setDataMaxNColor(2, 360, color(0, 255, 0));
    }

    void draw() {
        background(255);
        
        if(sgm1.isUpdate()) {
            sgm1.add(0, mouseX);
            sgm1.add(1, mouseY);
        }
        if(sgm2.isUpdate()) {
            sgm2.add(0, mouseX);
            sgm2.add(1, mouseY);
            sgm2.add(2, 180 + sin(radians(cntDegree)) * 178);
            cntDegree++;
            if(cntDegree > 360) cntDegree = 0;
        }
        sgm1.draw();
        sgm2.draw();
    }

    void mousePressed() {
        if(sgm1.isClick()) sgm1.toggle();
        if(sgm2.isClick()) sgm2.toggle();
    }
    ```

=== "Class 코드"
    ```java title="serialGraphMatrix.pde" linenums="1"
    class SensorGraphMatrix {
        String name_tag;
        
        int posX;
        int posY;
        int boxW;
        int boxH;
        
        int cnt_data_set;
        int cnt_data;
        int g_height;
        
        float data[][];
        float dataMax[];
        color graphColor[];
        
        Boolean chk_update = true;
        long t_now;
        long t_pre[];
        int  t_int;
        
        float lv_trigger;
        float lv_squalch;
        Boolean trigger[];
        
        SensorGraphMatrix(int x, int y, int w, int h) {
            name_tag = "Sensor Graph";  // Default Graph Name
            cnt_data_set = 10;          // Default Data Count : 10
            t_int = 10;                 // Default Data Refresh : 10ms
            lv_trigger = 0.75;          // Default Trigger Point
            lv_squalch = 0.25;          // Default Squalch Point
            
            posX = x;
            posY = y;
            boxW = w;
            boxH = h;
            
            cnt_data = w -2;
            g_height = h -2;
            
            data = new float[cnt_data_set][cnt_data];
            dataMax = new float[cnt_data_set];
            graphColor = new color[cnt_data_set];
            trigger = new Boolean[cnt_data_set];
            
            t_now = 0;
            t_pre = new long[cnt_data_set];
            for(int i=0; i < cnt_data_set; i++) {
                t_pre[i] = 0;
                trigger[i] = false;
            }
        }
        
        SensorGraphMatrix(int x, int y, int w, int h, int data_set_count) {
            name_tag = "Sensor Graph";  // Default Graph Name
            t_int = 10;                 // Default Data Refresh : 10ms
            cnt_data_set = data_set_count;
            lv_trigger = 0.75;          // Default Trigger Point
            lv_squalch = 0.25;          // Default Squalch Point
            
            posX = x;
            posY = y;
            boxW = w;
            boxH = h;
            
            cnt_data = w -2;
            g_height = h -2;
            
            data = new float[cnt_data_set][cnt_data];
            dataMax = new float[cnt_data_set];
            graphColor = new color[cnt_data_set];
            trigger = new Boolean[cnt_data_set];
            
            t_now = 0;
            t_pre = new long[cnt_data_set];
            for(int i=0; i < cnt_data_set; i++) {
                t_pre[i] = 0;
                trigger[i] = false;
            }
        }
        
        
        SensorGraphMatrix(String tag, int x, int y, int w, int h,
                                    int data_set, int time_interval) {
            posX = x;
            posY = y;
            boxW = w;
            boxH = h;
            
            name_tag = tag;
            
            cnt_data_set = data_set;
            cnt_data = w -2;
            g_height = h -2;
            
            data = new float[cnt_data_set][cnt_data];
            dataMax = new float[cnt_data_set];
            graphColor = new color[cnt_data_set];
            trigger = new Boolean[cnt_data_set];
            
            t_now = 0;
            t_pre = new long[cnt_data_set];
            for(int i=0; i < cnt_data_set; i++) {
                t_pre[i] = 0;
                trigger[i] = false;
            }
            t_int = time_interval;
            
            lv_trigger = 0.75;          // Default Trigger Point
            lv_squalch = 0.25;          // Default Squalch Point
        }
        
        void setDataMaxNColor(int no, float max, color lineColor) {
            dataMax[no] = max;
            graphColor[no] = lineColor;
        }
        
        void setTitle(String title) {
            name_tag = trim(title);
        }
        
        void setInterval(int time_interval) {
            t_int = time_interval;
            if(t_int < 2) t_int = 2;
        }
        
        void setTrigger(float val) {
            lv_trigger = val;
        }
        
        void setSqualch(float val) {
            lv_squalch = val;
        }
        
        void draw() {
            color boxBorder = color(0);
            color boxCenterLine = color(127, 127, 127);
            fill(boxBorder);
            text(name_tag, posX, posY -10);
            noFill();
            stroke(boxBorder);
            rect(posX, posY, boxW, boxH);
            stroke(boxCenterLine);
            line(posX, posY + boxH /2, posX +boxW, posY + boxH /2);
            // Time Line
            for(int i=0; i <= 10; i++) {
                line(posX + (i * boxW / 10),  posY +boxH, posX + (i * boxW / 10), posY +boxH +10);
                fill(boxCenterLine);
                text(t_int * 10* (10-i), posX + (i * boxW / 10) -5,  posY +boxH +24);
                noFill();
            }
            
            for(int j=0; j < cnt_data_set; j++) {
                stroke(graphColor[j]);
                for(int i=0; i < cnt_data -1; i++) {
                    float x1 = posX +1 +i;
                    float x2 = posX +2 +i;
                    float y1 = posY +boxH -(g_height * (data[j][i]+1) / dataMax[j]);
                    float y2 = posY +boxH -(g_height * (data[j][i+1]+1) / dataMax[j]);
                    line(x1, y1, x2, y2);
                }
                fill(graphColor[j]);
                text(nf(data[j][cnt_data-1], 0, 3), posX +boxW +10, posY +boxH -1 -(g_height * (data[j][cnt_data-1]+1) / dataMax[j]));
                noFill();
            }
            // Trigger & Squalch
            stroke(boxCenterLine);
            line(posX, posY +(boxH * lv_trigger), posX +boxW, posY +(boxH * lv_trigger));
            line(posX, posY +(boxH * lv_squalch), posX +boxW, posY +(boxH * lv_squalch));
            fill(boxCenterLine);
            text("THRE", posX -32, posY +boxH -(boxH * lv_trigger) +4);
            text("GATE", posX -32, posY +boxH -(boxH * lv_squalch) +4);
            noFill();
            for(int i=0; i < cnt_data_set; i++) {
                stroke(graphColor[i]);
                if(trigger[i] == true) {
                    fill(graphColor[i]);
                    rect(posX + 5 +(30 * i) + (5 * i), posY + 5, 30, 20);
                }
            }
        }
        
        void add(int no_set, float val) {
            t_now = millis();
            if((t_now - t_pre[no_set] > t_int) && chk_update) {
                t_pre[no_set] = t_now;
                for(int i=0; i < cnt_data -1; i++) {
                    data[no_set][i] = data[no_set][i+1];
                }
                data[no_set][cnt_data -1] = val;
                if(val / dataMax[no_set] > lv_trigger) trigger[no_set] = true;
                else trigger[no_set] = false;
            }
        }
        
        Boolean isClick() {
            if((mouseX > posX && mouseX < posX + boxW) && (mouseY > posY && mouseY < posY + boxH)) return true;
            else return false;
        }
        
        Boolean isPause() {
            if(chk_update) return false;
            else return true;
        }
        
        Boolean isUpdate() {
        if(chk_update) return true;
        else return false;
        }
        
        void setPause() {
            chk_update = false;
        }
        
        void setUpdate() {
            chk_update = true;
        }
        
        void toggle() {
            if(chk_update) chk_update = false;
            else chk_update = true;
        }
    }
    ```


