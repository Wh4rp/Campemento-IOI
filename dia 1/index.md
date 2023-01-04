# Temas

- Complejidad
- Búsqueda binaria
- Librería Estándar de `C++` (STL)

## Complejidad

Es una forma de mesurar la eficiencia de un programa. Daremos una idea intuitiva de complejidad con ejemplos.

### Ejemplo 1

Array de [1, 3, 4, 2, 5, 7] y queremos saber si esta el 5 en el array.

#### Solución 1

Recorrer el array y comparar cada elemento con el 5.

(animacion de tu eres el 5, si o no)

Cuantas preguntas hay que hacer para saber si esta el 5 o no. En este caso 6. Para el caso general es n.

La idea central es ver el peor de los casos. En este caso el peor de los casos es que el 5 no este en el array.

### Ejemplo 2

Encuenesta de diagnostico: cuanto se demora el siguiente programa

```py
    for i <= n:
        for j <= n:
            suma += j
```

La respuesta de un matematico olimpico seria 1/2 n^2 + 1/2 n. Pese a que es correcto, con la notacion o intenta dar una respuesta mas sencilla. La respuesta esperada es O(n^2), comportamiento cuadractico.

### Ejemplo 3

Encuenesta de diagnostico: cuanto se demora el siguiente programa

```py
    for i <= n:
        for j <= n:
            for k < j:
                suma += k
```

Esto es O(n^3).

## Ideas generales de la complejidad

- Hacer $k$ veces una cosa me toma $T(n)$ es $O(k \cdot T(n))$.
- El tiempo de $T_1(n) + T_2(n)$ es $O(T_1(n) + T_2(n))$. Pero si es ambos son $T(n)$ entonces el tiempo de ambos es $T(2 n)$ pero como $2$ es una constante se convierte a $O(n)$.

Aclaracion importante, las constantes en las complejidades no importan, _se eliminan_ mientras que las variables se mantienen. Por eso en el ejemplo 2 se elimina el $2$, mientras que en la idea general 1 no se elimina el $k$.

El calculo de la complejidad es algo general y puede ser que la constante si varie el tiempo de ejecucion. Esto es si queremos hilar mas fino, pero en general los problemas estan diseñados para que no importe la constante.

### Ejemplo mas dificil

```py
    for i <= n:
        for i <= j:
            if A[i] + A[j] == 6:
                print("Si")
```

Con las reglas que hemos visto, la complejidad seria de $O(n^2)$.

### Como saber si mi codigo pasa dado que se su complejidad

Primero hay que tener en cuenta que un computador hace $10^9$ operaciones por segundo (regla muy general pero sirve).

Veamos en cuanto tiempo cabe un algoritmo de complejidad $O(n^3)$.

| n | Tiempo maximo de ejecucion |
|-------------|----------------- |
| 10 |

### Ejemplo con InsertSort

La idea de InsertSort es que se va a ir ordenando el array de izquierda a derecha. Para esto se va a ir insertando el elemento en la posicion correcta.

```py
    for i <= n:
        for j <= i:
            if A[j] > A[i]:
                A.insert(j, A[i])
                A.pop(i + 1)
                break
```

La complejidad de este algoritmo es $O(n^2)$.

### Ejemplo con MergeSort

La idea de MergeSort es dividir el array en dos partes y ordenar cada una de ellas. Luego se mezclan las dos partes ordenadas.

Supongamos que tenemos los arrays A = [1, 3, 7, 10] y B = [2, 5, 9]. Dado que ambos estan ordenados como ordenarias un array combinado de A con B.

Podemos hacerlo con dos punteros (animacion inded):

AB = [1, 2, 3, 5, 7, 9, 10].

Cual es la complejidad de hacer esto? Es el largo de A mas el largo de B. Supongamos que el largo de A es n y el de B, m entonces la complejidad seria $O(n+m)$.

Ahora que sabemos como juntar dos arrays ordenados podemos ocupar esto para ordenar un arreglo desordenado completamente.

Vamos a tomar dividir a la mitad un arreglo. Luego cada mitad le aplicamos un algoritmo misterioso que los ordenar, y despues de que cada particion esta ordenada podemos ocupar el algoritmo que ya conocemos para regresar el array completo ordenado.

Cual es ese algoritmo misterior? pues el mismo (recursion!!!).

(imagen de mergeSort innded)

En la base de esto estara ordenar el array de un solo elemento, como ya esta ordenado podemos retornar este arreglo como ya ordenado.

EL código en python seria el siguiente

