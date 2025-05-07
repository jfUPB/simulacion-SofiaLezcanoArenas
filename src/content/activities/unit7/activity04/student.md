## Consolidando la integración p5.js + Matter.js

### Flujo de trabajo: describe brevemente el flujo de trabajo que seguiste para integrar Matter.js en p5.js. ¿Qué se configura en setup()? ¿Qué sucede en draw() respecto a Matter.js (actualizar el motor) y al dibujo de los cuerpos?
1. Pensar en la palabra junto con el concepto base
2. Plantear en que elemento podía usar matter.js para alterar físicamente. Lo que resultó en hacer crecer la "o" el "sol" con body.scale
3. Una vez funcional, añadir elementos visuales que refuercen el concepto, como lo es el brillo que tiene el sol luego de crecer y el cambiar el color del punto de la i por un cafe rojizo para representar a mercurio, el planeta con la óribita más cercana al sol.

En setup() se configura el motor de matter.js y se hace la gravedad cero, además de crear la instancia de la palabra helio. Mientras que en draw() se actualiza el motor y se carga la visualización de la palabra helio incluyendo el sol y el punto.

### Representación visual vs. simulación física: ¿Cómo manejaste la relación entre los cuerpos físicos de Matter.js (que tienen posición, ángulo, vértices) y su representación visual en el canvas de p5.js? ¿Hubo algún desafío en “dibujar” los cuerpos correctamente?
El desafío más grande fue lograr la combinación de dos herramientas para dos funcionalidades diferentes dentro de un mismo programa, pues ambas podían hacer ambas cosas pero había que tratar de limitar a matter para físicas y a p5.js para dibujar.

### Creación de formas complejas: ¿Qué técnica utilizaste para crear las formas de las letras con Matter.js? ¿Fue fácil o difícil? ¿Qué limitaciones encontraste?
Como la mayoría de las letras no se iban a mover ni verse afectadas, simplemente se representan como círculos para matter, mientras que en p5.js las incorporo como caracteres. El sol es un círculo también pero que tiene stroke sin fill. El punto de la i es un círculo con fill sin stroke.

### Física para la semántica: ¿Qué tan efectivo crees que fue usar una simulación física para representar el significado de una palabra? ¿Qué tipo de significados crees que se prestan mejor a este enfoque? ¿Cuáles serían más difíciles?
En mi caso por el enfoque tomado, no fue tan efectivo pues era un movimiento bastante sencillo que se vio complejizado por la inclusión del motor de físicas. Solo era crecer sin verse afectado por fuerzas externas. Creo que sería más útil para palabras que involucren fluidos, relación directa con fuerzas. Las más dificiles creo que serían las que involucren cambios de forma, o formas muy orgánicas.

### Potencial exploratorio: más allá de este reto, ¿Qué otras posibilidades creativas te sugiere la combinación de p5.js y Matter.js?
Me parece que podría ser útil en simulaciones de fenómenos meteorológicos como lluvia, arte abstracto, entre otras.
