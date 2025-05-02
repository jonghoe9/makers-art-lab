# Code

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
    //myMovie.pause();
    myMovie.stop();
}

void mouseReleased() {
    //myMovie.jump(1);
    myMovie.play();
}
```