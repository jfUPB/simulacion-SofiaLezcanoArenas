## En mi mundo los pixeles si tienen masa
### ¿Qué problema le ves a este planteamiento? ¿Qué implica que force se está pasando por referencia?
Intentando calcular la aceleración manipulando la fórmula que propone la segunda ley de Newton `a = F / m `, en la línea
``` js
 force.div(10); // pensando en la masa como 10, se divide la fuerza entre la masa
```

Se está cometiendo un error, pues al force ser un valor pasado por referencia, en la línea anterior se está haciendo el valor de force igual a esta división, es decir, cambia la fuerza original que se tenía y no solo la aceleración.
### ¿Qué solución propones? ¿Cómo lo implementarías en p5.js?
Se debería hacer una copia de la fuerza antes de dividirla para evitar afectar el valor original de la misma.
``` js
applyForce(force) {
    let f = force.copy();  // Crear una copia del vector fuerza
    f.div(10);             // Dividir por la masa
    this.acceleration.add(f); 
}
```
