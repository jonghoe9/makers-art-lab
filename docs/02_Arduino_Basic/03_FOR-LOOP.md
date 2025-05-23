# 3. FOR-Loop

서서히 밝아지고 서서히 꺼지는 LED를 구현해 보자.

## 코드 1. LED 밝기 조절

![빵판에 LED와 저항 연결](../img/ard-led-blink.png)

```cpp title="led-for-loop1.ino" linenums="1" hl_lines="1"
const int led_pin = 13;

void setup() {
  pinMode(led_pin, OUTPUT);
}

void loop() {
  for(int i=0; i <= 255; i++) {
    analogWrite(led_pin, i);
    delay(10);
  }
  for(int i=255; i >= 0; i--) {
    analogWrite(led_pin, i);
    delay(10);
  }
}
```

## 코드 2. LED 3구성, 컬러 밝기 조절

![빵판에 LED와 저항 연결](../img/ard-led3color.png)

```cpp title="led-for-loop2.ino" linenums="1" hl_lines="1"
const int red = 9;
const int green = 10;
const int blue = 11;

void setup() {
  pinMode(red, OUTPUT);
  pinMode(green, OUTPUT);
  pinMode(blue, OUTPUT);
}

void loop() {
  for(int i=0; i <= 255; i++) {
    analogWrite(red, i);
    analogWrite(green, 255-i);
    analogWrite(blue, 150);
    delay(10);
  }
  for(int i=255; i >= 0; i--) {
    analogWrite(red, i);
    analogWrite(green, 255-i);
    analogWrite(blue, 50);
    delay(10);
  }
}
```
