## Marco Motion 101
### Ubicación en el código mostrado
``` js
this.velocity.add(this.acceleration);
this.position.add(this.velocity);
```

El marco Motion 101 se encuentra en estas dos líneas. La velocidad varía por la aceleración y a su vez, la posición varía por la aceleración, dando como resultado movimiento.
### ¿Cómo se maneja la aceleración en la aplicación final de la unidad 2?
Se utilizan tres conceptos. Todos dentro del código de planeta.js:
- Simulación de fuerza gravitacional inversamente proporcional a la masa
  ``` js
  let strength = 0.5 / this.mass; //es la magnitud de la aceleración. Se calcula inversamente proporcional a la masa del planeta
  dir.setMag(strength); // Establece la magnitud del vector dirección, convirtiéndolo en el vector de aceleración.
  this.acceleration = dir; //Guarda el vector como la aceleración actual del planeta.
  ```
- Aceleración hacia el mouse
  ``` js
  let sun = createVector(mouseX, mouseY); //posición del mouse
  let dir = p5.Vector.sub(sun, this.position); //vector dirección que apunta del planeta hacia el Sol.
                                               //Es el vector fuerza gravitacional en una simulación básica
  ```
- Motion 101
  ``` js
  this.velocity.add(this.acceleration);
  this.velocity.limit(5); // no hace parte directamente del Motion 101 pero es importante colocarlo para una mejor visualización
  this.position.add(this.velocity);
  ```
### ¿Qué tiene que ver calcular la aceleración con las leyes de movimiento de Newton?
Estaría directamente relacionada con la segunda ley, pues nos proporciona una manera de calcularla teniendo la masa del objeto y la fuerza que se le está aplicando. Además de que en su definición de fuerza la ata directamente a la aceleración diciendo que "A force is a vector that causes an object with mass to accelerate."
