## Materialización
### Código de la aplicación
**planet.js**
``` js
// Planet.js

class Planet {
  constructor(x, y, mass, color, radius, hasRing = false) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-1, 1));
    this.acceleration = createVector();
    this.mass = mass;
    this.color = color;
    this.radius = radius;
    this.hasRing = hasRing;
    this.trail = []; // Array para los rastros
  }

  update() {
    let sun = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(sun, this.position);
    let strength = 0.5 / this.mass;
    dir.setMag(strength);
    this.acceleration = dir;

    this.velocity.add(this.acceleration);
    this.velocity.limit(5);
    this.position.add(this.velocity);

    // Agregar la posición actual al rastro
    this.trail.push(this.position.copy());
    if (this.trail.length > 50) {
      this.trail.shift();
    }
  }

  show() {
    noStroke();
    fill(this.color);
    circle(this.position.x, this.position.y, this.radius * 2);

    // Dibujar la estela como pequeñas centellas
    for (let i = 0; i < this.trail.length; i++) {
      let pos = this.trail[i];
      let alpha = map(i, 0, this.trail.length, 0, 255);
      fill(this.color.levels[0], this.color.levels[1], this.color.levels[2], alpha);
      circle(pos.x, pos.y, 3); // Centellas pequeñas
    }

    // Dibujar el anillo si el planeta lo tiene
    if (this.hasRing) {
      stroke(200, 200, 200);
      noFill();
      ellipse(this.position.x, this.position.y, this.radius * 3, this.radius);
    }
  }
}
```

**sketch.js**
``` js
// sketch.js

let planets = [];
let stars = [];

function setup() {
  createCanvas(800, 800);
  // Crear los 8 planetas
  planets.push(new Planet(100, height / 2, 0.3, color(169, 169, 169), 5)); // Mercurio
  planets.push(new Planet(200, height / 2, 0.8, color(255, 140, 0), 10)); // Venus
  planets.push(new Planet(300, height / 2, 1, color(0, 0, 255), 12)); // Tierra
  planets.push(new Planet(400, height / 2, 0.6, color(255, 0, 0), 8)); // Marte
  planets.push(new Planet(500, height / 2, 318, color(255, 215, 0), 20)); // Júpiter
  planets.push(new Planet(600, height / 2, 95, color(210, 180, 140), 18, true)); // Saturno (con anillo)
  planets.push(new Planet(700, height / 2, 14, color(0, 255, 255), 15)); // Urano
  planets.push(new Planet(750, height / 2, 17, color(0, 0, 139), 14)); // Neptuno

  // Crear estrellas estáticas
  for (let i = 0; i < 100; i++) {
    stars.push({ x: random(width), y: random(height) });
  }
}

function draw() {
  background(0);

  // Dibujar estrellas estáticas
  stroke(255);
  strokeWeight(2);
  for (let star of stars) {
    point(star.x, star.y);
  }

  // Sol amarillo en la posición del mouse
  fill(255, 204, 0);
  noStroke();
  circle(mouseX, mouseY, 50);

  for (let planet of planets) {
    planet.update();
    planet.show();
  }
}
```
### Captura del contenido generado
![image](https://github.com/user-attachments/assets/9bde85f6-5afd-4460-b98c-3fc9801e1eda)

![image](https://github.com/user-attachments/assets/d7c0cf7e-0dc5-4bc1-b887-6066b1f7c40b)

[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/6rweEh0lI)
### Variaciones al concepto original y razones
El concepto original surgió en la actividad 9 con el experimento de aceleración hacia el mouse, en donde primero pensé que podía tener representaciones de los planetas orbitando alrededor del sol. Al principio todos se movían de igual manera, pero luego me di cuenta que podía hacerlo más realista si su masa afectaba su movimiento, por lo que lo apliqué con chat. Finalmente me dio la idea de la estela que al principio era una línea, un trazo. Puliendo lo que me entregaba, este fue el resultado final.
