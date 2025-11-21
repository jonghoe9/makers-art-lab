# Camera Open

```java title="camera_open.pde" linenums="1" hl_lines="8"
import processing.video.*;

Capture cam;

void setup() {
  size(640, 480);
  cam = new Capture(this, width, height);
  //cam = new Capture(this, cameras[1]);
  //cam = new Capture(this, width, height, "pipeline:avfvideosrc device-index=2", 30);
  //cam = new Capture(this, "FaceTime HD Camera");
  cam.start();
}

void draw() {
  image(cam, 0, 0, width, height);
}

void captureEvent(Capture cam) {
  cam.read();
}
```