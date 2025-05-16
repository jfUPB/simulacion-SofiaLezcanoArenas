## Funciones sinusoides
### Repaso de conceptos
En matemática se denomina sinusoide o senoide a la curva que representa gráficamente la función seno y también a dicha función en sí. Es una curva que describe una oscilación repetitiva (es una función periódica) y suave. Se expresa matemáticamente como:

![image](https://github.com/user-attachments/assets/692d2059-6e4f-4c3f-962e-839d8f445c46)

- Velocidad angular (𝜔): Es la rapidez con la que cambia el ángulo en una oscilación o movimiento circular. Se mide en radianes por segundo y se calcula como:

  𝜔 = 2𝜋𝑓

  donde 𝑓 es la frecuencia.

- Frecuencia (𝑓): Es el número de ciclos completos que la onda realiza en un segundo. Se mide en hercios (Hz):

  𝑓 = 1/𝑇​
 
  donde 𝑇 es el período.

- Período (𝑇): Es el tiempo que tarda la onda en completar un ciclo completo. Se mide en segundos y se calcula como:

𝑇 = 1/𝑓​

- Amplitud (𝐴): Es el valor máximo que alcanza la onda desde su posición de equilibrio. En una gráfica, es la altura máxima de las crestas o la profundidad de los valles.

- Fase (𝜙): Es el desplazamiento horizontal de la onda con respecto a su posición normal. Se mide en radianes o grados.

  - Si 𝜙 > 0, la onda se desplaza hacia la izquierda.
  - Si 𝜙 < 0, la onda se desplaza hacia la derecha.
 
### Código de la simulación
``` js
let particles = []; // Arreglo para almacenar las partículas
let amplitudeSlider, frequencySlider, phaseSlider; // Sliders para modificar parámetros
let amplitude, frequency, phaseOffset; // Variables que almacenan los valores de los sliders
let star; // Objeto para la estrella principal

function setup() {
  createCanvas(500, 350); // Crear un lienzo de 500x350 píxeles
  
  // Crear sliders para ajustar amplitud, frecuencia y desfase
  amplitudeSlider = createSlider(10, 150, 75, 1); // Rango de 10 a 150, valor inicial 75
  frequencySlider = createSlider(0.1, 5, 1, 0.1); // Rango de 0.1 a 5, valor inicial 1
  phaseSlider = createSlider(0, 10, 0, 1); // Rango de 0 a 10, valor inicial 0
  
  // Posicionar sliders en la pantalla
  amplitudeSlider.position(20, height + 10);
  frequencySlider.position(20, height + 40);
  phaseSlider.position(20, height + 70);
  
  // Crear la estrella
  star = new Star();
}

function draw() {
  background(255); // Fondo blanco
  
  // Obtener valores actuales de los sliders
  amplitude = amplitudeSlider.value();
  frequency = frequencySlider.value();
  let phaseValue = phaseSlider.value();
  
  // Ajustar la cantidad de partículas según el valor del desfase
  while (particles.length < phaseValue) {
    particles.push(new Particle(particles.length)); // Agregar nuevas partículas si el valor del slider ha aumentado
  }
  while (particles.length > phaseValue) {
    particles.pop(); // Eliminar partículas si el valor del slider ha disminuido
  }
  
  // Mostrar nombres y valores de sliders en el lienzo
  fill(0);
  textSize(20);
  text("Amplitud: " + amplitude, 20, 20);
  text("Frecuencia: " + frequency, 20, 40);
  text("Desfase: " + phaseValue, 20, 60);
  
  // Actualizar y mostrar la estrella
  star.update(amplitude, frequency);
  star.show();
  
  // Actualizar y mostrar las partículas
  for (let i = 0; i < particles.length; i++) {
    particles[i].update(star.x, star.y, i, amplitude, frequency);
    particles[i].show();
  }
}

// Clase para representar partículas que siguen la oscilación
class Particle {
  constructor(delay) {
    this.x = width / 2;
    this.y = height / 2;
    this.delay = delay; // Almacena el retraso de la partícula respecto a la estrella
    this.history = []; // Almacena la trayectoria de la partícula
  }
  
  update(x, y, delay, amplitude, frequency) {
    // Calcular la nueva posición Y de la partícula con un retraso en la oscilación
    let newY = height / 2 + amplitude * sin(frequency * (frameCount * 0.05 - delay));
    this.history.push(createVector(x, newY)); // Guardar la posición actual
    if (this.history.length > 20) {
      this.history.shift(); // Limitar la cantidad de posiciones almacenadas
    }
  }
  
  show() {
    noStroke();
    fill(50, 100, 255, 150); // Color azul con transparencia
    for (let v of this.history) {
      ellipse(v.x, v.y, 5, 5); // Dibujar las posiciones almacenadas en la trayectoria
    }
  }
}

// Clase para representar la estrella
class Star {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }
  
  update(amplitude, frequency) {
    // Calcular la nueva posición Y de la estrella usando una función seno
    this.y = height / 2 + amplitude * sin(frequency * frameCount * 0.05);
  }
  
  show() {
    fill(255, 204, 0); // Color amarillo
    stroke(0);
    strokeWeight(2);
    beginShape(); // Dibujar una estrella con cinco puntas
    let angleStep = PI / 5;
    for (let i = 0; i < TWO_PI; i += angleStep) {
      let radius = (i % (2 * angleStep) === 0) ? 15 : 7; // Alternar radios para las puntas y los vértices internos
      let x = this.x + cos(i) * radius;
      let y = this.y + sin(i) * radius;
      vertex(x, y);
    }
    endShape(CLOSE);
  }
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/4O25l1wKg)

![image](https://github.com/user-attachments/assets/d49cc6ad-f5f0-4321-bd8d-69be24da4f18)

