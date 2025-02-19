## Paso por valor y paso por referencia
### ¿Cuál es la diferencia entre las dos líneas?
``` js
let friction = this.velocity.copy(); // Se está pasando una copia del valor de this.velocity a friction. Paso por valor.
let friction = this.velocity; // friction se convierte en una referencia del valor this.velocity. Paso por referencia.
```
### ¿Qué podría salir mal con let friction = this.velocity;?
Cualquier cambio que se haga en `friction` también modificará `this.velocity`. Lo que podría causar problemas, porque la fricción debería reducir la velocidad, pero con esta referencia, se modificaría la velocidad mientras se calcula la fricción, creando resultados inconsistentes o inesperados.
