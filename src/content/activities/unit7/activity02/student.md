## Investigación y experimentación con Matter.js

### Experimento 1
#### Código
#### Resultado

### Experimento 2
#### Código
#### Resultado

### Conceptos
- **Engine**: maneja la parte de la simulación física actualizando el estado de los objetos en el tiempo. Se asegura de que a cada objeto se le apliquen fuerzas como la gravedad, fricción y demás, de manera correcta y precisa.
- **World**: contiene todo lo de la simulación, objetos, cuerpos, constraints. Es donde la simulación toma lugar. Si algo está fuera del mundo, no se verá afectado por ninguna fuerza ni nada.
- **Body**: es un objeto en la simulación que tiene forma y propiedades definidas. Interactúa con los demás cuerpos de la simulación y puede ser estático (mantenerse quieto como el suelo) o dinámico (moverse).
- **Bodies**: es un namespace, contenedor para almacenar propiedades de métodos u objetos bajo un mismo nombre, facilitando así la creación de un tipo específico de cuerpo.
- **Constraint**: cómo se conectan y relacionan los cuerpos dentro de un mundo.
- **MouseConstraint**: complemento (plugin) que crea una especie de restricción invisible (constraint) entre el cursor del mouse y los cuerpos con los que se hace clic, permitiendo manipularlos de forma realista gracias a las leyes físicas simuladas.

### Dificultades con `Matter.js`
