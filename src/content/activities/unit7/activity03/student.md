## Animando la tipografía semántica
### Concepto para "Helio"
En la palabra helio, la "o" se comportará como un globo y mientras el usuario haga clic, este irá creciendo hasta abrazar el resto de letras de la palabra. Cuando tiene su tamaño final, el punto de la i se desprende y flota hasta quedar atrapada orbitando la letra "o".

Una idea extra de chatGPT que ofreció al mostrar mi concepto base, fue la de Transformar la "o" en un sol al final: Agregarle rayos que se expanden sutilmente, y un glow (efecto visual) con transparencia para dar calor/luz. Además de usar una tipografía infladita y redondeada para reforzar el concepto.

### Implementación

### Código
**sketch.js**
``` js
// sketch.js
const { Engine, Body, Bodies, Composite, Vector, Events } = Matter;

let engine;
let helioWord;
let growing = false;

function setup() {
  createCanvas(600, 400);
  engine = Engine.create();
  engine.gravity = Vector.create(0, 0);

  helioWord = new HelioWord(250, height / 2);
}

function draw() {
  background(220);
  Engine.update(engine);
  helioWord.update();
  helioWord.display(growing);
}

function mousePressed() {
  growing = true;
}

function mouseReleased() {
  growing = false;
}
```

**sun.js**
``` js
class Sun {
  constructor(x, y, r) {
    this.r = r;
    this.initialRadius = r;
    this.body = Bodies.circle(x, y, r, { isStatic: true });
    Composite.add(engine.world, this.body);
    this.maxR = r * 2; // Define el tamaño máximo si es necesario
  }

  grow(rate, maxR) {
    if (this.r < maxR) {
      this.r += rate;
      Body.scale(this.body, (this.r / this.initialRadius), (this.r / this.initialRadius));
      this.initialRadius = this.r;
    }
  }

  display() {
    push();

    // Activa el efecto glow cuando el sol alcanza su tamaño máximo
    if (this.r >= this.maxR) {
      drawingContext.shadowBlur = 50; // Tamaño del resplandor
      drawingContext.shadowColor = color(255, 150, 0); // Color del resplandor
    } else {
      drawingContext.shadowBlur = 0; // Sin resplandor si el sol no está en su tamaño máximo
    }

    noFill();
    stroke(255, 150, 0);
    strokeWeight(4);
    ellipse(this.body.position.x, this.body.position.y, this.r * 2);

    pop();
  }
}
```

**helioword.js**
``` js
class HelioWord {
  constructor(x, y) {
    this.x = x;
    this.y = y;

    let word = "heli";
    let spacing = 38;
    let startX = x - (word.length / 2) * spacing + spacing / 2;

    this.oRadius = 20;
    this.maxORadius = 180;
    this.growthRate = 2.5;
    this.glow = false;

    this.oCenter = createVector(startX + word.length * spacing, y);

    this.sun = new Sun(this.oCenter.x, this.oCenter.y, this.oRadius);

    this.letters = [];
    this.dot = null;

    for (let i = 0; i < word.length; i++) {
      let letter = new Letter(startX + i * spacing, y, word[i]);
      this.letters.push(letter);

      if (word[i] === "i") {
  this.dot = new FloatingDot(this.sun);
}

    }
  }

  update() {
    if (this.dot) {
      this.dot.update();  // Actualizamos la posición del punto
    }
  }

  display(growing) {
    if (growing) {
      this.sun.grow(this.growthRate, this.maxORadius);
    }

    // Aquí revisamos si el sol alcanzó su tamaño máximo para activar el resplandor
    if (this.sun.r >= this.maxORadius) {
      this.glow = true;
    }

    for (let letter of this.letters) {
      letter.display();
    }

    this.sun.display();
    if (this.dot) this.dot.display();  // Dibujamos el punto de la "i"
  }
}
```

**letter.js**
``` js
// letter.js
class Letter {
  constructor(x, y, letter) {
    this.letter = letter;
    this.size = 64;
    this.r = this.size / 2;
    this.body = Bodies.circle(x, y, this.r, { isStatic: true });
    Composite.add(engine.world, this.body);
  }

  display() {
    push();
    textAlign(CENTER, CENTER);
    textFont('sans-serif');
    textSize(this.r * 2);
    noStroke();
    fill(0);
    let x = this.body.position.x;
    let y = this.body.position.y;

    if (this.letter === "i") {
      stroke(0);
      strokeWeight(6);
      line(x, y - 10, x, y + 10);
    } else {
      text(this.letter, x, y);
    }
    pop();
  }
}
```

**floatingdot.js**
``` js

class FloatingDot {
  constructor(sun, radiusOffset = 5) {
    this.sun = sun;  // ahora el centro es el sol
    this.r = 6;
    this.radiusOffset = radiusOffset;

    this.initialSunRadius = this.sun.r;
    this.orbitRadius = this.sun.r + this.radiusOffset;

    this.orbitAngle = 0;
  }

  update() {
    let scaleFactor = this.sun.r / this.initialSunRadius;
    this.orbitRadius = (this.initialSunRadius + this.radiusOffset) * scaleFactor;

    this.orbitAngle += 0.03;

    this.x = this.sun.body.position.x + this.orbitRadius * cos(this.orbitAngle);
    this.y = this.sun.body.position.y + this.orbitRadius * sin(this.orbitAngle);
  }

  display() {
    push();
    noStroke();
    fill(152,17,17);
    ellipse(this.x, this.y, this.r * 2);
    pop();
  }
}

```

### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/NWhcuIGE3)

[GIF](https://www.canva.com/design/DAGmyFZItZA/y4NH_EtRmja-_MPS6bnKuQ/watch?utm_content=DAGmyFZItZA&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=hbbbf34b9e4)
