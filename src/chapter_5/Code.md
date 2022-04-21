# Code

### Bezierkurve Grad 4

```js
let P0;
let P1;
let P2;
let P3;
const thickness = 25;

function setup() {
  createCanvas(800, 600);
  P0 = createVector(100, 100);
  P1 = createVector(300, 550);
  P2 = createVector(550, 150);
  P3 = createVector(750, 300);
}

function draw() {
  background(20);

  fill(255);
  textSize(20);
  text("Punkte anklicken zum bewegen!", 5, 20);

  noFill();
  stroke(100);
  strokeWeight(1);
  line(P0.x, P0.y, P1.x, P1.y);
  line(P1.x, P1.y, P2.x, P2.y);
  line(P2.x, P2.y, P3.x, P3.y);

  drawLooped();

  stroke(255);
  strokeWeight(3);
  bezier(P0.x, P0.y, P1.x, P1.y, P2.x, P2.y, P3.x, P3.y);

  noStroke();

  fill(255, 255, 0);
  ellipse(P3.x, P3.y, thickness, thickness);

  fill(0, 0, 255);
  ellipse(P2.x, P2.y, thickness, thickness);

  fill(0, 255, 0);
  ellipse(P1.x, P1.y, thickness, thickness);

  fill(255, 0, 0);
  ellipse(P0.x, P0.y, thickness, thickness);

  moveIfDragged();
}

function moveIfDragged() {
  if (
    pointCircle(pmouseX, pmouseY, P0.x, P0.y, thickness) ||
    pointCircle(pmouseX, pmouseY, P1.x, P1.y, thickness) ||
    pointCircle(pmouseX, pmouseY, P2.x, P2.y, thickness) ||
    pointCircle(pmouseX, pmouseY, P3.x, P3.y, thickness)
  )
    cursor(HAND);
  else cursor(ARROW);

  if (!mouseIsPressed || mouseButton != LEFT) return;

  if (pointCircle(pmouseX, pmouseY, P0.x, P0.y, thickness)) {
    P0.x = mouseX;
    P0.y = mouseY;
    cursor(MOVE);
    return;
  }

  if (pointCircle(pmouseX, pmouseY, P1.x, P1.y, thickness)) {
    P1.x = mouseX;
    P1.y = mouseY;
    cursor(MOVE);
    return;
  }

  if (pointCircle(pmouseX, pmouseY, P2.x, P2.y, thickness)) {
    P2.x = mouseX;
    P2.y = mouseY;
    cursor(MOVE);
    return;
  }

  if (pointCircle(pmouseX, pmouseY, P3.x, P3.y, thickness)) {
    P3.x = mouseX;
    P3.y = mouseY;
    cursor(MOVE);
  }
}

// @link http://www.jeffreythompson.org/collision-detection/point-circle.php
function pointCircle(px, py, cx, cy, r) {
  var distX = px - cx;
  var distY = py - cy;
  var distance = sqrt(distX * distX + distY * distY);

  if (distance <= r) {
    return true;
  }
  return false;
}
```

### Linear Bezier Animated T

```js
let P0;
let P1;

const thickness = 25;
const step = 0.005;

function setup() {
  createCanvas(800, 600);
  P0 = createVector(100, 100);
  P1 = createVector(600, 400);
}

let dt = 0;
function draw() {
  background(20);

  noFill();
  stroke(100);
  strokeWeight(1);
  line(P0.x, P0.y, P1.x, P1.y);

  stroke(255);
  strokeWeight(3);
  let max = map(sin(dt), -1, 1, 0, 1);

  let x = P0.x;
  let y = P0.y;

  let nx = (1 - max) * P0.x + max * P1.x;
  let ny = (1 - max) * P0.y + max * P1.y;

  line(x, y, nx, ny);

  dt += step;

  noStroke();

  fill(0, 0, 255);
  ellipse(nx, ny, thickness / 2.0, thickness / 2.0);

  fill(0, 255, 0);
  ellipse(P1.x, P1.y, thickness, thickness);

  fill(255, 0, 0);
  ellipse(P0.x, P0.y, thickness, thickness);

  fill(255);
  textSize(20);
  text("Punkte anklicken zum bewegen!", 5, 20);
  text("t=" + max, 5, 40);

  moveIfDragged();
}

function moveIfDragged() {
  if (
    pointCircle(pmouseX, pmouseY, P0.x, P0.y, thickness) ||
    pointCircle(pmouseX, pmouseY, P1.x, P1.y, thickness)
  )
    cursor(HAND);
  else cursor(ARROW);

  if (!mouseIsPressed || mouseButton != LEFT) return;

  if (pointCircle(pmouseX, pmouseY, P0.x, P0.y, thickness)) {
    P0.x = mouseX;
    P0.y = mouseY;
    cursor(MOVE);
    return;
  }

  if (pointCircle(pmouseX, pmouseY, P1.x, P1.y, thickness)) {
    P1.x = mouseX;
    P1.y = mouseY;
    cursor(MOVE);
  }
}

// @link http://www.jeffreythompson.org/collision-detection/point-circle.php
function pointCircle(px, py, cx, cy, r) {
  let distX = px - cx;
  let distY = py - cy;
  let distance = sqrt(distX * distX + distY * distY);

  if (distance <= r) {
    return true;
  }
  return false;
}
```

