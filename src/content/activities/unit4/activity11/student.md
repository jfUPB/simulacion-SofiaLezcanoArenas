## Obra de arte generativa algorítmica interactiva en tiempo real
### Idea
Aplicación en p5.js que permita cargar una canción, la analice y mueva unos osciladores a partir de los datos entregados por la melodía y cuyos colores variarán interpolando entre dos tonos. La melodía se irá reproduciendo afectando a los osciladores en tiempo real.
### Código 
**oscillator-js**
``` js
class Oscillator {
  constructor(index) {
    this.index = index;
    this.angle = random(TWO_PI);
    this.angleVelocity = random(0.01, 0.05);
    this.amplitude = height / 4;
    
    // Colores base para interpolar
    this.colorA = color(random(100, 255), random(100, 255), random(255));
    this.colorB = color(random(255), random(100, 255), random(100, 255));
  }

  update(waveform) {
    let waveIndex = floor(map(this.index, 0, 10, 0, waveform.length)); 
    let level = waveform[waveIndex] * this.amplitude;
    
    // Modificar la posición del oscilador en base a la onda
    this.angle += this.angleVelocity;
    this.y = height / 2 + level; 
  }

  show() {
    let lerpedColor = lerpColor(this.colorA, this.colorB, (sin(this.angle) + 1) / 2);

    push();
    stroke(lerpedColor);
    strokeWeight(4);
    fill(lerpedColor);
    circle(this.index * (width / 10), this.y, 16); 
    pop();
  }
}
```

**sketch.js**
``` js
let sound, fft;
let statusText = "Cargar una canción";
let input;
let oscillators = [];

function setup() {
  createCanvas(640, 360);
  background(0);

  input = createFileInput(handleFile);
  input.position(20, 20); 

  fft = new p5.FFT();

  // Crear osciladores
  for (let i = 0; i < 10; i++) {
    oscillators.push(new Oscillator(i));
  }
}

function draw() {
  background(0);
  fill(255);
  textSize(16);
  text(statusText, 20, 60);

  if (sound && sound.isPlaying()) {
    let waveform = fft.waveform();

    for (let osc of oscillators) {
      osc.update(waveform);
      osc.show();
    }
  }
}

function handleFile(file) {
  if (file.type === 'audio') {
    if (sound) {
      sound.stop();  // Detiene la canción anterior
      sound.disconnect();  // Desconecta la canción anterior del análisis de sonido
    }

    sound = loadSound(file.data, () => {
      statusText = "¡Canción cargada!";
      sound.play();
    });
  } else {
    statusText = "Archivo no compatible";
  }
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/qiI49df39)

![image](https://github.com/user-attachments/assets/be264dfd-9f16-447b-8039-45a3efdda926)

![image](https://github.com/user-attachments/assets/e39680fc-43f6-4ae5-83db-62edf807fb37)
