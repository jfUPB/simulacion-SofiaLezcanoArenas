## Interpolamos?
### Ideas
- Manejar un contador decimal que oscile entre 0 y 1 y reemplazar el factor de interpolación por esta variable.
- Variar el valor del contador con condicionales
- Al ver que no era suficiente únicamente con el contador, implementé una variable bandera para saber si la flecha debía devolverse o no.
### Código que genera el resultado propuesto
``` js
let contador = 0;
let devolverse = false;

function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(200);
    let v0 = createVector(50, 50);  // Punto base
    let v1 = createVector(300, 0);  // Punto final de la flecha roja
    let v2 = createVector(0, 300);  // Punto final de la flecha azul

    let v4 = p5.Vector.sub(v2, v1); // Calcula el vector desde v1 hasta v2 restando sus coordenadas

    drawArrow(v0, v1, 'red');  // Flecha roja
    drawArrow(v0, v2, 'blue'); // Flecha azul

    // Flecha verde: Desde el final de la roja hasta el final de la azul
    drawArrow(p5.Vector.add(v0, v1), v4, 'green'); 
    // `p5.Vector.add(v0, v1)`: Ajusta la posición de inicio de la flecha verde sumando el vector base (v0) y v1.
    // `v4` representa la dirección desde v1 hasta v2, asegurando que la flecha se dibuje correctamente.

    // Calcular posición interpolada entre v1 y v2
    let v3 = p5.Vector.lerp(v1, v2, contador); // `lerp()` genera un punto intermedio entre v1 y v2

    // Interpolar color entre rojo y azul
    let colorInterpolado = lerpColor(color('red'), color('blue'), contador); // `lerpColor()` cambia el color progresivamente

    drawArrow(v0, v3, colorInterpolado); // Flecha morada con color dinámico

    // Actualizar contador para que la flecha oscile
    if (!devolverse) {  // Si `devolverse` es `false` se ejecuta este if, si es true se ejecuta el else
        contador += 0.01; // Aumenta gradualmente el valor del contador
        if (contador >= 1) { // Si llega al valor máximo (1), empieza a devolverse
            devolverse = true;
        }
    } else {  // Si `devolverse` es `true`, retrocede hacia la flecha roja
        contador -= 0.01; // Disminuye el valor del contador
        if (contador <= 0) { // Si llega al mínimo (0), vuelve a avanzar
            devolverse = false;
        }
    }
}

// Función para dibujar una flecha
function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y); // Mueve el punto base al origen local
    line(0, 0, vec.x, vec.y); // Dibuja la línea de la flecha
    rotate(vec.heading()); // Ajusta la rotación de la flecha según su dirección
    let arrowSize = 7; // Tamaño de la punta de la flecha
    translate(vec.mag() - arrowSize, 0); // Ajusta la posición de la punta de la flecha
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0); // Dibuja la punta de la flecha
    pop();
}
```
### Enlace a la simulación
[Enlace a la simulación exitosa](https://editor.p5js.org/SofiaLezcanoArenas/sketches/KA9UqrslB4)
### ¿Cómo funciona lerp() y lerpColor().
**Lerp()**: en p5.js sirve para hacer interpolación lineal entre dos valores. 
Básicamente, toma dos números y devuelve un valor intermedio entre ellos según un factor de interpolación. 
Estos son los parámetros que recibe`lerp(valor1, valor2, amt)` siendo estos valores lo siguiente:
- valor1: el valor inicial.
- valor2: el valor final.
- amt: un número entre 0 y 1 que determina la posición entre valor1 y valor2.

**LerpColor()**: es una función de p5.js que interpola entre dos colores basándose en un valor de interpolación entre 0 y 1.
  Recibe estos parámetros `lerpColor(color1, color2, amt);` que serían:
- color1: Primer color.
- color2: Segundo color.
- amt: Valor entre 0 y 1 que determina qué tan cerca está el color interpolado de color1 o color2.
### ¿Cómo se dibuja una flecha usando drawArrow()?
`drawArrow(base, vec, myColor);`
- base: Punto inicial de la flecha (p5.Vector).
- vec: Vector que indica la dirección y la longitud de la flecha.
- myColor: Color de la flecha.
  
**Explicación función**
  ``` js
  function drawArrow(base, vec, myColor) {
    push(); // Guarda el estado de dibujo actual
    stroke(myColor); // Establece el color del trazo
    strokeWeight(3); // Grosor de la línea
    fill(myColor); // Rellena el triángulo de la punta de la flecha
    translate(base.x, base.y); // Mueve el sistema de coordenadas al punto de inicio
    line(0, 0, vec.x, vec.y); // Dibuja la línea de la flecha
    rotate(vec.heading()); // Rota para que la punta apunte en la dirección del vector
    let arrowSize = 7; // Tamaño de la punta de la flecha
    translate(vec.mag() - arrowSize, 0); // Desplaza hasta la punta de la flecha
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0); // Dibuja el triángulo
    pop(); // Restaura el estado de dibujo anterior
}
  ```
