## Experimentando con la aceleración
### Aceleración constante
#### Ejemplo
Para facilitar la comprensión de este tipo de aceleración, tomé el ejemplo _Example 1.8: Motion 101 (Velocity and Constant Acceleration)_ 
y lo modifique para que sea un movimiento rectilíneo uniformemente acelarado en el eje x.
#### Resultados
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/CAQJQfNt2)

**Observaciones**

Con este tipo de aceleración, el objeto aumenta su velocidad lo que indique la aceleración, por lo que cada vez va más rápido. 
La aceleración no varía, la velocidad aumenta de manera constante y la posición aumenta de manera exponencial. 
### Aceleración aleatoria
#### Ejemplo
Para este ejercicio tomé de referencia el elemplo _Example 1.9: Motion 101 (Velocity and Random Acceleration)_ y 
le pedí a chatGPT que me ayudara a transformar el código en el movimiento de unas abejas. La propuesta fue de hacer que tuvieran 
aceleración aleatoria pero que tendieran un poco más al centro que a otros lugares para dar más naturalidad. El diseño de las abejas 
fue muy básico con óvalos amarillos, por lo que lo mejoré dibujando un par de alas y una raya negra en la mitad del cuerpo.
#### Resultados
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/RWCUjNwHs)

![image](https://github.com/user-attachments/assets/ec174db8-8518-4351-8c5e-c860edf028be)

**Observaciones**

Al ser aleatorio no quiere decir que no podamos generar una tendencia en el movimiento, podemos usar el concepto de distribución no uniforme y favorecer valores. Realmente el comportamiento de las abejas aparte de tirar hacia el centro, es completamente aleatorio.

El movimiento de las abejas, aparte de tender hacia el centro, a veces acelera y a veces desacelera por el factor de aleatoriedad del código. Se puede observar como cada una va a su propia velocidad y en algunas ocasiones se le ve más rápida, mientras que en otras se le ve más lenta.

### Aceleración hacia el mouse
#### Ejemplo
Para este ejercicio, como es usual revisé lo que tenía para ofrecer el libro, en este caso fue _Example 1.10: Accelerating Toward the Mouse_ para observar un poco cómo era el comportamiento esperado con este tipo de aceleración. Como no tenía ideas al respecto, le pedí a Chat algunas y aunque no me gustó ninguna, me inspiró a crear mi propio concepto: el mouse es el sol y los planetas se aceleran hacia él teniendo en cuenta su masa. Los más pesados son más lentos y los más ligeros son más rápidos.
#### Resultados
[Enlace a la simulación](https://editor.p5js.org/SofiaLezcanoArenas/sketches/hLExJ-4el)

![image](https://github.com/user-attachments/assets/ccd3b1c6-fda3-4b22-ad62-20fd25a4f39d)

**Observaciones**
- La aceleración hace que los planetas cambien su velocidad en cada frame, moviéndolos más rápido hacia el sol con el tiempo.
- La irección de la aceleración se calcula como la diferencia entre la posición del sol (mouse) y la posición del planeta.
- La magnitud de la aceleración está inversamente relacionada con la masa del planeta (planetas más pequeños se aceleran más).
