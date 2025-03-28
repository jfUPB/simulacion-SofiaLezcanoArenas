## **Conceptos fundamentales**

### Manejo de ángulos

- **¿Qué está pasando en esta simulación? ¿Cuál es la interacción?**
    
    En esta simulación se observa que dos círculos están conectados mediante una línea en el centro del lienzo. Al presionar cualquier tecla los objetos rotan 0.1 radianes respecto al centro del lienzo.

- **Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?**
    
    Para evitar que el conjunto de figuras salga del lienzo y facilitar el manejo de la rotación, pues si no se hiciera esto, estarían rotando alrededor de la esquina superior izquierda que es el origen por defecto de p5.js.
    
- **'¿Cuál es la relación entre el sistema de coordenadas y la función `rotate()`?**

  La función `rotate(angle);` rota el sistema de coordenadas en torno al origen actual. Esto significa que cualquier elemento dibujado después de la rotación girará alrededor del origen, en lugar de moverse independientemente. Dado que se trasladó el origen al centro de la pantalla, la rotación ocurrirá alrededor de este punto central.

- **Teniendo en cuenta este código**
    
    ```jsx
    line(-50, 0, 50, 0);
      stroke(0);
      strokeWeight(2);
      fill(127);
      circle(50, 0, 16);
      circle(-50, 0, 16);
    ```
    
    **Al dibujar los elementos gráficos parece que se está dibujando en la posición `(0, 0)` del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?**
    
    - Después de la traslación (`translate(width / 2, height / 2);`), el punto `(0,0)` ya no es la esquina superior izquierda, sino el centro del lienzo. Como los elementos se dibujan en coordenadas relativas al nuevo origen, parece que están en `(0,0)`, pero en realidad están ubicados con respecto al centro.
    - El código que controla la rotación está en la función `keyPressed()`, donde `angle += 0.1;`. Esto significa que cada vez que se presiona una tecla, `angle` aumenta, lo que cambia el ángulo en `rotate(angle);`. Como `rotate()` usa el valor acumulado de `angle`, la transformación se va sumando en cada iteración del `draw()`, haciendo que los elementos parezcan rotar progresivamente.

### Apuntar en la dirección del movimiento

- **Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?**
    
    ```jsx
    this.velocity.add(this.acceleration);
        this.velocity.limit(this.topspeed); //esta línea no haría parte
        this.position.add(this.velocity);
    ```
    
    Le suma la aceleración a la velocidad y luego le suma la velocidad a la posición, lo cual permite que se aprecie el movimiento del objeto afectado por los factores involucrados (aceleración y velocidad).
    

**Observa detenidamente este fragmento de código de la simulación:**

```jsx
display() {
    let angle = this.velocity.heading();
 
    stroke(0);
    strokeWeight(2);
    fill(127);
    push();
    rectMode(CENTER);
    translate(this.position.x, this.position.y);
    rotate(angle);
    rect(0, 0, 30, 10);
 
    pop();
  }
```

- **¿Qué hace la función `heading()`?**
    
    La función `heading()` devuelve el **ángulo de dirección** de un vector en radianes.
    
    En este código, `this.velocity.heading();` obtiene el ángulo de dirección del vector `this.velocity`, lo que significa que el rectángulo girará en la misma dirección en la que se mueve el objeto.
    
    Si el vector de velocidad apunta hacia la derecha, el ángulo será **0 radianes**. Si apunta hacia arriba, será **-π/2 radianes (-90°)**, y así sucesivamente.
    
- **¿Qué hace la función `push()` y `pop()`? Realiza algunos experimentos para entender su funcionamiento**.
    - `push()`: Guarda la configuración actual (posición, rotación, escalado, etc.).
    - `pop()`: Restaura la configuración guardada con `push()`, evitando que las transformaciones afecten a otros elementos del dibujo.
    - Experimento → https://editor.p5js.org/SofiaLezcanoArenas/sketches/pWeSJIT0U
        
        En el experimento comenté las funciones `push()` y `pop()` y dibujé un cuadrado al final de la función `draw()` . El resultado es que el cuadrado también se ve afectado por el movimiento, cosa que no debería suceder si `push()` y `pop()` estuvieran presentes, pues la transformación solo afecta al rectángulo dentro de esas funciones.
        
- **¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.**
    
    La función `rectMode(CENTER);` establece el modo de dibujo de los rectángulos para que su punto de referencia sea el **centro** del mismo rectángulo en lugar de la esquina superior izquierda como es usual.
    Dibujando un rectángulo en (100, 100) en vez del origen se aprecia raro porque:
    
    - Al usar **`translate()`**, el nuevo `(0,0)` es la posición del objeto.
    - Si dibujas en `(100,100)`, los elementos se alejan del centro y rotan en órbitas.
    - **Para que el rectángulo rote sobre sí mismo**, siempre hay que dibujarlo en `(0,0)`, no en otro punto.
- **¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.**
    
    El ángulo de rotación del rectángulo se basa en la dirección del vector de velocidad.
    
    **Paso a paso de la transformación:**
    
    1. **Traslación (`translate(this.position.x, this.position.y);`)**
        - Mueve el origen de coordenadas a la posición del objeto.
    2. **Rotación (`rotate(angle);`)**
        - Gira el sistema de coordenadas según el ángulo de la velocidad (`this.velocity.heading()`).
    3. **Dibujado del rectángulo (`rect(0, 0, 30, 10);`)**
        - Como `rectMode(CENTER);` está activado, el rectángulo se dibuja centrado en `(0,0)`, pero en el sistema de coordenadas rotado.
