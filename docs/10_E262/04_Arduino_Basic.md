# 0922_Arduino Basic

### 변수 사용
```cpp title="ew_0401.ino" linenums="1" hl_lines="4"
//
// LED 핀 번호를 변수로 대체하여 변화주기
//
const int led_pin = 9;

void setup() {
  pinMode(led_pin, OUTPUT);
}

void loop() {
  analogWrite(led_pin, 30);
  delay(500);
  analogWrite(led_pin, 255);
  delay(500);
}
```

```cpp title="ew_0402.ino" linenums="1"
//
// RGB LED 사용하기
//
#define led_r 9
#define led_g 10
#define led_b 11

void setup() {
  pinMode(led_r, OUTPUT);
  pinMode(led_g, OUTPUT);
  pinMode(led_b, OUTPUT);
}

void loop() {
  digitalWrite(led_r, HIGH);
  digitalWrite(led_g, LOW);
  digitalWrite(led_b, LOW);
  delay(300);
  digitalWrite(led_r, LOW);
  digitalWrite(led_g, HIGH);
  digitalWrite(led_b, LOW);
  delay(300);
  digitalWrite(led_r, LOW);
  digitalWrite(led_g, LOW);
  digitalWrite(led_b, HIGH);
  delay(300);
}
```

```cpp title="ew_0403.ino" linenums="1"
//
// FOR Loop 사용해 밝기 변화
//
const int led_pin = 9;

void setup() {
  pinMode(led_pin, OUTPUT);
}

void loop() {
  for(int i=0; i < 255; i++) {
    analogWrite(led_pin, i);
    delay(10);
  }
  delay(500);
}
```

```cpp title="ew_0404.ino" linenums="1"
//
// 입력 볼륨 사용하기
//
#define vol_pin A0
#define led_r 9

void setup() {
  pinMode(led_r, OUTPUT);
}

void loop() {
  int val = analogRead(A0);
  val = (int)(val / 4);       // 입력값 범위 0~1023, LED 밝기 0~255
  analogWrite(led_r, val);
}
```

```cpp title="ew_0405.ino" linenums="1"
//
// 시리얼 통신으로 입력 데이터 그래프와 숫자로 표시하기
//
#define vol_pin A0
#define led_r 9

void setup() {
  Serial.begin(115200);
  pinMode(led_r, OUTPUT);
}

void loop() {
  int data = analogRead(A0);
  int val = (int)(data / 4);
  analogWrite(led_r, val);
  Serial.print("0, 1023, ");
  Serial.print(data);
  Serial.print(", ");
  Serial.println(val);
}
```