## Péndulos
### ¿Cómo funciona un sistema de dos péndulos en serie?
Un sistema de dos péndulos en serie (también llamado péndulo doble) consiste en un péndulo que tiene otro péndulo colgando de su extremo. Ambos pueden oscilar libremente, lo que crea un sistema no lineal y caótico, ya que pequeños cambios en las condiciones iniciales pueden producir trayectorias de movimiento muy distintas.
### ¿Cómo se refleja esto en nuestro código?
**Unión del segundo péndulo al primero**
- El primer péndulo (Péndulo 1) oscila desde un pivote fijo en la parte superior.
- El segundo péndulo (Péndulo 2) cuelga del extremo del Péndulo 1 y se comporta como otro péndulo independiente.

**Cálculo de fuerzas y aceleraciones**
- La aceleración angular del primer péndulo (angleAcceleration1) depende de la gravedad y la fuerza que ejerce el segundo péndulo sobre él.
- La aceleración angular del segundo péndulo (angleAcceleration2) depende de la gravedad y de la fuerza que transmite el primer péndulo.

**Transformación de coordenadas**
- La posición del segundo péndulo (bob2) se calcula a partir de la posición del primero (bob1).
- Ambos péndulos se dibujan conectados, con líneas entre ellos.

**Interacción con el usuario**
- Se permite hacer clic y arrastrar ambos péndulos.

### Código
**pendulum.js**
``` js
class Pendulum {
  constructor(x, y, r, parent = null) {
    this.parent = parent; // * Referencia al péndulo padre si existe
    this.pivot = parent ? parent.bob : createVector(x, y); // * Si hay un padre, el pivote es su bob
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;

    this.angleVelocity = 0.0;
    this.angleAcceleration = 0.0;
    this.damping = 0.995;
    this.ballr = 24.0;
    this.dragging = false;
    this.color = parent ? color(255, 0, 255) : color(0, 255, 255); // * Magenta si es el segundo péndulo
  }

  update() {
    if (!this.dragging) {
      let gravity = 0.4;
      this.angleAcceleration = ((-1 * gravity) / this.r) * sin(this.angle);

      this.angleVelocity += this.angleAcceleration;
      this.angle += this.angleVelocity;
      this.angleVelocity *= this.damping;
    }

    if (this.parent) {
      this.pivot = this.parent.bob; // * El pivote se actualiza con la posición del bob del padre
    }
  }

  show() {
    this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle), 0);
    this.bob.add(this.pivot);

    stroke(0);
    strokeWeight(2);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);
    fill(this.color);
    circle(this.bob.x, this.bob.y, this.ballr * 2);
  }

  clicked(mx, my) {
    let d = dist(mx, my, this.bob.x, this.bob.y);
    if (d < this.ballr) {
      this.dragging = true;
    }
  }

  stopDragging() {
    this.angleVelocity = 0;
    this.dragging = false;
  }

  drag() {
    if (this.dragging) {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY));
      this.angle = atan2(-1 * diff.y, diff.x) - radians(90);
    }
  }
}
```

**sketch.js**
``` js
// sketch.js
// The Nature of Code
// Daniel Shiffman
// Adapted for a double pendulum system

let pendulum1, pendulum2;

function setup() {
  createCanvas(640, 360);
  pendulum1 = new Pendulum(width / 2, 100, 100);
  pendulum2 = new Pendulum(0, 0, 100, pendulum1);
}

function draw() {
  background(255);
  pendulum1.update();
  pendulum1.show(color(0, 255, 255));

  pendulum2.update();
  pendulum2.show(color(255, 0, 255));

  pendulum1.drag();
  pendulum2.drag();
}

function mousePressed() {
  pendulum1.clicked(mouseX, mouseY);
  pendulum2.clicked(mouseX, mouseY);
}

function mouseReleased() {
  pendulum1.stopDragging();
  pendulum2.stopDragging();
}
```

### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/auj-4xKMC)

![image](https://github.com/user-attachments/assets/293abf36-ab5f-4fde-9746-b588aec569d5)
![image](https://github.com/user-attachments/assets/c48380ee-0a23-4ce1-af59-88630e3f9e95)
![image](https://github.com/user-attachments/assets/5a4f2da4-d353-44c1-b11a-701755daa0c7)
