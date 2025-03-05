## Practica un poco
### Código de la simulación
**mover.js**
``` js
// mover.js
class Mover {
  constructor() {
    this.position = createVector(width / 2, height - 30); // Posición en la parte inferior
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.topspeed = 4;
    this.angle = 0;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    
    // Frenado progresivo
    this.velocity.mult(0.95);
    
    // Solo actualizar el ángulo si hay movimiento
    if (this.velocity.mag() > 0.1) {
      this.angle = this.velocity.heading();
    }
  }

  display() {
    stroke(0);
    strokeWeight(2);
    fill(8, 177, 98);
    push();
    translate(this.position.x, this.position.y);
    rotate(this.angle);
    //triángulo
    triangle(-15, 10, -15, -10, 15, 0);
    pop();
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = 0;
    } else if (this.position.x < 0) {
      this.position.x = width;
    }
  }
}
```

**sketch.js**
``` js
// sketch.js
let mover;

function setup() {
  createCanvas(500, 240);
  mover = new Mover();
}

function draw() {
  background(200);
  let force = createVector(0, 0);

  if (keyIsDown(LEFT_ARROW)) {
    force.x = -0.1;
  }
  if (keyIsDown(RIGHT_ARROW)) {
    force.x = 0.1;
  }

  mover.applyForce(force);
  mover.update();
  mover.checkEdges();
  mover.display();
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/uClafQMNc)

![image](https://github.com/user-attachments/assets/3e1a5f7f-4fda-466c-a406-e152a818247c)

![image](https://github.com/user-attachments/assets/a253369e-2e3f-43d1-a243-0f4ce441c894)
