# Diseño: exploración de la idea
## Intención de diseño.
## ¿Cómo piensas usar los tres conceptos y por qué estos?
## Código
``` js
let columnas, filas;
let escala;
let ruidoZ = 0;
let img;
let colorPromedio;
let inputButton;

function setup() {
  createCanvas(600, 400);
  inputButton = createFileInput(handleFile);
  inputButton.position(10, 10);
  escala = floor(random(5, 30)); // Generar tamaño aleatorio de cuadros
  columnas = width / escala;
  filas = height / escala;
}

function draw() {
  background(10);
  if (img) {
    img.loadPixels();
    colorPromedio = calcularColorPromedio();
  }
  ruidoZ += 0.01;
  for (let y = 0; y < filas; y++) {
    for (let x = 0; x < columnas; x++) {
      let ruidoValor = noise(x * 0.1, y * 0.1, ruidoZ);
      let brillo = map(ruidoValor, 0, 1, 0, 255);
      fill(lerpColor(colorPromedio || color(255), color(0), brillo / 255));
      noStroke();
      rect(x * escala, y * escala, escala, escala);
    }
  }
}

function keyPressed() {
  if (key.toLowerCase() === 's') {
    escala = floor(random(5, 30)); // Generar nuevo tamaño aleatorio
    columnas = width / escala;
    filas = height / escala;
  }
}

function handleFile(file) {
  if (file.type === 'image') {
    img = loadImage(file.data);
  }
}

function calcularColorPromedio() {
  let r = 0, g = 0, b = 0, total = 0;
  for (let y = 0; y < img.height; y += 10) {
    for (let x = 0; x < img.width; x += 10) {
      let index = (x + y * img.width) * 4;
      r += img.pixels[index];
      g += img.pixels[index + 1];
      b += img.pixels[index + 2];
      total++;
    }
  }
  return color(r / total, g / total, b / total);
}
```
## Resultado
[Enlace a la simulación](https://editor.p5js.org/)
## Referentes
[sandspiel](https://sandspiel.club/)
