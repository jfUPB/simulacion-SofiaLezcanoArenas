## La fuerza neta debe ser acumulativa
### ¿Por qué es necesario multiplicar la aceleración por cero en cada frame? ¿Por qué se multiplica por cero justo al final de update()?
Es necesario multiplicar la aceleración por cero para asegurarnos de que no se acumule para el siguiente ciclo. De no hacerse, las fuerzas aplicadas de un frame (como viento o gravedad) se acumularían sin control a lo largo de los ciclos, causando un crecimiento exponencial y un comportamiento errático del objeto.

Debe hacerse justo al final de `update()` para que mantenga su valor al calcular el movimiento del frame actual.
