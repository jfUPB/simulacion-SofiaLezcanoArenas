## Experimenta
### Entiende el ejemplo
En el código tenemos una clase para crear un objeto en movimiento que se encuentra en un archivo aparte del principal. 

La clase dota al objeto con vector de posición y vector de movimiento generados de manera aleatoria, 
además de un método para actualizar la posición, un método para dibujar el objeto y un método para revisar si se sale de los bordes y
en caso de ser cierto, una manera de proceder al respecto.

En el archivo principal se crea una variable para almacenar la información del objeto, se crea el objeto y en la función draw() 
se usan cada uno de los métodos que posee para moverlo, chequear bordes y dibujarlo de manera precisa.
### Propuesta de modificación
¿Qué pasa si en vez de un objeto hubieran dos y que los vectores posición y velocidad del objeto 2, 
fueran múltiplos de los vectores del objeto 1?
### ¿Qué te imaginas que pasará?
Ambos objetos se moverán de manera paralela separados por la cantidad de pixeles que indique
el número por el que se multipliquen los vectores posición y velocidad del objeto 2.
### ¿Qué pasó y por qué?
**Problemas:**
- Si la velocidad es igual para ambos, no se visualizan en la pantalla los dos, sino solo uno.
- Si la velocidad es diferente si se visualizan los dos, pero en este caso como la velocidad del 2 era 5 veces la del 1, iba muy rápido.

**Solución:**
Como no entendía cuál era el problema, le pregunté a chat y esto fue lo que me dijo: Tu código casi logra el efecto deseado, 
pero hay un problema: en el setup(), cuando creas mover2,
intentas establecer su posición y velocidad en función de mover,
pero mover.position sigue estando en (0,0) en ese momento, por lo que mover2 también empieza en (0,0).

Así que me dispuse a darle un valor específico al vector posición y hacer que la velocidad solo tenga valores en y. Además, al iniciar el programa, se le asigna la velocidad del objeto 1 al objeto 2 e igual con la posición, exceptuando que al del objeto 2, le sumamos 100 a la componente x para que se observe un poco más a la derecha.

**Código clase mover**
``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Mover {
  constructor() { //El objeto tiene los vectores de posición y velocidad
    this.position = createVector(30,30);
   this.velocity = createVector(0, random(0, 5));
  }
  

  update() { //tiene un método para actualizar la posición
    this.position.add(this.velocity);
  }

  show() { //tiene un método para dibujar el objeto
    stroke(0);
    strokeWeight(2);
    fill(127);
    circle(this.position.x, this.position.y, 48);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = 0;
    } else if (this.position.x < 0) {
      this.position.x = width;
    }

    if (this.position.y > height) {
      this.position.y = 0;
    } else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}
```

**Código sketch**
``` js
let mover, mover2;

function setup() {
  createCanvas(400, 400);
  
  // Se crea el primer objeto con una posición aleatoria
  mover = new Mover();
  
  // Se crea el segundo objeto con la misma velocidad pero desplazado en X
  mover2 = new Mover();
  
  // Asignar a mover2 la misma velocidad de mover
  mover2.velocity.set(mover.velocity.x, mover.velocity.y);
  
  // Desplazar mover2 para que no inicie en la misma posición
  mover2.position.set(mover.position.x + 100, mover.position.y);
}

function draw() {
  background(255);

  mover.update();
  mover.checkEdges();
  mover.show();

  mover2.update();
  mover2.checkEdges();
  mover2.show();
}
```

[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/6x7w2HGIq)
### Conclusiones
- Siempre hay que tener cuidado en la sintaxis, cosas que pensamos obvias podrían no serlo y es mejor confirmar con fuentes como documentación o IA.
- Es viable crear una clase de movimiento que directamente inicialice los vectores posición y movimiento porque esto no significa que todos los objetos que creemos sean iguales, pues se pueden usar funciones como random() para garantizar aleatoriedad o set() para cambiar las componentes de un vector luego de que se ha creado.
