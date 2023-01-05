# Búsqueda binaria

Generalmente cuando les presentan búsqueda binaria se muestra con el ejemplo de un arreglo ordenado y encontrar un elemento dado.

Dado el siguiente array verifique si está el 8.

`A = [1, 3, 7, 10, 12, 17, 23]`

La solución naive es recorrer el arreglo y comparar en cada elemento si es igual o no a 8. Esto tiene complejidad $\mathcal{O}(n)$ para un largo de arreglo $n$.

(Animacion inded)

Antes cuando el profesor (Javier Marinkovic) iba al colegio (hace muuucho) usaban algo llamado diccionarios, que eran libros con palabras ordenadas alfabéticamente. Si por ejemplo quería buscar la palabra "casa" lo que hacia era ir a la sección de la "C" y veía si donde estaba abierto el libro estaba la palabra "casa". Si no estaba, entonces veía que palabra si estaba "caos" debía ir a una pagina mas a la derecha. Si esta nueva pagina empezaba con la palabra "cosa" debía ir a una hoja a la izquierda y asi...

Regresando al problema original la idea de la búsqueda binaria es dividir el arreglo en dos partes y ver si el elemento esta en la primera o segunda mitad. Si esta en la primera mitad, entonces la segunda mitad no la consideramos. Si esta en la segunda mitad, entonces la primera mitad no la consideramos. Asi hasta que el elemento este en el arreglo.

<!-- (animacion inded) -->

Su complejidad es $O(\log_2(n))$ ya que consta de cuantas veces puedo dividir el arreglo por 2 en el peor de los casos.

Pero la búsqueda binaria es mucho mas general que esto.

## Forma mas generalizada de búsqueda binaria

La búsqueda binaria nos ayuda a problemas en donde hay una _pregunta monótona_ en un rango. Ya sea algo como $x^2 \leq n$ o $x^2+3x \leq n$.

La propiedad monótona es a grandes rasgos, que la pregunta no vuelve a tomar un valor menor que uno anterior (a su izquierda en la recta numérica).

Veamos para la pregunta $P(x) := x^2 \leq n$ para los números $n = 0, 1, 2, 3, 4, 5, ...$ con $n = 9$.

| $x$ | 0 | 1 | 2 | 3 | 4 | 5 | ... |
|-----|---|---|---|---|---|---|-----|
| P(x)| V | V | V | V | F | F | ... |

¿Será acaso el $P(7984)$ verdadero o falso? O sea, ¿$7984^2 \leq 9$?

Veamos que dado que a su izquierda hay un valor que es falso entonces 7984 también lo es. Es más, todos los valores a su derecha serán falsos.

Volviendo al problema original del arreglo ordenado, podemos definir nuestra pregunta como $P(i) := A[i] \leq x$, con $x = 8$. 

Notemos que la pregunta es monótona:

|  $i$ | 1 | 3 | 8 | 16 | 19 |
|------|---|---|---|----|----|
| P(i) | V | V | V |  F |  F |

Si nosotros encontramos el indice mas grande que cumple esta propiedad, vamos a encontrar en donde debería estar el elemento, si es que está. El 8 debería estar en el último verdadero.

De forma más general, supongamos que tenemos nuestra función monótona y nos paramos en una posición que donde $P(x)$ es verdadera. Algo como lo siguiente:

V V V [V] ... V F F F F

Sabemos que el último verdadero estará a la derecha, por lo que podemos restringir nuestra búsqueda a la derecha de la posición en que nos paramos.

En cambio si nos paramos en un falso

V V V V ... V F F [F] F

Sabemos que el último verdadero estará a la izquierda, por lo que podemos restringir nuestra búsqueda a la izquierda de la posición en que nos paramos.

### Ejemplo con cuadrado para cuadrados perfectos

Quiero saber si el 1024 es un cuadrado perfecto. Aplicando búsqueda binaria podemos armar la pregunta $P(X) := x^2 \leq 1024$.