```py
    def mergeSort(A):
        if len(A) == 1:
            return A
        else:
            mid = len(A) // 2
            left = mergeSort(A[:mid])
            right = mergeSort(A[mid:])
            return merge(left, right)

    def merge(left, right):
        result = []
        while len(left) > 0 and len(right) > 0:
            if left[0] < right[0]:
                result.append(left[0])
                left.pop(0)
            else:
                result.append(right[0])
                right.pop(0)
        result += left
        result += right
        return result
```

Ahora veamos cual es su complejidad. Primero recordemos que la complejidad de hacer merge es de $O(n)$ con $n$ el largo del array. Pero cada nivel de subdivision toma $O(n)$, pero cuantos niveles van a haber? en cada paso dividimos el arreglo en 2. Asi que cuantas veces puedo dividir un arreglo en 2 para que quede tama;o 1, pues $n \log_2(n)$.

Asi, el algoritmo de MergeSort tiene complejidad $O(n \log_2(n))$.

Si no sabes que es un logaritmo ver apéndice.

### Las operaciones toman tiempo $O(1)$

Vamos a asumir que las operaciones de $c++$ toman tiempo $O(1)$, esto si nos ponemos muy detallistas no es tan cierto. Para los rangos de tipos de datos que trabajamos los procesadores trabajan muy bien con sus operaciones.

---

## Búsqueda binaria

Generalmente cuando les presentan búsqueda binaria se muestra con el ejemplo de un arreglo ordenado y encontrar un elemento dado.

A = [1, 3, 7, 10, 12, 17, 23]

El algoritmo naive de buscar un elemento seria recorrer el arreglo y ver si el elemento esta. Esto tiene complejidad $O(n)$.

(Animacion inded)

Antes cuando el profesor (Javier Marinkovic) iba al colegio usaban algo llamado diccionarios, que eran libros con palabras ordenadas alfabeticamente. Si querian buscar una palabra. Si por ejemplo queria buscar la palabra "casa" lo que hacia era ir a la seccion de la "C" y veia si donde estaba abierto el libro estaba la palabra "casa". Si no estaba, entonces veia que palabra si estaba "caos" debia ir a una pagina mas a la derecha. Si esta nueva pagina empezaba con la palabra "cosa" debia ir a una hoja a la izquierda y asi...

Regresando al problema orginal la idea de la busqueda binaria es dividir el arreglo en dos partes y ver si el elemento esta en la primera o segunda mitad. Si esta en la primera mitad, entonces la segunda mitad no la consideramos. Si esta en la segunda mitad, entonces la primera mitad no la consideramos. Asi hasta que el elemento este en el arreglo.

(animacion inded)

Su complejidad es $O(\log_2(n))$ ya que consta de cuantas veces puedo dividir el arreglo por 2 en el peor de los casos.

Pero la busqueda binaria es mucho mas general que eso.

### Forma mas generalizada de búsqueda binaria

La busqueda binaria nos ayuda a problemas en donde hay una _pregunta_. Ya sea algo como $x^2 \leq n$ o $x^2+3x \leq n$. Lo importante es que cumpla la propiedad de monotonía.

Veamos que para la pregunta $x^2 \leq n$ revisando los numeros naturales

v v v v v f f ...
0 1 2 3 4 5 6 ...

Sera acaso el 7984 verdadero o falso?

Veamos que dado que a su izquierda hay un valor que es falso entonces 7984 tambien lo es y ademas todos a su derecha tambien.

Volviendo al problema original del arreglo ordenado, podemos definir nuestra pregunta como $P(i) = A[i] \leq x$. Notemos que la pregunta es monótona

1 3 8 16 19
V V V  F  F

Si nosotros encontramos el indice mas grande que cumple esta propiedad, vamos a encontrar en donde debería estar el elemento, si es que esta. El 8 debería estar en el ultimo verdadero.

Supongamos que tenemos nuestra funcion monotona y nos paramos en una posicion que mapea verdadero.

V V V [V] ... V F F F F

Sabemos que el ultimo verdadero estara a la derecha, por lo que podemos restringir nuestra busqueda a la derecha de la posicion en que nos paramos

En cambio si nos paramos en un falso

V V V V ... V F F [F] F

Sabemos que el ultimo verdadero estara a la izquierda, por lo que podemos restringir nuestra busqueda a la izquierda de la posicion en que nos paramos.

### Ejemplo con cuadrado para cuadrados perfectos

Quiero saber si el 1024 es un cuadrado perfecto. Aplicando busqueda binaria podemos armar la pregunta $P(X) := x^2 \leq 1024$.

Por que esta función seria monótona?

Primero necesitamos un rango, requerimos que el valor de la izquierda sea verdadero y el limite de la derecha sea falso. Para eso podemos tomar $x = 1$ y $x = 100$.

Rango de busqueda: [1, 100]

