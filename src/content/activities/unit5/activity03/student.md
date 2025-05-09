## Obra de arte generativa algorítmica interactiva en tiempo real
### Conceptos
#### Unidad 1: Vuelo de Lévy
Este concepto se usa para determinar la posición donde se crearán los emitters.
``` js
function createFirework(){
.
.
.
 let displacement = levyFlight(); // Generamos un desplazamiento con Vuelo de Lévy
.
.
.
emitters.push(new Emitter(x, y, color1, color2));
}

function levyFlight(beta = 1.5) {
  let u = randomGaussian(0, 1);
  let v = randomGaussian(0, 1);
  let stepSize = u / pow(abs(v), 1 / beta); // Genera un paso siguiendo la distribución de Lévy
  let angle = random(TWO_PI);
  return p5.Vector.fromAngle(angle).mult(abs(stepSize) * 100); // Escalamos el paso
}
```
#### Unidad 2: Interpolación de colores
Cada partícula tiene un color único, que siempre está dentro del gradiente definido por el emitter que la generó. El emitter no cambia de color con el tiempo, pero crea partículas de diferente color entre dos que se le asignaron. Además, la opacidad de cada partícula disminuye con el tiempo (this.lifespan), lo que hace que desaparezcan gradualmente. La partícula mantiene el mismo color desde su creación hasta su muerte, solo que varía su opacidad.
#### Unidad 3: Modelado de fuerzas y motion 101
El código modela fuerzas de la siguiente manera:

- Fuerza de aceleración: Se agrega a cada partícula con applyForce(force), permitiendo que las partículas respondan a diferentes fuerzas.

- Movimiento con motion 101: En update():

``` js
this.velocity.add(this.acceleration);
this.position.add(this.velocity);
this.acceleration.mult(0);
```
Aquí, la aceleración influye en la velocidad y esta en la posición, simulando una física realista.
#### Unidad 4: Coordenadas polares
La implementación de coordenadas polares se encuentra en la clase SpiralParticle, donde la posición de la partícula se actualiza usando las funciones trigonométricas cos() y sin(). Estas funciones convierten el ángulo (this.angle) y el radio (this.radius) en coordenadas cartesianas, permitiendo que la partícula siga un movimiento en espiral. El uso de estas coordenadas facilita la creación y manejo del movimiento circular.
### Proceso de creación
- **Idea Inicial:** un programa para crear fuegos artificiales al son de una melodía. Cuando haya una frecuencia muy alta, se generará un emitter. Las personas podrán elegir la melodía y presionar un botón para alternar de colores.
- **Cambios:** a medida que experimentaba, pensé en un ejercicio de un compañero que era como con aplausos y se me ocurrió que podría ser mejor interactividad el generar fuegos artificiales con aplausos que verlos al son de una melodía.
- **Idea Final:** un programa para crear fuegos artificiales con aplausos. Se generarán ciertos emitters a medida que la persona aplauda y tendrán colores diferentes cada uno. Habrá diferente comportamiento de partículas para cumplir la cuota de herencia y polimorfismo.
### Creación, eliminación de partículas y gestión de memoria
Cada vez que se aplauda o se genere cierto nivel de ruido, se creará un emitter que generará partículas de tipo standard, twinkle o spiral de manera completamente aleatoria. Este se agrega a la lista de emitters y después de tres segundos se eliminan para evitar saturar demasiado la memoria, además, las partículas dentro de un Emitter se eliminan cuando su lifespan llega a 0. El vuelo de Lévy dictaminará la posición del nuevo emitter.
### Código
**emitter.js**
``` js
class Emitter {
  constructor(x, y, color1, color2) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.color1 = color1;
    this.color2 = color2;
    this.lifespan = 255;
    this.startTime = millis();
  }

  addParticle() {
    let type = random([StandardParticle, TwinkleParticle, SpiralParticle]);
    this.particles.push(new type(this.origin.x, this.origin.y, this.color1, this.color2, this.lifespan));
  }

  run() {
    let elapsed = millis() - this.startTime;
    this.lifespan = map(elapsed, 0, 3000, 255, 0, true);

    for (let i = this.particles.length - 1; i >= 0; i--) {
      if (typeof this.particles[i].updateLifespan === "function") {
        this.particles[i].updateLifespan(this.lifespan);
      }
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }

  isDead() {
    return this.lifespan <= 0;
  }
}
```