¿Por qué esta función sería monótona?

Primero necesitamos un rango, requerimos que el valor de la izquierda sea verdadero y el límite de la derecha sea falso. Para eso podemos tomar $x = 1$ y $x = 100$.

- Rango de búsqueda: $[1, 100]$

Quiero encontrar el ultimo verdadero.

Veamos el valor de al medio, $x = 50$. Es $50^2 \leq 1024$? No, entonces el ultimo verdadero esta a la izquierda. Acortamos la búsqueda a la izquierda.

- Rango de búsqueda: $[1, 50]$

Veamos el valor de al medio, $x = 25$. Es $25^2 \leq 1024$? Si, entonces el ultimo verdadero esta a la derecha. Acortamos la búsqueda a la derecha.

- Rango de búsqueda: $[25, 50]$

Veamos el valor de al medio, $x = 37$. Es $37^2 \leq 1024$? No, entonces el ultimo verdadero esta a la izquierda. Acortamos la búsqueda a la izquierda.

- Rango de búsqueda: $[25, 37]$

Veamos el valor de al medio, $x = 31$. Es $31^2 \leq 1024$? Si, entonces el ultimo verdadero esta a la derecha. Acortamos la búsqueda a la derecha.

y así...

Esto terminará cuando nuestro rango se acote a [32, 32], verificamos y tenemos que $32^2 = 1024$ por lo que si es un cuadrado perfecto.

### Complejidad de búsqueda binaria general

¿Cuántas veces puedo dividir nuestro rango a la mitad?

R: $O(\log_2(n))$.

## Ejemplo real de la hamburguesa

Resumen:

Polycarpo quería hacer hamburguesas y tiene la receta de la hamburguesa, que es `BBHLB`. Ademas sabe cuanto cuesta el pan, la hamburguesa y la lechuga. Se sabe cuantos de estos ingredientes tiene ya en su casa. Dado el dinero que tiene la pregunta es cuanta hamburguesas puede hacer Polycarpo ajustandose a su presupuesto.

Definamos la pregunta $Q(n) :=$ ¿puede Polycarpo hacer $n$ hamburguesas?

Primero veamos que es monótona. Claramente si puede hacer 50 hamburguesas entonces también puede hacer 49. Por otro lado, si no puede hacer 1 trillion de hamburguesas entonces tampoco puede hacer 1 trillion + 1 hamburguesas.

Ahora respondamos la pregunta para $n$.

Los datos son:

- $R_B, R_H, R_L$, la cantidad de ingredientes que necesita para hacer una hamburguesa.
- $\$B, \$H, \$L$, el costo de cada ingrediente.
- $\#B, \#H, \#L$, la cantidad de ingredientes que tiene Polycarpo.

El dinero que se gasta en panes para hacer $n$ hamburguesas es $(n \cdot R_B - \#B) \cdot \$B$, siempre y cuando sea positivo, caso contrario seria $0$ ya que polycarpo tiene suficientes panes. Notemoslo como $N_B(n) = \max(0, (n \cdot R_B - \#B) \cdot \$B)$. Lo mismo para la lechuga $N_L(n)$ y la hamburguesa $N_H(n)$.

El dinero total necesario es $N_B(n) + N_L(n) + N_H(n)$. Si este es menor o igual al dinero que tiene Polycarpo entonces podemos decir que si puede hacer $n$ hamburguesas.

La pregunta se reduce a $N_B(n) + N_L(n) + N_H(n) \leq P$. Podemos ocupar búsqueda binaria.

### Complejidad del problema

Calcular $N_B(n) + N_L(n) + N_H(n) \leq P$ es solo hacer operaciones básicas por lo que es $\mathcal{O}(1)$ y por búsqueda binaria haremos la verificación a lo mas $\log_2(n)$ veces, con $n$ el rango largo del rango de búsqueda. Para este problema podríamos haber usado $[0, P]$.
