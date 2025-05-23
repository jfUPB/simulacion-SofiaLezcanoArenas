## Comparando algoritmos y consolidando conceptos

### Diferencias fundamentales: ¿Cuál dirías que es la diferencia principal en cómo Flow Fields y Flocking logran el movimiento coordinado o dirigido de los agentes? (piensa en dónde reside la “inteligencia” o las reglas: ¿En el entorno o en las interacciones entre agentes?)
- **Flowfields**: las reglas de movimiento las propone el entorno, el "tablero" es el que indica en qué dirección debe ir un objeto u otro. Se ve coordinado porque siguen un movimiento preestablecido.
- **Flocking**: las reglas de movimiento las dictan los demás agentes, se calculan respecto al comportamiento de los vecinos. Se ve coordinado porque sigue un movimiento muy similar al de los vecinos.

### Tipos de comportamiento emergente: basado en tu análisis y aplicación, ¿Qué tipo de comportamiento colectivo o patrón visual crees que es más fácil o natural lograr con Flow Fields? ¿Y con Flocking? Da ejemplos.
Con flow fields emular fenómenos naturales y el movimiento de objetos inanimados que deben desplazarse me parece más acertado (hojas que van con el viento, partículals de arena, ceniza, etc), pues estos objetos no están conscientes de su entorno, sino que hacen lo que dicta una fuerza externa. Mientras que flocking es más útil para seres vivos, se ve bastante natural, muy acercado a cómo se comportaría un miembro de un enjambre, cardumen, o cualquier otro grupo de criaturas.

### Ventajas y desventajas: en tu opinión, ¿Cuáles podrían ser las ventajas o desventajas de usar uno u otro algoritmo para ciertos tipos de efectos visuales o simulaciones?
- **Flowfields**: son más rápidos y requieren menos líneas de código para ser implementados, son más sencillos de controlar al tener menos parámetros, y se pueden aplicar a grandes cantidades de partículas sin que ellas interactúen directamente, lo que puede llevar a que se sientan desconectadas para tener un comportamiento colectivo.
- **Flocking**: simula un comportamiento colectivo bastante creíble, pues cada ente está consciente de los que lo rodean, lo que lleva a un sentimiento de vida en ellas. También permite agregar fácilmente comportamientos responsivos como seguir al mouse, entre otras. Sin embargo, al cada partícula estar consciente de otras, el proceso se vuelve más pesado, sin mencionar que deben evaluar tres condiciones base, no solo una.

### El agente autónomo: ¿Cómo te ayudaron estos dos ejemplos (Flow Fields y Flocking) a entender mejor el concepto de “agente autónomo”? ¿Qué características definen a un agente en estos sistemas?
- Es un ente que actúa por sí mismo siguiendo las reglas de su entorno, ya sea el tablero o lo que dicten sus vecinos. 
- Aunque tiene reglas, él toma decisiones y no se le dice exactamente lo que debe hacer.
- Está consciente de su entorno.
- No necesita fuerzas externas que lo empujen.
- Cada agente es único y tiene sus propias propiedades.

### Emergencia: ¿En qué momento observaste “comportamiento emergente” (complejidad o patrones no programados explícitamente) al trabajar con estos algoritmos?
Cuando en momentos los agentes en los ejemplos de flow field se iban en direcciones parecidas a las indicadas en el tablero, pero no exactamente y resultaban en bifurcaciones del grupo o en la unión de grupos separados.
