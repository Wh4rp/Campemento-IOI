# Discusion de problemas

## [D - The Coin Change Problem](https://vjudge.net/problem/HackerRank-coin-change)

Podemos pensar este problema de forma top-down, es decir, definiendo bien una función recursiva que nos permita calcular la solución solo llamando al caso de más arriba.

Tiene todo el sentido del mundo definir nuestra función recursiva con argumento $k$ el monto a cambiar, luego queremos algún otro parámetro que nos dé la información de las monedas que podemos usar. Así si definimos

$$
\text{formas}(k, i) = \# \text{de formas de dar cambio de } k \text{ con solo las primeras } i \text{ monedas}
$$

Notemos que hay dos opciones, o agregar la moneda $\text{monedas}[i]$ al cambio, lo que nos baja el monto a $k - \text{monedas}[i]$ o no decidir no seguir agregando esta moneda y seguir con las monedas anteriores.

$$
\text{formas}(k, i) = \text{formas}(k, i-1) + \text{formas}(k - \text{monedas}[i], i)
$$

Ahora tenemos que definir los casos base, que son:

- Que no quede monto a cambiar

$$
\text{formas}(0, i) = 1 \text{ si } i > 0
$$

- Que ya no nos queden monedas a usar

$$
\text{formas}(k, i) = 0 \text{ si } i < 0
$$

- Que estemos regresando más dinero del que nos dieron

$$
\text{formas}(k, i) = 0 \text{ si } k < 0
$$

Luego utilizamos una memorización para guardar los resultados de las llamadas recursivas. La complejidad es de $O(n \cdot k)$.

## C - Caesar's Legions

Similar al problemas de las monedas, lo podemos pensar de forma recursiva. Tomamos como intuición;pararnos en un estado y ver de cuales son las formas en que se puede continuar creando las filas hermosas.

Definamos nuestra función como:

$$
\text{formas}(A, B, V, T) = \# \text{de formas de crear filas hermosas con } A \text{ solados de infantería y } B \text{ solados a caballo tal que hay } V \text{ solados de tipo } T
$$

Por ejemplo si tenemos de momento:

$$
AA\{...\}
$$

donde $\{...\}$ es el resto de la fila que falta por completar. La función que lo completa seria:

$$
\text{formas}(A-2, B, 2, 'A')
$$

La función recursiva seria:

$$ 
\text{formas}(A, B, V, T) = \begin{cases}
\text{formas}(A-1, B, V+1, 'A') + \text{formas}(A, B-1, 1, 'A') & \text{si } T = 'A' \\
\text{formas}(A, B-1, V+1, 'B') + \text{formas}(A-1, B, 1, 'B') & \text{si } T = 'B'
\end{cases}
$$

Los casos bases son:

- Que no queden solados de infantería o a caballo

$$
\text{formas}(A, B, V, 'A') = 0 \text{ si } V > k_1
$$

$$
\text{formas}(A, B, V, 'B') = 0 \text{ si } V > k_2
$$

$$
\text{formas}(0, 0, V, T) = 0
$$

$$
\text{formas}(A, B, V, T) = 0 \text{ si } A < 0 \text{ o } B < 0
$$

La solución entonces es:

$$
\text{formas}(A, B, 0, 'A')
$$

Utilizando programación dinámica, basta ocupar un array de 4 dimensiones para guardar los resultados de las llamadas recursivas.

```cpp
int dp[n_1][n_2][12][2];
```

La complejidad es de $O(n_1 n_2 \cdot 12 \cdot 2)$.

## E - Mixtures

Notar que el color no cambia segun el orden de mezclar las sustancias. Haremos un arreglo de sumas acumuladas para poder calcular el costo de mezclar un rango de sustancias en $\mathcal{O}(1)$.

<!-- Hacer apendice con la array de sumas acumuladas -->

$$
S[i][j] = \sum_{k=i}^{j} p_k
$$

El color de mezclar $i$ a $j$ es:
 
$$
(S[i+1] - S[j]) \operatorname{mod} 100
$$

Ahora esto nos ayudara a calcular la cantidad de humo que se libera si mezclamos $(0, i]$ con $[i+1, n)$:

$$
(S[i+1] - S[0]) \operatorname{mod} 100 \cdot (S[n] - S[i+1]) \operatorname{mod} 100
$$

Entonces podemos pensarlo de manera recursiva:

$$
\text{humo}(i, j) = \text{el mínimo humo que liberamos al mezclar el rango } [i, j)
$$

$$
\text{humo}(i, j) = \min_{i < k < j} \text{humo}(i, k) + \text{humo}(k, j) + (100 + S[k+1] - S[i]) \operatorname{mod} 100 \cdot (100 + S[j+1] - S[k+1]) \operatorname{mod} 100
$$

Le sumamos $100$ a cada lado para evitar problemas con los módulos. Puede suceder que nos quede un modulo negativo debido a que estamos restando.

Los casos base son:

$$
\text{humo}(i, i+1) = 0 \text{ si } i < n
$$
