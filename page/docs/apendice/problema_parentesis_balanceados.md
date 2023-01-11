# Problema paréntesis balanceados

Dado que consiste de sólo paréntesis determine si la expresión está balanceada o no. Se entiende por paréntesis balanceado que cada paréntesis `(` tiene su par `)` correspondiente que lo cierra a si derecha.

Por ejemplo `(()())` es balanceado, pero `(()` no.

La solución a este problema es con una pilas. Se recorre la expresión y se van guardando los paréntesis que se van encontrando. Si se encuentra un paréntesis de cierre `)` se saca el último paréntesis de la pila y se verifica que sea de apertura `(`. Si no es así, la expresión no está balanceada. Si la pila está vacía y se encuentra un paréntesis de cierre, la expresión no está balanceada. Al final, si la pila está vacía, la expresión está balanceada.

```cpp
bool balanceado(string s){
    stack<char> pila;
    for(char c : s){
        if(c == '('){
            pila.push(c);
        }else{
            if(pila.empty()){
                return false;
            }
            char d = pila.top();
            pila.pop();
            if(d != '('){
                return false;
            }
        }
    }
    return pila.empty();
}
```
