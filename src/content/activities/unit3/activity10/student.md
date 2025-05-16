## Modelando fuerzas
### Fricción
#### Modelado de la fuerza
**Concepto:** La fricción es una fuerza que se opone al movimiento de un objeto cuando está en contacto con una superficie. Siempre actúa en la dirección contraria al movimiento y depende de dos factores principales:
- La naturaleza de la superficie (qué tan rugosa o lisa es).
- La fuerza normal (la fuerza con la que el objeto presiona la superficie, que suele ser su peso).

**¿Qué implica la fricción en el movimiento?**
- Si un objeto está en movimiento, la fricción lo desacelera hasta que se detiene (si no hay otra fuerza que lo impulse).
- Si un objeto está en reposo, la fricción puede evitar que comience a moverse.

**Fórmula matemática:** Fricción = −μ⋅N⋅𝑣
- μ → Coeficiente de fricción (depende del tipo de suelo).
- 𝑁 → Fuerza normal (en este caso, es constante porque no estamos considerando pendientes, por lo que se simplifica a un valor fijo).
- 𝑣 → Dirección opuesta a la velocidad (se obtiene invirtiendo el vector de velocidad).

**Dirección de la fuerza:** 
- Se obtiene multiplicando por -1 la velocidad para invertir la dirección de la misma.
- En el código se debe hacer una copia del valor de velocidad y multiplicarlo por -1.
  ``` js
  friction = mover.velocity.copy();
  friction.mult(-1);
  ```

**Magnitud de la fuerza:** 
- Se obtiene igualando la fricción con el producto del coeficiente de rozamiento y el inverso de la velocidad.
- En el código se ajusta la magnitud de la fricción usando el coeficiente de fricción c, en vez de multiplicarlo por el valor inverso de la velocidad para mayor practicidad.
  ``` js
  friction.setMag(c);
  ```

#### Idea
Hacer una aplicación que tenga un botón para cambiar la superficie del suelo, manteniendo la misma bola para observar claramente cómo afecta la fricción al movimiento en función del coeficiente de rozamiento.

#### Código
**mover.js**
``` js
class Mover {
  constructor(x, y, m) {
    // La masa del objeto
    this.mass = m;

    // Radio del objeto, proporcional a la masa
    this.radius = m * 8;

    // Posición inicial del objeto en el lienzo
    this.position = createVector(x, y);

    // Velocidad inicial (sin movimiento)
    this.velocity = createVector(0, 0);

    // Aceleración inicial (sin fuerzas aplicadas)
    this.acceleration = createVector(0, 0);
  }

  // Método para aplicar una fuerza externa al objeto
  applyForce(force) {
    // Se divide la fuerza por la masa según la Segunda Ley de Newton: F = m * a  →  a = F / m
    let f = p5.Vector.div(force, this.mass);

    // Se suma la aceleración generada por la fuerza aplicada
    this.acceleration.add(f);
  }

  // Método para actualizar la posición del objeto en cada cuadro de animación
  update() {
    // Se actualiza la velocidad sumando la aceleración acumulada
    this.velocity.add(this.acceleration);

    // Se actualiza la posición sumando la velocidad (movimiento)
    this.position.add(this.velocity);

    // Se resetea la aceleración para el siguiente cuadro (evita acumulación infinita de fuerza)
    this.acceleration.mult(0);
  }

  // Método para dibujar el objeto en la pantalla
  show() {
    stroke(0);            // Contorno negro
    strokeWeight(2);      // Grosor del contorno
    fill(16, 194, 155);   // Color de relleno verde-azulado
    circle(this.position.x, this.position.y, this.radius * 2); // Dibuja el círculo en la posición actual
  }

  // Método para manejar el rebote en los bordes del lienzo
  bounceEdges() {
    let bounce = -0.9; // Factor de rebote (reduce velocidad al chocar)

    // Si el objeto toca el borde derecho del lienzo
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius; // Lo reposiciona en el borde
      this.velocity.x *= bounce; // Invierte y reduce la velocidad en el eje X
    }
    // Si el objeto toca el borde izquierdo del lienzo
    else if (this.position.x < this.radius) {
      this.position.x = this.radius; // Lo reposiciona en el borde
      this.velocity.x *= bounce; // Invierte y reduce la velocidad en el eje X
    }
  }
}
```

