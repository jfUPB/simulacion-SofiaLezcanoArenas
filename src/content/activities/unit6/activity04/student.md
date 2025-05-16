## Aplicación creativa e inesperada de agentes autónomos
### Algoritmo
flow fields

### Concepto
#### Lluvia de ideas
- Lluvia
- Tormenta
- Fuego
- Humo
#### Final
Danza de fuego. El usuario deberá hacer clic sobre el área del lienzo, así se encenderá la fogata y comenzará la danza de fuego. Al hacer clic nuevamente en la pantalla el flow field cambiará aleatoriamente, variando aún más el movimiento del fuego. El color del fuego cambiará según la altura, mientras más abajo más amarillo rojizo será y mientras más alto, más azul será, pues este fuego es más potente que el amarillo.

### Lógica
Se generarán partículas desde el centro de la fogata que seguirán un flow field invisible para el usuario, representando una danza de fuego.

### Código
**flowfield.js**
``` js
class FlowField {
  constructor(cols, rows, scl, inc) {
    this.cols = cols;
    this.rows = rows;
    this.scl = scl;
    this.inc = inc;
    this.zoff = 0;
    this.field = new Array(cols * rows);
    this.update();
  }

  update() {
    let yoff = 0;
    for (let y = 0; y < this.rows; y++) {
      let xoff = 0;
      for (let x = 0; x < this.cols; x++) {
        let angle = noise(xoff, yoff, this.zoff) * TWO_PI * 2;
        let v = p5.Vector.fromAngle(angle);
        v.setMag(0.5); // magnitud baja
        this.field[x + y * this.cols] = v;
        xoff += this.inc;
      }
      yoff += this.inc;
    }
    this.zoff += 0.003;
  }

  lookup(pos) {
    let col = constrain(floor(pos.x / this.scl), 0, this.cols - 1);
    let row = constrain(floor(pos.y / this.scl), 0, this.rows - 1);
    return this.field[col + row * this.cols].copy();
  }
}
```

**sketch.js**
``` js
let flowfield;
let sparks = [];
let cols, rows;
let inc = 0.1;
let scl = 20;
let origin;
let fireStarted = false;

function setup() {
  createCanvas(550, 450);
  cols = floor(width / scl);
  rows = floor(height / scl);
  flowfield = new FlowField(cols, rows, scl, inc);
  origin = createVector(width / 2, height - 10);
}

function draw() {
  background(0, 30);

  if (fireStarted) {
    flowfield.update();

    // Crear nuevas partículas constantemente
    for (let i = 0; i < 5; i++) {
      sparks.push(new Spark(origin.x, origin.y));
    }

    for (let i = sparks.length - 1; i >= 0; i--) {
      let spark = sparks[i];
      spark.follow(flowfield);
      spark.update();
      spark.show();
      spark.edges(origin);

      if (spark.isOffscreen()) {
        sparks.splice(i, 1);
      }
    }
  } else {
    fill(255);
    textAlign(CENTER);
    textSize(16);
    text("Haz clic para encender la fogata", width / 2, height / 2);
  }
}

function mousePressed() {
  if (!fireStarted) {
    fireStarted = true;
  } else {
    flowfield.zoff += random(100);
  }
}
```

**spark.js**
``` js
class Spark {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.vel.mult(random(0.5, 1));
    this.acc = createVector(0, 0);
    this.maxSpeed = 2;
    this.r = random(6, 12); // Tamaño de la partícula
    this.alpha = 255;
    this.color = color(random(200, 255), random(100, 150), 0, this.alpha);
  }

  follow(flowfield) {
    let x = floor(this.pos.x / flowfield.scl);
    let y = floor(this.pos.y / flowfield.scl);
    let index = x + y * flowfield.cols;
    if (flowfield.field[index]) {
      let force = flowfield.field[index];
      this.applyForce(force);
    }
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.alpha -= 1;
  }

  show() {
     noStroke();

  let t = map(this.pos.y, 0, height, 0, 1); // 0 en la parte superior, 1 en la inferior

  // Color azul potente a rojo/amarillo cálido
  let fromColor = color(0, 150, 255); // Azul
  let toColor = color(255, 150, 0);   // Amarillo/Naranja

  let mixed = lerpColor(fromColor, toColor, t);
  mixed.setAlpha(this.alpha);

  fill(mixed);
  ellipse(this.pos.x, this.pos.y, this.r);
  }

  isOffscreen() {
    return this.alpha <= 0;
  }

  edges(origin) {
    if (this.pos.y < 0 || this.pos.x < 0 || this.pos.x > width) {
      this.alpha = 0;
    }
  }
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/rQTSmiA0l)

![image](https://github.com/user-attachments/assets/f9fc880e-56b7-4392-af3a-ce26dcf2f97e)

![image](https://github.com/user-attachments/assets/0f08203d-5d1a-4caa-8e89-f902f84c6d22)
