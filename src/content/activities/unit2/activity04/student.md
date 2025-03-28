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

La diferencia es que la versión estática p5.Vector.dot(v1, v2) recibe dos vectores como parámetros y calcula su producto punto sin necesidad de que pertenezcan a un objeto en particular. En cambio, la versión de instancia v1.dot(v2) se usa en un vector específico y calcula el producto punto con otro vector pasado como argumento. Básicamente, la estática trabaja con cualquier par de vectores, mientras que la de instancia solo con el vector desde el que se llama.
### Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
El producto cruz de dos vectores en 3D da como resultado un nuevo vector que es perpendicular al plano definido por los dos vectores originales. En cuanto a su magnitud, esta es igual al área del paralelogramo formado por los dos vectores originales, lo que significa que si los vectores son paralelos, el producto cruz es cero (no hay área). La dirección la podemos ver imaginando un tornillo, si giras u hacia v (u x v), el producto cruz apunta en la dirección en la que avanzaría el tornillo si lo enroscaras siguiendo ese giro.
### ¿Para que te puede servir el método dist()?
Calcula la distancia entre dos puntos que están representados por vectores.

Versión estática `p5.Vector.dist(v1, v2)`

Versión dinámica `v1.dist(v2)`
### ¿Para qué sirve el método limit()?
Limita la magnitud de un vector a un valor que asignemos. La magnitud no podrá ser más grande que el valor límite establecido. Limit() también tiene una versión estática `p5.Vector.limit(v, 5)` que retorna un nuevo vector de p5 sin cambiar el original.
