# Analiza una aplicación interactiva
## Vectores
En esta unidad se van a tratar los vectores geométricos, que son entidades con magnitud y dirección. Los vectores usualmente se representan con una flecha, su dirección es hacia donde apunta la misma, 
mientras que su magnitud es qué tan larga es.

Un vector es la distancia entre dos puntos o instrucciones para caminar de un punto a otro.

La posición nos da información relativa desde el origen (0,0) hasta donde se encuentra algo. Mientras que la velocidad indica cómo se mueve algo de un punto a otro.

## Explicación ejemplo 1.2: Bouncing Ball with Vectors!
En un entorno 2D se necesitan dos variables por cada propiedad. En uno 3D se necesitarían 3 por cada una. Las propiedades son: aceleración, posición, velocidad, fricción, posición objetivo y viento.

Es importante resaltar que en este caso el origen se encuentra en la esquina superior izquierda en vez del centro del plano.

P5.js ya cuenta con una clase para crear vectores así que en lugar de crearla, solo queda llamarla `createVector(x,y)`, sabiendo que solo puede usarse dentro de `setup()` y `draw()`.
#### Código
``` js
let position;
let velocity;

function setup() {
  createCanvas(640, 240);
  // Note how createVector() has to be called inside of setup().
  position = createVector(100, 100);
  velocity = createVector(2.5, 2);
}
function draw() {
  background(255);
  position.add(velocity); // se le suman las componentes del vector velocity a las componentes del vector position

  //{!6 .bold .code-wide} We still sometimes need to refer to the individual components of a p5.Vector and can do so using the dot syntax: position.x, velocity.y, etc.
  if (position.x > width || position.x < 0) { // si la posición es más grande o más pequeña que los límites del lienzo horizontalmente
    velocity.x = velocity.x * -1; // la velocidad invierte su dirección en x
  }
  if (position.y > height || position.y < 0) { // si la posición es más grande o más pequeña que los límites del lienzo verticalmente
    velocity.y = velocity.y * -1; // la velocidad invierte su dirección en y
  }

  stroke(0);
  fill(127);
  strokeWeight(2);
  circle(position.x, position.y, 48);
}
```

## ¿Cómo funciona la suma dos vectores?
Para sumar dos vectores normalmente lo que debe hacerse es sumar las componentes del mismo. La componente x1 con la componente x2 y la componente y1 con la componente y2 para obtener el nuevo vector.
#### En p5.js
Se utiliza la función `Add()` que recibe de parámetro otro vector. Esta función suma las componentes de "x" y "y" de manera separada.

## ¿Por qué esta línea position = position + velocity; no funciona?
No funciona porque en JavaScript, la operación de suma se usa con enteros, flotantes y otros valores primitivos, mas no con vectores.
En vez de esa línea, se usaría esta:
``` js
position.add(velocity);
```
