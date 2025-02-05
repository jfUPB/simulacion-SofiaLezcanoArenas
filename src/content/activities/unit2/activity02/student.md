## Repasa
### Enunciado
Modifica uno de los ejemplos de caminata aleatoria del capítulo 0 para que use vectores.
**Ejemplo seleccionado**: Example 0.1: A Traditional Random Walk
### ¿Que tuviste que hacer para hacer la conversión propuesta?
Tenía presente que la manera de usar vectores en el ejercicio era manejar la posición
como un vector en lugar de valores aparte para "x" y "y". Sin embargo, 
al involucrar una clase dentro del código, me confundí porque pensé que habría
conflicto a la hora de ubicar la función `createVector()`, ya que se enfatiza en que
debe estar dentro de `setup()` y `draw()`. Pero es que al estar dentro de una clase, 
de todas maneras estaba ubicada dentro de `draw()` sin problemas.

Lo único que había que cuidar era la sintaxis de `this.position` no se puede 
omitir el this porque es lo que indica que pertenece a esa clase. 
En caso de omitirlo, cosa que ya probé, sale el error de que position no está definida. 

### Código
``` js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(width / 2, height / 2); // se crea un vector para manejar la posición
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
  }
}
```
