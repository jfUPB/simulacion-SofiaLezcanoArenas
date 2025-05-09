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

### Experimento 2: constraint para conectar temporalmente una bola al mouse mientras crece a estar haciendo clic
#### Código
**boundary.js**
``` js
class Boundary {
  constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    
    this.body = Bodies.rectangle(x, y, this.w, this.h, {isStatic: true});
    Composite.add(engine.world, this.body);
  }
  
  display() {
    fill(0);
    rectMode(CENTER);
    let x = this.body.position.x;
    let y = this.body.position.y;
    rect(x, y, this.w, this.h);
  }
}
```

**circle.js**
``` js
class Circle {
  constructor(x, y, r) {
    this.r = r
    this.c = color(random(colorPalette));
    this.done = false;
    this.body = Bodies.circle(x, y, this.r);
    
    let magnitude = 5;
    let velocity = Vector.create(random(-magnitude, magnitude), random(-magnitude, magnitude));
    Body.setVelocity(this.body, velocity);
    Composite.add(engine.world, this.body);
  }
  
  checkEdges() {
    let x = this.body.position.x;
    let y = this.body.position.y;
    if (x + this.r < 0 || x - this.r > width ||
       y + this.r < 0 || y - this.r > height) {
      this.done = true;
    } else {
      this.done = false;
    }
  }
  
  removeCircle() {
    Composite.remove(engine.world, this.body);
  }
  
  display() {
    fill(this.c);
    ellipse(this.body.position.x, this.body.position.y, this.r*2, this.r*2);
  }
}
```

**sketch.js**
``` js
const {Engine, Body, Bodies, Composite, Vector} = Matter;

let duration, startTime;
let currentBall = null; // ← guarda la bola actual que está creciendo
let mouseConstraint = null;

let engine; 
let balls = []; 

let bottomEdge, topEdge, rightEdge, leftEdge;
let thickness = 20;

let colorPalette = ["#abcd5e", "#14976b", "#2b67af", "#62b6de", "#f589a3", "#ef562f", "#fc8405", "#f9d531"]; 

function setup() {
  createCanvas(400, 400);
  engine = Engine.create();
  engine.gravity = Vector.create(0, 0);
  
  bottomEdge = new Boundary(width/2, height - thickness/2, width, thickness);
  topEdge = new Boundary(width/2, thickness/2, width, thickness);
  leftEdge = new Boundary(thickness/2, height/2, thickness, height);
  rightEdge = new Boundary(width - thickness/2, height/2, thickness, height);
}

function draw() {
  background(220);
  Engine.update(engine);
  
  bottomEdge.display();
  topEdge.display();
  leftEdge.display();
  rightEdge.display();

  for (let i=balls.length-1; i>0; i--) {
    balls[i].checkEdges();
    balls[i].display();
    
    if (balls[i].done) {
      balls[i].removeCircle();
      balls.splice(i, 1);
    }
  }
  
  if (mouseIsPressed && currentBall) {
  currentBall.r += 0.3; // ← aumenta el radio visualmente
  Body.scale(currentBall.body, 1.01, 1.01); // ← escala también el cuerpo físico en Matter.js
    mouseConstraint.pointA = { x: mouseX, y: mouseY }; // ← el punto del constraint sigue al mouse
}
 

}

function mousePressed() {
  startTime = millis();
  currentBall = new Circle(mouseX, mouseY, 5); // ← crea una bola con radio    inicial 5
  balls.push(currentBall); // ← la añade al arreglo
  
  // Crear un constraint entre el mouse y la bola
  mouseConstraint = Matter.Constraint.create({
    pointA: { x: mouseX, y: mouseY },
    bodyB: currentBall.body,
    length: 0,
    stiffness: 0.05
  });

  Composite.add(engine.world, mouseConstraint);
}

function mouseReleased() {
  if (currentBall) {
    // Aplica una velocidad aleatoria
    let magnitude = 5;
    let velocity = Vector.create(random(-magnitude, magnitude), random(-magnitude, magnitude));
    Body.setVelocity(currentBall.body, velocity);

    currentBall = null; // ← indica que ya no hay bola activa
  }
  
  if (mouseConstraint) {
    Composite.remove(engine.world, mouseConstraint); // ← eliminar la cuerda
    mouseConstraint = null;
  }
}
```
#### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/LpdpWKJYK)

![image](https://github.com/user-attachments/assets/70328180-44e8-402f-9abd-12f44338525a)

### Conceptos
- **Engine**: maneja la parte de la simulación física actualizando el estado de los objetos en el tiempo. Se asegura de que a cada objeto se le apliquen fuerzas como la gravedad, fricción y demás, de manera correcta y precisa.
- **World**: contiene todo lo de la simulación, objetos, cuerpos, constraints. Es donde la simulación toma lugar. Si algo está fuera del mundo, no se verá afectado por ninguna fuerza ni nada.
- **Body**: es un objeto en la simulación que tiene forma y propiedades definidas. Interactúa con los demás cuerpos de la simulación y puede ser estático (mantenerse quieto como el suelo) o dinámico (moverse).
- **Bodies**: es un namespace, contenedor para almacenar propiedades de métodos u objetos bajo un mismo nombre, facilitando así la creación de un tipo específico de cuerpo.
- **Constraint**: cómo se conectan y relacionan los cuerpos dentro de un mundo.
- **MouseConstraint**: complemento (plugin) que crea una especie de restricción invisible (constraint) entre el cursor del mouse y los cuerpos con los que se hace clic, permitiendo manipularlos de forma realista gracias a las leyes físicas simuladas.

### Dificultades con `Matter.js`
- Separar la creación e interacción de fuerzas desde matter.js y dibujar desde p5.js
- No terminar de saber con qué se puede crear un constraint, pensaba que solo era entre los mismos cuerpos pero al parecer también se puede hacer con el mouse.
