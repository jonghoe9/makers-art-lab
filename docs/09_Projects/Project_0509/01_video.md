# Code 1: Video Control

* 프로세싱으로 동영상 재생하기

## 동영상 재생 기본 형식

* 동영상이지만, '여러장의 이미지' 개념으로 처리한다.
* video 라이브러리 임포트하기 (line 1)
* Movie 클라스 변수 지정 (line 3)
* 클라스 변수 초기화 (line 7)
* 동영상 파일은 프로세싱 스케치가 있는 폴더 안에 `data` 폴더를 만들어 넣는다.
* 동영상을 재생하면 (line 8), `movieEvent` 가 발생한다. (line 15)
* `movieEvent` 이벤트 핸들러는 다음 프레임을 읽는다.
* `draw()`에서 메모리에 로드된 이미지를 출력한다. (line 12)
* 이벤트 발생과 이미지 출력을 반복한다.


```cpp title="p59_video1.pde" linenums="1" hl_lines="12"
import processing.video.*;

Movie video1;

void setup() {
    size(800, 500);
    video1 = new Movie(this, "6162.mov");
    video1.play();
}

void draw() {
    image(video1, 0, 0);
}

void movieEvent(Movie m) {
    m.read();
}

```

## 동영상 정보 출력

* 동영상 진행 상태를 출력한다. (전체 시간과 진행 시간 표시)
* 가로x세로 비율을 16:9로 출력하기 위해 화면 크기를 조절하고, 이미지 영역이 아닌 곳에 동영상 정보를 표시한다.

```cpp title="p59_video2.pde" linenums="1" hl_lines="16"
import processing.video.*;

Movie video1;

int scrW, scrH;

void setup() {
    size(800, 500);
    scrW = width;
    scrH = (int)(width * 9/16);
    video1 = new Movie(this, "6162.mov");
    video1.play();
}

void draw() {
    background(0);
    image(video1, 0, 0, scrW, scrH);
    text(video1.time(),     20, scrH +16);
    text(video1.duration(), 20, scrH +32);
}

void movieEvent(Movie m) {
    m.read();
}

```

## 동영상 반복 재생

* 동영상 남은 시간이 0.1초 미민일 때 다시 처음부터 재생한다.
* 반복 횟수를 화면에 표시한다.

```cpp title="p59_video3.pde" linenums="1" hl_lines="21-24"
import processing.video.*;

Movie video1;

int scrW, scrH;
int cntLoop = 1;

void setup() {
    size(800, 500);
    scrW = width;
    scrH = (int)(width * 9/16);
    video1 = new Movie(this, "6162.mov");
    video1.play();
}

void draw() {
    background(0);
    image(video1, 0, 0, scrW, scrH);
    float vt = video1.time(); 
    float vd = video1.duration();
    if(vd - vt < 0.1) {
        video1.jump(0);
        cntLoop++;
    }
    text(vt, 20, scrH +15);
    text(vd, 20, scrH +30);
    text(cntLoop, 20, scrH + 45);
}

void movieEvent(Movie m) {
    m.read();
}

```


## 동영상 마우스로 컨트롤 하기

* 동영상 남은 시간이 0.1초 미민일 때 다시 처음부터 재생한다.
* 반복 횟수를 화면에 표시한다.
* 화면 왼쪽 15%를 클릭하면 5초 전으로 이동
* 화면 오른쪽 15%를 클릭하면 5초 앞으로 이동
* 화면 중앙을 클릭하면 PAUSE, 다시 클릭하면 RESUME
* 화면 아래쪽 구역, 영상이 아닌 구역을 누르면 STOP (포지션 0으로 이동하여 Pause)

```cpp title="p59_video4.pde" linenums="1" hl_lines="13-15"
import processing.video.*;

Movie video1;

int scrW, scrH, scrRew, scrFF;
int cntLoop = 1;
Boolean chkPause = false; 

void setup() {
    //fullScreen();
    size(800, 500);
    scrW = width;
    scrH = (int)(width * 9/16);
    scrRew = (int)(width * 15/100);
    scrFF =  (int)(width * 85/100);
    video1 = new Movie(this, "6162.mov");
    video1.play();
}

void draw() {
    background(0);
    image(video1, 0, 0, scrW, scrH);
    float vt = video1.time(); 
    float vd = video1.duration();
    if(vd - vt < 0.1) {
        video1.jump(0);
        cntLoop++;
    }
    text(vt, 20, scrH +15);
    text(vd, 20, scrH +30);
    text(cntLoop, 20, scrH + 45);
}

void movieEvent(Movie m) {
    m.read();
}

void mousePressed() {
    if(mouseX < scrRew) {
        // Rewind
        float jt = video1.time() - 5.0;
        if(jt < 0) jt = 0;
        video1.jump(jt);
    } else if(mouseX > scrFF) {
        // Fast Forward
        float jt = video1.time() + 5.0;
        if(jt > video1.duration()) jt = video1.duration();
        video1.jump(jt);
    } else {
        // Pause & Resume
        if(chkPause) {
            video1.play();
            chkPause = false;
        } else {
            // Stop
            if(mouseY > scrH) {
                video1.jump(0);
            }
            video1.pause();
            chkPause = true;
        }
    }
}
```