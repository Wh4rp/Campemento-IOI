# Complejidad

Es una forma de mesurar la eficiencia de un programa. Daremos una idea intuitiva de complejidad con ejemplos.

## Ejemplo 1

Tenemos el array `[1, 3, 4, 2, 5, 7]` y queremos saber si esta el $5$ en el array o no.

### Solución 1

Recorremos el array y comparamos cada elemento con el $5$.

(Animación de tu eres el 5, si o no)

¿Cuantas preguntas hay que hacer para saber si esta el $5$ o no? En este caso 5.

La idea central es ver el peor de los casos. En este ejemplo el peor de los casos es que el 5 no este en el array por lo que habría que hacer la pregunta 6 veces. Para el caso general donde el array es de largo $n$ es $n$.

Esto es lo que buscamos en la complejidad, la mayor cantidad de operaciones que tienen que hacerse en el peor de los casos.

## Ejemplo 2

¿Cuánto se demora el siguiente programa?

```cpp
int suma = 0;
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        suma += j;
    }
}
```

### Solución 2

Va a pasar sumarle a la variable `suma` $n^2$, en notación asintótica esto se escribe como $\mathcal{O}(n^2)$.

## Ejemplo 3

¿Cuánto se demora el siguiente programa?

```cpp
int suma = 0;
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        for (int k = 0; k < n; k++) {
            suma += k;
        }
    }
}
```

### Solución 3

Haciendo el mismo análisis que en el ejemplo 2, la complejidad es $\mathcal{O}(n^3)$.

## Ideas generales de la complejidad

- Hacer $k$ veces una cosa que toma $T(n)$ es $\mathcal{O}(k \cdot T(n))$.
- Hacer un proceso que toma $T_1(n)$ y después otro que toma $T_2(n)$ es $\mathcal{O}(T_1(n) + T_2(n))$. Pero si ambos son $T(n)$ entonces el tiempo queda $T(2 n)$ pero como $2$ es una constante se convierte a $\mathcal{O}(n)$.

Aclaración importante, las constantes en la notación asintótica se pueden _eliminar_ mientras que las variables se mantienen. Por eso en el ejemplo 2 se elimina el $2$, mientras que en la idea general 1 no se elimina el $k$.

El calculo de la complejidad es algo general y puede ser que la constante si varíe el tiempo de ejecución. Esto es si queremos hilar más fino, pero en general los problemas están diseñados para que no importe la constante.

## Ejemplo más difícil

```py
for i <= n:
    for i <= j:
        if A[i] + A[j] == 6:
            print("Si")
```

Con las reglas que hemos visto, la complejidad sería de $\mathcal{O}(n^2)$.

## Cómo saber si mi código pasa dado que se su complejidad

Como regla general se considera que un computador hace $10^8$ operaciones por segundo (regla muy general pero sirve). Por lo que puede deducir que si un algoritmo tiene complejidad $\mathcal{O}(n^2)$ y $n = 10^3$ entonces hará $10^6$ operaciones y como $10^6 < 10^8$ entonces el algoritmo se ejecutará en menos de un segundo.

| Tamaño input ($n$) | Posibles complejidades |
|--------------------|------------------------|
| $n \leq 10$        | $\mathcal{O}(n !), \mathcal{O}\left(n^7\right), \mathcal{O}\left(n^6\right)$ |
| $n \leq 20$        | $\mathcal{O}\left(2^n \cdot n\right), \mathcal{O}\left(n^5\right)$ |
| $n \leq 80$        | $\mathcal{O}\left(n^4\right)$ |
| $n \leq 400$       | $\mathcal{O}\left(n^3\right)$ |
| $n \leq 7500$      | $\mathcal{O}\left(n^2\right)$ |
| $n \leq 7 \cdot 10^4$ | $\mathcal{O}(n \sqrt{n})$ |
| $n \leq 5 \cdot 10^5$ | $\mathcal{O}(n \log n)$ |
| $n \leq 5 \cdot 10^6$ | $\mathcal{O}(n)$ |
| $n \leq 10^{18}$   | $\mathcal{O}\left(\log ^2 n\right), \mathcal{O}(\log n), \mathcal{O}(1)$ |