### Quadratic Bezier Animated T

```js
let P0;
let P1;
let P2;

const thickness = 25;
const step = 0.005;

function setup() {
  createCanvas(800, 600);
  P0 = createVector(100, 300);
  P1 = createVector(300, 100);
  P2 = createVector(600, 400);
}

let dt = 0;
function draw() {
  background(20);

  let max = map(sin(dt), -1, 1, 0, 1);

  let x = P0.x;
  let y = P0.y;

  let l0x = lerp(P0.x, P1.x, max);
  let l0y = lerp(P0.y, P1.y, max);

  let l1x = lerp(P1.x, P2.x, max);
  let l1y = lerp(P1.y, P2.y, max);

  let nx = (1 - max) * l0x + max * l1x;
  let ny = (1 - max) * l0y + max * l1y;

  noFill();
  stroke(100);
  strokeWeight(1);
  line(P0.x, P0.y, P1.x, P1.y);
  line(P1.x, P1.y, P2.x, P2.y);
  line(l0x, l0y, l1x, l1y);

  fill(100);
  noStroke();
  ellipse(l0x, l0y, thickness / 3.0, thickness / 3.0);
  ellipse(l1x, l1y, thickness / 3.0, thickness / 3.0);

  stroke(255);
  strokeWeight(3);
  let t = 0;
  let sx = x;
  let sy = y;
  while (t <= max) {
    t += step;
    let ex = lerp(lerp(P0.x, P1.x, t), lerp(P1.x, P2.x, t), t);
    let ey = lerp(lerp(P0.y, P1.y, t), lerp(P1.y, P2.y, t), t);

    line(sx, sy, ex, ey);
    sx = ex;
    sy = ey;
  }

  dt += step;

  noStroke();

  fill(0, 0, 255);
  ellipse(nx, ny, thickness / 2.0, thickness / 2.0);

  fill(255, 255, 0);
  ellipse(P2.x, P2.y, thickness, thickness);

  fill(0, 255, 0);
  ellipse(P1.x, P1.y, thickness, thickness);

  fill(255, 0, 0);
  ellipse(P0.x, P0.y, thickness, thickness);

  fill(255);
  textSize(20);
  text("Punkte anklicken zum bewegen!", 5, 20);
  text("t=" + max, 5, 40);

  moveIfDragged();
}

function moveIfDragged() {
  if (
    pointCircle(pmouseX, pmouseY, P0.x, P0.y, thickness) ||
    pointCircle(pmouseX, pmouseY, P1.x, P1.y, thickness) ||
    pointCircle(pmouseX, pmouseY, P2.x, P2.y, thickness)
  )
    cursor(HAND);
  else cursor(ARROW);

  if (!mouseIsPressed || mouseButton != LEFT) return;

  if (pointCircle(pmouseX, pmouseY, P0.x, P0.y, thickness)) {
    P0.x = mouseX;
    P0.y = mouseY;
    cursor(MOVE);
    return;
  }

  if (pointCircle(pmouseX, pmouseY, P1.x, P1.y, thickness)) {
    P1.x = mouseX;
    P1.y = mouseY;
    cursor(MOVE);
    return;
  }

  if (pointCircle(pmouseX, pmouseY, P2.x, P2.y, thickness)) {
    P2.x = mouseX;
    P2.y = mouseY;
    cursor(MOVE);
  }
}

// @link http://www.jeffreythompson.org/collision-detection/point-circle.php
function pointCircle(px, py, cx, cy, r) {
  let distX = px - cx;
  let distY = py - cy;
  let distance = sqrt(distX * distX + distY * distY);

  if (distance <= r) {
    return true;
  }
  return false;
}
```

