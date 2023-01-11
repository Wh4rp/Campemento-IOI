# Temario

- Discusion de problemas

## Discusion de problemas

### B - Road Reparation

Problema visto en la clase de grafos 1.

<!-- Explicar mas a detalle Kruskal -->

### A - String Problem

La idea es ver en cada posicion calcular el costo de cambiar ambos caracteres por una letra arbitraria.

[b, ..., ]

[b, ..., ]

Y eso costo se hace con Dijkstra. Desde cada carácter hacia todos los demás nodos.

Como nota adicional este problema se puede hacer con Floyd Warshall.

## DP 2

Una forma de optimizar una forma de recursion. El ejemplo mas tipico es Fibonacci.

### Problema de la mochila

<!-- Explicar el problema -->

Podemos definir la funcion

$$
\operatorname{dp}(n, w) = \text{Considerando los primeros } n \text{ objetos y la mochila de peso } w
$$

Pongámonos en todos los casos:

- Si no ponemos el $n$ elemento entonces, basta ver la solucion para los primeros $n-1$ elementos y la mochila de peso $w$.

- Si ponemos el $n$ elemento entonces, basta ver la solucion para los primeros $n-1$ elementos y la mochila de peso $w - w_n$.

Asi, podemos definir la funcion de la siguiente manera:

$$
\operatorname{dp}(n, w) = \max(\operatorname{dp}(n-1, w), \operatorname{dp}(n-1, w - w_n) + v_n)
$$

A partir de esto, definimos los casos borde y los casos base y tenemos resuelto el problema.

Ver la cantidad de estados posibles del dp.

### Longest Increasing Subsequence

<!-- Explicar el problema -->

Se puede resolver con busqueda binaria.

<!-- compiar codigo -->

<!-- dar una breve explicion -->

Con programacion dinamica la funcion recursiva que nos sirve es:

$$
\operatorname{dp}(i) = \text{La longitud de la subsecuencia creciente mas larga que termina en el elemento } i
$$
