##  Implementación

### Audio
- Creé una carpeta llamada assets para tener todo más ordenado y allí subí el mp3 de la canción. 
- En sketch.js creé una variable global `audio` para poder referirme a la canción fácilmente y una para medir la amplitud.
- Usando la función `preload()` cargo la canción antes de cualquier cosa.
  ``` js
  function preload() {
  soundFormats('mp3', 'ogg'); // Especifica los formatos de audio compatibles
  audio = loadSound('assets/AURORA - The Seed.mp3'); // Carga el archivo de audio antes de iniciar el sketch
  }
  ```
- Ya en `setup()` la canción se reproduce en bucle usando `audio.loop();` y tengo un objeto para poder medir el volumen de la canción `amplitude = new p5.Amplitude();`
- En `draw()` Con ` let level = amplitude.getLevel();` se obtiene el nivel actual del volumen del audio
  
### Audio modulando lo visual
#### Volumen afectando la velocidad de las raíces
``` js
draw(){
let speed = map(level, 0, 0.3, 0.5, 3); // convierte el volumen (entre 0 y 0.3) en una velocidad (entre 0.5 y 3).

.
.
.

r.update(speed); // Pasa a las raíces la velocidad determinada por el audio
}
```

#### Volumen afectando el brillo de las raíces
``` js
setup(){
colorMode(HSB, 360, 100, 100, 255); // Usa modo de color HSB para colores más vivos (matiz, saturación, brillo)
}

draw(){
let brightness = map(level, 0, 0.3, 20, 100); // Convierte el volumen en un valor de brillo (de 20 a 100 en HSB)

.
.
.

r.display(brightness); // Pasa a las raíces el dato de brillo para que puedan usarlo
}
```

Además de esto es importante resaltar que el movimiento es controlado por un flowfield el cual puede ser cambiado haciendo clic, esto como manera de interacción con el usuario pensando en que podía estresarse al ver muy vacía una zona y que podría querer cambiar eso o interactuar al son de la música y verlo reflejado.
