# Revisa y repasa algunos conceptos
## Sistemas de partículas
Un sistema de partículas es una colección de muchísimas partículas diminutas que, en conjunto, representan un objeto difuso. Con el tiempo, las partículas se generan en un sistema, se mueven y cambian dentro del sistema, y ​​mueren. En el tiempo se han usado para modelar varios tipos irregulares de fenómenos naturales, como fuego, humo, cascadas, niebla, hierba, burbujas, etc.

Hay un elemento muy importante llamado emisor, que es quien controla la configuración inicial de las partículas (posición, velocidad, etc.). También hay que saber que las partículas no pueden vivir por siempre, deben ser eliminadas después de cierto tiempo o cierta acción, como pasar el límite del lienzo, chocar con algo, etc.

## Herencia y polimorfismo
- **Herencia:** Es un principio de la programación orientada a objetos donde una clase (hija) puede heredar atributos y métodos de otra clase (padre), reutilizando su código. Un gato y un perro son animales, por lo que comparten características como respirar y moverse, pero cada uno tiene sus propias diferencias.
- **Polimorfismo:** Es la capacidad de un objeto de tomar diferentes formas, permitiendo que un mismo método funcione de manera diferente según la clase que lo implemente. Un gato y un perro pueden hacer un sonido, pero el gato maúlla y el perro ladra, aunque ambos están ejecutando la acción de "hacer sonido".
## Simulación 4.2: an Array of Particles
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
- **Creación de partículas**: En cada cuadro de la animación (draw()), se crea una nueva partícula con new Particle(width / 2, 20). Esta partícula se agrega al arreglo particles[], que almacena todas las partículas activas.
- **Desaparición de partículas y gestión de memoria**: Cada partícula tiene una propiedad lifespan, que disminuye en cada actualización (update()), simulando su envejecimiento. Cuando lifespan llega a cero o menos, el método isDead() devuelve true. Se recorre el arreglo particles[] de atrás hacia adelante en draw(), y si isDead() es true, la partícula se elimina con particles.splice(i, 1). Esto libera la memoria ocupada por la partícula eliminada, evitando acumulaciones innecesarias.
### Modificación: Ruido de Perlin
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
Empecé buscano en la unidad 1 para ir más o menos en orden y realmente no se me ocurría ningún concepto específico para agregar. Así que apliqué el ruido de Perlin implementando interpolación de color en las partículas.

``` js
let t = noise(this.noiseSeed + noiseOffset);
let c = lerpColor(color1, color2, t);
```

- this.noiseSeed → Un valor único para cada partícula (para que no todas cambien igual), es entre 0 y 1000 para que haya bastante variedad, pero tampoco es tan grande como para que las diferencias sean prácticamente innotables.
- noiseOffset → Un valor global que cambia cada frame porque en `draw()` se le aumenta 0.01, moviendo así la "onda" de colores.
- noise(this.noiseSeed + noiseOffset) → Devuelve un número entre 0 y 1, asegurando un cambio de color progresivo.
- `fill(c.levels[0], c.levels[1], c.levels[2], this.lifespan);` → pinta las partículas con el color obtenido y el alfa que tiene según su tiempo de vida restante.
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
La gestión no se ha modificado en el código, por lo que sigue igual que en la explicación anterior a la implementación del ruido de Perlin.
#### *Código*
**particle.js**
``` js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    this.noiseSeed = random(1000); // Cada partícula tendrá una variación única en el ruido
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    // Colores de interpolación
    let color1 = color(10, 201, 120); // Verde agua (#00CED1)
    let color2 = color(123, 127, 235);   // Azul (#0000FF)

    // Usar ruido Perlin para suavizar la transición de color
    let t = noise(this.noiseSeed + noiseOffset);
    let c = lerpColor(color1, color2, t);

    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(c.levels[0], c.levels[1], c.levels[2], this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return (this.lifespan < 0.0);
  }
}
```

**sketch.js**
``` js
let particles = [];
let noiseOffset = 0; // Offset para el ruido Perlin

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);
  particles.push(new Particle(width / 2, 20));

  // Looping through backwards to delete
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();
    if (particle.isDead()) {
      //remove the particle
      particles.splice(i, 1);
    }
  }

  // Incrementamos el offset para que el ruido cambie con el tiempo
  noiseOffset += 0.01;
}
```
#### _Resultado_
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/w_GmM20Gc)

