## Experimenta
###  Explicación del código base
``` js
let position; // Declara una variable 'position', pero aún no le asigna un valor inicial.

function setup() {
    createCanvas(400, 400); // Crea un lienzo de 400x400 píxeles en la pantalla para dibujar.
    posicion = createVector(6, 9); // Crea un vector con las coordenadas (6, 9) y lo asigna a la variable 'posicion'.
    playingVector(posicion); // Llama a la función 'playingVector' pasándole el vector 'posicion' como argumento.
    noLoop(); // Detiene el ciclo de dibujo. Esto significa que 'draw' solo se ejecutará una vez.
}

function playingVector(v) { // v es un nombre local dentro de la función. Representa el vector que se le ha pasado a la función
    v.x = 20; // Modifica la componente x del vector 'v' a 20.
    v.y = 30; // Modifica la componente y del vector 'v' a 30.
}

function draw() {
    background(220); // Establece el color de fondo a un gris claro (220 es un valor de escala de gris).
    console.log("Only once"); // Imprime "Only once" en la consola una sola vez, ya que 'noLoop()' detiene el ciclo de dibujo.
}
```
### ¿Qué resultado esperas obtener?
Imprimir el valor que tenía el vector inicialmente además del que luego se le asigna.
### ¿Qué resultado obtuviste?
### Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.
### ¿Qué tipo de paso se está realizando en el código?
### ¿Qué aprendiste?
