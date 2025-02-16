## Diseño: exploración de la idea
### Intención de diseño
La intención de diseño de este código es crear una pieza de arte generativo algorítmico que simule de manera estilizada el movimiento gravitacional de los planetas del Sistema Solar en torno a un sol interactivo, el cual seguirá la posición del mouse.

El fondo estrellado estático creará un ambiente espacial inmersivo y contrastará con el movimiento continuo de los planetas, que tienen colores y tamaño distintivos, además de que saturno cuenta con un anillo, representando sus características reales de forma abstracta. Las estelas de centellas que dejan los planetas a su paso aportan un elemento visual atractivo y dinámico, enfatizando el movimiento orbital.
### Marco Motion 101
En este código se utilizará un vector para posición, uno para velocidad y uno para aceleración. Se utilizará el concepto de aceleración hacia el mouse.

Como factor diferenciador, la aceleración de los planetas estará influenciada por la posición del mouse, que representa el sol, generando una fuerza de atracción hacia este punto. La magnitud de esta aceleración es inversamente proporcional a la masa del planeta, creando una dinámica donde los planetas más pequeños se mueven más rápidamente que los más grandes, evocando la ley de la gravitación universal de manera simplificada.

### Referentes e inspiración
- [El mapa de la física](https://www.youtube.com/watch?v=ZihywtixUYo)
- [The Nature of Code: Vectors/Acceleration](https://natureofcode.com/vectors/#acceleration)
- Lista de ideas de ChatGPT de la cual la verdadera inspiración fue ver un emoji de estrella en una idea y luego en otra que el tema era varias partículas siguiendo el mouse.
  
  ![image](https://github.com/user-attachments/assets/d2c60ea9-a9ff-4fb7-9f46-840007ac9974)
  
  Esto me llevo a pensar en el sistema solar y que el mouse representara al sol.