![image](https://github.com/user-attachments/assets/fab6a66a-48a5-452a-b75e-c18bb8b4b7f0)

## Simulación 4.4: a System of Systems
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
#### Creación de partículas
Cada vez que el usuario da clic, se crea un nuevo `Emitter` en esa posición:
``` js
function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}
```

Cada Emitter agrega una nueva Particle en cada fotograma con:
``` js
addParticle() {
  this.particles.push(new Particle(this.origin.x, this.origin.y));
}
```

Es decir que el número de partículas en pantalla incrementa con cada `Emitter` que se crea.
####  Desaparición de partículas
Cada `Particle` tiene un lifespan (duración de vida), que disminuye en cada fotograma en update():
``` js
this.lifespan -= 2;
```

Cuando lifespan < 0, la partícula se considera "muerta". El sistema revisa cada partícula en orden inverso y la elimina del array cuando muere, evitando que estas ocupen memoria:
``` js
for (let i = this.particles.length - 1; i >= 0; i--) {
  if (this.particles[i].isDead()) {
    this.particles.splice(i, 1);
  }
}
```
#### Gestión de Memoria
- **Uso de splice(i, 1):** Se eliminan las partículas muertas para que el array no crezca indefinidamente. Se recorre de atrás hacia adelante para evitar errores de índice al eliminar elementos.

- **Cada Emitter sigue activo hasta que se borren todas sus partículas:** No hay límite de partículas, si el usuario hace muchos clics puede haber una sobrecarga de partículas. Cada Emitter genera partículas indefinidamente en draw(), lo que puede afectar el rendimiento a largo plazo.
### Modificación: aceleración aleatoria
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
Me pareció interesante seguir usando la obra de las abejas que implementaba aceleración aleatoria además de una tendencia hacia el centro gracias a una fuerza de atracción que se le aplicaba.

En el código en vez de utilizar la fuerza de la gravedad para que actúe en las partículas e incitar movimiento en ellas,  aquí se les da una aceleración aleatoria inicial a cada una.

Para la atracción al emitter, se calcula un vector de atracción desde la partícula hasta el `Emitter`. `p5.Vector.sub(this.emitter.origin, this.position)` da un vector que apunta desde la partícula hacia el `Emitter`. Se multiplica por 0.001 para suavizar la atracción, haciendo que las partículas no sean arrastradas instantáneamente de vuelta al emisor. `applyForce()` suma este vector a la aceleración de la partícula, influyendo en su movimiento de manera progresiva.
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
- **Creación de partículas:**
  - En cada fotograma (draw()), cada Emitter llama a addParticle() cada dos frames (frameCount % 2 == 0).
  - addParticle() crea un nuevo objeto Particle y lo añade al arreglo this.particles en Emitter.

- **Desaparición de partículas:**
  - Cada Particle tiene una propiedad lifespan que se reduce en update(). Si lifespan baja de 0, la partícula se considera "muerta".
  - En run() de Emitter, el sistema recorre las partículas al revés para evitar errores al eliminar elementos mientras se itera sobre la lista. Si una partícula está muerta (isDead() devuelve true), se elimina con splice(i, 1), liberando su memoria.

- **Gestión de memoria:**
  - Al eliminar partículas cuando su lifespan llega a 0, se evita que la memoria se llene con objetos obsoletos.
  - Para evitar demasiados Emitter y afectar el rendimiento, en el array global emitters, si hay más de 5, el más antiguo se elimina con shift(), liberando espacio.
#### *Código*
**emitter.js**
``` js
// emitter.js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y, this));
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```

**particle.js**
``` js
// particle.js
class Particle {
  constructor(x, y, emitter) {
    this.position = createVector(x, y);
    this.emitter = emitter;
    this.acceleration = p5.Vector.random2D().mult(0.05); // Aceleración aleatoria
    this.velocity = p5.Vector.random2D().mult(2);
    this.lifespan = 255.0;
  }

  run() {
    this.applyForce(p5.Vector.sub(this.emitter.origin, this.position).mult(0.001)); // Atracción al emitter
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(175, 235, 222, this.lifespan);
    ellipse(this.position.x + 3, this.position.y - 6, 5, 7);
    fill(255, 204, 0, this.lifespan);
    ellipse(this.position.x, this.position.y, 10, 7);
    stroke(0, this.lifespan);
    line(this.position.x, this.position.y - 6, this.position.x, this.position.y + 3);
    line(this.position.x - 3, this.position.y - 4, this.position.x - 3, this.position.y + 2);
    fill(175, 235, 222, this.lifespan);
    ellipse(this.position.x, this.position.y - 6, 5, 7);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}
```

**sketch.js**
``` js
// sketch.js
let emitters = [];

function setup() {
  createCanvas(550, 450);
  createP("Click to add particle systems");
}

function draw() {
  background(255);
  for (let emitter of emitters) {
    emitter.run();
    if (frameCount % 2 == 0) {
      emitter.addParticle();
    }
  }
}

function mousePressed() {
  if (emitters.length >= 5) {
    emitters.shift(); // Eliminar el emisor más antiguo
  }
  emitters.push(new Emitter(mouseX, mouseY));
}
```
#### _Resultado_
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/EB_GY9skU)