### Quadratic Bezier Changing T

```js
let P0;
let P1;
let P2;

const thickness = 25;
let step = 3;
let start;

function setup() {
  createCanvas(800, 600);
  P0 = createVector(100, 300);
  P1 = createVector(300, 100);
  P2 = createVector(600, 400);
  start = millis();
}
function draw() {
  background(20);

  let delta = 1 / step;

  let x = P0.x;
  let y = P0.y;

  noFill();
  stroke(100);
  strokeWeight(1);
  line(P0.x, P0.y, P1.x, P1.y);
  line(P1.x, P1.y, P2.x, P2.y);

  beginShape();
  vertex(P0.x, P0.y);
  quadraticVertex(P1.x, P1.y, P2.x, P2.y);
  endShape();

  stroke(255);
  strokeWeight(3);
  let t = 0;
  let sx = x;
  let sy = y;
  while (t <= 1.0) {
    let ex = lerp(lerp(P0.x, P1.x, t), lerp(P1.x, P2.x, t), t);
    let ey = lerp(lerp(P0.y, P1.y, t), lerp(P1.y, P2.y, t), t);

    line(sx, sy, ex, ey);
    sx = ex;
    sy = ey;

    t += delta;
  }

  line(sx, sy, P2.x, P2.y);

  if (millis() - start > 1000) {
    step += 1;
    start = millis();
  }

  if (step > 20) {
    step = 3;
  }

  noStroke();

  fill(255, 255, 0);
  ellipse(P2.x, P2.y, thickness, thickness);

  fill(0, 255, 0);
  ellipse(P1.x, P1.y, thickness, thickness);

  fill(255, 0, 0);
  ellipse(P0.x, P0.y, thickness, thickness);

  fill(255);
  textSize(20);
  text("Punkte anklicken zum bewegen!", 5, 20);
  text("Amount of Lines: " + step, 5, 40);

  moveIfDragged();
}

function moveIfDragged() {
  if (
    pointCircle(pmouseX, pmouseY, P0.x, P0.y, thickness) ||
    pointCircle(pmouseX, pmouseY, P1.x, P1.y, thickness) ||
    pointCircle(pmouseX, pmouseY, P2.x, P2.y, thickness)
  )
    cursor(HAND);
  else cursor(ARROW);

  if (!mouseIsPressed || mouseButton != LEFT) return;

  if (pointCircle(pmouseX, pmouseY, P0.x, P0.y, thickness)) {
    P0.x = mouseX;
    P0.y = mouseY;
    cursor(MOVE);
    return;
  }

  if (pointCircle(pmouseX, pmouseY, P1.x, P1.y, thickness)) {
    P1.x = mouseX;
    P1.y = mouseY;
    cursor(MOVE);
    return;
  }

  if (pointCircle(pmouseX, pmouseY, P2.x, P2.y, thickness)) {
    P2.x = mouseX;
    P2.y = mouseY;
    cursor(MOVE);
  }
}

// @link http://www.jeffreythompson.org/collision-detection/point-circle.php
function pointCircle(px, py, cx, cy, r) {
  let distX = px - cx;
  let distY = py - cy;
  let distance = sqrt(distX * distX + distY * distY);

  if (distance <= r) {
    return true;
  }
  return false;
}
```

### Railway Curveform ganze Klasse

