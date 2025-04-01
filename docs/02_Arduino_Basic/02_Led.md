# LED 켜보기

```cpp title="led-blink.ino" linenums="1" hl_lines="1"
const int led = 13;

void setup() {
  pinMode(led, OUTPUT);
}

void loop() {
  digitalWrite(led, HIGH);
  delay(500);

  digitalWrite(led, LOW);
  delay(500);
}
```

```cpp title="led-analog.ino" linenums="1" hl_lines="1"
const int led = 13;

void setup() {
  pinMode(led, OUTPUT);
}

void loop() {
  analogWrite(led, 550);
  delay(500);

  analogWrite(led, 250);
  delay(500);
}
```

```cpp title="led3color.ino" linenums="1" hl_lines="1"
const int red   = 9;
const int green = 10;
const int blue  = 11;

void setup() {
  pinMode(red, OUTPUT);
  pinMode(green, OUTPUT);
  pinMode(blue, OUTPUT);
}

void loop() {
  analogWrite(red, 250);
  analoglWrite(green, 150);
  analogWrite(blue, 100);
  delay(500);

  analogWrite(red, 100);
  analogWrite(green, 250);
  analogWrite(blue, 150);
  delay(500);
}
```
