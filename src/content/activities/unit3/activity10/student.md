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

**Dirección de la fuerza:** se invierte la dirección del vector velocidad.

**Magnitud de la fuerza:** 
#### Idea
Crear una aplicación que muestre un pez que se mueve en dirección hacia el mouse y que no llega inmediatamente porque experimenta la resistencia del agua. También podría haber un botón para alternar entre tres peces con diferente masa y observar como influye la misma en la resistencia del agua y a su vez en el movimiento.
#### Código
#### Resultado
[Enlace a la simulación]()
### Atracción gravitacional
#### Modelado de la fuerza
#### Idea
#### Código
#### Resultado
[Enlace a la simulación]()
