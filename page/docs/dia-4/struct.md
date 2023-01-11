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