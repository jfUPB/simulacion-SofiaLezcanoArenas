## Consolidando la experiencia audiovisual generativa

### Conexión sonido-visión: ¿Qué tan efectiva crees que fue la conexión entre las características del audio que elegiste y los parámetros visuales que controlaban? ¿Lograste una respuesta visual que se sintiera “musical” o sincronizada? ¿Qué fue lo más difícil de esta conexión?

Es totalmente efectiva. La visualización además de guardar una conexión con la letra y el significado de la canción se siente viva, musical y plenamente sincronizada con la pieza.

Lo más complejo fue que se pudieran distinguir un poco las raíces por color a pesar del cambio del brillo. Aunque no está demasiado definido, de igual manera aporta un efecto interesante. También hubo cosas que no logré implementar como la interacción del usuario mediante la cámara.

No tuve ningún problema con la carga o análisis de audio porque ya había experimentado con esto anteriormente.

### Generatividad vs. Control: ¿Cómo balanceaste la necesidad de que los visuales respondieran al audio (control) con el objetivo de que fueran generativos y no repetitivos? ¿Qué técnicas usaste para introducir variación o aleatoriedad controlada?

Utilizando un flowfield logré que se mantuviera aleatoriedad en la visualización y que él mismo es diferente cada que se inicia una nueva visualización desde el inicio. Además, este también puede modificarse dando clic en el lienzo.

Un flowfield era mi mejor opción porque además de la aleatoriedad, también ayuda a mantener un movimiento coordinado que se ve bastante bien para seres vivos parte de un gran organismo como lo son las raíces de un árbol.

### Integración de conceptos: ¿Cómo aplicaste o combinaste conceptos de unidades anteriores (fuerzas, sistemas, agentes, física, etc.) en tu algoritmo generativo para este proyecto?
- Interpolación de color -> para una mayor variedad visual, cada partícula se crea con un color resultado de la interpolación entre un azul y un verde.
- Flowfield -> está encargado de controlar la dirección del movimiento de las partículas para que se vean como parte de un organismo pero también para que haya bastante variedad en las direcciones y sea inesperado el movimiento.
- Sistema de partículas -> para más dinamismo y versatilidad en el movimiento.
- Marco motion 101 -> sin él las partículas no podrían moverse. Aquí se interpreta la dirección y la velocidad para calcular la nueva posición de las partículas.

### Desafíos de p5.sound: ¿Encontraste alguna dificultad particular al usar p5.sound para el análisis de audio en tiempo real (rendimiento, precisión, complejidad de la API)?
Como ya había experimentado con audio en p5 y justamente analizando la amplitud de una canción, entonces ya conocía un camino para lograrlo sin problemas. 

### Resultado final: ¿El resultado final se acerca a tu concepto visual original? ¿Qué aspecto de tu simulación te enorgullece más? ¿Qué mejorarías si tuvieras más tiempo?
Se acerca bastante, aunque tuve que omitir algunas cosas para asegurar el rendimiento y una mejor visualización, priorizando el elemento más impactante (las raíces) para que ocupara todo el lienzo.
