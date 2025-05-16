## El problema de los n-cuerpos
### Código
**mover.js**
``` js
// mover.js
class Mover {
  constructor(x, y, vx, vy, m, col) {
    this.pos = createVector(x, y);
    this.vel = createVector(vx, vy);
    this.acc = createVector(0, 0);
    this.mass = m;
    this.r = sqrt(this.mass) * 2;
    this.color = col;
    this.maxSpeed = 10; // Límite de velocidad
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  attract(mover) {
    let force = p5.Vector.sub(this.pos, mover.pos);
    let distanceSq = constrain(force.magSq(), 100, 1000);
    let G = 0.5;
    let strength = (G * (this.mass * mover.mass)) / distanceSq;
    force.setMag(strength);
    mover.applyForce(force);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed); // Normalizar velocidad
    this.pos.add(this.vel);
    this.acc.set(0, 0);
  }

  show() {
    noStroke();
    fill(this.color);
    ellipse(this.pos.x, this.pos.y, this.r * 2);
  }
}
```

**sketch.js**
``` js
// sketch.js
let movers = [];
let sun;
let colors;
let slider;

function setup() {
  createCanvas(640, 360);
  colors = [color('#E63946'), color('#F4A261'), color('#2A9D8F'), color('#264653')];
  
  for (let i = 0; i < 6; i++) {
    let pos = p5.Vector.random2D();
    let vel = pos.copy();
    vel.setMag(random(2, 5));
    pos.setMag(random(100, 150));
    vel.rotate(PI / 2);
    let m = random(10, 20);
    let col = random(colors);
    movers[i] = new Mover(pos.x, pos.y, vel.x, vel.y, m, col);
  }
  sun = new Mover(0, 0, 0, 0, 500, color(255, 204, 0));
  
  slider = createSlider(10, 60, 30, 1);
  slider.position(10, height + 10);
}

function draw() {
  background(240, 240, 230, 50);
  translate(width / 2, height / 2);
  frameRate(slider.value());
  
  for (let mover of movers) {
    sun.attract(mover);
    for (let other of movers) {
      if (mover !== other) {
        mover.attract(other);
      }
    }
  }

  for (let mover of movers) {
    mover.update();
    mover.show();
  }
  
  // Dibujar líneas como si fueran las varillas de un móvil
  stroke(0, 100);
  strokeWeight(2);
  for (let i = 0; i < movers.length; i++) {
    for (let j = i + 1; j < movers.length; j++) {
      line(movers[i].pos.x, movers[i].pos.y, movers[j].pos.x, movers[j].pos.y);
    }
  }
}
```
### Modelado del problema de los n-cuerpos
El código modela el problema de los n-cuerpos simulando la atracción gravitacional entre múltiples objetos (los "movers") y un objeto central (el "sol", el cual no se muestra pero se tiene en cuenta para los cálculos). Cada objeto tiene una posición, velocidad y aceleración, y su movimiento es afectado por la fuerza gravitacional que ejercen los demás, exceptuando el "sol" que no es afectado por ninguna fuerza y se mantiene estático. La fuerza gravitacional se calcula usando la ley de gravitación de Newton, donde la atracción depende de la masa y la distancia entre los objetos. En cada fotograma, se actualizan las posiciones de los objetos en función de estas fuerzas, generando trayectorias orbitales dinámicas que pueden cambiar con el tiempo. Para una mejor visualización la velocidad se encuentra limitada a 10 para evitar que los objetos se salgan del lienzo.
### Resultado
![image](https://github.com/user-attachments/assets/e07e0b1f-d433-475a-a656-b1ccd69c6982)

[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/7PfjkFE_r)

### Referencias
Trabajo de Alexander Calder
