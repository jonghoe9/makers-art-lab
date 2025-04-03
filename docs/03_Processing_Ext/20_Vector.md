# 20. Vector 사용하기

* 클라스로 2개 원 그리기 코드에서, Class 부분만 Vector 방식으로 바꿨다.
* 벡터 방식은 원을 이동하거나 중력/마찰/충돌 등의 변화를 표현하기에 적합하다.
* 여기서는 기존 방식을 그대로 두고 Class만 수정해서 동작하는 것에 주목한다.
* 클라스 코드만 바꿨기 땨문애 클라스를 이용하는 다른 프로그램에서도 사용 가능하다.

## 벡터를 사용한 원 그림 2개

```java title="proc-020.pde" linenums="1" hl_lines="20-43"
ClockArrow ca1, ca2;

void setup() {
    size(640, 480);

    ca1 = new ClockArrow(width/2 -100, height/2, 150);
    ca2 = new ClockArrow(width/2 +100, height/2, 150);
}

void draw() {
    background(255);

    ca1.show(mouseX, mouseY);
    ca2.show(mouseX, mouseY);
    
    line(mouseX, 0, mouseX, height);
    line(0, mouseY, width, mouseY);
}

class ClockArrow {
    PVector vLoc;
    float r;
    float tx;
    float ty;
    
    ClockArrow(float posX, float posY, float dia) {
        r = dia/2;
        vLoc = new PVector(posX, posY);
    }
    
    void show(float x, float y) {
        PVector vTarget = new PVector(x, y);
        PVector vDir = PVector.sub(vTarget, vLoc);

        vDir.normalize();
        vDir.mult(r);
        tx = vDir.x;
        ty = vDir.y;
        
        circle(vLoc.x, vLoc.y, r*2);
        line(vLoc.x, vLoc.y, vLoc.x +tx, vLoc.y +ty);
    }
}
```

## 추가 코드 (아이디어)

```java title="CircleArrow.pde" linenums="1"
void update() {
    PVector vTarget = new PVector(mouseX, mouseY);
    PVector vDir = PVector.sub(vTarget, vLoc);
    vDir.normalize();
    vDir.mult(r);
    tx = vDir.x;
    ty = vDir.y;
    heading = vDir.heading();
}

void trace() {
    PVector vTarget = new PVector(mouseX, mouseY);
    PVector vDir = PVector.sub(vTarget, vLoc);
    if(vDir.mag() > r) {
        vDir.normalize();
        vDir.mult(1.23);
        vLoc.add(vDir);
    }
    vDir.normalize();
    vDir.mult(r);
    tx = vDir.x;
    ty = vDir.y;
}

void show() {
    circle(vLoc.x, vLoc.y, r*2);
    line(vLoc.x, vLoc.y, vLoc.x +tx, vLoc.y +ty);
}
```