**sketch.js**
``` js
let mover; // Variable para el objeto Mover
let sueloIndex = 0; // Índice para seleccionar el tipo de suelo
let coeficientesFriccion = [0.0001, 0.1, 0.9]; // Coeficientes de fricción según el tipo de suelo
let nombresSuelo = ["Liso", "Medio Rugoso", "Muy Rugoso"]; // Nombres de los tipos de suelo
let botonSuelo; // Botón para cambiar el tipo de suelo

function setup() {
  createCanvas(550, 300); // Crear un lienzo de 550x300 píxeles
  mover = new Mover(width / 2, height - 30, 5); // Crear el objeto Mover en el centro y cerca del suelo

  // Crear un botón para cambiar el tipo de suelo
  botonSuelo = createButton("Suelo: " + nombresSuelo[sueloIndex]);
  botonSuelo.position(10, 10); // Posición del botón en la pantalla
  botonSuelo.mousePressed(cambiarSuelo); // Llamar a la función cambiarSuelo cuando se presione

  // Texto explicativo en la pantalla
  createP("Usa las flechas izquierda y derecha para mover la bola. Cambia el suelo con el botón.");
}

function draw() {
  background(200); // Fondo gris claro
  dibujarSuelo(); // Dibujar el suelo según el tipo seleccionado

  // Aplicar una fuerza hacia la izquierda si la tecla de flecha izquierda está presionada
  if (keyIsDown(LEFT_ARROW)) {
    mover.applyForce(createVector(-10, 0)); // Fuerza negativa en el eje X
  }

  // Aplicar una fuerza hacia la derecha si la tecla de flecha derecha está presionada
  if (keyIsDown(RIGHT_ARROW)) {
    mover.applyForce(createVector(10, 0)); // Fuerza positiva en el eje X
  }

  // Aplicar fricción solo si el objeto se está moviendo
  if (mover.velocity.mag() > 0) {
    let c = coeficientesFriccion[sueloIndex]; // Obtener el coeficiente de fricción según el suelo seleccionado
    let friction = mover.velocity.copy(); // Copiar la velocidad actual
    friction.mult(-1); // Invertir la dirección para que la fricción se oponga al movimiento
    friction.setMag(c); // Ajustar la magnitud de la fricción usando el coeficiente
    mover.applyForce(friction); // Aplicar la fuerza de fricción al objeto
  }

  mover.bounceEdges(); // Verificar colisiones con los bordes y hacer que rebote
  mover.update(); // Actualizar la posición y velocidad del objeto
  mover.show(); // Dibujar el objeto en pantalla
}

// Función para cambiar el tipo de suelo cuando se presiona el botón
function cambiarSuelo() {
  sueloIndex = (sueloIndex + 1) % coeficientesFriccion.length; // Cambiar al siguiente tipo de suelo de forma cíclica
  botonSuelo.html("Suelo: " + nombresSuelo[sueloIndex]); // Actualizar el texto del botón con el nuevo tipo de suelo
}

// Función para dibujar el suelo dependiendo del tipo seleccionado
function dibujarSuelo() {
  stroke(0); // Color negro para las líneas del suelo
  strokeWeight(2); // Grosor de la línea

  if (sueloIndex === 0) {
    // Suelo liso: una sola línea recta
    line(0, height - 5, width, height - 5);
  } else if (sueloIndex === 1) {
    // Suelo medio rugoso: líneas cortas en ángulo
    for (let x = 0; x < width; x += 10) {
      line(x, height - 5, x + 5, height);
    }
  } else if (sueloIndex === 2) {
    // Suelo muy rugoso: líneas más largas y separadas
    for (let x = 0; x < width; x += 15) {
      line(x, height - 5, x + 10, height);
    }
  }
}
```
#### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/BZrpUw51U)
### Resistencia del aire y de fluidos
#### Modelado de la fuerza
**Concepto:** La resistencia del aire (o resistencia de fluidos en general) es una fuerza opuesta al movimiento de un objeto cuando se desplaza a través de un fluido (como el aire o el agua). Esta resistencia depende de varios factores y es crucial en la física del movimiento porque reduce la velocidad de los objetos y afecta su trayectoria.

**¿Qué implica en el movimiento?** Cuando un objeto se mueve en un fluido:
- La resistencia del aire disminuye su velocidad con el tiempo.
- Si no hay otra fuerza empujándolo, el objeto se detendrá eventualmente.
- Cuanto más rápido se mueve un objeto, más fuerte es la resistencia.
- Cuando un objeto cae en el aire, la resistencia aumenta hasta que la fuerza de gravedad y la resistencia se equilibran, alcanzando la velocidad terminal (velocidad máxima de caída sin aceleración).

**Fórmula matemática:** Resistencia del aire = 1/2 * C * ρ * A * v^2
- 𝐶: Coeficiente de arrastre (depende de la forma del objeto y su superficie).
- 𝜌: Densidad del fluido (kg/m³, por ejemplo, 1.225 kg/m³ para el aire a nivel del mar).
- 𝐴: Área frontal del objeto (m², el área que "choca" con el aire).
- 𝑣: Velocidad del objeto en el fluido (m/s).

**Dirección de la fuerza:** 
- Se invierte la dirección del vector velocidad.
- En el código se debe hacer una copia del valor de velocidad y multiplicarlo por -1.
  ``` js
  friction = mover.velocity.copy();
  friction.mult(-1);
  ```
  
