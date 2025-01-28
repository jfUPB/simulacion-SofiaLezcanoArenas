# Distribución Normal
## Explicación del ejemplo

## Código del ejemplo
``` js
let circleSize = 50;
let squareSize = 50;
let triangleSize = 50;

function setup() {
  createCanvas(400, 400);
  textSize(16);
  textAlign(CENTER, CENTER);
}

function draw() {
  background(220);

  // Generar un número aleatorio con distribución normal
  let randomNum = randomGaussian(2, 1); // Media = 2, Desviación estándar = 1
  let mappedNum = floor(constrain(randomNum, 0, 4)); // Asegurar que el valor esté entre 0 y 4

  // Aumentar el tamaño de la figura correspondiente
  if (mappedNum === 0) {
    circleSize++;
  } else if (mappedNum === 2) {
    squareSize++;
  } else if (mappedNum === 4) {
    triangleSize++;
  }

  // Dibujar el círculo
  fill(255, 0, 0); // Rojo
  ellipse(width / 4, height / 2, circleSize);

  // Dibujar el cuadrado
  fill(0, 255, 0); // Verde
  rectMode(CENTER);
  rect(width / 2, height / 2, squareSize, squareSize);

  // Dibujar el triángulo
  fill(0, 0, 255); // Azul
  let x1 = (3 * width) / 4;
  let y1 = height / 2 - triangleSize / 2;
  let x2 = (3 * width) / 4 - triangleSize / 2;
  let y2 = height / 2 + triangleSize / 2;
  let x3 = (3 * width) / 4 + triangleSize / 2;
  let y3 = height / 2 + triangleSize / 2;
  triangle(x1, y1, x2, y2, x3, y3);

  // Mostrar información en pantalla
  fill(0);
  text(`Random Value: ${randomNum.toFixed(2)}`, width / 2, 30);
  text(`Mapped Value: ${mappedNum}`, width / 2, 50);
}
```
## Captura de pantalla
## Una breve explicación de cómo se refleja la distribución normal en la visualización
En la captura se observa claramente cómo el cuadrado crece más rápido que las otras dos figuras, 
ya que el número que da la instrucción de que crezca es el 2, que a su vez es la media de esta distribución.
