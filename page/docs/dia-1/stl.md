# Librería Estándar de `C++` (STL)

Cuando ocupan vectores en `C++` están usando la librería estándar (STL). Quizás para ocupar vectores añades la línea `#include <vector>` al principio de tu código. Podemos importar todas las herramientas que trae la librería estándar de `C++` (vectores incluidos) con `#include <bits/stdc++.h>`.

Lo que nos interesa es aprovechar las estructuras que trae consigo la STL.

## Set

Set, conjunto en inglés, es una estructura que viene en la STL. Set es un conjunto en el cual añadir elementos y preguntar si un elemento está en el conjunto toma tiempo $\mathcal{O}(\log(N))$.

```cpp
set<int> cartas;  // crea un set vacio, en vez de int
                  // puedes poner otros tipos de datos
                  // ver importante 2
cartas.insert(x); // inserta el elemento x
cartas.count(x);  // pregunta si x esta en el set
                  // regresa 0 si no esta y 1 si esta
cartas.erase(x);  // borra el elemento x
```

Importante 1: el set no permite elementos repetidos. Para tener elementos repetidos ocupas un `multiset`. Sin embargo (Marinkvich) no lo recomienda, en vez de eso puedes ocupar un `map` que es otra estructura que viene en la STL.

Importante 2: Sólo se permiten sets de valores comparables. El tipo de dato con el cual se crea el set debe tener un sentido de orden. Se pueden hacer sets de `string` o `vectores`.

Importante 3: No podemos acceder a una posición del set por indice.

### Problema de Cartas de Pony

Resumen: Javier tiene $N$ cartas de un juego de cartas que pueden tener el valor de $1$ al $10^9$. Tú tarea es responser una cantidad $M$ (de a lo más $10^5$) preguntas, responder si una carta está en el mazo o no.

<!-- (resumen del problema inded) -->

#### Solución con búsqueda binaria

Se podia resolver metiendo todas las cartas en un arreglo o vector, ordenando el arreglo y luego buscar cada carta con una búsqueda binaria. Esto es $\mathcal{O}(N\log(N))$.

#### Solución con set

Otra solución es ocupar un `set`. Guardamos los valores de las cartas en un set y luego preguntamos si cada carta está en el set.

```cpp
set<int> cartas;
for (int i = 0; i < N; i++) {
  int x;
  cin >> x;
  cartas.insert(x);
}

int M;
cin >> M;
for (int i = 0; i < M; i++) {
  int x;
  cin >> x;
  if (cartas.count(x)) {
    cout << "SI" << endl;
  } else {
    cout << "NO" << endl;
  }
}
```

## Map

El `map` es el hijo del `vector` y el `set`. En un vector tenemos hartos valores y podemos acceder a ellos por su indice. En un set tenemos hartos valores y podemos preguntar si un valor está o no en el set.

En el mapa podemos tener hartos valores, podemos preguntar si un valor esta en el mapa y podemos acceder a ellos por su llave. Esta llave, a diferencia del vector que debe ser desde el $0$, puede ser cualquier valor (hasheable) que queramos.

```cpp
map<int, int> mapa; // crea un mapa vacio
                    // en vez de int, int puedes poner
                    // cualquier tipo de dato

mapa[x] = y;        // asigna el valor y a la llave x
mapa[x];            // regresa el valor asociado a la llave x
mapa.count(x);      // pregunta si la llave x esta en el mapa
                    // regresa 0 si no esta y 1 si esta
mapa.erase(x);      // borra la llave x y su valor asociado
```

### Cómo contar cosas con un mapa

Supongamos que tenemos un arreglo de $n$ elementos y queremos saber cuantas veces está cada elemento. Podemos tener un mapa `M` que el valor de `M[x]` sea cuantas veces está el elemento `x`.

`M.count(x)` responde `0` no existe la llave `x` y `1` si existe la llave `x`.

Todas sus operaciones son $O(\log(n))$.

#### Las posibilidades están en tu imaginación

Podemos tener arreglos que accedamos por strings y que regresa un vector de enteros.

### Problema del pony 2

Resumen: Javier ahora quiere intercambiar cartas con Martín. Te piden verificar si una cierta cantidad intercambio de cartas son posible. Para ello, tienes que responder si es posible intercambiar la carta $x$ por la carta $y$. Se puede intercambiar siempre y cuando Javier disponga de la carta $x$. Te dan la lista de cartas que tiene Javier que pueden ser números del $1$ al $10^9$.

#### Solución con map

Podemos ocupar un mapa que cuenta las veces que se tiene cada carta.

```cpp
map<int, int> cartas;
for (int i = 0; i < N; i++) {
  int x;
  cin >> x;
  cartas[x]++;
}

int M;
cin >> M;
for (int i = 0; i < M; i++) {
  int x, y;
  cin >> x >> y;
  if (cartas.count(x) && cartas[x] > 0) {
    cout << "SI" << endl;
    cartas[x]--;
    cartas[y]++;
  } else {
    cout << "NO" << endl;
  }
}
```

