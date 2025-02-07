## Explora posibilidades
### ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
**Mag():** Calcula la magnitud, es decir, el largo de un vector 2D. Se usa pasando las componentes a la función así `mag(x, y)`

**MagSq():** Calcula la magnitud, es decir, el largo de un vector pero elevada al cuadrado. Este método es más eficiente porque
- Evita la raíz cuadrada en comparaciones de magnitudes o distancias, lo que mejora el rendimiento.
- Optimiza cálculos de distancias en gráficos, geometría computacional y algoritmos de machine learning.
- Facilita las comparaciones de longitudes o distancias sin la necesidad de realizar operaciones matemáticas más costosas.
### ¿Para qué sirve el método normalize()?
Se utiliza para normalizar un vector. Este método escala los componentes de un p5.Vector de modo que su magnitud sea 1. La versión estática de normalize(), como en p5.Vector.normalize(v), devuelve un nuevo p5.Vector y no cambia el original.

Un vector normalizado tiene la misma dirección que el vector original, pero su longitud (magnitud) es igual a 1. Esto es útil para trabajar con direcciones sin preocuparte por las magnitudes originales del vector, y se utiliza frecuentemente en gráficos, simulaciones físicas y muchas otras aplicaciones matemáticas.
### Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
El método dot() sirve para calcular el producto punto entre dos vectores. Si le pasas un valor entre paréntesis, lo toma como un vector, si le pasas dos o tres separados por comas, toma los valores como componentes de un vector.
### El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?
La diferencia principal es cómo se llaman:
- El método de instancia se llama sobre un vector (por ejemplo, v1.dot(v2)).
- El método estático se llama desde la clase p5.Vector (por ejemplo, p5.Vector.dot(v1, v2)).
### Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
### ¿Para que te puede servir el método dist()?
### ¿Para qué sirven los métodos normalize() y limit()?