**particle.js**
``` js
class BaseParticle {
  constructor(x, y, color1, color2, lifespan) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = p5.Vector.random2D().mult(random(2, 5));
    this.lifespan = lifespan;
    this.color1 = color1;
    this.color2 = color2;
    this.t = random(1);
  }

  
  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2; // Desvanecimiento individual
  }

  updateLifespan(newLifespan) {
    this.lifespan = newLifespan; // Ajusta la opacidad del sistema
  }

  show() {
    let interpolatedColor = lerpColor(this.color1, this.color2, this.t);
    let fadedColor = color(
      red(interpolatedColor),
      green(interpolatedColor),
      blue(interpolatedColor),
      this.lifespan
    );
    fill(fadedColor);
    noStroke();
    circle(this.position.x, this.position.y, 8);
  }
  
   run() {
    this.update();
    this.show();
  }

  isDead() {
    return this.lifespan < 0;
  }
}

class StandardParticle extends BaseParticle {
  constructor(x, y, color1, color2, lifespan) {
    super(x, y, color1, color2, lifespan);
  }

  update() {
    super.update();
  }
}

class TwinkleParticle extends BaseParticle {
  constructor(x, y, color1, color2, lifespan) {
    super(x, y, color1, color2, lifespan);
  }

  show() {
    if (frameCount % 5 > 2) {
      super.show();
    }
  }
}

class SpiralParticle extends BaseParticle {
  constructor(x, y, color1, color2, lifespan) {
    super(x, y, color1, color2, lifespan);
    this.angle = random(TWO_PI);
    this.radius = 0;
  }

  update() {
    this.angle += 0.5;
    this.radius += 0.5;
    this.position.x += cos(this.angle) * this.radius * 0.05;
    this.position.y += sin(this.angle) * this.radius * 0.05;
    super.update();
  }
}
```

**sketch.js**
``` js
let emitters = [];
let mic;
let threshold = 0.2; // Ajustar según la sensibilidad deseada

function setup() {
  createCanvas(windowWidth, windowHeight);
  noStroke();

  // Inicializar micrófono
  mic = new p5.AudioIn();
  mic.start();
}

function draw() {
  background(0, 50);

  // Obtener volumen del micrófono
  let vol = mic.getLevel();

  // Si el volumen supera el umbral, genera un nuevo emitter
  if (vol > threshold) {
    createFirework();
  }

  // Dibujar y actualizar los emitters
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }

  // Eliminar emitters viejos después de 3 segundos
  emitters = emitters.filter(e => millis() - e.startTime < 3000);
}

function createFirework() {
  let centerX = width / 2; // Punto base en el centro del canvas
  let centerY = height / 3; // Un poco arriba para mejor efecto

  let displacement = levyFlight(); // Generamos un desplazamiento con Vuelo de Lévy
  let x = centerX + displacement.x; // Nueva posición X
  let y = centerY + displacement.y; // Nueva posición Y

  // Asegurar que el punto esté dentro de los límites del canvas
  x = constrain(x, 50, width - 50);
  y = constrain(y, 50, height / 2);

  let color1 = color(random(255), random(255), random(255));
  let color2 = color(random(255), random(255), random(255));

  emitters.push(new Emitter(x, y, color1, color2));
}


function levyFlight(beta = 1.5) {
  let u = randomGaussian(0, 1);
  let v = randomGaussian(0, 1);
  let stepSize = u / pow(abs(v), 1 / beta); // Genera un paso siguiendo la distribución de Lévy
  let angle = random(TWO_PI);
  return p5.Vector.fromAngle(angle).mult(abs(stepSize) * 100); // Escalamos el paso
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/Am3m4NnQB)

![image](https://github.com/user-attachments/assets/66411a11-b68d-4483-8445-d14e33168d20)

![image](https://github.com/user-attachments/assets/e19f8a93-7faf-4688-9c02-b2c430fb3d1a)
