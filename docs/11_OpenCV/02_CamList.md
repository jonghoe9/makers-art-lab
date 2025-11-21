# Camera List

```java title="camera_list.pde" linenums="1" hl_lines="8"
import processing.video.*;

Capture cam;

String[] CamerasList;

void setup() {
  CamerasList = Capture.list();
  int cntCameras = CamerasList.length;
  println("Cameras Count : " + cntCameras);
  for(int i = 0; i < cntCameras; i++) {
    println(i + " : " + CamerasList[i]);
  }
}

void draw() {
}
```