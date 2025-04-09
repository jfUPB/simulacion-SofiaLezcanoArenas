## Ondas
### Código de la simulación
``` js

// Velocidad a la que cambia el ángulo en cada iteración
let angleVelocity = 0.2;
// Amplitud de la onda (altura máxima de los picos)
let amplitude = 100;
// Offset inicial para animar la onda con el tiempo
let offset = 0;

function setup() {
  // Crear un lienzo de 640 píxeles de ancho y 240 píxeles de alto
  createCanvas(640, 240);
}

function draw() {
  // Limpiar la pantalla en cada frame para actualizar la animación
  background(255);

  // Configuración del borde de los círculos
  stroke(0); // Color del borde negro
  strokeWeight(2); // Grosor del borde

  // Definir los colores entre los que se hará la interpolación
  let color1 = color(0, 102, 255);  // Azul
  let color2 = color(255, 0, 127);  // Rosa

  // Usamos el offset como ángulo inicial para animar la onda
  let angle = offset;

  // Recorrer el lienzo en el eje X en pasos de 24 píxeles
  for (let x = 0; x <= width; x += 24) {
    // Calcular la posición Y usando la función seno para crear la onda
    let y = amplitude * sin(angle);

    // Normalizar x para obtener un valor entre 0 y 1
    // Esto nos ayudará a interpolar entre los dos colores
    let amt = map(x, 0, width, 0, 1);
    
    // Obtener un color intermedio entre color1 y color2 basado en amt
    let c = lerpColor(color1, color2, amt);
    
    // Usar el color interpolado para el relleno del círculo
    fill(c);

    // Dibujar el círculo en la posición calculada
    // Se suma height / 2 para que la onda quede centrada en el lienzo
    circle(x, y + height / 2, 48);

    // Incrementar el ángulo para calcular el siguiente punto de la onda
    angle += angleVelocity;
  }

  // Incrementar el offset en cada frame para que la onda se mueva
  offset += 0.05;
}
```
### Resultado
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/2cfMcUnWI)

![image](https://github.com/user-attachments/assets/7fe8eb81-c95d-40b8-8f89-12eeef51d97d)

![image](https://github.com/user-attachments/assets/cd1425ae-922c-49fd-9d95-3f27b3f438cd)
