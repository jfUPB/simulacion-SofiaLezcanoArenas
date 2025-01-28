# Caminatas aleatorias
## Experimento a realizar
Voy a crear un segundo caminante y en su método `step()`, solo dejaré la acción de crear un número aleatorio y en la función draw, despúes de llamar al método `step()` colocaré el condicional con las acciones correspondientes. Esto, porque serían diferentes para cada caminante.
## ¿Qué pregunta quieres responder con este experimento?
¿Qué pasaría si dejara el método `step()` únicamente generando un número al azar y el condicional que indica el paso a seguir dentro de la función `draw()`?
## ¿Qué resultados esperas obtener?
Quisiera que dos caminatas comiencen al mismo tiempo y vayan en direcciones contrarias.
## ¿Qué resultados obtuviste?
``` js
let choice;
let walker;
let oppositeW;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  oppositeW = new Walker();
  
  background(255);
}

function draw() {
  walker.step();
  if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  stroke(0);
  walker.show();
  //Caminante opuesto
  oppositeW.step();
  if (choice == 0) {
      this.x--;
    } else if (choice == 1) {
      this.x++;
    } else if (choice == 2) {
      this.y--;
    } else {
      this.y++;
    }
  stroke(0,100,50);
  oppositeW.show();
  
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    
    point(this.x, this.y);
  }

  step() {
    choice = floor(random(4));
    
  }
}
```
Con este código el programa no estaba funcionando, por lo que decidí intentar con un solo caminante para comprobar si estaba teniendo un mal manejo de varias instancias de una clase. El resultado fue igual, el programa seguía sin funcionar aún con un solo caminante.

Decidí preguntarle a ChatGPT, cómo lograría mi deseo para la funcionalidad del código, tomando de base el original y esto fue lo que arrojó:
``` js
let walker1, walker2;

function setup() {
  createCanvas(640, 240);
  walker1 = new Walker(width / 2, height / 2); // Caminante 1
  walker2 = new Walker(width / 2, height / 2); // Caminante 2
  background(255);
}

function draw() {
  walker1.step();
  walker2.oppositeStep(walker1.lastStep); // Caminante 2 sigue el movimiento opuesto
  walker1.show();
  walker2.show();
}

class Walker {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.lastStep = { x: 0, y: 0 }; // Guarda el último paso
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
      this.lastStep = { x: 1, y: 0 }; // Derecha
    } else if (choice == 1) {
      this.x--;
      this.lastStep = { x: -1, y: 0 }; // Izquierda
    } else if (choice == 2) {
      this.y++;
      this.lastStep = { x: 0, y: 1 }; // Abajo
    } else {
      this.y--;
      this.lastStep = { x: 0, y: -1 }; // Arriba
    }
  }

  oppositeStep(lastStep) {
    // Movimiento opuesto basado en el último paso del otro caminante
    this.x -= lastStep.x;
    this.y -= lastStep.y;
  }
}
```

Propuso tener dos métodos diferentes para step, uno normal y uno opuesto. Cada caminante llama a su método step correspondiente para lograr el resultado de ir en direcciones completamente opuestas. Finalmente agregué unas modificaciones al color, el fondo es gris, el trazo del normal es negro y del opuesto es blanco.
``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker1, walker2;

function setup() {
  createCanvas(400, 400);
  walker1 = new Walker(width / 2, height / 2); // Caminante 1
  walker2 = new Walker(width / 2, height / 2); // Caminante 2
  background(128);
}

function draw() {
  walker1.step();
  walker2.oppositeStep(walker1.lastStep); // Caminante 2 sigue el movimiento opuesto
  stroke(0);
  walker1.show();
  stroke(255);
  walker2.show();
}

class Walker {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.lastStep = { x: 0, y: 0 }; // Guarda el último paso
  }

  show() {
    
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
      this.lastStep = { x: 1, y: 0 }; // Derecha
    } else if (choice == 1) {
      this.x--;
      this.lastStep = { x: -1, y: 0 }; // Izquierda
    } else if (choice == 2) {
      this.y++;
      this.lastStep = { x: 0, y: 1 }; // Abajo
    } else {
      this.y--;
      this.lastStep = { x: 0, y: -1 }; // Arriba
    }
  }

  oppositeStep(lastStep) {
    // Movimiento opuesto basado en el último paso del otro caminante
    this.x -= lastStep.x;
    this.y -= lastStep.y;
  }
}
```
Imagen
## ¿Qué aprendiste de este experimento?
- Es un error tener la mitad de la funcionalidad que se espera de un método fuera de él.
- Es posible tener métodos en una clase que no todas las instancias usen.
- Se puede igualar una variable a una componente de un arreglo.
