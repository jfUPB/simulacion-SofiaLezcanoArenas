## Consolidación de lo aprendido
### 1. ¿Cómo se gestiona la creación y la desaparición de las partículas y cómo se gestiona la memoria?
Cada vez que se aplauda o se genere cierto nivel de ruido, se creará un emitter que generará partículas de tipo standard, twinkle o spiral de manera completamente aleatoria. Este se agrega a la lista de emitters y después de tres segundos se eliminan para evitar saturar demasiado la memoria, además, las partículas dentro de un Emitter se eliminan cuando su lifespan llega a 0. El vuelo de Lévy dictaminará la posición del nuevo emitter.

### 2. ¿Cómo se aplica el marco de movimiento motion 101 en los sistemas de partículas que analizaste en la actividad anterior?
Se aplica en el movimiento de cada partícula, pues siempre tienen en su método `update()` que es el que maneja el movimiento de las partículas el código correspondiente al marco:
``` js
update() {
    this.velocity.add(this.acceleration); //se le añade la aceleración a la velocidad
    this.position.add(this.velocity); // se le añade la velocidad a la posición
    this.acceleration.mult(0); // se reinicia la aceleración para evitar errores
    .
    .
    .
  }
```

### 3. ¿Cómo se aplican fuerzas externas a los sistemas de partículas que trabajaste en la unidad? ¿Qué fuerzas se aplicaron y cómo están modeladas?
Generalmente se aplicaba la fuerza de la gravedad. En `sketch` tenemos que se crea el vector de gravedad y se llama al método `applyForce` contenido en la clase `Emitter`, dentro del cual se llama al método `applyForce` contenido en la clase `Particle` para cada una de las partículas, y que tiene el siguiente código:
``` js
applyForce(force) {
    let f = force.copy(); // pasa por valor la fuerza evitando afectar directamente el valor de la misma, sino una copia de ella.
    f.div(this.mass); // divide la fuerza por la masa del objeto, aplicando la segunda ley de Newton (F = ma, por lo que a = F/m).
    this.acceleration.add(f); // suma lo obtenido a la aceleración del objeto, modificándola y por ende su velocidad y posición
  }
```

### 4. ¿Cómo se aplicaste los conceptos de herencia y polimorfismo en los sistemas de partículas que trabajaste en la unidad?
- **Herencia:** Se creó una clase padre de partículas `BaseParticle` y se crearon diferentes tipos de partículas `StandardParticle`, `TinkleParticle` y `SpiralParticle`. Cada una de ellas hereda de `BaseParticle` el método constructor, el método para moverse, el método para mostrarse, el de actualizar la variable lifeSpan, el de detectar si está muerta, entre otros.
- **Polimorfismo:** Cada tipo de partícula sobreescrible alguno de los métodos. Por ejemplo `SpiralParticle` sobreescribe el método `update()` que es el que maneja el movimiento, pues este tipo de partícula se mueve en espiral mientras se desplaza, mientras que `TinkleParticle` sobreescribe el método de `show()`, pues este tipo de partícula parpadea.
