## Relación con el marco motion 101
### ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas? ¿Por qué es necesario hacer esta modificación?
La modificación que se requiere al tratar con fuerzas acumulativas es multiplicar la aceleración por cero luego de haberla calculado (como la suma de las fuerzas que actúan en el cuerpo) y usado, para que así sea calculada en cada frame y no crezca de manera exhorbitante causando el mal funcionamiento del programa.

En el código esto eso:
``` js
applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f); // calcular la aceleración
  }

 update() {
    this.velocity.add(this.acceleration); //Marco motion 101
    this.position.add(this.velocity); //Marco motion 101
    .
    .
    .
    this.acceleration.mult(0); //Se reinicia la aceleración para el siguiente frame
  }
```

### Identifica dónde está el Attractor en la simulación. Cambia el color de este.
**Modificación:**
``` js
display() {
    ellipseMode(CENTER);
    stroke(0); // esta es la línea del attractor
    if (this.dragging) {
      fill(50);
    } else if (this.rollover) {
      fill(100);
    } else {
      fill(175, 200, 50); // aquí se encuentra el color de relleno del attractor
    }
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
```
![image](https://github.com/user-attachments/assets/93e93811-bbd4-425b-be97-bca21ff17ef6)

### Observa que el Attractor tiene dos atributos this.dragging y this.rollover. Estos atributos no se modifican en el código, pero permitirían mover el attractor con el mouse y cambiar su color cuando el mouse está sobre él ¿Cómo podrías modificar el código para que esto funcione?
[Enlace a la simulación funcional](https://editor.p5js.org/SofiaLezcanoArenas/sketches/pjS_36Vw_)

Para permitir que el Attractor se mueva con el mouse cuando se arrastra y cambie de color cuando el cursor esté sobre él, hay que:

- Detectar si el mouse está sobre el Attractor modificando this.rollover en Attractor.
- Permitir arrastrarlo cuando se presiona el mouse (mousePressed).
- Actualizar su posición mientras se arrastra (mouseDragged).
- Liberarlo cuando se suelta el mouse (mouseReleased).
