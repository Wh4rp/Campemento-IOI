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
