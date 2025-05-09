## Diseño del algoritmo generativo (proceso y outputs)

Sistemas de partículas que nacerá desde la parte inferior del árbol (más o menos el centro del lienzo para dar espacio a la visualización del árbol y de las raices), conformando visualmente las raices. El movimiento de estas estará dado por flowfields con ruido de perlin para mayor suavidad y naturalidad. El ritmo dictaminará la velocidad del movimiento.

El árbol contará con una textura construida también con flowfields, pero que se limirará a habitar dentro del espacio de la silueta del árbol. El brillo de sus colores estará controlado por la amplitud del sonido, mientras menos amplitud, menos brillante el color y viceversa.
