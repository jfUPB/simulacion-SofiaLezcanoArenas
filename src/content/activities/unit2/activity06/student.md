## Hagamos que todo se mueva
### Código
``` js
let contador = 0;  // Variable para controlar la interpolación entre las flechas
let devolverse = false;  // Variable que controla si la interpolación va hacia adelante o hacia atrás

function setup() {
    createCanvas(400, 400);  // Crea un lienzo de 400x400 píxeles
}

function draw() {
    background(200);  // Dibuja un fondo gris claro

    // Usar el mouse para cambiar la base de las flechas
    let v0 = createVector(mouseX, mouseY);  // La base de las flechas sigue las coordenadas del mouse
    
    // Modificar la escala de los vectores v1 y v2 usando la posición vertical del mouse (mouseY)
    let v1 = createVector(300 * (mouseY / height), 0);  // La longitud de v1 cambia con el mouseY, manteniendo la dirección horizontal
    let v2 = createVector(0, 300 * (mouseY / height));  // La longitud de v2 cambia con el mouseY, manteniendo la dirección vertical

    // Calcular el vector que va de v1 a v2
    let v4 = p5.Vector.sub(v2, v1);  // v4 es el vector que va desde el final de v1 hasta el final de v2
    
    // Dibujar las flechas principales
    drawArrow(v0, v1, 'red');  // Dibuja una flecha roja desde la base v0 hasta v1
    drawArrow(v0, v2, 'blue'); // Dibuja una flecha azul desde la base v0 hasta v2

    // Dibujar la flecha verde: Desde el final de la flecha roja hasta el final de la flecha azul
    drawArrow(p5.Vector.add(v0, v1), v4, 'green'); // La flecha verde se dibuja en la posición final de la flecha roja y se dirige hacia v4

    // Calcular la posición interpolada entre las flechas roja y azul usando el contador
    let v3 = p5.Vector.lerp(v1, v2, contador);  // Interpolación entre v1 y v2 en función del contador

    // Interpolar el color entre rojo y azul según el valor del contador
    let colorInterpolado = lerpColor(color('red'), color('blue'), contador);  // El color de la flecha morada cambia entre rojo y azul

    // Dibujar la flecha morada con el color interpolado
    drawArrow(v0, v3, colorInterpolado); // Dibuja la flecha morada desde la base v0 hasta la posición interpolada v3

    // Actualizar el contador para que la flecha oscile entre rojo y azul
    if (!devolverse) {  // Si no se ha llegado al extremo azul
        contador += 0.01; // Aumenta el contador para que la flecha avance hacia el extremo azul
        if (contador >= 1) {  // Si el contador llega a 1, significa que ha llegado al extremo azul
            devolverse = true; // Cambia la dirección para que la flecha se devuelva
        }
    } else {  // Si la flecha se está devolviendo
        contador -= 0.01; // Disminuye el contador para que la flecha retroceda hacia el extremo rojo
        if (contador <= 0) {  // Si el contador llega a 0, significa que ha llegado al extremo rojo
            devolverse = false; // Cambia la dirección para que la flecha avance hacia el azul
        }
    }
}

// Función para dibujar una flecha
function drawArrow(base, vec, myColor) {
    push();  // Guarda el estado actual de la transformación
    stroke(myColor);  // Establece el color del contorno de la flecha
    strokeWeight(3);  // Establece el grosor de la línea
    fill(myColor);  // Establece el color de relleno de la flecha
    translate(base.x, base.y);  // Traslada el origen de la coordenada a la base de la flecha

    line(0, 0, vec.x, vec.y);  // Dibuja la línea de la flecha desde la base (0, 0) hasta el punto final del vector vec
    rotate(vec.heading());  // Rota la flecha según el ángulo de dirección de vec (esto hace que apunte en la dirección correcta)
    
    let arrowSize = 7;  // Define el tamaño de la punta de la flecha
    translate(vec.mag() - arrowSize, 0);  // Mueve el punto de dibujo de la flecha hasta el final de la línea (menos el tamaño de la punta)

    // Dibuja la punta de la flecha como un triángulo
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0); // Dibuja la punta de la flecha en la dirección de la línea

    pop();  // Restaura el estado de la transformación para no afectar a otros elementos
}
```
### Enlace a la simulación
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/BVoeFyfun)

### Explicación de la solución
- **Se hace un cambio de base de las flechas:**
`let v0 = createVector(mouseX, mouseY);` hace que la base de las flechas siga la posición del mouse en el lienzo.
- **Escala de los vectores:**
`let v1 = createVector(300 * (mouseY / height), 0);` y `let v2 = createVector(0, 300 * (mouseY / height));` escalamos los vectores v1 y v2 basados en la posición vertical del mouse (mouseY). Cuanto más abajo esté el mouse, más grandes serán las flechas.