Quiero encontrar el ultimo verdadero.

Veamos el valor de al medio, $x = 50$. Es $50^2 \leq 1024$? No, entonces el ultimo verdadero esta a la izquierda. Acortamos la busqueda a la izquierda.

Rango: [1, 50]

Veamos el valor de al medio, $x = 25$. Es $25^2 \leq 1024$? Si, entonces el ultimo verdadero esta a la derecha. Acortamos la busqueda a la derecha.

[25, 50]

Veamos el valor de al medio, $x = 37$. Es $37^2 \leq 1024$? No, entonces el ultimo verdadero esta a la izquierda. Acortamos la busqueda a la izquierda.

[25, 37]

Veamos el valor de al medio, $x = 31$. Es $31^2 \leq 1024$? Si, entonces el ultimo verdadero esta a la derecha. Acortamos la busqueda a la derecha.

y asi...

Esto terminaria en que nuestro rango se acota a [32, 32], verificamos y tenemos que $32^2 = 1024$ por lo que si es un cuadrado perfecto.

### Complejidad de busqueda binaria general

Cuantas veces puedo dividir nuestro rango a la mitad? $O(\log_2(n))$.


## Ejemplo real de la hamburguesa

Resumen:

Polycarpo queria hacer hamburguesas y tiene la receta de la hamburguesa, que es BBHLB. Ademas sabe cuanto cuesta el pan, la hamburguesa y la lechuga. Se sabe cuantos de estos ingredientes tiene ya en su casa. Dado el dinero que tiene la pregunta es cuanta hamburguesas puede hacer Polycarpo ajustandose a su presupuesto.

Definamos la pregunta Q(n) := puede Polycarpo hacer $n$ hamburguesas?

Primero veamos que es monótona. Claramente si puede hacer 50 hamburguesas entonces tambine puede hacer 49. Por otro lado, si no puede hacer 1 trillon de hamburguesas entonces tampoco puede hacer 1 trillon + 1 hamburguesas.

Ahora respondamos la pregunta para $n$.

Los datos son:
- $R_B, R_H, R_L$, la cantidad de ingredientes que necesita para hacer una hamburguesa.
- $\$B, \$H, \$L$, el costo de cada ingrediente.
- $#B, #H, #L$, la cantidad de ingredientes que tiene Polycarpo.