## Ejemplo con Insertion Sort

Insertion Sort es un algoritmo de ordenación cuya idea principal es ir insertando el elemento en la posición correcta recorriendo de izquierda a derecha.

### Código de Insertion Sort

El código de Insertion Sort es el siguiente:

```cpp
for (int i = 1; i < n; i++) {
    int j = i;
    while (j > 0 && A[j] < A[j - 1]) {
        swap(A[j], A[j - 1]);
        j--;
    }
}
```

La complejidad de este algoritmo es $\mathcal{O}(n^2)$.

## Ejemplo con Merge Sort

La idea de Merge Sort es dividir el array en dos partes y ordenar cada una de ellas por separado. Luego mezclar las dos partes ordenadas.

Supongamos que tenemos los arrays `A = [1, 3, 7, 10]` y `B = [2, 5, 9]`. Dado que ambos están ordenados ¿Cómo ordenarías un array combinado de `A` con `B`?

Podemos hacerlo con dos punteros, quedando: <!-- (animación inded): -->

`AB = [1, 2, 3, 5, 7, 9, 10]`

¿Cuál es la complejidad de hacer esto? Es el largo de `A` más el largo de `B`. Supongamos que el largo de `A` es $n$ y el de `B`, $m$ entonces la complejidad sería $\mathcal{O}(n+m)$.

Ahora que sabemos cómo juntar dos arrays ordenados podemos ocupar esto para ordenar un arreglo desordenado completamente.

Vamos dividir a la mitad arreglo que deseamos ordenar. Luego cada mitad le aplicamos un _algoritmo misterioso_ que los ordenar, y después de que cada partición está ordenada podemos ocupar el algoritmo que ya conocemos para regresar el array completo ordenado.

¿Cuál es ese algoritmo misterioso? pues, el mismo (recursion).

<!-- (imagen de mergeSort innded) -->

En la base de esto estará ordenar el array de un solo elemento, como ya está ordenado (es solo un elemento) podemos retornar este arreglo unitario.

Ahora veamos cual es su complejidad. Primero recordemos que la complejidad de unir dos arrays ordenados en otro ordenado (merge) es de $\mathcal{O}(n)$ con $n$ el largo del array final. Pero cada nivel del merge toma $\mathcal{O}(n)$. ¿Cuántos niveles van a haber en total? como en cada paso dividimos el arreglo en 2 va a ser hasta que queden arreglos de tamaño 1. Y esta cantidad sera aproximadamente $\log_2(n)$.

Asi, el algoritmo de Merge Sort tiene complejidad $\mathcal{O}(n \cdot \log_2(n))$.

Si no sabes que es un logaritmo ver [apéndice](../apeendice/logaritmos).

### Código de Merge Sort

EL código en `C++` sería el siguiente:

```cpp
void mergeSort(vector<int> &A) {
    if (A.size() == 1) {
        return;
    } else {
        int mid = A.size() / 2;
        vector<int> left(A.begin(), A.begin() + mid);
        vector<int> right(A.begin() + mid, A.end());
        mergeSort(left);
        mergeSort(right);
        merge(left, right, A);
    }
}

void merge(vector<int> &left, vector<int> &right, vector<int> &A) {
    int i = 0, j = 0, k = 0;
    while (i < left.size() && j < right.size()) {
        if (left[i] < right[j]) {
            A[k] = left[i];
            i++;
        } else {
            A[k] = right[j];
            j++;
        }
        k++;
    }
    while (i < left.size()) {
        A[k] = left[i];
        i++;
        k++;
    }
    while (j < right.size()) {
        A[k] = right[j];
        j++;
        k++;
    }
}
```

## Las operaciones toman tiempo constante

Vamos a asumir que las operaciones de $c++$ toman tiempo $\mathcal{O}(1)$, esto si nos ponemos muy detallistas no es tan cierto. Para los rangos de tipos de datos que trabajamos los procesadores trabajan muy bien con sus operaciones.
