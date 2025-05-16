## Resortes
### ¿Cómo funciona un sistema de resortes en serie?
En un sistema de resortes en serie, el comportamiento es diferente al de un solo resorte porque ambos resortes y masas interactúan entre sí.

Cuando dos resortes están conectados en serie, el segundo resorte "hereda" las fuerzas que actúan sobre el primero. En este caso:
- Resorte 1 está anclado al techo y sostiene a Bob 1.
- Resorte 2 está conectado a Bob 1 y sostiene a Bob 2.
- Bob 2 ejerce una fuerza hacia abajo sobre Bob 1 debido a su peso (gravedad).
- Bob 1 siente tanto su propio peso como la fuerza extra de Bob 2, lo que hace que el resorte 1 se estire más.

En términos físicos:
- La tensión en ambos resortes es la misma porque están en serie.
- La elongación total del sistema es la suma de las elongaciones de cada resorte.
- La rigidez equivalente del sistema es menor que la de los resortes individuales.
### ¿Cómo se refleja en nuestro código?
- El movimiento de Bob 2 afecta a Bob 1 porque la fuerza que lo jala hacia abajo se transmite a través del Resorte 2.
- El Resorte 1 se estira más cuando Bob 2 cuelga porque ahora sostiene dos masas en lugar de una.
- Si mueves Bob 1, Bob 2 también se mueve porque su punto de anclaje (Bob 1) ha cambiado.
### Código
**bob.js**
``` js
// bob.js

// Clase Bob que representa una masa conectada a un resorte
class Bob {
  constructor(x, y, color) { 
    this.position = createVector(x, y); // Posición de la bola
    this.velocity = createVector(); // Velocidad de la bola
    this.acceleration = createVector(); // Aceleración de la bola
    this.mass = 24; // Masa de la bola
    this.damping = 0.98; // Amortiguación para simular fricción
    this.dragOffset = createVector(); // Offset para el arrastre con el mouse
    this.dragging = false; // Estado de arrastre
    this.color = color; // Se almacena el color de la bola
  }

  update() {
    this.velocity.add(this.acceleration); // Se actualiza la velocidad
    this.velocity.mult(this.damping); // Se aplica amortiguación
    this.position.add(this.velocity); // Se actualiza la posición
    this.acceleration.mult(0); // Se reinicia la aceleración
  }

  applyForce(force) {
    let f = force.copy(); // Copia la fuerza aplicada
    f.div(this.mass); // Se ajusta según la masa
    this.acceleration.add(f); // Se agrega la aceleración
  }

  show() {
    stroke(0); // Color del borde
    strokeWeight(2); // Grosor del borde
    fill(this.color); // Se usa el color específico de la bola
    if (this.dragging) {
      fill(200); // Color diferente cuando es arrastrada
    }
    circle(this.position.x, this.position.y, this.mass * 2); // Dibuja la bola
  }

  handleClick(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y); // Calcula distancia del mouse
    if (d < this.mass) { // Si el mouse está sobre la bola
      this.dragging = true; // Activa el arrastre
      this.dragOffset.x = this.position.x - mx;
      this.dragOffset.y = this.position.y - my;
    }
  }

  stopDragging() {
    this.dragging = false; // Desactiva el arrastre
  }

  handleDrag(mx, my) {
    if (this.dragging) { // Si se está arrastrando
      this.position.x = mx + this.dragOffset.x;
      this.position.y = my + this.dragOffset.y;
    }
  }
}
```

**spring.js**
``` js
// spring.js

// Clase Spring que representa un resorte que conecta dos objetos
class Spring {
  constructor(anchorX, anchorY, length) {
    this.anchor = createVector(anchorX, anchorY); // Punto de anclaje del resorte
    this.restLength = length; // Longitud en reposo del resorte
    this.k = 0.2; // Constante de elasticidad
  }

  connect(bob) {
    let force = p5.Vector.sub(bob.position, this.anchor); // Vector de anclaje al objeto
    let currentLength = force.mag(); // Longitud actual del resorte
    let stretch = currentLength - this.restLength; // Diferencia con la longitud en reposo
    force.setMag(-1 * this.k * stretch); // Se ajusta la fuerza según Hooke
    bob.applyForce(force); // Se aplica la fuerza al objeto
  }

  updateAnchor(newAnchor) {
    this.anchor.set(newAnchor.x, newAnchor.y); // Actualiza el anclaje dinámico
  }

  constrainLength(bob, minlen, maxlen) {
    let direction = p5.Vector.sub(bob.position, this.anchor); // Vector de anclaje al objeto
    let length = direction.mag(); // Longitud actual

    if (length < minlen) { // Si es menor que el mínimo
      direction.setMag(minlen);
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    } else if (length > maxlen) { // Si es mayor que el máximo
      direction.setMag(maxlen);
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    }
  }

  show() {
    fill(127);
    circle(this.anchor.x, this.anchor.y, 10); // Dibuja el punto de anclaje
  }

  showLine(bob) {
    stroke(0);
    line(bob.position.x, bob.position.y, this.anchor.x, this.anchor.y); // Dibuja la conexión con el resorte
  }
}
```

**sketch.js**
``` js
// sketch.js

let bob1, bob2;
let spring1, spring2;

function setup() {
  createCanvas(640, 400);

  // Resorte 1 anclado al techo
  spring1 = new Spring(width / 2, 50, 100);
  bob1 = new Bob(width / 2, 150, color(255, 0, 0)); // Rojo

  // Resorte 2 anclado dinámicamente a Bob 1
  spring2 = new Spring(bob1.position.x, bob1.position.y, 100);
  bob2 = new Bob(width / 2, 250, color(0, 0, 255)); // Azul
}

function draw() {
  background(255);

  let gravity = createVector(0, 2);
  bob1.applyForce(gravity);
  bob2.applyForce(gravity);

  bob1.update();
  bob1.handleDrag(mouseX, mouseY);
  
  // Anclaje dinámico del segundo resorte a Bob 1
  spring2.updateAnchor(bob1.position);

  bob2.update();
  bob2.handleDrag(mouseX, mouseY);

  spring1.connect(bob1);
  spring2.connect(bob2);

  spring1.constrainLength(bob1, 30, 200);
  spring2.constrainLength(bob2, 30, 200);

  spring1.showLine(bob1);
  spring2.showLine(bob2);

  bob1.show();
  bob2.show();

  spring1.show();
  spring2.show();
}

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/TT9G06whS)

![image](https://github.com/user-attachments/assets/ceb97a06-02a1-4219-95cc-59b339bb258d)

![image](https://github.com/user-attachments/assets/3d344dbc-d57f-455f-b67a-f1389cc86bf1)