```java
package de.pixelboystm.railway.renderer.forms;

import de.pixelboystm.railway.util.Color;
import de.pixelboystm.railway.scene.Scene;
import de.pixelboystm.railway.renderer.Shader;
import org.joml.Math;
import org.joml.Vector2f;
import org.joml.Vector4f;
import org.lwjgl.BufferUtils;

import java.nio.FloatBuffer;
import java.nio.IntBuffer;

import static org.lwjgl.opengl.GL20.*;
import static org.lwjgl.opengl.GL30.glBindVertexArray;
import static org.lwjgl.opengl.GL30.glGenVertexArrays;

public class CurveForm implements FormBase{
    private int x1,x2,x3, y1,y2,y3; float width;
    private Vector4f color;
    private Scene sc;

    private float[] origin_x;
    private float[] origin_y;

    private Shader shader;
    private int vaoID, vboID, eboID;
    private float[] vertexArray;
    private int[] elementArray;

    public CurveForm(int x1, int y1, int x2, int y2, int x3, int y3, float width, Color color) {
        this.x1 = x1;
        this.x2 = x2;
        this.x3 = x3;
        this.y1 = y1;
        this.y2 = y2;
        this.y3 = y3;
        this.width = width;
        this.color = color.get();
    }

    @Override
    public void init(Scene scene) {
        this.sc = scene;

        int density = 100;

        origin_x = new float[(int) density];
        origin_y = new float[(int) density];

        for (int i = 0; i < density; i++) {
            float t = i/(density * 1.0f);
            float x = (1 - t)*(1 - t) * x1 + 2*t*(1-t)*x2 + (t*t) * x3;
            float y = (1 - t)*(1 - t) * y1 + 2*t*(1-t)*y2 + (t*t) * y3;
            origin_x[i] = x;
            origin_y[i] = y;
        }

        //vao Array;
        vertexArray = new float[density * 7 * 2];
        int index = 0;
        int point = 0;
        float upper_rad1 = (float) (Math.atan2(y2- y1, x2 - x1 ) - (Math.PI / 2.0f));
        float lower_rad1 = (float) (Math.atan2(y2- y1, x2 - x1 ) + (Math.PI / 2.0f));

        vertexArray[index    ] = origin_x[point] + Math.cos(lower_rad1) * width;
        vertexArray[index + 1] = origin_y[point] + Math.sin(lower_rad1) * width;
        vertexArray[index + 2] = 0.0f;

        vertexArray[index + 3] = color.x;
        vertexArray[index + 4] = color.y;
        vertexArray[index + 5] = color.z;
        vertexArray[index + 6] = color.w;
        index += 7;
        vertexArray[index    ] = origin_x[point] + Math.cos(upper_rad1) * width;
        vertexArray[index + 1] = origin_y[point] + Math.sin(upper_rad1) * width;
        vertexArray[index + 2] = 0.0f;

        vertexArray[index + 3] = color.x;
        vertexArray[index + 4] = color.y;
        vertexArray[index + 5] = color.z;
        vertexArray[index + 6] = color.w;
        index += 7;
        point++;

        for (int i = 0; i < density - 2; i++) {
            float y_dist = origin_y[point] - origin_y[point - 1];
            float x_dist = origin_x[point] - origin_x[point - 1];
            float ru = (float) (Math.atan2(y_dist, x_dist) - (Math.PI / 2.0f));
            float rl = (float) (Math.atan2(y_dist, x_dist) + (Math.PI / 2.0f));

            vertexArray[index    ] = origin_x[point] + Math.cos(rl) * width;
            vertexArray[index + 1] = origin_y[point] + Math.sin(rl) * width;
            vertexArray[index + 2] = 0.0f;

            vertexArray[index + 3] = color.x;
            vertexArray[index + 4] = color.y;
            vertexArray[index + 5] = color.z;
            vertexArray[index + 6] = color.w;
            index += 7;

            vertexArray[index    ] = origin_x[point] + Math.cos(ru) * width;
            vertexArray[index + 1] = origin_y[point] + Math.sin(ru) * width;
            vertexArray[index + 2] = 0.0f;

            vertexArray[index + 3] = color.x;
            vertexArray[index + 4] = color.y;
            vertexArray[index + 5] = color.z;
            vertexArray[index + 6] = color.w;
            index += 7;
            point++;
        }

        float upper_rad2 = (float) (Math.atan2(y3- y2, x3 - x2 ) - (Math.PI / 2.0f));
        float lower_rad2 = (float) (Math.atan2(y3- y2, x3 - x2 ) + (Math.PI / 2.0f));

        vertexArray[index    ] = origin_x[point] + Math.cos(lower_rad2) * (width / 2.0f);
        vertexArray[index + 1] = origin_y[point] + Math.sin(lower_rad2) * (width / 2.0f);
        vertexArray[index + 2] = 0.0f;

        vertexArray[index + 3] = color.x;
        vertexArray[index + 4] = color.y;
        vertexArray[index + 5] = color.z;
        vertexArray[index + 6] = color.w;
        index += 7;
        vertexArray[index    ] = origin_x[point] + Math.cos(upper_rad2) * (width / 2.0f);
        vertexArray[index + 1] = origin_y[point] + Math.sin(upper_rad2) * (width / 2.0f);
        vertexArray[index + 2] = 0.0f;

        vertexArray[index + 3] = color.x;
        vertexArray[index + 4] = color.y;
        vertexArray[index + 5] = color.z;
        vertexArray[index + 6] = color.w;

        //ebo Array;
        elementArray = new int[(density - 1) * 6];
        int inicie = 0;
        int from = 0;
        for (int i = 0; i < density - 1; i++) {
            elementArray[inicie] = from;
            elementArray[inicie + 1] = from + 1;
            elementArray[inicie + 2] = from + 2;

            elementArray[inicie + 3] = from + 1;
            elementArray[inicie + 4] = from + 3;
            elementArray[inicie + 5] = from + 2;

            inicie += 6;
            from += 2;
        }

        shader = Shader.getShader("std.glsl");

        vaoID = glGenVertexArrays();
        glBindVertexArray(vaoID);

        FloatBuffer vertexBuffer = BufferUtils.createFloatBuffer(vertexArray.length);
        vertexBuffer.put(vertexArray).flip();

        vboID = glGenBuffers();
        glBindBuffer(GL_ARRAY_BUFFER, vboID);
        glBufferData(GL_ARRAY_BUFFER, vertexBuffer, GL_STATIC_DRAW);

        IntBuffer elementBuffer = BufferUtils.createIntBuffer(elementArray.length);
        elementBuffer.put(elementArray).flip();

        eboID = glGenBuffers();
        glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, eboID);
        glBufferData(GL_ELEMENT_ARRAY_BUFFER, elementBuffer, GL_STATIC_DRAW);

        int positionSize = 3;
        int colorSize = 4;
        int floatSizeBytes = Float.BYTES;
        int vertexSizeBytes = (positionSize + colorSize) * floatSizeBytes;
        glVertexAttribPointer(0, positionSize, GL_FLOAT, false, vertexSizeBytes, 0);
        glEnableVertexAttribArray(0);
        glVertexAttribPointer(1,colorSize, GL_FLOAT, false, vertexSizeBytes,positionSize * floatSizeBytes);
        glEnableVertexAttribArray(1);
    }

    @Override
    public void render() {
        shader.bind();
        shader.uploadMat4f("uProj", sc.getWindow().projectionMatrix);
        shader.uploadMat4f("uView", sc.getViewMatrix());

        glBindVertexArray(vaoID);

        glEnableVertexAttribArray(0);
        glEnableVertexAttribArray(1);

        glDrawElements(GL_TRIANGLES, elementArray.length, GL_UNSIGNED_INT, 0);

        glDisableVertexAttribArray(0);
        glDisableVertexAttribArray(1);

        glBindVertexArray(0);

        shader.unbind();

    }

    @Override
    public void gui() {
       ImGui.begin("Curve");
       int[] x = new int[]{x1,x2,x3};
       int[] y = new int[]{y1,y2,y3};
       ImGui.sliderInt3("x", x, - 800, 800);
       ImGui.sliderInt3("y", y, - 800, 800);
       x1 = x[0];
       x2 = x[1];
       x3 = x[2];
       y1 = y[0];
       y2 = y[1];
       y3 = y[2];
       ImGui.end();
    }

    @Override
    public boolean overlapsPoint(Vector2f p) {
        for (int i = 0; i < origin_x.length; i++) {
            if (p.distance(origin_x[i], origin_y[i]) <= width)
                return true;
        }
        return false;
    }

    @Override
    public Vector2f getStart() {
        return new Vector2f(x1,y1);
    }

    @Override
    public Vector2f getEnd() {
        return new Vector2f(x3,y3);
    }

    public Vector2f getControlPoint() {return new Vector2f(x2,y2);}
}
```