## Investigación y experimentación con Matter.js

### Experimento 1: crear cajas en donde se haga clic con el mouse
#### Código
**ground.js**
``` js
class Ground { // clase para el suelo
  constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    
    this.body = Bodies.rectangle(x, y, this.w, this.h, {isStatic: true}); // creamos un suelo estático
    Composite.add(engine.world, this.body); // añadimos el suelo al cuerpo para visualizarlo correctamente
  }
  
  display() {
    push();
    rectMode(CENTER);
    let x = this.body.position.x;
    let y = this.body.position.y; 
    translate(x, y);
    rect(0, 0, this.w, this.h);
    pop();
  }
}
```

**rect.js**
``` js
class Rect { // clase para los rectángulos
  constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    this.color = lerpColor(color("#BB2B5C"), color("#368AB0"), random(0, 1));  // Genera un color aleatorio entre dos colores al crear el objeto
    
    this.body = Bodies.rectangle(x, y, this.w, this.h);
    Body.setAngularVelocity(this.body, 0.2);
    Composite.add(engine.world, this.body); // se añade el rectángulo al mundo para poder visualizarlo correctamente y que se vea afectado por las fuerzas del mundo
  }
  
  display() { // transforma el cuadrado y lo dibuja como se indica con el código de matter.js
    push(); // se delimita la transformación con push y pop para evitar que los demás objetos se vean afectados por ella
    rectMode(CENTER); // ubica en el centro del cuadrado el punto de origen
    let x = this.body.position.x;
    let y = this.body.position.y; 
    let angle = this.body.angle;
    translate(x, y);
    rotate(angle);
    fill(this.color); // Usa el color definido al inicio
    rect(0, 0, this.w, this.h);
    pop();
  }
}
```

**sketch.js**
``` js
const {Engine, Body, Bodies, Composite} = Matter;

let engine;
let boxes = []; 
let ground;

function setup() {
  createCanvas(400, 400);
  engine = Engine.create(); // creamos el motor
 
  ground = new Ground(200, 300, 400, 10);
}

function draw() {
  background(220);
  Engine.update(engine);
  for (let i=0; i<boxes.length; i++) {
    boxes[i].display();
  }
  ground.display();
}

function mousePressed() { // se llama cada vez que se presiona el mouse
  boxes.push(new Rect(mouseX, mouseY, 20, 20)); // añade un nuevo rectángulo en la posición del mouse de 20x20
}
```
#### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/_x0_1ajEm)

![image](https://github.com/user-attachments/assets/8d8fdd02-716d-4731-89e7-3cfddaadbeaf)

### Experimento 2
#### Código
#### Resultado

### Conceptos
- **Engine**: maneja la parte de la simulación física actualizando el estado de los objetos en el tiempo. Se asegura de que a cada objeto se le apliquen fuerzas como la gravedad, fricción y demás, de manera correcta y precisa.
- **World**: contiene todo lo de la simulación, objetos, cuerpos, constraints. Es donde la simulación toma lugar. Si algo está fuera del mundo, no se verá afectado por ninguna fuerza ni nada.
- **Body**: es un objeto en la simulación que tiene forma y propiedades definidas. Interactúa con los demás cuerpos de la simulación y puede ser estático (mantenerse quieto como el suelo) o dinámico (moverse).
- **Bodies**: es un namespace, contenedor para almacenar propiedades de métodos u objetos bajo un mismo nombre, facilitando así la creación de un tipo específico de cuerpo.
- **Constraint**: cómo se conectan y relacionan los cuerpos dentro de un mundo.
- **MouseConstraint**: complemento (plugin) que crea una especie de restricción invisible (constraint) entre el cursor del mouse y los cuerpos con los que se hace clic, permitiendo manipularlos de forma realista gracias a las leyes físicas simuladas.

### Dificultades con `Matter.js`
