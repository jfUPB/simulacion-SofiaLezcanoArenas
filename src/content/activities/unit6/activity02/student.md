## Analizando los campos de flujo (flow fields)
### ¿Cómo es la estructura de datos usada para el campo de flujo y cómo se generan sus vectores?
Se usa una estructura de datos de array 2D. Cada elemento del array es un vector que tiene una dirección fija determinada por un ángulo que varía suavemente mediante un valor de ruido para que cada vector tenga cierta dirección.

Inicialmente el vector en cada punto se calcula así:
- Se usa Perlin Noise (noise(xoff, yoff)) para generar una variación suave en los ángulos de los vectores.
- El valor de noise(xoff, yoff) se mapea a un ángulo entre 0 y TWO_PI (usando map()).
- Luego, p5.Vector.fromAngle(angle) crea un vector unitario con esa dirección.
- xoff y yoff se incrementan en cada iteración para generar diferentes valores de ruido en cada punto, asegurando una transición suave entre los vectores adyacentes.
### ¿Cómo es que un agente utiliza el campo para calcular su fuerza de dirección?
El agente determina que vector del campo debe seguir mirando su propia posición en el espacio para encontrar en qué celda de la cuadrícula del campo de flujo se encuentra. Luego, obtiene el vector correspondiente en esa celda y lo usa como dirección deseada.

Para usarlo como su dirección, crea una copia del vector y la ajusta para que tenga la misma velocidad máxima que puede alcanzar, calcula la diferencia entre su velocidad actual y la dirección deseada (esto le dice cuánto debe girar o acelerar), limita ese cambio para que no sea demasiado brusco, aplica la fuerza de dirección que hace que poco a poco su velocidad y trayectoria se alineen con el flujo del campo.
### Parámetros clave identificados
- `r` maneja la resolución del campo de flujo, es decir, el tamaño de cada celda del mismo. En el código del agente esta variable es el radio del agente.
- `cols` cantidad de columnas en la cuadrícula, calculadas dividiendo el ancho del lienzo por la resolución.
- `rows` cantidad de filas en la cuadrícula, calculadas dividiendo el alto del lienzo por la resolución.
- `angle` dirección del vector en cada celda, calculada a partir del ruido de Perlin.
- `position` posición actual del agente en el lienzo (x, y).
- `maxspeed` velocidad máxima que puede alcanzar el agente.
- `maxforce` máxima fuerza de dirección que puede aplicar para cambiar su trayectoria.
- `steer` fuerza de dirección calculada como la diferencia entre el vector deseado y la velocidad actual.
### Modificación