![image](https://github.com/user-attachments/assets/e25c825d-5b16-4ef8-92fc-c9a958c34b28)

![image](https://github.com/user-attachments/assets/dd3d4d2d-2667-4636-93b1-43af7069a768)

## Simulación 4.5: a Particle System with Inheritance and Polymorphism
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
#### Creación de partículas:
- En cada fotograma (draw() en sketch.js), el Emitter genera una nueva partícula con addParticle().
- Hay una probabilidad del 50% (random(1) < 0.5) de que la partícula sea un Particle o un Confetti, lo que añade variedad visual.
- La nueva partícula se añade al array this.particles en Emitter.
#### Desaparición de partículas:
- Cada partícula tiene una propiedad lifespan que comienza en 255 y se reduce en update() dentro de Particle.
- Cuando lifespan baja de 0, el método isDead() devuelve true.
- En run() de Emitter, se recorre this.particles en orden inverso para evitar errores al eliminar elementos mientras se itera.
- Las partículas muertas se eliminan con splice(i, 1), liberando memoria.
#### Gestión de la memoria:
- **Liberación de memoria automática:** Las partículas viejas se eliminan cuando lifespan < 0, evitando acumulaciones innecesarias en this.particles.

- **Estrategia de iteración eficiente:** Se recorre el array de partículas en orden inverso (for (let i = this.particles.length - 1; i >= 0; i--)), asegurando que splice() no cause errores de índice.

- **No hay límite explícito en el número de partículas:** A diferencia del código anterior, aquí no se controla cuántos Emitter hay en pantalla, lo que podría hacer que el sistema se vuelva pesado si se deja ejecutando por mucho tiempo.
### Modificación: El vuelo de Lévy 
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
Me llamaba la idea de usar un algoritmo para evitar que hubiera igual probabilidad de tener un confetti o un particle. Por lo que el vuelo de Lévy era una buena opción para favorecer la aparición de particles y que los confettis se sintieran más espontáneos.

El vuelo de Lévy es un modelo de movimiento aleatorio que combina pasos cortos frecuentes con saltos largos ocasionales. Se encuentra en patrones de movimiento de animales y partículas en la naturaleza. Su ecuación es:
s= (r^b)/1
​
Donde:
- 𝑠 es el tamaño del paso.
- 𝑟 es un número aleatorio entre 0 y 1.
- 𝑏 es un parámetro (usualmente entre 1 y 3).

En el código solo había que modificar el método`addParticle()` en emitter.js
``` js
  addParticle() {
    let beta = 0.3; // Controla la frecuencia de saltos largos (mientras más alto, más saltos largos y viceversa)
    let r = random(1);
    let levyValue = 1 / pow(r, beta); // Genera valores de Lévy con la ecuación

    if (levyValue > 2) { // Umbral ajustado para que los Confetti sean más raros
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    }
  }
```
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
- **Creación de Partículas**: Cada fotograma el emitter agrega una nueva partícula. El Vuelo de Lévy hace que sea poco probable la aparición de confetti y muy probable la aparición de particles sencillas.
- **Desaparición de Partículas**: Cada partícula tiene una propiedad lifespan, que disminuye con el tiempo. Cuando lifespan < 0, la partícula "muere". En Emitter.run(), se recorre la lista de partículas de atrás hacia adelante para eliminarlas sin problemas. La eliminación se hace en orden inverso para evitar errores en la manipulación del array.
- **Gestión de Memoria**:
  - Cada partícula es creada dinámicamente con new Particle() o new Confetti().
  - Eliminamos partículas cuando su lifespan se agota, liberando memoria progresivamente.
  - No hay acumulación infinita de partículas, ya que siempre estamos eliminando las que mueren.
  - No hay fugas de memoria significativas, ya que splice(i, 1) elimina referencias innecesarias.
#### *Código*
**confetti.js**
``` js
// Child class constructor
class Confetti extends Particle {
  // Override the show method
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);

    rectMode(CENTER);
    fill(219, 60, 166, this.lifespan);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}
```

**emitter.js**
``` js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let beta = 0.3; // Parámetro del vuelo de Lévy (controla la frecuencia de saltos largos)
    let r = random(1);
    let levyValue = 1 / pow(r, beta); // Genera valores de Lévy

    if (levyValue > 2) { // Umbral ajustado para que los Confetti sean más raros
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```

**particle.js**
``` js
// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(60, 219, 125, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}
```

**sketch.js**
``` js
let emitter;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 20);
}

function draw() {
  background(255);
  emitter.addParticle();
  emitter.run();
}
```
#### _Resultado_
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/L-QhoZdB0)

![image](https://github.com/user-attachments/assets/9df2b55b-50da-4d21-9289-d0b0174fc878)


## Simulación 4.6: a Particle System with Forces
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
#### Creación de partículas: 
- En cada fotograma (draw()), se llama a emitter.addParticle(), lo que genera una nueva partícula en la posición del emisor.
- La partícula se almacena en el arreglo this.particles dentro de Emitter.
#### Gestión de memoria y eliminación de partículas:
- En Emitter.run(), se recorre el arreglo this.particles en orden inverso.
- Se llama a particle.run(), que actualiza la posición y dibuja la partícula.
- Si la partícula ha agotado su lifespan (isDead() devuelve true), se elimina con splice(i, 1), liberando memoria.
- No hay un límite explícito de partículas en pantalla. En el peor caso, pueden acumularse alrededor de 127 partículas en un momento dado por la manera en que se crean y se eliminan.
### Modificación: concepto
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
#### *Código*
``` js
```
#### _Resultado_
[Enlace a la simulación]()

## Simulación 4.7: a Particle System with a Repeller
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
### Modificación: concepto
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
#### *Código*
#### _Resultado_
[Enlace a la simulación]()
