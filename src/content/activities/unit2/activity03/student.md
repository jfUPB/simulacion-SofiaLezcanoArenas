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
- Ejecutar un número de veces definida draw() y visualizar un mensaje por cada vez que se ejecute esa función.
- Imprimir el valor que tenía el vector inicialmente además del que luego se le asigna.
### ¿Qué resultado obtuviste?
Para el primero obtuve el resultado esperado.
![image](https://github.com/user-attachments/assets/812d3a07-bf66-4338-81c7-771595e4a483)

Para el segundo, hubo algo que no me esperaba. La función toString() interpretó los vectores con tres componentes por defecto, aunque mantuvo la bidimensionalidad colocando el valor de la tercera componente en cero.
![image](https://github.com/user-attachments/assets/f6bde3e4-bb7d-47b9-ab10-6a0c65399acc)

### Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.
#### Paso por valor
Cuando se pasa una variable como argumento a una función, se pasa una copia del valor de esa variable. Es decir, los cambios realizados dentro de la función no afectan a la variable original fuera de la función.
En JavaScript, los tipos primitivos (como number, string, boolean, null, undefined, symbol, bigint) se pasan por valor.

**Ejemplo:**
``` js
let x = 10;

function cambiarValor(a) {
    a = 20;  // Modifica la copia local
}

cambiarValor(x);  // Pasamos x por valor

console.log(x);  // Imprime 10, porque x no ha cambiado
```
#### Paso por referencia
Cuando se pasa una variable a una función, se pasa una referencia a la ubicación de la memoria donde se almacena el valor de esa variable. Esto significa que si se modifica el valor dentro de la función, se modifica el valor original fuera de la función. En JavaScript, los objetos (incluyendo arrays y funciones) se pasan por referencia.

**Ejemplo:**
``` js
let persona = { nombre: "Juan", edad: 25 };

function cambiarNombre(obj) {
    obj.nombre = "Carlos";  // Modifica el objeto original
}

cambiarNombre(persona);  // Pasamos el objeto persona por referencia

console.log(persona.nombre);  // Imprime "Carlos", porque el objeto original fue modificado
```
### ¿Qué tipo de paso se está realizando en el código?
Se está realizando un paso por referencia porque el valor del vector está siendo modificado
### ¿Qué aprendiste?

- El número dos en fondo azul es es parte de la herramienta de consola del navegador y generalmente se refiere al identificador de la invocación de un mensaje en la consola.
  
  ![image](https://github.com/user-attachments/assets/1910c55b-227e-4f56-82a8-e459052c2e52)
- El concepto de paso por valor y paso por referencia.
- Cómo imprimir vectores por consola
