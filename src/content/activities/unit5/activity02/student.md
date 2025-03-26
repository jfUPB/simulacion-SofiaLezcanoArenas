# Revisa y repasa algunos conceptos
## Sistemas de partículas
Un sistema de partículas es una colección de muchísimas partículas diminutas que, en conjunto, representan un objeto difuso. Con el tiempo, las partículas se generan en un sistema, se mueven y cambian dentro del sistema, y ​​mueren. En el tiempo se han usado para modelar varios tipos irregulares de fenómenos naturales, como fuego, humo, cascadas, niebla, hierba, burbujas, etc.

Hay un elemento muy importante llamado emisor, que es quien controla la configuración inicial de las partículas (posición, velocidad, etc.). También hay que saber que las partículas no pueden vivir por siempre, deben ser eliminadas después de cierto tiempo o cierta acción, como pasar el límite del lienzo, chocar con algo, etc.

## Herencia y polimorfismo
- **Herencia:** Es un principio de la programación orientada a objetos donde una clase (hija) puede heredar atributos y métodos de otra clase (padre), reutilizando su código. Un gato y un perro son animales, por lo que comparten características como respirar y moverse, pero cada uno tiene sus propias diferencias.
- **Polimorfismo:** Es la capacidad de un objeto de tomar diferentes formas, permitiendo que un mismo método funcione de manera diferente según la clase que lo implemente. Un gato y un perro pueden hacer un sonido, pero el gato maúlla y el perro ladra, aunque ambos están ejecutando la acción de "hacer sonido".
## Simulación 4.2: an Array of Particles
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
- **Creación de partículas**: En cada cuadro de la animación (draw()), se crea una nueva partícula con new Particle(width / 2, 20). Esta partícula se agrega al arreglo particles[], que almacena todas las partículas activas.
- **Desaparición de partículas y gestión de memoria**: Cada partícula tiene una propiedad lifespan, que disminuye en cada actualización (update()), simulando su envejecimiento. Cuando lifespan llega a cero o menos, el método isDead() devuelve true. Se recorre el arreglo particles[] de atrás hacia adelante en draw(), y si isDead() es true, la partícula se elimina con particles.splice(i, 1). Esto libera la memoria ocupada por la partícula eliminada, evitando acumulaciones innecesarias.
### Modificación: Ruido de Perlin
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
#### *Código*
``` js
```
#### _Resultado_
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/w_GmM20Gc)

![image](https://github.com/user-attachments/assets/fab6a66a-48a5-452a-b75e-c18bb8b4b7f0)

## Simulación 4.4: a System of Systems
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
### Modificación: concepto
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
#### *Código*
``` js
```
#### _Resultado_
[Enlace a la simulación]()

## Simulación 4.5: a Particle System with Inheritance and Polymorphism
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
### Modificación: concepto
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
#### *Código*
``` js
```
#### _Resultado_
[Enlace a la simulación]()

## Simulación 4.6: a Particle System with Forces
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
### Modificación: concepto
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
#### *Código*
``` js
```
#### _Resultado_
[Enlace a la simulación]()

## Simulación 4.7: a Particle System with a Repeller
### ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria?
### Modificación: concepto
#### _¿Por qué este concepto? ¿Cómo se aplicó el concepto?_
#### _¿Cómo se está gestionando ahora la creación y la desaparción de las partículas y cómo se gestiona la memoria?_
#### *Código*
#### _Resultado_
[Enlace a la simulación]()
