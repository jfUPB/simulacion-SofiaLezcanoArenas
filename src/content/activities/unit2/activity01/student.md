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
## ¿Cómo funciona la suma dos vectores?

## ¿Por qué esta línea position = position + velocity; no funciona?
Se estaría tratando con tipos de datos y unidades diferentes.
