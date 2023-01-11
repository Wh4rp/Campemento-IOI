# Grafos 1

¿Qué es un grafo? Es una forma representar _conexiones_ entre objetos. Por ejemplo, las relaciones de amistad entre personas.

El problema real que originó la teoría de grafos fue el [Problema de los puentes de Königsberg](https://es.wikipedia.org/wiki/Problema_de_los_puentes_de_K%C3%B6nigsberg){target="_blank"}. Que consiste en encontrar un camino que pase por todos los puentes de la ciudad de Königsberg. Pero no puede pasar por el mismo puente dos veces.

=== "Imagen de los puentes"

    ![Königsberg](../src/K%C3%B6nigsberg.png)

=== "Vista con grafos"

    ![Königsberg_grafos](../src/K%C3%B6nigsberg_grafos.png)

Un grafo consiste de un conjunto de nodos y un conjunto de aristas. Las aristas conectan (relacionan) dos nodos.

![Grafo](../src/grafo_ejemplo.png)

En el campamento sólo vamos a ver grafos simples. Es decir, que no contienen _lopps_ (aristas que conectan a un nodo consigo mismo) ni _aristas paralelas_ (más de una arista que conectan el mismo par de nodos).

Las aristas pueden ser dirigidas o no dirigidas. Si son dirigidas, la arista va de un nodo a otro. Si no son dirigidas, la arista va de un nodo a otro y viceversa.

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