**Magnitud de la fuerza:** 
-  ![image](https://github.com/user-attachments/assets/b151deab-23ee-420c-bb4a-73a7472b59dc)

- En el código sería
  ``` js
  let c = 0.1;
  let speed = this.velocity.mag();
  let dragMagnitude = c * speed * speed;
  ```
#### Idea
Crear una aplicación que muestre un pez que se mueve en dirección hacia el mouse y que no llega inmediatamente porque experimenta la resistencia del agua. También podría haber un botón para alternar entre tres peces con diferente masa y observar como influye la misma en la resistencia del agua y a su vez en el movimiento.
#### Código
**liquid.js**
``` js
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
    fill(0, 100, 200, 150);
    rect(this.x, this.y, this.w, this.h);
  }
}
```

**mover.js**
``` js
class Fish {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.maxSpeed = 3;
    this.size = 20; // Tamaño del pez
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  follow(targetX, targetY) {
    let target = createVector(targetX, targetY);
    let force = p5.Vector.sub(target, this.position);
    force.setMag(0.1); // Fuerza de atracción al mouse
    this.applyForce(force);
  }

  update() {
    // Si está en el agua, aplica resistencia
    if (this.position.y > height / 2) {
      let drag = this.velocity.copy();
      drag.mult(-0.1); // Resistencia del agua
      this.applyForce(drag);
    }

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  display() {
    push();
    translate(this.position.x, this.position.y);

    // Cuerpo del pez
    fill(255, 150, 0);
    ellipse(0, 0, this.size * 2, this.size); 

    // Cola
    fill(200, 100, 0);
    triangle(-this.size, 0, -this.size * 1.5, -this.size / 2, -this.size * 1.5, this.size / 2);

    // Ojo
    fill(255);
    ellipse(this.size / 2, -this.size / 4, this.size / 3, this.size / 3);
    fill(0);
    ellipse(this.size / 2, -this.size / 4, this.size / 6, this.size / 6);

    pop();
  }
}
```

**sketch.js**
``` js
let fish;
let liquid;

function setup() {
  createCanvas(600, 400);
  fish = new Fish(width / 2, height / 2);
  liquid = new Liquid(0, height / 2, width, height / 2, 0.1); // Agua con resistencia
}

function draw() {
  background(200, 230, 255); // Fondo azul claro

  liquid.display(); // Dibuja el agua
  fish.update();
  fish.display();
}

function mouseMoved() {
  fish.follow(mouseX, mouseY);
}
```
#### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/47EQJDKoZ)
### Atracción gravitacional
#### Modelado de la fuerza
**Concepto:** La atracción gravitacional es la fuerza que ejerce un objeto con masa sobre otro debido a la gravedad. Es la misma fuerza que mantiene los planetas en órbita alrededor del Sol y que hace que los objetos caigan al suelo en la Tierra.

**¿Qué implica en el movimiento?** Cuando un objeto experimenta la atracción gravitacional de otro, su trayectoria cambia. Si la fuerza es lo suficientemente fuerte, puede hacer que el objeto:
- Acelere hacia el objeto más masivo.
- Entre en órbita si tiene la velocidad adecuada.
- Escape si supera la velocidad de escape necesaria.

**Fórmula matemática:** La Ley de Gravitación Universal de Newton nos dice que la fuerza gravitacional entre dos objetos de masas 𝑚1 y 𝑚2 separados por una distancia 𝑟 es:

**𝐹 = (𝐺 * 𝑚1 * 𝑚2)/𝑟^2**
- F es la magnitud de la fuerza gravitacional.
- 𝐺 es la constante de gravitación universal, aproximadamente 6.674×10^−11 N * m^2/kg^2
- 𝑚1 y 𝑚2 son las masas de los dos objetos.
- 𝑟 es la distancia entre los centros de los dos objetos.

**Dirección de la fuerza:** La fuerza de gravedad siempre apunta hacia el centro del objeto más masivo. Para calcular su dirección usando vectores:
- Determinar el vector de separación
  ![image](https://github.com/user-attachments/assets/cf78239a-da1f-44cd-b683-ad7749920a57)

- En el código para la atracción hacia el mouse
  ``` js
  let direction = p5.Vector.sub(mouse, position);
  ```
- En el código para la atracción entre dos objetos
  ``` js
  let direction = p5.Vector.sub(position1, position2);
  direction.normalize();
  ```
  
**Magnitud de la fuerza:** La magnitud de la fuerza es simplemente la fórmula de Newton: **𝐹 = (𝐺 * 𝑚1 * 𝑚2)/𝑟^2**
Para convertir esta magnitud en un vector de fuerza aplicable en un sistema de simulación, se multiplica el vector unitario por la magnitud de la fuerza: 𝐹 = 𝐹 ⋅ 𝑟 Esto nos da un vector que podemos aplicar a un objeto para simular su aceleración debido a la gravedad.
#### Idea
Crear una aplicación en la que se aprecie la tierra en el centro del lienzo. Incorporar un botón para lanzar asteroides con diferentes velocidades para mayor estética. Los asteroides serán atraídos hacia la tierra pero también se atraerán entre ellos. Al colisionar entre ellos o con la tierra se verán pequeñas partículas.
#### Código
**attractor.js**
``` js
class Attractor {
  constructor(x, y, m) {
    this.position = createVector(x, y);
    this.mass = m;
    this.radius = m;
  }

  attract(mover) {
    let force = p5.Vector.sub(this.position, mover.position);
    let distance = constrain(force.mag(), 10, 100); // Evita fuerzas extremadamente grandes
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    return force.setMag(strength);
  }

  show() {
    fill(0, 100, 255);
    noStroke();
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }
}
```

**mover.js**
``` js
class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    this.radius = mass * 2;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    // Asignar un color aleatorio entre los dos al momento de crearlo
    let c1 = color('#B90C31'); // Rojo oscuro
    let c2 = color('#FD8B00'); // Naranja
    this.color = lerpColor(c1, c2, random(0, 1)); 
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    fill(this.color);
    stroke(this.color);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  attract(other) {
    let force = p5.Vector.sub(this.position, other.position);
    let distance = constrain(force.mag(), 5, 50);
    let strength = (G * this.mass * other.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  collidesWith(other) {
    let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
    return d < this.radius + other.radius;
  }
}
```

**particle.js**
``` js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(random(1, 3));
    this.lifespan = 60; // Duración en frames (~1 segundo)
  }

  update() {
    this.position.add(this.velocity);
    this.lifespan -= 1;
  }

  show() {
    noStroke();
    fill(255, 212, 126, this.lifespan * 4); // Se desvanece con el tiempo
    ellipse(this.position.x, this.position.y, 4);
  }

  isDead() {
    return this.lifespan <= 0;
  }
}
```

**sketch.js**
``` js
let asteroids = [];
let earth;
let G = 10; // Aumentamos la gravedad
let stars = [];

function setup() {
  createCanvas(400, 400);
  earth = new Attractor(width / 2, height / 2, 30);
  
  // Generar estrellas estáticas
  for (let i = 0; i < 50; i++) {
    stars.push(createVector(random(width), random(height)));
  }

  // Botón para lanzar asteroides
  let launchButton = createButton("Lanzar asteroide");
  launchButton.position(300, 20);
  launchButton.mousePressed(() => {
    let asteroid = new Mover(380, 20, 5);
    asteroid.velocity = createVector(random(-2, 2), random(2, 4)); // Velocidad aleatoria
    asteroids.push(asteroid);
  });
}

function draw() {
  background(0);

  // Dibujar estrellas
  fill(255);
  noStroke();
  for (let star of stars) {
    circle(star.x, star.y, 2);
  }

  earth.show();

  let toRemove = []; // Lista de asteroides a eliminar

  for (let i = 0; i < asteroids.length; i++) {
    let asteroid = asteroids[i];
    
    let force = earth.attract(asteroid);
    asteroid.applyForce(force);

    // Atracción gravitacional entre asteroides
    for (let j = 0; j < asteroids.length; j++) {
      if (i !== j) {
        let forceBetween = asteroids[j].attract(asteroid);
        asteroid.applyForce(forceBetween);
      }
    }

    asteroid.update();
    asteroid.show();

    // Detectar colisiones con la Tierra
    if (asteroid.collidesWith(earth)) {
      explode(asteroid.position);
      toRemove.push(i);
    }

    // Detectar colisiones entre asteroides
    for (let j = i + 1; j < asteroids.length; j++) {
      if (asteroid.collidesWith(asteroids[j])) {
        explode(asteroid.position);
        toRemove.push(i, j);
      }
    }
  }

  // Eliminar asteroides después de revisar colisiones
  toRemove = [...new Set(toRemove)]; // Eliminar duplicados
  toRemove.sort((a, b) => b - a); // Ordenar de mayor a menor
  for (let index of toRemove) {
    asteroids.splice(index, 1);
  }
  // Dibujar y actualizar partículas
  for (let i = particles.length - 1; i >= 0; i--) {
    particles[i].update();
    particles[i].show();
    if (particles[i].isDead()) {
      particles.splice(i, 1);
    }
  }
}

// Función para simular explosión
let particles = []; // Array para almacenar las partículas

function explode(position) {
  for (let i = 0; i < 10; i++) {
    particles.push(new Particle(position.x, position.y));
  }
}
```
#### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/8QV2zHT2M)
