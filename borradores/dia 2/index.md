# Temario

- Discusión de problemas del contest anterior
- Grafos

## Problemas

### C - Problema de la deque

Idea central: primero se debe precomuptar hasta el movimiento 10^5, y luego ver que lo siguiente va a formar un ciclo ya que todo esta ordenado.

Cada $n-1$ operaciones se forma un ciclo y el orden regresa al inicio. Por lo que la formula queda `deque[(q-1)%(n-1)]`.

Al principio podría haberse ocurrido hacer una deque y simular todo, pero esto no entra en tiempo debido a que se debe simular hasta el movimiento 10^17+5 que no entra en el tiempo.

### B - Problema del trabajo

Optimizar el tiempo de trabajo que es $x + \ceil{\frac{d}{x+1}} \leq n$. Buscar el dia minimo en el que se puede hacer el trabajo.

Explicar busqueda de cuando deja de barjar o empieza a subir la funcion que es convexa.

$$
P(x) := \text{es } f(x+1) > f(x)
$$

La gracia es que este punto, va a ser el minimo y por ende el menor valor de la funcion posible.

(mostrar codigo de busqueda binaria, mostrar el de Pablo >:c)

Nota de no usar `ceil` y ocupar la funcion propia de ceil con division.

```py
def f(x):
    x + (d+x)/(x+1)

def p(x):
    return f(x+1) > f(x)

def binary(n):
    n, d
    left = 0
    right = n
    while(left < right):
        mid = (left + night) / 2
        if p(mid):
            right = mid
        else:
            left = mid + 1
```

### D - Adivina el numero

Idea central: usar un left y un right igual a la busqueda binaria e ir actualizando segundo el input. Si se cae en algun momento printear que es imposible.

### E - Problema de los cables

Idea central: ocupar el problema tipico de los parentesis balanceados (hacer apendice). Usar stack.

### F - Score

Lo mas importante era explicar bien el problema. Explicar código...

## Grafos

Zzz...

Que es un grafo? es una forma de conectar cosas entre si.

Poner ejemplo de los puentes y la islas. Y explicar su representacion.

Grafo simple es que van a ver.

Grafos con dirigidos y no dirigidos.

Camino: secuencia de nodos (distintos) unidos por aristas.

Largo del camino: cantidad de aristas.

Distancia: largo del camino mas corto entre dos nodos.

Grafo conexo: si existe un camino entre todos los nodos. Desde cualquier nodo se puede llegar a cualquier otro.

Un ciclo (simple) es un camino que empieza y termina en el mismo nodo.

Un árbol es un grafo conexo sin ciclos. Y tiene ciertas propiedades. (ponerlas)

### Representación

#### Matriz de adyacencia

<!-- Poner imagen de la matriz de adyacencia. -->

Las ventajas de usar esta representacion es que es facil de implementar y es facil de ver si dos nodos estan conectados. Pero es ineficiente en memoria y en tiempo.

Las desventajas son ver los vecinos de un nodo, y ver si dos nodos estan conectados. Y en memoria siempre ocupa $\mathcal{O}(|V|^2)$.

#### Lista de adyacencia

Es la representacion mas comun. Es una lista de listas. Donde cada lista es la lista de vecinos de un nodo. Se lista para cada nodo, sus vecinos.

<!-- Poner imagen de la lista de adyacencia. -->

Implementacion:

```cpp
vector<vector<int>> adj;

adj.assign(n, {});

int u, v;
adj[u].push_back(v);
adj[v].push_back(u); // si no es dirigido
```

Las ventajas son que obtener los vecinos de un nodo es facil, y ocupa menos memoria. Ademas el espacio de memoria $\mathcal{O}(|V| + |E|)$.

La desventaja es que saber si dos nodos conectados es de complejidad lineal. Y pasa lo mismo con eliminar una arista.

### Grafos implicitos

Algunas veces la estructura de un problema tiene asociado un grafo implicito. Por ejemplo, el problema del laberinto.

### Algoritmos de busqueda

#### BFS

BFS es un algoritmo de busqueda en grafos. Se usa para encontrar el camino mas corto entre dos nodos.

Se usa una cola para ir guardando los nodos que se van a visitar. Y se va visitando cada nodo y se van guardando los vecinos en la cola.

```cpp
vector<vector<int>> adj;
vector<int> dist;
queue<int> q;

dist.assign(n, -1);
dist[s] = 0;
q.push(s);

while(!q.empty()){
    int u = q.front();
    q.pop();
    for(int v : adj[u]){
        if(dist[v] == -1){
            dist[v] = dist[u] + 1;
            q.push(v);
        }
    }
}
```

Si queremos recuperar el camino, se puede guardar el padre de cada nodo y asi ir recuperando el camino.

```cpp
vector<vector<int>> adj;
vector<int> dist, parent;
queue<int> q;

dist.assign(n, -1);
parent.assign(n, -1);
dist[s] = 0;
q.push(s);

while(!q.empty()){
    int u = q.front();
    q.pop();
    for(int v : adj[u]){
        if(dist[v] == -1){
            dist[v] = dist[u] + 1;
            parent[v] = u;
            q.push(v);
        }
    }
}

vector<int> path;
for(int v = t; v != -1; v = parent[v]){
    path.push_back(v);
}
```

Notas:

- Su complejidad es $\mathcal{O}(|V| + |E|)$. Porque se para en cada nodo y se revisan todas las aristas dos veces, una por cada extremo de la arista.
- Con BFS podemos calcular la distancia de un nodo a todos los demás nodos.

Encontrar el diametro de un grafo.

<!-- explicar como hacer con dos BFS -->

### DFS

DFS es un algoritmo de busqueda en grafos. Se usa para encontrar el camino mas corto entre dos nodos.

Podriamos ocupar la misma implementacion del BFS, salvo que en vez de una cola deberiamos ocupar una stack (mas o menos).

<!-- Colocar imagen de un DFS -->