## Queue

Fila en inglés. Nos permite simular una fila. No conviene usar un `vector` ya que este no nos permite sacar elementos del principio.

```cpp
queue<int> fila; // crea una fila vacia
fila.push(x);    // añade un elemento al final
fila.pop();      // elimina el elemento del principio
fila.front();    // regresa el valor elemento del principio
fila.empty();    // pregunta si la fila esta vacia
                 // regresa false si no esta vacia y true si esta vacia
```

### Problema de registro IOI

Resumen: Hay una fila que tiene que ser atendida. Existen dos eventos, la llegada de una persona y que Nacho atienda la inscripción. En el primer caso está persona se añade a la cola. En el segundo si no hay nadie en la cola se imprime un `1` y si hay alguien se imprime el número de la persona que está siendo atendida y se elimina de la cola.

#### Solución con queue

Resuelve este problema de manera directa usando un `queue`.

```cpp
queue<int> fila;
int N;
cin >> N;
for (int i = 0; i < N; i++) {
  string s;
  int x;
  cin >> s >> x;
  if (s == "A") {
    fila.push(x);
  } else {
    if (fila.empty()) {
      cout << 1 << endl;
    } else {
      cout << fila.front() << endl;
      fila.pop();
    }
  }
}
```

## Stack

Pila en ingles. Nos permite simular una pila. Donde lo que sacamos es el primer elemento y añadimos elementos es al inicio de la pila. Como una pila de platos.

```cpp
stack<int> pila; // crea una pila vacia
pila.push(x);    // añade un elemento al principio
pila.pop();      // elimina el elemento del principio
pila.top();      // regresa el valor elemento del principio
pila.empty();    // pregunta si la pila esta vacia
                 // regresa false si no esta vacia y true si esta vacia
```

## Deque

Es una cola doble. Nos permite sacar y quitar elementos del principio y del final.

```cpp
deque<int> cola; // crea una cola vacia
cola.push_back(x);    // añade un elemento al final
cola.push_front(x);   // añade un elemento al principio
cola.pop_back();      // elimina el elemento del final
cola.pop_front();     // elimina el elemento del principio
cola.back();          // regresa el valor elemento del final
cola.front();         // regresa el valor elemento del principio
cola.empty();         // pregunta si la cola esta vacia
                      // regresa false si no esta vacia y true si esta vacia
```

Todo lo hace en $\mathcal{O}(1)$.

Nota: el `deque` pese a ser muy útil, muchas veces basta con sólo ocupar una cola o una pila. Además que la `deque` es menos eficiente.

## Priority Queue

Es una cola de prioridad. Nos permite tener una cola la cual el primer elemento es siempre el valor mas grande. Por defecto esta ordenada de menor a mayor según el comparador por defecto.

```cpp
priority_queue<int> cola; // crea una cola vacia
cola.push(x);    // añade un elemento
cola.pop();      // elimina el elemento del principio
cola.top();      // regresa el valor elemento del principio
cola.empty();    // pregunta si la cola esta vacia
                 // regresa false si no esta vacia y true si esta vacia
```

Los costos de hacer `push`, `pop` y `top` son $\mathcal{O}(\log(n))$. Mientras que `top` es $\mathcal{O}(1)$.

### Comparar de menor a mayor

Podemos ocupar un comparador personalizado para que la cola de prioridad ordene de mayor a menor.

```cpp
priority_queue<int, vector<int>, greater<int>> cola;
```

El `int` se puede cambiar por la clase/tipo de dato de la cola, ejemplo `greater<pair<int, int>>`.

## Algoritmos ya implementados en la STL

- `sort`: ordena un vector de menor a mayor. El costo es $\mathcal{O}(n \log(n))$.

```cpp
sort(v.begin(), v.end()); // ordena el vector v
```

- `lower_bound`: le pasas un vector y un valor y el iterador que apunta al primer elemento mayor o igual al valor. Si no existe retorna el iterador al final del vector. Hace una búsqueda binaria.

```cpp
auto pos = lower_bound(v.begin(), v.end(), x) - v.begin();
if (pos == v.end()) {
  // no existe
} else {
  if (*v == x) {
    // x esta en v
  } else {
    // hay un elemento mayor a x
  }
}
```

- `upper_bound`: lo mismo que `lower_bound` pero te regresa el primer elemento mayor al valor. Si no existe retorna el iterador al final del vector. Hace una búsqueda binaria.

```cpp
auto pos = upper_bound(v.begin(), v.end(), x) - v.begin();
```

- `find`: le pasas un vector y un valor y regresa el iterador al primer elemento igual al valor. Si no existe retorna el iterador al final del vector. Hace una búsqueda lineal.

```cpp
auto pos = find(v.begin(), v.end(), x) - v.begin();
if (pos == v.end()) {
  // no existe
} else {
  // existe
}
```

## Referencias de la STL

- [C++ Reference](http://www.cplusplus.com/reference/)
- [Cpp Reference](https://en.cppreference.com/w/)
