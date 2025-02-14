## Motion 101
### ¿En qué consiste motion 101?
En palabras simples consiste en estos dos pasos:
- Sumarle la velocidad (indica cuánto cambia la posición en cada instante) a la posición (indica cuánto cambia la posición en cada instante).
- Dibujar el objeto en la nueva posición.
### Ejemplo
En este ejemplo una bolita se movera desde la esquina superior izquierda (0,0) hasta la esquina inferior derecha utilizando el algoritmo Motion 101. Cuando la bolita llegue al final del lienzo, su movimiento parará al hacer la velocidad cero.
**Código**
``` js
class Mover {
  constructor() {
    // Se crea un vector para la posición, iniciando en el centro del lienzo
    this.position = createVector(width / 2, height / 2);
    
    // Se crea un vector para la velocidad con valores constantes
    this.velocity = createVector(2, 1); // Se moverá 2 píxeles en X y 1 píxel en Y por cuadro
  }

  update() {
    // Se suma la velocidad a la posición en cada cuadro, haciendo que el objeto se mueva
    this.position.add(this.velocity);
  }

  display() {
    // Se establece el color blanco para el círculo
    fill(255);
    
    // Se dibuja un círculo en la posición actual
    ellipse(this.position.x, this.position.y, 20, 20);
  }
}

let mover; // Se declara una variable para el objeto Mover

function setup() {
  createCanvas(400, 400); // Se crea un lienzo de 400x400 píxeles
  mover = new Mover(); // Se instancia un objeto de la clase M
```

[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/-lkDqN_FT)
