# Temario

- Discusión de problemas
- DP

## Discusión de problemas

### E - Navigation System

1. Hacer bfs del grafo transpuesto. El bfs debe partir desde el nodo al que llega.
2. Hacer el camino de Polycarpo. Ver si el camino que tomó es mejorable o no. Si no es mejorable sumarle 1 la cantidad mínima y máxima de reubuilds. Si lo era, sumarle 1 a la cantidad máxima de rebuilds siempre y cuando haya al menos otro camino igual de óptimo desde ahí.

### F - Kuro and Walking Route

1. Hacer DFS desde el nodo de la flor para encontrar la abeja y hacer el árbol con los padres.

2. Borrar la arista de

SOLUCION MIA CON DFS.

# MUDOULO 10

Zzz...

## DP

### Fibonazzzi

Definicion de fibonacci

$$
f(n) = \begin{cases}
0 & n = 0 \\
1 & n = 1 \\
f(n-1) + f(n-2) & n > 1
\end{cases}
$$

Explicar por que la solución recursiva de Fibonacci no es optima

### Problema de las monedas Programacion Dinamica

#### Buttom up y top down

#### Solucion

=== "Buttom up"

    ```cpp
    int mm(int i){
        if(i < 0) return 0;
        if(DP[i] != -1) return DP[i];
        DP[i] = max(A[i] + mm(i - 2), mm(i - 1));
        return DP[i];
    }
    ```
=== "Top down"

    ```cpp
    int dp[1000001];
    int mm(vector<int> &A){
        int v1 = 0, v2 = 0; // mm(i - 1) y mm(i - 2)
        for(int i = 0; i < A.size(); i++){
            int v = max(A[i] + v2, v1);
            v2 = v1;
            v1 = v;
        }
    }
    ```

La complejidad es $\mathcal{O}(n)$.
