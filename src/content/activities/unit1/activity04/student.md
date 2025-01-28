# Distribuciones de probabilidad
## Diferencia entre distribuciones uniformes y no uniformes
Uniforme: Todos tienen la misma oportunidad de ser elegidos.
No Uniforme - normal - randomGaussian(): Algunos tienen más oportunidad que otros. Específicamente, usando la función  `randomGaussian()`, mientras más cerca esté un número de la media, más probable es que salga, y mientras más alejado esté, menos probable es que salga.
## Código con distribución de probabilidad no uniforme
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
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(randomGaussian(0)); //en vez de random(), se usa la función randomGaussian()
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else if (choice == 3){
      this.y--;
    }
  }
}
```

Para lograr favorecer la derecha en la caminata aleatoria, cambié la función `random()` por la función `randomGaussian()` y como argumento colocqué 0, que es la opción que indica aumentar la variable x, lo que visualmente se traduce a una caminata que tira más a la derecha que hacia otra dirección.

Imagen
