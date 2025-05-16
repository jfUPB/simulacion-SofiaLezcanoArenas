## Repasa conceptos de las unidades anteriores
### Idea
Pensé en que hubiera un botón para intercambiar entre ambiente normal y ambiente de fluido, como lo es el agua. Además de que en lugar de generar de manera completamente aleatoria la velocidad y amplitud de cada oscilador, se empleara el ruido perlin para que el movimiento fuera más armónico.
### Conceptos utilizados
- **Ruido perlin:** de aleatoriedas, el profe mencionó la posibilidad de aplicar ruido perlin por lo que lo implementé. En el código se utilizan valores basados en noise(), lo que da una variación más suave y orgánica.
- **Resistencia de fluidos:** El concepto de fuerzas más acertado que veía para esta simulación es el de resistencia de fluidos. Por lo que cuando se activa el modo de agua, los osciladores se ven afectados por la resistencia del agua.
### Código de la simulación
**liquid.js**
``` js
// liquid.js
class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c; // Coeficiente de fricción
  }

  display() {
    noStroke();
    fill(0, 100, 200, 80);
    rect(this.x, this.y, this.w, this.h);
  }

  calculateDrag(velocity) {
    let speed = velocity.mag();
    let dragMagnitude = this.c * speed * speed;
    let drag = velocity.copy().mult(-1);
    drag.setMag(dragMagnitude);
    return drag;
  }
}
```

**oscillator.js**
``` js
// oscillator.js
class Oscillator {
  constructor(noiseSeed) {
    this.position = createVector(width / 2, height / 2);
    let angleNoise = noise(noiseSeed) * TWO_PI;
    let speedNoise = noise(noiseSeed + 100) * 0.05;
    this.velocity = createVector(cos(angleNoise) * speedNoise, sin(angleNoise) * speedNoise);
    this.acceleration = createVector(0, 0);
    this.angle = createVector();
    this.amplitude = createVector(
      noise(noiseSeed + 200) * (width / 2 - 20) + 20, 
      noise(noiseSeed + 300) * (height / 2 - 20) + 20
    );
    this.baseSpeed = this.velocity.mag();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  restoreSpeed() {
    if (this.velocity.mag() < this.baseSpeed) {
      this.velocity.setMag(this.baseSpeed);
    }
  }

  update() {
    this.velocity.add(this.acceleration);
    this.angle.add(this.velocity);
    this.acceleration.mult(0);
  }
  
  show() {
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(width / 2, height / 2);
    stroke(0);
    strokeWeight(2);
    fill(127);
    line(0, 0, x, y);
    circle(x, y, 32);
    pop();
  }
}
```

**sketch.js**
``` js
// sketch.js
let oscillators = [];
let underWater = false;
let button;
let liquid;
let noiseOffset = 0;

function setup() {
  createCanvas(640, 240);
  noiseDetail(4, 0.5); // Ajusta la calidad del ruido Perlin
  for (let i = 0; i < 10; i++) {
    oscillators.push(new Oscillator(i * 0.1));
  }
  
  button = createButton('Modo Agua: OFF');
  button.position(10, 10);
  button.mousePressed(toggleWaterMode);
  
  liquid = new Liquid(0, 0, width, height, 0.1); // Agua con resistencia
}

function draw() {
  background(255);
  
  if (underWater) {
    liquid.display();
  }
  
  for (let i = 0; i < oscillators.length; i++) {
    if (underWater) {
      let drag = liquid.calculateDrag(oscillators[i].velocity);
      oscillators[i].applyForce(drag);
    } else {
      oscillators[i].restoreSpeed();
    }
    oscillators[i].update();
    oscillators[i].show();
  }
  
  noiseOffset += 0.01;
}

function toggleWaterMode() {
  underWater = !underWater;
  button.html(underWater ? 'Modo Agua: ON' : 'Modo Agua: OFF');
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/vwxzoOke9)

![image](https://github.com/user-attachments/assets/209bc3d4-9894-4f2c-8224-b1e15243e6b2)

![image](https://github.com/user-attachments/assets/85830214-6449-4f0e-87ce-5e4809530194)
