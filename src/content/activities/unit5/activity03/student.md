## Obra de arte generativa algorítmica interactiva en tiempo real
### Conceptos
#### Unidad 1: Vuelo de Lévy
#### Unidad 2: Interpolación de colores
#### Unidad 3: Modelado de fuerzas y gravedad
#### Unidad 4: Coordenadas polares

### Proceso de creación
- **Idea:** un programa para crear fuegos artificiales con aplausos. La persona podrá elegir la melodía y presionar un botón para cambiar los colores de manera aleatoria.
### Creación, eliminación de partículas y gestión de memoria
Cada vez que haya una frecuencia bastante alta se creará un emitter que generará partículas, el cual durará tres segundos para evitar saturar la memoria. El vuelo de Lévy dictaminará la posición del nuevo emitter.
### Código
**emitter.js**
``` js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.startTime = millis();
  }

  addParticles() {
    for (let i = 0; i < 5; i++) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].update();
      this.particles[i].show();
      if (this.particles[i].isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```

**particle.js**
``` js
class Particle {
  constructor(x, y) {
    let angle = random(TWO_PI);
    let speed = random(2, 5);
    this.position = createVector(x, y);
    this.velocity = p5.Vector.fromAngle(angle).mult(speed);
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
    this.color = lerpColor(color(random(255), 0, random(255)), color(random(255), random(255), 0), random(1));
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 4;
    this.acceleration.mult(0);
  }

  show() {
    noStroke();
    fill(this.color.levels[0], this.color.levels[1], this.color.levels[2], this.lifespan);
    ellipse(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0;
  }
}
```

**sketch.js**
``` js
let emitters = [];
let mic;

function setup() {
  createCanvas(windowWidth, windowHeight);
  mic = new p5.AudioIn();
  mic.start();
}

function draw() {
  background(0, 25);
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticles();
  }
  
  // Limpiar emitters viejos
  emitters = emitters.filter(e => millis() - e.startTime < 3000);
  
  detectClap();
}

function detectClap() {
  let level = mic.getLevel();
  if (level > 0.2) { // Umbral para detectar aplausos
    let { x, y } = levyFlight();
    emitters.push(new Emitter(x, y));
  }
}

function levyFlight() {
  let angle = random(TWO_PI);
  let distance = pow(random(1), -1.5) * 100;
  let x = constrain(width / 2 + cos(angle) * distance, 0, width);
  let y = constrain(height / 2 + sin(angle) * distance, 0, height);
  return { x, y };
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/Am3m4NnQB)

![image](https://github.com/user-attachments/assets/f955bc72-34ff-485c-a883-7253e78a6b67)

![image](https://github.com/user-attachments/assets/23461eb2-abeb-4b73-ac2e-baf854ee634d)
