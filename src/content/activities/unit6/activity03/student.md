## Analizando el comportamiento de enjambre (Flocking)

### Reglas del Flocking
#### Separación:  evitar el hacinamiento con vecinos cercanos
**Objetivo:** evitar concentrar demasiada información en un solo punto y choques entre los miembros del enajmbre.
**Lógica:** compara cada la posición de cada boid con cada uno de los restantes, si la distancia entre ambos es mayor que 0 y menor que la separación deseada (establecida con anterioridad), calcula un vector apuntando lejos del vecino con el que se está comparando actualmente.
#### Alineación: dirigirse en la misma dirección promedio que los vecinos cercanos
**Objetivo:** mantener una dirección bastante homogénea evitando el desorden.
**Lógica:** se define una distancia a la que debe estar otro boid del actual para considerarlo un vecino. Se compara con cada uno de los otros boids existentes y si se cumple la distancia, se aumenta una unidad un contador, además de que la velocidad de los vecinos se va acumulando en un vector. Cuando se termina la comparación se saca un promedio de las velocidades para saber en general a donde están yendo los vecinos, se normaliza el vector y se limita para tomar la dirección sin alterar la velocidad actual del boid y se le devuelve para usarlo.
#### Cohesión: moverse hacia la posición promedio de los vecinos cercanos
**Objetivo:** mantener un objetivo en común de desplazamiento como si realmente fueran parte de un grupo.
**Lógica:** 
### Parámetros clave identificados

### Modificación
