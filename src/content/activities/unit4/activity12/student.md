## Consolidación
### ¿Qué concepto de oscilación utilizaste como base para tu obra? Describe cómo lo implementaste
1. **Funciones sinusoides:** Se usan en la interpolación de colores y la representación de ondas de sonido. Implementación:
   - En la clase `Oscillator`, la variable `angle` cambia constantemente con `angleVelocity`, lo que genera un movimiento oscilatorio.
   - El método `sin(this.angle)` se usa para interpolar entre el color 1 y el color 2 de ida y vuelta, haciendo que se vea coordinado y cíclico. Podría decirse que los osciladores no solo oscilan en movimiento sino también en color.
   - La onda de sonido en sí es una suma de muchas funciones seno con diferentes frecuencias y amplitudes. `fft.waveform()` nos permite extraer y visualizar esa mezcla, pues toma esa señal y extrae un conjunto de valores entre -1 y 1, que representan cómo cambia la onda en cada momento. En el eje x tenemos el tiempo y en el eje y tenemos la amplitud de la onda (en una onda sonora, esto es el volumen)

2. **Osciladores:** Cada punto visual es un oscilador que sigue la onda de la canción. Implementación:
   - Cada instancia de `Oscillator` representa un oscilador que se mueve en función de la señal de audio.
   - `angleVelocity` controla la velocidad del movimiento oscilatorio.
   - `this.y = height / 2 + level;` ajusta la posición vertical del oscilador basándose en la onda de audio.
    
3. **Ondas:** Se analiza la onda de sonido y se usa para animar los osciladores. Implementación:
   - `fft.waveform()` obtiene una representación digital de la onda de sonido cargada.
   - `waveform[waveIndex]` obtiene un valor específico de la onda. Lo multiplicamos por `this.amplitude` para hacerlo más grande. Luego, sumamos height / 2 para que se mueva arriba y abajo desde el centro de la pantalla.

### ¿Cómo funciona la interacción en tu obra?
Los usuarios pueden elegir desde los archivos de su computador, un archivo .wav o .mp3 con una melodía de su preferencia y subirla a la aplicación. Así podrán observar como la música afecta visualmente los osciladores.
### ¿Qué desafíos encontraste durante el proceso de creación? ¿Cómo los superaste?
El mayor desafío era cargar correctamente el archivo de audio a la aplicación. Tuve que implementar un mensaje de estado del archivo para poder visualizar en qué momento se daba el problema, si directamente no se tomaba para cargarlo o si había un error al cargarlo. También con ayuda de Chat GPT se implementaron más pasos para asegurar la carga correcta del archivo, además de verificar bien el orden de las líneas de código para evitar errores y que realmente los osciladores recibieran los datos del sonido.
### ¿Qué aprendiste sobre las oscilaciones y su aplicación en el arte generativo?
Las oscilaciones son muy útiles a la hora de armar una aplicación con audio, pues en esencia el sonido es una onda, por lo que es la representación visual más acertada. Aunque la función sinusoide es simple y armónica, también puede usarse para una gran cantidad de posibilidades visuales sin que se vea aburrido, explorando temas como el color y el sonido. Finalmente, las coordenadas polares son útiles para algunas aplicaciones con oscilaciones, pues permiten describir de mejor manera movimientos circulares.
