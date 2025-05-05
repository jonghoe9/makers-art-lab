# Code 1: 동영상 재생

* 프로세싱에서 동영상 파일 재생하기


## 동영상 재생, 마우스 컨트롤
* 마우스 클릭하면 Pause
* 마우스 클릭 풀면 Resume

```cpp title="p59_videoPlayer.pde" linenums="1" hl_lines="6"
import processing.video.*;

Movie myMovie;

void setup() {
    size(1280, 720);
    myMovie = new Movie(this, "video1.mov");
    myMovie.play();
}

void draw() {
    image(myMovie, 0, 0);
}

void movieEvent(Movie m) {
    m.read();
}

void mousePressed() {
    myMovie.pause();
    //myMovie.stop();       // 다시 처음부터 가려면 Stop 사용
}

void mouseReleased() {
    myMovie.jump(1);
    myMovie.play();
}
```


## 동영상 2개, 클릭하면 다른 영상 나오기

* 동영상 2개 사용
* 기본 영상은 반복 재생된다.
* 클릭하면 다른 영상이 처음부터 재생된다.
* 클릭 영상을 길게 틀고 있으면 반복 재생, 클릭할 때마다 처음부터 재생

```cpp title="p59_videoPlayer2.pde" linenums="1" hl_lines="29"
import processing.video.*;

Movie movNor, movTrg;

int chk = 0;

void setup() {
    size(1280, 720);
    movNor = new Movie(this, "6044.mov");
    movTrg = new Movie(this, "6162.mov");
    movNor.loop();
    movTrg.loop();
}

void draw() {
    if(chk == 1) {
        image(movTrg, 0, 0);
    } else {
        image(movNor, 0, 0);
    }
}

void movieEvent(Movie m) {
    m.read();
}

void mousePressed() {
    chk = 1;
    movTrg.jump(0);
}

void mouseReleased() {
    chk = 0;
}
```

## 동영상 2개, 수정 버전

* Loop 반복하다 멈추는 현상이 있어서 Loop 방식을 수정함
* Video 크기를 16x9 사이즈로 고정
* 영상 가로 크기에 맞게 자동 수정

```cpp title="p59_videoPlayer3.pde" linenums="1" hl_lines="16"
import processing.video.*;

Movie movNor, movTrg;

float loopPos = 0.2;
String movFile1 = "6162.mov";
String movFile2 = "5824.mov";

int scrW, scrH;

int chk = 0;

void setup() {
    size(1280, 720);
    scrW = width;
    scrH = (int)(width *9/16);
    movInit();
    movNor.play();
    movTrg.pause();
}

void draw() {
    if(chk == 1) {
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

void movInit() {
    movNor = new Movie(this, movFile1);
    movTrg = new Movie(this, movFile2);
}

void mousePressed() {
    chk = 1;
    movTrg.jump(0);
    movTrg.play();
}

void mouseReleased() {
    chk = 0;
    movTrg.pause();
    movTrg.jump(0);
}
```


## 동영상 2개, 디버깅 버전

* 화면을 3개 구성하여 동영상 확인
* Loop 횟수를 카운트하고, 화면에 표시

```cpp title="p59_videoPlayer4.pde" linenums="1" hl_lines="16"
import processing.video.*;

Movie movNor, movTrg;

float loopPos = 0.2;
String movFile1 = "6162.mov";
String movFile2 = "5824.mov";

int scrW, scrH, scrW2, scrH2;

int chk = 0;
int cnt = 1;
int cnt2 = 0;

void setup() {
    size(1280, 720+360);
    scrW = width;
    scrH = (int)(width *9/16);
    scrW2 = (int)(scrW /2);
    scrH2 = (int)(scrH /2);
    movInit();
    movNor.play();
    movTrg.pause();
}

void draw() {
    if(chk == 1) {
        image(movTrg, 0, 0, scrW, scrH);
        image(movNor, 0, scrH, scrW2, scrH2);
        image(movTrg, scrW2, scrH, scrW2, scrH2);
    } else {
        image(movNor, 0, 0, scrW, scrH);
        image(movNor, 0, scrH, scrW2, scrH2);
        image(movTrg, scrW2, scrH, scrW2, scrH2);
    }
    if(movNor.duration() - movNor.time() < loopPos) {
      movNor.jump(0);
      cnt++;
    }
    text(cnt,  20, scrH +30);
    text(cnt2, scrW2 +20, scrH +30);
}

void movieEvent(Movie m) {
    m.read();
}

void movInit() {
    movNor = new Movie(this, movFile1);
    movTrg = new Movie(this, movFile2);
}

void mousePressed() {
    chk = 1;
    cnt2++;
    movTrg.jump(0);
    movTrg.play();
}

void mouseReleased() {
    chk = 0;
    movTrg.pause();
    movTrg.jump(0);
}
```