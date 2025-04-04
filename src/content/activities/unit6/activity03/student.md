## Analizando el comportamiento de enjambre (Flocking)

### Reglas del Flocking
#### Separación:  evitar el hacinamiento con vecinos cercanos
- **Objetivo:** evitar concentrar demasiada información en un solo punto y choques entre los miembros del enajmbre.

- **Lógica:** compara cada la posición de cada boid con cada uno de los restantes, si la distancia entre ambos es mayor que 0 y menor que la separación deseada (establecida con anterioridad), calcula un vector apuntando lejos del vecino con el que se está comparando actualmente.
#### Alineación: dirigirse en la misma dirección promedio que los vecinos cercanos
- **Objetivo:** mantener una dirección bastante homogénea evitando el desorden.

- **Lógica:** se define una distancia a la que debe estar otro boid del actual para considerarlo un vecino. Se compara con cada uno de los otros boids existentes y si se cumple la distancia, se aumenta una unidad un contador, además de que la velocidad de los vecinos se va acumulando en un vector. Cuando se termina la comparación se saca un promedio de las velocidades para saber en general a donde están yendo los vecinos, se normaliza el vector y se limita para tomar la dirección sin alterar la velocidad actual del boid y se le devuelve para usarlo.
#### Cohesión: moverse hacia la posición promedio de los vecinos cercanos
- **Objetivo:** mantener un objetivo en común de desplazamiento como si realmente fueran parte de un grupo. Evitar que los miembros se pierdan.

- **Lógica:** Si el otro boid no es él mismo y está cerca, sumamos su posición y aumentamos el contador de vecinos. Calculamos el promedio de las posiciones de los vecinos (el centro del grupo cercano) y lo usa para moverse hacia allá.
### Parámetros clave identificados
- `desiredSeparation` contenido en cada una de las funciones de las reglas (dentro de ellas tienen valores diferentes)
- **Pesos o multiplicadores**:
  ``` js
  flock(boids) {
    let sep = this.separate(boids); // Separation
    let ali = this.align(boids); // Alignment
    let coh = this.cohere(boids); // Cohesion
    // Arbitrarily weight these forces
    sep.mult(1.5);
    ali.mult(1.0);
    coh.mult(1.0);
    // Add the force vectors to acceleration
    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);
  }

  ```
- `maxspeed` contenido en el consturctor de boid.
- `maxforce` contenido en el consturctor de boid.
### Modificación
**Aumentar drásticamente el radio de percepción de la separación**
Se observan muy separados los boids.
![image](https://github.com/user-attachments/assets/40e8b63b-bf46-4f74-a400-af7cfe82e15d)

**Disminuir drásticamente el radio de percepción de la separación**
Se observan muy juntos los boids.
![image](https://github.com/user-attachments/assets/d846951f-e304-4375-860a-8723695a5585)