El dinero que se gasta en panes para hacer $n$ hamburguesas es $(n \cdot R_B - #B) \$B$, siempre y cuando sea positivo, caso contrario seria $0$ ya que polycarpo tiene suficientes panes. Notemoslo como $N_B(n) = \max(0, (n \cdot R_B - #B) \$B)$. Lo mismo para la lechuga $N_L(n)$ y la hamburguesa $N_H(n)$.

El dinero total necesario es $N_B(n) + N_L(n) + N_H(n)$. Si este es menor o igual al dinero que tiene Polycarpo entonces podemos decir que si puede hacer $n$ hamburguesas.

La pregunta se reduce a $N_B(n) + N_L(n) + N_H(n) \leq P$. Y podemos ocupar busqueda binaria.

### Complejidad del problema

Calcular $N_B(n) + N_L(n) + N_H(n) \leq P$ es solo hacer operaciones basicas por lo que es $O(1)$ y por busqueda binaria haremos la verificacion a lo mas $\log_2(n)$ veces, con $n$ el rango largo del rango de busqueda. Para este problema podriamos haber usado $[0, P]$.

## Libreria Estandar de `C++` (STL)

Cuando ocupan vectores en `C++` ocupan la libreria estandar de `C++` (STL). Quizas para ocupar vectores ocupas a;adir la linea `#include <vector>` al principio de tu codigo. Podemos importar todas las herramientas que trae la libreria estandar de `C++` con `#include <bits/stdc++.h>`.

Lo que nos interesa es aprovechar las estructuras que trae consigo la STL.

### Problema del pony

(resumen del problema inded)

Se podia resolver metiendo todas las cartas en el arreglo, ordenando el arreglo y luego buscando la carta que queríamos.

Pero podemos ocupar un `set` que es una estructura que viene en la STL.

`set<int> cartas;`

Lo que hace un `set` es insertar un elemento $x$

`cartas.insert(x);` $O(\log(n))$

Y también hacer preguntas del tipo: esta $y$ en el `set`?

`cartas.count(y)` $O(\log(n))$

Y borrar un elemento $x$ del `set`.

`cartas.erase(x)` $O(\log(n))$

Importante 1: el set no te permite elementos repetidos. Para tener elementos repetidos ocupas un `multiset`. Sin enmbargo (Marinkvich) no lo recomienda y es preferible usar mapas.

Importante 2: Valores comparables. Para ocupar un set siempre tiene que haber un sentido de orden. Por eso se pueden hacer sets de `string` o `vectores`.

Importante 3: No podemos acceder a una posicion del set por indice.

### Map

El `map` es el hijo del `vector` y el `set`. En un vector tenemos hartos valores y podemos acceder a ellos por su indice. En un set tenemos hartos valores y podemos preguntar si un valor esta en el set.

En el mapa podemos tener hartos valores y podemos preguntar si un valor esta en el mapa y podemos acceder a ellos por su llave. Esta llave, a diferencia del vector que debe ser desde el $0$, puede ser cualquier valor (hasheable) que queramos.

Definimos el mapa

`map<int, int> mapa;`

Asociamos una llave $x$ a un valor $y$

`mapa[1] = 2;`

Accedemos al valor asociado a la llave $x$

`mapa[1];`

#### Como contar cosas con un mapa

Supongamos que tenemos un arreglo de $n$ elementos y queremos saber cuantas veces esta cada elemento. Podemos tener un mapa `M` que te responda a `M[x]` cuantas veces esta el elemento `x`.

El mapa tiene las mismas operaciones que el `set`. `M.count(x)` responde `0` no existe la llave `x` y `1` si existe la llave `x`.

Todas sus operaciones son $O(\log(n))$.

#### Las posibilidades estan en su imaginacion

Podemos tener arreglos que accedamos por strings y que regresa un vector de enteros.

### Problema del pony 2

(resumen del problema inded)

Ta cambio la carta $x$ por la carta $y$?

Podemos ocupar un mapa que cuenta las veces que se tiene cada carta.

### `Queue`

Fila en ingles. Nos permite simular una fila. No podemos usar un `vector` para esto ya que este no nos permite sacar elementos del principio del arreglo.

`queue<int> fila;`

A;ade un elemento

`fila.push(x)` $O(1)$

Elimina el elemento del principio

`fila.pop()` $O(1)$

Accede al elemento del principio

`fila.front()` $O(1)$

#### Problema de registro IOI

(resumen del problema inded)

Resuelve este problema de manera directa. Ocupa una cola para simular la fila de personas.

### `Stack`

Pila en ingles. Nos permite simular una pila. Donde lo que sacamos es el primer elemento y lo que a;adimos es al incio de la pila. Como una pila de platos.

`stack<int> pila;`

A;ade un elemento al inicio

`pila.push(x)` $O(1)$

Elimina el elemento del principio

`pila.pop()` $O(1)$

Accede al elemento del principio

`pila.top()` $O(1)$

### `Deque`

Es una cola doble. Nos permite sacar y quitar elementos del principio y del final.

`deque<int> cola;`

A;ade un elemento al final

`cola.push_back(x)` $O(1)$

A;ade un elemento al principio

`cola.push_front(x)` $O(1)$

Elimina el elemento del final

`cola.pop_back()` $O(1)$

Elimina el elemento del principio

`cola.pop_front()` $O(1)$

Accede al elemento del final

`cola.back()` $O(1)$

Accede al elemento del principio

`cola.front()` $O(1)$

Nota: el `deque` pese a ser muy util muchas veces solo queremos ocupar la utilidad de una cola o una pila. Ademas que la `deque` es menos eficiente.

### `Priority Queue`

Es una cola de prioridad. Nos permite tener una cola la cual el primer elemento es siempre el valor mas grande. Por defecto esta ordenada de menor a mayor segun el comparador por defecto.

`priority_queue<int> cola;`

A;ade un elemento

`cola.push(x)` $O(\log(n))$

Elimina el elemento del principio

`cola.pop()` $O(\log(n))$

Accede al elemento del principio

`cola.top()` $O(1)$

#### Comparar de menor a mayor

Podemos ocupar un comparador personalizado para que la cola de prioridad ordene de mayor a menor.

`priority_queue<int, vector<int>, greater<int>> cola;`

El `int` se puede cambiar por la clase/tipo de dato de la cola.

## Algoritmos ya implementados

- `sort`: ordena un vector. $O(n\log(n))$ Explicar mas a detalle.

- `lower_bound`: le pasas un vector y un valor y te dice la posicion del primer elemento mayor o igual al valor. Si no existe te dice la posicion del primer elemento mayor al valor. Hace una busqueda binaria.

- `upper_bound`: le pasas un vector y un valor y te dice la posicion del primer elemento mayor al valor. Si no existe te dice la posicion del primer elemento mayor al valor. Hace una busqueda binaria.

- `find`: le pasas un vector y un valor y te dice la posicion del primer elemento igual al valor. Si no existe te dice la posicion del primer elemento mayor al valor. Hace una busqueda lineal.
