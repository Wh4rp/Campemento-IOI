# Temario

- Problemas

## Resolucion de problemas

### C - Two sets

Explicar lo de las sumas S(A) y S(B), con A y B conjuntos disjuntos que ambos formas {1, 2, ..., n}.

Ver D[i][s] = # formas de sumar S con algunos de los numeros {1, 2, ..., i}.

1 2 3 4  5  6  7  8 9 10
1 3 6 10 15 21 26

S(A) = n(n+1)/4
n(n+1) %= 0 mod 4
4|n o 4|n+1

Problema con contar dos veces.

Problema con modulo.

### A

DP[i] si partimos de i, cuanto cuesta llegar al final. Se asume que se inicia con energía 0. No se considera que venimos con energía de antes.

El caso base va desde el final. DP[n+1] = 0.

Si compramos un refresco j, tendremos energía g_{i, j} ahora y podemos llegar hasta un edificio en especifico. Por ende, su costo seria P_i,j + min_{niveles k que llegar con energia g_{i, j}} DP[k].

Ahora veamos el minimo entre todas las tiendas en i para saber DP[i].

Podriamos aplicar for, pero eso no da por su complejidad.

Notemos que estamos pidiendo el minimo, entre un rango contiguo de niveles. Por otro lado, podemos hacer busqueda binaria para saber hasta que nivel se llega.

### F

DP[i][x] = mayor ganancia de haber elegido x en la posicion i. Tomando i como la ultima posición del array.

Ver el max |x-y| + DP[i-1][y].

Pero esto no pasa porque toma O(nb^2).
