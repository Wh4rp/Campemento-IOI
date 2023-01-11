# Temario

- Discusion de problemas
- Grafos 2

## Discusion de problemas

### D - The Coin Change Problem HackerRank - coin-change

Pensar en la siguiente funcion recursiva:

$$
\text{formas}(k, i) = \# \text{de formas de dar cambio de } k \text{ con solo las primeras } i \text{ monedas}
$$

$$
\text{formas}(k, i) = \text{formas}(k, i-1) + \text{formas}(k - \text{monedas}[i], i)
$$

Ver casos base:

$$
\text{formas}(0, i) = 1 \text{ si } i > 0
$$

$$
\text{formas}(k, i) = 0 \text{ si } i < 0
$$

$$
\text{formas}(k, i) = 0 \text{ si } k < 0
$$

### C - Caesar's Legions

Similar al problemas de las monedas, pensadolo de manera recursiva.

$$
\text{formas}(A, B, V, T) = \# \text{de formas de crear filas hermosas con } A \text{ solados de infanteria y } B \text{ solados a caballo tal que hay } V \text{ solados de tipo } T
$$

Por ejemplo si tenemos de momento:

$$
AA\{...\}
$$

la funcion que lo completa seria:

$$
\text{formas}(A-2, B, 2, 'A')
$$

La funcion recursiva seria:

$$
\text{formas}(A, B, V, T) = \begin{cases}
\text{formas}(A-1, B, V+1, 'A') + \text{formas}(A, B-1, 1, 'A') & \text{si } T = 'A' \\
\text{formas}(A, B-1, V+1, 'B') + \text{formas}(A-1, B, 1, 'B') & \text{si } T = 'B'
\end{cases}
$$

Los casos bases son:

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

### E - Mixtures

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

### Estructuras de datos propias

Hagamos nuestro propio `pair`

Necesitamos un constructor.

```cpp
struct Par {
    int primero, segundo;
    Par(){
        primero = 0;
        segundo = 0;
    }
    Par(int primeroprima){
        primero = primeroprima;
    }
    Par(int p, int s){
        primero = p;
        segundo = s;
    }
};
```

Ahora podemos hacer lo siguiente en el `main`

```cpp
Par punto;
cout << punto.primero << endl;
```

Tambien podemos asignar un valor a su atributo `primero`:

```cpp
punto.primero = 5;
```

O utlizar el constructor al momento de definirlo:

```cpp
Par punto2(5, 10);
```

Por ejemplo el constructor del `vector` de la libreria estandar es:

```cpp
vector<int> v(10, 5);
```

Para generalizar el tipo de dato con que hacemos el struct se puede ocupar Template:

```cpp
template <typename T>
struct Par {
    T primero, segundo;
    Par(){

    }
    Par(T primeroprima){
        primero = primeroprima;
    }
    Par(T p, T s){
        primero = p;
        segundo = s;
    }
};
```

Para que se puede definir un `Par` sin especificar sus atributos debemos matener `Par()` pero sin nada dentro ya que de no ser asi no podriamos hacer:

```cpp
Par punto;
```

Estamos asumiendo que el tipo de dato del `Par` es el mismo para ambos atributos. Para generalizarlo mas se puede ocupar:

```cpp
template <typename T, typename U>
struct Par {
    T primero;
    U segundo;
    Par(){

    }
    Par(T primeroprima){
        primero = primeroprima;
    }
    Par(T p, U s){
        primero = p;
        segundo = s;
    }
};
```

Y en el `main` podemos hacer:

```cpp
Par<int, double> punto(5, 10.5);
```

Ejercicio: Definan sus propia estructura de `stack` llamada `mi_stack` con los tipicos metodos `push`, `pop`, `top` y `empty`. Pueden ocupar un `vector` como base.

=== "base del ejercicio"
    ```cpp
    struct mi_stack {
        vector<int> v;

        bool empty(){
            
        }
        int size(){

        }
        int top(){
            
        }
        void pop(){
            
        }
        void push(){
            
        }
    };
    ```
=== "solucion"
    ```cpp
    struct mi_stack {
        vector<int> v;

        bool empty(){
            return v.empty();
        }
        int size(){
            return v.size();
        }
        int top(){
            return v.back();
        }
        void pop(){
            v.pop_back();
        }
        void push(int x){
            v.push_back(x);
        }
    };
    ```

### Segment Tree

El segment tree es una estructura de datos que nos permite responder preguntas por rango en un arreglo en $\mathcal{O}(\log n)$.

El ejemplo tipico es ver el mayor elemento en un rango de un arreglo.

Respondamos esta pregunta para el array = `[]`.

La primera idea es que podemos dividir el arreglo en $4$ partes iguales y ver el mayor elemento en cada una de ellas.

Para cualquier rango que busquemos nunca va a pasar que hagamos mas de 2 consultas.

Esta idea la podemos extender mas aun. Deividamos de nuevo:

<!-- copiar imagen de la diapo -->

El Segment Tree completo se ve asi:

<!-- copiar imagen -->

Cuantos niveles deberia tener el segment tree? $\log_2 n$.

<!-- explicar camino de busqueda -->

Otra cosa buena del Segment Tree es que se puede actualizar un elemento en $\mathcal{O}(\log n)$.

<!-- Poner implementacion de Segment TRee-->
