## **Coordenadas polares**
### Concepto de coordenadas polares y coordenadas cartesianas
#### Coordenadas Polares (r, θ)
Este sistema usa una distancia y un ángulo en lugar de coordenadas rectangulares.
- **r (radio):** Distancia desde el origen hasta el punto.
- **θ (theta, ángulo):** Ángulo medido desde el eje positivo de las X en sentido contrario a las agujas del reloj.

Cada punto se representa como (r, θ):
- r indica qué tan lejos está el punto del origen.
- θ indica la dirección del punto en grados o radianes.

**Ejemplo:**
Un punto en (5, 45°) significa: 5 unidades de distancia desde el origen, un ángulo de 45° desde el eje X positivo.
#### Coordenadas Cartesianas (x, y)
Son el sistema de coordenadas más común y se basan en dos ejes perpendiculares:
- Eje X: Representa la horizontal (izquierda a derecha).
- Eje Y: Representa la vertical (abajo a arriba).

Cada punto en el plano cartesiano se representa como (x, y), donde:
- x indica qué tan lejos está el punto a la derecha (+) o izquierda (-) del origen (0,0).
- y indica qué tan lejos está el punto hacia arriba (+) o hacia abajo (-) del origen.
**Ejemplo:** Un punto en (3, 2) significa: 3 unidades a la derecha del origen. 2 unidades hacia arriba.

### ¿Cuál es la relación entre r y theta con las posiciones x y y?
El código usa las siguientes fórmulas para convertir de coordenadas polares a cartesianas:

![image](https://github.com/user-attachments/assets/eaefce38-4104-4ba8-b0b2-186959f717c4)

**¿Qué significa esto?**
- r (radio): Es la distancia desde el origen (en este caso, el centro de la pantalla) hasta el punto que estamos dibujando.
- theta (ángulo): Es el ángulo en radianes que define la dirección del punto respecto al eje X positivo.

**¿Cómo afecta esto a la animación?**
- Cada vez que se llama a draw(), el código incrementa theta en 0.02.
- Esto hace que el punto gire en torno al origen siguiendo un círculo de radio r.
- x y y cambian dinámicamente según el valor de theta, lo que genera el movimiento circular.

**Visualización**
- Si r es constante, el punto se moverá en un círculo de radio fijo.
- Si r cambia con el tiempo, el punto podría hacer una espiral en lugar de un círculo.
### ¿Qué ocurre al hacer la primera modificación propuesta a la función `draw()`? ¿Por qué?
Sale un error en la siguiente línea:
``` js
line(0, 0, x, y);
```
Porque x y y ya no están definidos en el código. Arreglando eso, el movimiento del círculo casi no se aprecia porque en la línea
``` js
circle(v.x, v.y, 48);
```
v.x y v.y están entre -1 y 1, lo que indica que el movimiento es mínimo. Pues no se está contemplando el valor del radio para ajustar el tamaño de la trayectoria, por lo que el vector tiene una magnitud de 1 que viene por defecto en la función.
### ¿Qué ocurre al hacer la segunda modificación propuesta a la función `draw()`? ¿Por qué?
El movimiento se observa de manera correcta, tal como se veía en la primera versión. Esto es porque se ha corregido el problema de visualización de x y y que deberían ser v.x y v.y, y se está contemplando la medida del radio para la trayectoria.
