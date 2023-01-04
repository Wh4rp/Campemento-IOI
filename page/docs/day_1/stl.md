# Librería Estandar de `C++` (STL)

Cuando ocupan vectores en `C++` ocupan la librería estándar de `C++` (STL). Quizás para ocupar vectores ocupas añadir la línea `#include <vector>` al principio de tu código. Podemos importar todas las herramientas que trae la librería estándar de `C++` con `#include <bits/stdc++.h>`.

Lo que nos interesa es aprovechar las estructuras que trae consigo la STL.

## Problema del pony

<!-- (resumen del problema inded) -->

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
