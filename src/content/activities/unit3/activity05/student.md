## ¿Y qué relación tiene esto de las leyes de Newton con al arte generativo?
### ¿Qué problema le ves a este planteamiento?
En este planteamiento ambas fuerzas no se estarían aplicando. Si se aplica una, la otra no lo hace, porque simplemente se está reemplazando el valor de la aceleración por el valor de una fuerza, no por la suma de ambas.
### ¿Qué solución propones? ¿Cómo lo implementarías en p5.js?
La solución que vería posible es reemplazar el contenido del método `applyForce` de
``` js
applyForce(force) {
  this.acceleration = force;
}
```

Por lo siguiente:
``` js
applyForce(force) {
  this.acceleration.add(force); // Acumula la fuerza
}
```

Además, para asegurarnos de que la aceleración no se acumule para el siguiente ciclo, se añadiría esta línea en el método `update`
```js
update() {
.
.
.
  // Después de mover el objeto
  this.acceleration.mult(0); // Reseteamos la aceleración para el próximo ciclo multiplicándola por cero
}
```
