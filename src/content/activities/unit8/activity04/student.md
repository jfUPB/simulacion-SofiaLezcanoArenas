##  Implementación

### Audio
- Creé una carpeta llamada assets para tener todo más ordenado y allí subí el mp3 de la canción. 
- En sketch.js creé una variable global `audio` para poder referirme a la canción fácilmente y una para medir la amplitud.
- Usando la función `preload()` cargo la canción antes de cualquier cosa.
  ``` js
  function preload() {
  soundFormats('mp3', 'ogg'); // Especifica los formatos de audio compatibles
  audio = loadSound('assets/AURORA - The Seed.mp3'); // Carga el archivo de audio antes de iniciar el sketch
  }
  ```
- Ya en `setup()` la canción se reproduce en bucle usando `audio.loop();` y tengo un objeto para poder medir el volumen de la canción `amplitude = new p5.Amplitude();`
- En `draw()` Con ` let level = amplitude.getLevel();` se obtiene el nivel actual del volumen del audio
  
### Audio modulando lo visual
#### Volumen afectando la velocidad de las raíces
``` js
draw(){
let speed = map(level, 0, 0.3, 0.5, 3); // convierte el volumen (entre 0 y 0.3) en una velocidad (entre 0.5 y 3).

.
.
.

r.update(speed); // Pasa a las raíces la velocidad determinada por el audio
}
```

#### Volumen afectando el brillo de las raíces
``` js
setup(){
colorMode(HSB, 360, 100, 100, 255); // Usa modo de color HSB para colores más vivos (matiz, saturación, brillo)
}

draw(){
let brightness = map(level, 0, 0.3, 20, 100); // Convierte el volumen en un valor de brillo (de 20 a 100 en HSB)

.
.
.

r.display(brightness); // Pasa a las raíces el dato de brillo para que puedan usarlo
}
```

Además de esto es importante resaltar que el movimiento es controlado por un flowfield el cual puede ser cambiado haciendo clic, esto como manera de interacción con el usuario pensando en que podía estresarse al ver muy vacía una zona y que podría querer cambiar eso o interactuar al son de la música y verlo reflejado.

### Código
**sketch.js**
``` js
let roots = [];
let flowfield;
let audio, amplitude;
let spacing = 20;
let cols, rows;

function preload() {
  soundFormats('mp3', 'ogg');
  audio = loadSound('assets/AURORA - The Seed.mp3');
}

function setup() {
  createCanvas(640, 580);
  colorMode(HSB, 360, 100, 100, 255);
  audio.loop();
  amplitude = new p5.Amplitude();

  cols = floor(width / spacing);
  rows = floor(height / spacing);
  flowfield = new FlowField(spacing);

  background(0);
}

function draw() {
  let level = amplitude.getLevel();
  let brightness = map(level, 0, 0.3, 20, 100);
  let speed = map(level, 0, 0.3, 0.5, 3); // velocidad sensible al audio

  flowfield.update();

  for (let i = 0; i < 4; i++) {
    roots.push(new Root(width / 2, height / 2, flowfield)); // inicia desde el centro
  }

  for (let i = roots.length - 1; i >= 0; i--) {
    let r = roots[i];
    r.update(speed);
    r.display(brightness);
    if (r.isDead()) {
      roots.splice(i, 1);
    }
  }

}

// Make a new flowfield
function mousePressed() {
  flowfield.init();
}
```

**root.js**
``` js
class Root {
  constructor(x, y, flowfield) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(0.5); // Más variedad inicial
    this.acc = createVector(0, 0);
    this.r = 10;
    this.flowfield = flowfield;

    // Interpolación entre verde lima y verde bosque
    let c1 = color(103, 68, 80); // Equivale a #9bc90b en HSB
    let c2 = color(235, 54, 80); // Equivale a #30a55e en HSB
    this.baseColor = lerpColor(c1, c2, random());
  }

  update(speed) {
    let force = this.flowfield.lookup(this.pos);
    this.acc.add(force);
    this.vel.add(this.acc);
    this.vel.limit(speed);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.r *= 0.995;
  }

  display(brightness) {
    let c = color(
      hue(this.baseColor),
      saturation(this.baseColor),
      brightness,
      200
    );
    fill(c);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.r * 2);
  }

  isDead() {
    return (
      this.r < 0.5 ||
      this.pos.x < -10 || this.pos.x > width + 10 ||
      this.pos.y < -10 || this.pos.y > height + 10
    );
  }
}
```

**flowfield.js**
``` js
class FlowField {
  constructor(spacing) {
    this.spacing = spacing;
    this.cols = floor(width / spacing);
    this.rows = floor(height / spacing);
    this.field = this.make2DArray(this.cols, this.rows);
    this.zoff = 0;
    this.init();
  }

  init() {
    // Reseed noise for a new flow field each time
    noiseSeed(random(10000));
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        //{.code-wide} In this example, use Perlin noise to create the vectors.
        let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
        this.field[i][j] = p5.Vector.fromAngle(angle);
        yoff += 0.1;
      }
      xoff += 0.1;
    }
  }
  
  make2DArray(cols, rows) {
    let arr = new Array(cols);
    for (let i = 0; i < cols; i++) {
      arr[i] = new Array(rows);
    }
    return arr;
  }

 
  update() {
    this.zoff += 0.001;
    for (let x = 0; x < this.cols; x++) {
      for (let y = 0; y < this.rows; y++) {
        let angle = noise(x * 0.05, y * 0.05, this.zoff) * TWO_PI * 2;
        this.field[x][y] = p5.Vector.fromAngle(angle).mult(-0.5); // sin sesgo hacia abajo
      }
    }
  }

  lookup(lookup) {
    let column = floor(constrain(lookup.x / this.spacing, 0, this.cols - 1));
    let row = floor(constrain(lookup.y / this.spacing, 0, this.rows - 1));
    return this.field[column][row].copy();
  }
}

```

### Resultado
[Enlace a simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/_NxGhGmMN)

[Enlace a video](https://www.canva.com/design/DAGnnifBWWU/rF-YM1HcfgaNARchvp-oMg/watch?utm_content=DAGnnifBWWU&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=h7335f1a003)
