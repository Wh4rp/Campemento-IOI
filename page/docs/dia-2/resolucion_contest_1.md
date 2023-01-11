# Resolución problemas del contest 1

Las explicaciones de los problemas presentados son los conversados en este día y en el orden en que se mostraron. No están todos los problemas del contest.

## [C - Valeriy and Deque](https://codeforces.com/problemset/problem/1180/C){target=_blank}

Idea central: primero se debe precomputar hasta el movimiento $10^5$, y luego notar que las siguientes operaciones van a formar un ciclo de largo $n-1$ ya que todo esta ordenado.

Si la consulta es una operación menor que $10^5$ se responde con el resultado guardado, caso contrario podemos ocupar la fórmula `deque[(q-1)%(n-1)]`.

Al principio podría haberse ocurrido hacer una deque y simular todo, pero esto no entra en tiempo debido a que se debe simular hasta el movimiento $10^17+5$ que no entra en el tiempo.

## [B - Deadline](https://codeforces.com/problemset/problem/1288/A){target=_blank}

Para este problema hay que notar que va a haber un rango de $x$ en donde hacer usar un día más para optimizar su programa baja su tiempo de trabajo. Es decir

$$
x + \left \lceil \frac{d}{x+1} \right \rceil \leq (x + 1) + \left \lceil \frac{d}{x+2} \right \rceil
$$

pero va a llegar un momento en el que usar un día más, no va a baja el tiempo de trabajo, sino que lo aumenta. Es decir

$$
x + \left \lceil \frac{d}{x+1} \right \rceil > (x + 1) + \left \lceil \frac{d}{x+2} \right \rceil
$$

Ese punto de quiebre, en el que le deja de convenir pasarse un día más optimizando su trabajo va a coincidir con el mínimo de la función $f(x) = x + \ceil{\frac{d}{x+1}}$. Por lo que podemos ocupar una búsqueda binaria para encontrar ese punto.

```cpp
int f(int x) {
    return x + (d+x)/(x+1);
}

bool p(int x) {
    return f(x+1) > f(x);
}

int binary(int n) {
    int left = 0;
    int right = n;
    while(left < right) {
        int mid = (left + right) / 2;
        if(p(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

Notemos que en vez de usar la función `ceil` de `c++` podemos ocupar el viejo truco de sumarle el denominador al numerador y dividir entre el denominador más uno. Es decir

$$
\left \lceil \frac{a}{b} \right \rceil = \frac{a+b-1}{b}
$$

Es mejor ocupar esta fórmula ya que con `ceil` corremos el riesgo de presentar un error de redondeo.

## [D - Guess a number!](https://codeforces.com/problemset/problem/416/A){target=_blank}

La idea de este problema es usar el inside de búsqueda binaria. Definimos una variable left y right, y vamos actualizando el rango a medida que nos van dando las respuestas. Si en algún punto el rango pierde coherencia (left > right) entonces el número no existe.

## [E - Alternating Current](https://codeforces.com/problemset/problem/343/B){target=_blank}

Mirando el cable desde el inicio, no se puede desenredar si en la secuencia en algún momento hay un `+` que no se pareó con un `-`. Es decir, cables del estilo `-++`, acá el primer `+` no se puede desenredar. Por lo que si en algún momento hay un `+` que no se pareó con un `-` entonces la respuesta es `No`.

Para este problema se puede usar una pila para simular el proceso. Si el caracter actual es igual al último caracter de la pila, entonces se saca el último caracter de la pila, caso contrario se agrega el caracter actual a la pila. Es básicamente el problema de los [paréntesis balanceados](../apendice/problema_parentesis_balanceados.md).

## [F - Winner](https://codeforces.com/problemset/problem/2/A){target=_blank}

Para este problema se puede usar un map para guardar los puntajes de cada jugador. Mientras se van obteniendo los puntajes parciales, agregan a un vector el nombre del jugador unido al puntaje parcial que tenía en aquel momento.

Luego se revisa cuántos jugadores obtuvieron el mayor puntaje. Si no hay empate entonces se imprime el nombre del jugador que tiene el puntaje mayor. Si hay empate entonces se recorre la cola hasta obtener el primer jugador cuyo puntaje parcial fue al menos igual al mayor puntaje y este obtuvo un puntaje final ganador, y se imprime su nombre.
