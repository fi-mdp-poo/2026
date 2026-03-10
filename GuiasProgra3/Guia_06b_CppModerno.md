# Guía de Trabajos Prácticos - Unidad 6b
## Materia: Programación 3
### C++ Moderno: Templates, STL, Smart Pointers, Move Semantics y Lambdas

> Esta guía cubre las características del C++ moderno (C++11 en adelante) introducidas en la Unidad 6 extendida. Los temas se presentan en orden de complejidad creciente. Se recomienda resolver los ejercicios de cada sección antes de avanzar a la siguiente.
>
> **Temas cubiertos:**
> 1. Templates de funciones y clases (Ej. 1.1–1.6)
> 2. Standard Template Library — vector, list, map, set, stack, queue, pair, complex, algoritmos (Ej. 2.1–2.10)
> 3. Punteros inteligentes — `unique_ptr`, `shared_ptr`, `weak_ptr` (Ej. 3.1–3.6)
> 4. Semántica de movimiento — lvalues, rvalues, `std::move`, Regla de los Cinco (Ej. 4.1–4.5)
> 5. Funciones lambda — captura, `std::function`, lambdas genéricas (Ej. 5.1–5.7)

---

## Sección 1 — Templates

### Preguntas orientadoras

1. ¿Qué problema resuelven los templates en C++? ¿Cuál es la diferencia entre sobrecarga de funciones y templates?
2. ¿Cuándo se genera el código concreto de un template? ¿En tiempo de compilación o de ejecución?
3. ¿Qué diferencia hay entre `template <class T>` y `template <typename T>`?
4. ¿Puede un template de función tener más de un parámetro de tipo? ¿Y parámetros que no sean tipos (non-type parameters)?
5. ¿Qué es la especialización de templates? ¿Para qué sirve?
6. ¿Puede una clase template heredar de otra clase (no template)? ¿Y de otra clase template?

---

### Ejercicios

#### Ejercicio 1.1 — Template de función básico

Implemente una función template `maximo` que reciba dos valores del mismo tipo y retorne el mayor. Pruébela con `int`, `double` y `std::string`.

```cpp
// Ejemplo de uso esperado:
std::cout << maximo(3, 7) << "\n";          // 7
std::cout << maximo(3.14, 2.71) << "\n";    // 3.14
std::cout << maximo(std::string("hola"), std::string("mundo")) << "\n";  // mundo
```

**Extensión:** Agregue una versión con dos parámetros de tipo distinto (`T` y `U`) que retorne el mayor convertido al tipo del primero.

---

#### Ejercicio 1.2 — Templates con múltiples tipos

Implemente las siguientes funciones template:

a) `void intercambiar(T& a, T& b)` — intercambia dos valores del mismo tipo sin usar variables temporales adicionales (puede usar `std::move`).

b) `T sumatoria(const std::vector<T>& v)` — retorna la suma de todos los elementos de un vector. Pruébela con `int`, `double` y `std::string` (concatenación).

c) `bool contiene(const T arr[], int n, const T& valor)` — retorna `true` si `valor` está en el arreglo `arr` de `n` elementos.

---

#### Ejercicio 1.3 — Clase template `CPila<T>`

Implemente una clase template `CPila<T>` que represente una pila (LIFO) genérica. La clase debe usar internamente un `std::vector<T>` y proveer:

| Método | Descripción |
|--------|-------------|
| `void push(const T& valor)` | Agrega un elemento al tope |
| `void pop()` | Elimina el elemento del tope (lanza excepción si vacía) |
| `T& top()` | Retorna referencia al elemento del tope |
| `bool empty() const` | Retorna `true` si la pila está vacía |
| `int size() const` | Retorna la cantidad de elementos |
| `void print(std::ostream& os) const` | Imprime todos los elementos (tope primero) |

Sobrecargue el operador `<<` para que funcione con `cout`.

Pruébela con `CPila<int>`, `CPila<std::string>` y `CPila<double>`.

---

#### Ejercicio 1.4 — Clase template `CPar<T, U>`

Implemente una clase template `CPar<T, U>` que almacena un par de valores de tipos potencialmente distintos. Provea:

- Constructor: `CPar(const T& primero, const U& segundo)`
- Getters: `getPrimero()`, `getSegundo()`
- Método `void intercambiar()` — solo válido si `T == U` (puede implementarlo sin restricción y documentar el requisito)
- Sobrecarga de `<<` para imprimir `(primero, segundo)`
- Sobrecarga de `==` para comparar dos `CPar<T,U>`

Ejemplo de uso:
```cpp
CPar<std::string, int> alumno("García", 95);
CPar<double, double> punto(3.14, 2.71);
std::cout << alumno << "\n";  // (García, 95)
std::cout << punto << "\n";   // (3.14, 2.71)
```

---

#### Ejercicio 1.5 — Función template con función genérica de impresión

Implemente una función template genérica `vPrint` que imprima cualquier contenedor STL a un `std::ostream`:

```cpp
template <class Contenedor>
void vPrint(const Contenedor& c, std::ostream& os, const std::string& separador = ", ");
```

Luego, implemente una sobrecarga que reciba un nombre de archivo y escriba en él:

```cpp
template <class Contenedor>
void vPrint(const Contenedor& c, const std::string& nombreArchivo);
```

Pruébela con `std::vector<int>`, `std::list<std::string>` y `std::map<std::string, int>`.

---

#### Ejercicio 1.6 — Clase template `Vector<T>` con control de rango

Implemente una clase template `Vector<T>` que funcione como un array dinámico seguro. Debe gestionar la memoria manualmente (sin usar `std::vector` internamente) y proveer:

| Método/Operador | Descripción |
|---|---|
| `Vector(int capacidad)` | Constructor: reserva memoria para `capacidad` elementos |
| `~Vector()` | Destructor: libera la memoria |
| `void redimensionar(int nueva_cap)` | Reserva nueva memoria, copia elementos existentes, libera la anterior |
| `T& operator[](int i)` | Acceso con control de rango: lanza `std::out_of_range` si `i` es inválido |
| `const T& operator[](int i) const` | Versión `const` del operador anterior |
| `Vector(const Vector<T>& otro)` | Constructor de copia: **no debe compartir** el array interno |
| `Vector& operator=(const Vector<T>& otro)` | Asignación con gestión correcta de memoria |
| `int size() const` | Retorna la cantidad de elementos almacenados |
| `int capacity() const` | Retorna la capacidad actual |
| `void push_back(const T& val)` | Agrega al final (redimensiona x2 si necesario) |

**Extensión:** Agregue un constructor de copia desde `Vector<U>` donde `U` es convertible a `T`:

```cpp
template <typename U>
Vector(const Vector<U>& otro);  // convierte elemento a elemento
```

Pruebe con `Vector<int>`, `Vector<double>` y la conversión `Vector<int>` → `Vector<double>`.

---

## Sección 2 — Standard Template Library (STL)

### Preguntas orientadoras

1. ¿Cuáles son los tres pilares de la STL? Explique brevemente cada uno.
2. ¿Qué diferencia hay entre un contenedor secuencial y uno asociativo? Dé un ejemplo de cada tipo.
3. ¿Qué es un iterador? ¿Por qué la STL usa iteradores en lugar de índices?
4. ¿Qué hace la palabra clave `typename` en `typename vector<T>::iterator`?
5. ¿Cuál es la diferencia entre `std::vector` y `std::list` en términos de acceso aleatorio e inserción?
6. ¿Qué ocurre si se intenta acceder con `[]` a una clave inexistente en un `std::map`?
7. ¿Para qué sirve `std::sort`? ¿Qué requiere del tipo de los elementos?

---

### Ejercicios

#### Ejercicio 2.1 — Operaciones básicas con `std::vector`

Dado el siguiente vector inicial: `{5, 2, 8, 1, 9, 3, 7, 4, 6}`, realice las siguientes operaciones (cada una partiendo del estado anterior):

a) Imprimir todos los elementos usando un iterador (no índices).
b) Ordenar el vector de menor a mayor e imprimirlo.
c) Ordenar el vector de mayor a menor usando un comparador personalizado e imprimirlo.
d) Eliminar todos los elementos menores que 5 (usar `remove_if` + `erase`).
e) Calcular la suma de todos los elementos con `std::accumulate`.
f) Encontrar el primer elemento mayor que 6 con `std::find_if` e imprimir su posición.

---

#### Ejercicio 2.2 — `std::list` con operaciones de lista

Implemente un programa que maneje una lista de nombres (`std::list<std::string>`) con las siguientes operaciones mediante menú:

1. Agregar nombre al inicio.
2. Agregar nombre al final.
3. Eliminar todas las ocurrencias de un nombre dado.
4. Invertir la lista.
5. Ordenar la lista alfabéticamente.
6. Imprimir la lista con posición de cada elemento.
7. Salir.

---

#### Ejercicio 2.3 — `std::pair` y código ASCII

a) Implemente una función que dado un código ASCII (entero), retorne un `std::pair<char, char>` donde el primer elemento es la versión mayúscula del carácter y el segundo su versión minúscula. Si el código no corresponde a una letra, ambos elementos deben ser el carácter original.

```cpp
std::pair<char, char> mayusMinusc(int ascii);
// mayusMinusc(65) → ('A', 'a')
// mayusMinusc(98) → ('B', 'b')
// mayusMinusc(51) → ('3', '3')
```

b) Use `std::make_pair` explícitamente en la implementación. Muestre la diferencia con construir el par directamente: `{mayusc, minusc}`.

c) Implemente una función que recorra los códigos ASCII del 32 al 126 y cargue en un `std::map<char, std::pair<char,char>>` la tabla completa de mayúsculas/minúsculas. Imprima solo los pares correspondientes a letras.

---

#### Ejercicio 2.4 — Agenda telefónica con `std::map`

Implemente una agenda telefónica usando `std::map<std::string, std::string>` (nombre → teléfono) con las siguientes operaciones:

1. Agregar contacto (verificar si ya existe).
2. Buscar contacto por nombre (exacto).
3. Buscar contactos que comiencen con una subcadena dada.
4. Eliminar contacto.
5. Imprimir todos los contactos ordenados alfabéticamente.
6. Guardar agenda en archivo de texto.
7. Cargar agenda desde archivo de texto.
8. Salir.

---

#### Ejercicio 2.5 — `std::stack` y `std::queue`: simulación

a) **Validador de paréntesis:** Usando `std::stack<char>`, implemente una función que reciba una cadena y verifique si los paréntesis, corchetes y llaves están correctamente balanceados.

```cpp
bool validar(const std::string& expr);
// validar("({[]})") → true
// validar("({[})") → false
// validar("((())") → false
```

b) **Simulador de cola de atención:** Usando `std::queue<std::string>`, simule una cola de turnos con las opciones: agregar persona, atender siguiente, ver quién es el próximo, ver cuántas personas esperan.

---

#### Ejercicio 2.6 — Algoritmos STL con predicados

Dado un `std::vector<Alumno>` donde `Alumno` tiene `nombre` (string) y `promedio` (double):

```cpp
struct Alumno {
    std::string nombre;
    double promedio;
};
```

Realice las siguientes operaciones usando algoritmos STL (`sort`, `find_if`, `count_if`, `remove_if`, `transform`, `for_each`):

a) Ordenar por promedio descendente.
b) Ordenar por nombre alfabético.
c) Contar cuántos alumnos aprobaron (promedio ≥ 6.0).
d) Encontrar el primer alumno con promedio perfecto (10.0).
e) Imprimir solo los nombres de los alumnos que aprobaron.
f) Eliminar del vector a los alumnos con promedio < 4.0.
g) Calcular el promedio general del curso con `std::accumulate`.

---

#### Ejercicio 2.7 — Números primos con `std::vector` y algoritmos

a) Implemente una función `std::vector<int> generarPrimos(int hasta)` que retorne todos los números primos menores o iguales a `hasta`, usando la **Criba de Eratóstenes**. Use `std::vector<bool>` como bitmap auxiliar.

b) Con el vector de primos obtenido, use algoritmos STL para:
   - Contar cuántos primos son menores o iguales a 11.
   - Contar cuántos primos son múltiplos de 5 (respuesta obvia, pero practique `count_if`).
   - Contar cuántos primos son iguales a 2.
   - Encontrar el mayor primo del vector.
   - Calcular la suma de todos los primos usando `std::accumulate`.

c) Use `std::generate` para llenar un `std::vector<int>` de 10 elementos con valores generados por una lambda que produce números del 1 al 10. Luego use `std::transform` para elevar cada elemento al cuadrado en un nuevo vector.

---

#### Ejercicio 2.8 — Vector de números complejos con `<complex>`

La biblioteca estándar provee `std::complex<T>` en `<complex>`. Úsela para:

a) Crear un `std::vector<std::complex<double>>` con los **números complejos de módulo 1** cuyos ángulos son múltiplos de π/3 en el intervalo [0, 2π). Es decir: e^(i·k·π/3) para k = 0, 1, 2, 3, 4, 5.

```cpp
// Pista: std::complex<double> c = std::polar(1.0, angulo);
```

b) Imprima cada número en forma rectangular (`a + bi`) y en forma polar (módulo y fase en grados).

c) Use `std::find_if` para encontrar el primer número con parte real negativa.

d) Use `std::transform` para calcular el cuadrado de cada complejo (multiplicación compleja) y almacene los resultados en un nuevo vector.

e) Verifique que todos los resultados del inciso d) son también de módulo 1 (use `std::all_of`).

---

#### Ejercicio 2.9 — Operaciones entre conjuntos con `std::set`

Incluya `<set>` y `<algorithm>` para trabajar con conjuntos matemáticos.

a) Defina dos `std::set<int>`: `A = {1, 2, 3, 4, 5, 6}` y `B = {4, 5, 6, 7, 8, 9}`.

b) Use `std::set_union`, `std::set_intersection`, `std::set_difference` y `std::set_symmetric_difference` para calcular A∪B, A∩B, A-B y A△B. Almacene cada resultado en un `std::vector<int>` usando `std::back_inserter`.

c) Implemente una función template genérica:
```cpp
template <class T>
void imprimirConjunto(const std::string& nombre, const std::set<T>& s);
```

d) Cargue dos conjuntos de palabras (strings) desde el usuario y muestre cuáles palabras están en ambos conjuntos (intersección) y cuáles son exclusivas de cada uno (diferencia simétrica).

e) Explique por qué `std::set` garantiza elementos únicos y ordenados, y cuándo conviene usar `std::multiset`.

---

#### Ejercicio 2.10 — Función template `vPrintMap` (Nivel Examen)

Retomando el Ejercicio 1.5 (`vPrint`), implemente ahora una función template `vPrintMap` especializada para `std::map<K, V>`, que imprima tanto a un `ostream` como a un archivo:

```cpp
template <class K, class V>
void vPrintMap(std::map<K, V>& mapa, std::ostream& os);

template <class K, class V>
void vPrintMap(std::map<K, V>& mapa, const std::string& nombreArchivo);
```

Luego úsela para manejar dos mapas distintos:
- `std::map<std::string, int>` con nombres de alumnos y sus notas.
- `std::map<int, std::string>` con códigos y nombres de materias.

El operador `<<` de cada tipo de valor (`V`) debe estar definido.

---

## Sección 3 — Punteros Inteligentes

### Preguntas orientadoras

1. ¿Qué problema concreto resuelven los smart pointers respecto a los punteros crudos?
2. ¿Qué es RAII? ¿Cómo lo implementan los smart pointers?
3. ¿Por qué `unique_ptr` no puede copiarse? ¿Qué operación sí puede realizarse?
4. ¿Cuándo se destruye el objeto apuntado por un `shared_ptr`?
5. ¿Qué es un ciclo de referencias? ¿Cómo se rompe con `weak_ptr`?
6. ¿Qué ventaja tiene `make_unique` y `make_shared` sobre `new`?
7. ¿Puede un `unique_ptr` convertirse en `shared_ptr`? ¿Y al revés?
8. ¿Qué significa que un objeto esté en "estado válido pero no especificado" después de ser movido?

---

### Ejercicios

#### Ejercicio 3.1 — Análisis de código: trazas de construcción/destrucción

Dado el siguiente código, indique la salida exacta **sin ejecutarlo**. Justifique el orden en que se imprimen los mensajes:

```cpp
#include <iostream>
#include <memory>

class Recurso {
    std::string nombre;
public:
    Recurso(const std::string& n) : nombre(n) {
        std::cout << "Construyendo: " << nombre << "\n";
    }
    ~Recurso() {
        std::cout << "Destruyendo: " << nombre << "\n";
    }
    void usar() { std::cout << "Usando: " << nombre << "\n"; }
};

int main() {
    std::cout << "--- Bloque 1 ---\n";
    {
        auto p1 = std::make_unique<Recurso>("A");
        auto p2 = std::make_unique<Recurso>("B");
        p1->usar();
    }
    std::cout << "--- Bloque 2 ---\n";
    {
        auto s1 = std::make_shared<Recurso>("C");
        std::cout << "count=" << s1.use_count() << "\n";
        {
            auto s2 = s1;
            std::cout << "count=" << s1.use_count() << "\n";
            s2->usar();
        }
        std::cout << "count=" << s1.use_count() << "\n";
    }
    std::cout << "--- Fin ---\n";
    return 0;
}
```

---

#### Ejercicio 3.2 — Reemplazar punteros crudos con smart pointers

El siguiente código usa punteros crudos y tiene múltiples problemas (memory leaks, posibles double deletes). Reescríbalo usando smart pointers adecuados:

```cpp
class Motor {
public:
    Motor(int cilindros) { std::cout << "Motor(" << cilindros << ")\n"; }
    ~Motor() { std::cout << "~Motor\n"; }
    void encender() { std::cout << "Motor encendido\n"; }
};

class Auto {
    Motor* motor;
public:
    Auto(int cilindros) { motor = new Motor(cilindros); }
    ~Auto() { delete motor; }
    void arrancar() { motor->encender(); }
};

void crearAuto() {
    Auto* a = new Auto(4);
    a->arrancar();
    // Falta: delete a;
}

Motor* crearMotor(int c) {
    return new Motor(c);
}
```

Reescríbalo usando `unique_ptr` y `make_unique`. El `Auto` debe tomar ownership del motor. La función `crearAuto` debe retornar el auto. La función `crearMotor` debe retornar un `unique_ptr<Motor>` que luego se puede mover al constructor de `Auto`.

---

#### Ejercicio 3.3 — `shared_ptr` y reference counting

Implemente un sistema de **recursos compartidos** entre módulos:

```cpp
class ConfiguracionGlobal {
    std::string servidor;
    int puerto;
    int timeoutMs;
public:
    ConfiguracionGlobal(const std::string& srv, int p, int t);
    // getters...
    void print() const;
};
```

- Cree una instancia de `ConfiguracionGlobal` con `make_shared`.
- Pásela a tres "módulos" (funciones o clases) que guarden un `shared_ptr` a la misma configuración.
- Muestre el `use_count()` en cada paso.
- Destruya los módulos de a uno y observe cómo cambia el contador.
- Verifique que el objeto se destruye solo cuando el último módulo se destruye.

---

#### Ejercicio 3.4 — Ciclo de referencias y `weak_ptr`

Implemente una lista doblemente enlazada usando smart pointers. Cada nodo tiene:
- `int valor`
- `shared_ptr<Nodo> siguiente`
- `weak_ptr<Nodo> anterior` ← **debe ser `weak_ptr` para evitar ciclos**

Provea las operaciones:
- `void agregarAlFinal(int valor)`
- `void imprimirHaciaAdelante()`
- `void imprimirHaciaAtras()` (usando los `weak_ptr`)
- `void eliminar(int valor)`

Demuestre con trazas de destrucción que **no hay memory leaks**.

---

#### Ejercicio 3.5 — Sistema de empleados con `unique_ptr` y polimorfismo

Implemente un sistema de recursos humanos:

```
Empleado (abstracta)
├── EmpleadoFijo    (sueldo mensual fijo)
└── EmpleadoPorHora (horas trabajadas × tarifa)
```

- `Empresa` contiene un `std::vector<std::unique_ptr<Empleado>>`.
- Método `contratar(std::unique_ptr<Empleado> emp)`: agrega al vector.
- Método `liquidar()`: imprime nombre y sueldo de cada empleado.
- Método `totalNomina() const`: retorna el total a pagar.
- Use una función factory:

```cpp
std::unique_ptr<Empleado> crearEmpleado(const std::string& tipo, ...);
```

No debe haber ningún `new` ni `delete` visible fuera de `make_unique`.

---

#### Ejercicio 3.6 — Caché con `weak_ptr`

Implemente una clase `CacheImagenes` que almacena imágenes ya cargadas para reutilizarlas sin mantenerlas vivas innecesariamente:

```cpp
class Imagen {
    std::string nombre;
    std::vector<uint8_t> datos;  // Simular datos pesados
public:
    Imagen(const std::string& n);
    ~Imagen();
    void render() const;
};

class CacheImagenes {
    std::map<std::string, std::weak_ptr<Imagen>> cache;
public:
    std::shared_ptr<Imagen> cargar(const std::string& nombre);
    void limpiar();  // Elimina entradas expiradas
    int size() const;
};
```

Demuestre que:
- Cuando alguien tiene la imagen, el caché la reutiliza.
- Cuando nadie la tiene, el caché la recarga desde cero.
- No hay memory leaks.

---

## Sección 4 — Semántica de Movimiento

### Preguntas orientadoras

1. ¿Qué es un lvalue y qué es un rvalue? Dé tres ejemplos de cada uno.
2. ¿Cuál es la diferencia entre una referencia lvalue (`T&`) y una referencia rvalue (`T&&`)?
3. ¿Qué hace `std::move()`? ¿Genera movimiento por sí solo o solo cambia el tipo?
4. ¿Qué es el constructor de movimiento? ¿Cuándo se invoca automáticamente?
5. ¿Cuál es la diferencia entre el constructor de movimiento y el operador de asignación por movimiento?
6. ¿Por qué se marca `noexcept` el constructor de movimiento? ¿Qué le pasa a `std::vector` si no lo hace?
7. Enuncie la Regla de los Cinco. ¿Cuándo se aplica?
8. ¿Qué es RVO (Return Value Optimization)? ¿Por qué no se debe usar `std::move` en un `return`?

---

### Ejercicios

#### Ejercicio 4.1 — Clasificar expresiones como lvalue o rvalue

Para cada expresión, indique si es **lvalue** o **rvalue** y justifique:

```cpp
int x = 5;
int y = 10;
std::string s = "hola";

// a) x
// b) x + y
// c) ++x
// d) x++
// e) std::string("mundo")
// f) s
// g) s + " mundo"
// h) *(&x)
// i) (x > 0 ? x : y)
```

---

#### Ejercicio 4.2 — Análisis de código: copia vs. movimiento

Sin ejecutar el código, indique en cada caso si se invoca el **constructor de copia**, el **constructor de movimiento**, o el **operador de asignación** (copia o movimiento). Justifique:

```cpp
class MiClase {
public:
    MiClase() { std::cout << "Default\n"; }
    MiClase(const MiClase&) { std::cout << "Copia\n"; }
    MiClase(MiClase&&) noexcept { std::cout << "Movimiento\n"; }
    MiClase& operator=(const MiClase&) { std::cout << "Asig. Copia\n"; return *this; }
    MiClase& operator=(MiClase&&) noexcept { std::cout << "Asig. Movimiento\n"; return *this; }
};

MiClase crear() { return MiClase(); }   // (a)

int main() {
    MiClase a;
    MiClase b = a;                      // (b)
    MiClase c = std::move(a);           // (c)
    MiClase d = crear();                // (d)
    b = c;                              // (e)
    b = std::move(c);                   // (f)
    std::vector<MiClase> v;
    v.push_back(MiClase());             // (g)
    v.push_back(a);                     // (h)
    return 0;
}
```

---

#### Ejercicio 4.3 — Implementar la Regla de los Cinco

Implemente una clase `Buffer` que gestiona un arreglo dinámico de `int`:

```cpp
class Buffer {
    int*   datos;
    size_t capacidad;
    size_t tamanio;
public:
    // Constructor con capacidad
    explicit Buffer(size_t cap);
    
    // Destructor
    ~Buffer();
    
    // Constructor de copia (copia profunda)
    Buffer(const Buffer& otro);
    
    // Operador de asignación de copia
    Buffer& operator=(const Buffer& otro);
    
    // Constructor de movimiento (noexcept)
    Buffer(Buffer&& otro) noexcept;
    
    // Operador de asignación por movimiento (noexcept)
    Buffer& operator=(Buffer&& otro) noexcept;
    
    // Métodos útiles
    void push(int valor);
    int pop();
    size_t size() const;
    void print() const;
};
```

Cada constructor/destructor debe imprimir un mensaje identificatorio. Verifique con el siguiente `main` que la semántica es correcta:

```cpp
int main() {
    Buffer b1(10);
    b1.push(1); b1.push(2); b1.push(3);
    
    Buffer b2 = b1;                  // Copia
    Buffer b3 = std::move(b1);      // Movimiento (b1 queda vacío)
    
    b2.print();    // 1 2 3
    b3.print();    // 1 2 3
    b1.print();    // (vacío)
    
    b2 = std::move(b3);             // Asignación por movimiento
    b2.print();    // 1 2 3
    b3.print();    // (vacío)
}
```

---

#### Ejercicio 4.4 — `std::move` con contenedores STL

Analice el rendimiento entre copia y movimiento:

a) Implemente una función `std::vector<std::string> generarNombres(int n)` que genere `n` strings de 1000 caracteres cada uno.

b) Compare las dos formas de pasar el vector a otra función:

```cpp
void procesarCopia(std::vector<std::string> v);   // Por valor (copia)
void procesarMove(std::vector<std::string> v);    // Por valor (pero se mueve)

procesarCopia(nombres);           // Genera copia
procesarMove(std::move(nombres)); // Mueve, nombres queda vacío
```

c) Implemente un `swap` manual usando `std::move`:

```cpp
template<typename T>
void miSwap(T& a, T& b) {
    T temp = std::move(a);
    a = std::move(b);
    b = std::move(temp);
}
```

Verifique que funciona con `std::string`, `std::vector<int>` y `Buffer` del ejercicio anterior.

---

#### Ejercicio 4.5 — Clase con recursos: integración completa

Implemente una clase `Matriz` que almacena una matriz de `double` de forma dinámica:

```cpp
class Matriz {
    double** datos;
    size_t filas;
    size_t columnas;
public:
    Matriz(size_t f, size_t c);            // Constructor
    ~Matriz();                              // Destructor
    Matriz(const Matriz& m);               // Copia profunda
    Matriz& operator=(const Matriz& m);    // Asignación copia
    Matriz(Matriz&& m) noexcept;           // Movimiento
    Matriz& operator=(Matriz&& m) noexcept;// Asignación movimiento
    
    double& operator()(size_t i, size_t j);
    const double& operator()(size_t i, size_t j) const;
    
    Matriz operator+(const Matriz& otra) const;  // Retorno por valor (aprovecha movimiento)
    Matriz transpuesta() const;                   // Retorno por valor
    
    void print(std::ostream& os = std::cout) const;
};
```

El operador `+` debe crear una nueva matriz resultado. Verifique que el compilador aplica RVO o movimiento automático al retornar por valor.

---

## Sección 5 — Funciones Lambda

### Preguntas orientadoras

1. ¿Qué es una función lambda? ¿En qué se diferencia de una función normal y de un functor?
2. Describa la sintaxis completa de una lambda: `[captura](parámetros) especificadores -> tipo { cuerpo }`.
3. ¿Cuál es la diferencia entre capturar por valor `[x]` y por referencia `[&x]`?
4. ¿Para qué sirve `mutable` en una lambda?
5. ¿Cuál es la diferencia entre `[=]` y `[&]`? ¿Cuándo es peligroso usar `[&]`?
6. ¿Qué es `std::function`? ¿Cuándo se prefiere `auto` sobre `std::function`?
7. ¿Qué son las lambdas genéricas (C++14)? ¿En qué se parecen a un template?
8. ¿Puede una lambda ser recursiva? ¿Cómo?

---

### Ejercicios

#### Ejercicio 5.1 — Sintaxis básica: reemplazar functors

Reescriba el siguiente código que usa functors, usando lambdas equivalentes:

```cpp
// Versión con functors (C++03):
struct EsPar {
    bool operator()(int x) const { return x % 2 == 0; }
};

struct MayorQue {
    int limite;
    MayorQue(int l) : limite(l) {}
    bool operator()(int x) const { return x > limite; }
};

struct Duplicar {
    int operator()(int x) const { return x * 2; }
};

std::vector<int> v = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

int pares = std::count_if(v.begin(), v.end(), EsPar());
auto it = std::find_if(v.begin(), v.end(), MayorQue(6));
std::transform(v.begin(), v.end(), v.begin(), Duplicar());
```

Reescríbalo usando lambdas. El código debe ser equivalente pero más conciso.

---

#### Ejercicio 5.2 — Modos de captura

Para cada lambda, indique si compila, qué imprime o qué error produce:

```cpp
int x = 10;
int y = 20;

// a) Sin captura
auto l1 = []() { return x + y; };           // ¿Compila?

// b) Captura por valor
auto l2 = [x, y]() { return x + y; };
x = 100;
std::cout << l2() << "\n";                   // ¿Qué imprime?

// c) Captura todo por referencia
auto l3 = [&]() { return x + y; };
x = 100;
std::cout << l3() << "\n";                   // ¿Qué imprime?

// d) Captura mixta
auto l4 = [x, &y]() { return x + y; };
x = 50; y = 50;
std::cout << l4() << "\n";                   // ¿Qué imprime?

// e) Con mutable
auto l5 = [x]() mutable {
    x += 5;
    return x;
};
std::cout << l5() << "\n";                   // ¿Qué imprime?
std::cout << l5() << "\n";                   // ¿Qué imprime?
std::cout << x << "\n";                      // ¿Qué imprime?
```

---

#### Ejercicio 5.3 — Lambdas con algoritmos STL

Dado un `std::vector<std::string> palabras` con al menos 10 palabras de distinto largo, use lambdas para:

a) Ordenar por longitud ascendente.
b) Ordenar por longitud descendente, y si son iguales, alfabéticamente.
c) Contar las palabras que tienen más de 5 caracteres.
d) Encontrar la primera palabra que empieza con vocal.
e) Imprimir todas las palabras en mayúsculas usando `std::for_each` y `std::transform`.
f) Eliminar las palabras con menos de 4 caracteres.
g) Calcular la longitud promedio de las palabras con `std::accumulate`.

---

#### Ejercicio 5.4 — Lambda con estado: generadores

Implemente los siguientes generadores usando lambdas con captura:

a) `auto generadorSecuencial(int inicio)` — retorna una lambda que en cada llamada devuelve el siguiente entero (`inicio`, `inicio+1`, `inicio+2`, ...).

b) `auto generadorFibonacci()` — retorna una lambda que en cada llamada devuelve el siguiente número de Fibonacci.

c) `auto generadorAleatorio(int min, int max)` — retorna una lambda que en cada llamada devuelve un número aleatorio en `[min, max]`.

Pruebe cada generador llamándolo 10 veces.

```cpp
auto gen = generadorSecuencial(5);
for (int i = 0; i < 5; i++) std::cout << gen() << " ";  // 5 6 7 8 9
```

---

#### Ejercicio 5.5 — `std::function` como callback

Implemente una clase `CBoton` que simula un botón de interfaz gráfica:

```cpp
class CBoton {
    std::string etiqueta;
    std::function<void()> onClick;
    std::function<void(const std::string&)> onHover;
public:
    CBoton(const std::string& etiqueta);
    void setOnClick(std::function<void()> callback);
    void setOnHover(std::function<void(const std::string&)> callback);
    void click();
    void hover();
    void print() const;
};
```

Cree tres botones con distintos callbacks definidos como lambdas:
- Botón "Guardar": imprime "Guardando..." y el nombre de un archivo capturado.
- Botón "Cancelar": incrementa un contador de cancelaciones capturado por referencia.
- Botón "Ayuda": imprime diferentes mensajes según el número de clicks (estado interno).

---

#### Ejercicio 5.6 — Lambdas genéricas y combinación con STL

a) Implemente una función template que reciba un contenedor y un predicado lambda y retorne un nuevo vector con los elementos que cumplen el predicado:

```cpp
template<typename Contenedor, typename Predicado>
auto filtrar(const Contenedor& c, Predicado pred)
    -> std::vector<typename Contenedor::value_type>;
```

b) Implemente una función `pipeline` que aplique una secuencia de transformaciones sobre un vector:

```cpp
template<typename T, typename... Funciones>
std::vector<T> pipeline(std::vector<T> datos, Funciones... fns);
```

Pruébela con:
```cpp
auto resultado = pipeline(
    std::vector<int>{1, 2, 3, 4, 5, 6, 7, 8, 9, 10},
    [](std::vector<int> v) {   // Filtrar pares
        v.erase(std::remove_if(v.begin(), v.end(), [](int x){ return x % 2 != 0; }), v.end());
        return v;
    },
    [](std::vector<int> v) {   // Duplicar
        std::transform(v.begin(), v.end(), v.begin(), [](int x){ return x * 2; });
        return v;
    }
);
// resultado: {4, 8, 12, 16, 20}
```

---

#### Ejercicio 5.7 — Integración: lambdas + smart pointers + STL (Nivel Examen)

Implemente un sistema de gestión de tareas con los siguientes requisitos:

```cpp
enum class Prioridad { BAJA, MEDIA, ALTA, CRITICA };
enum class Estado    { PENDIENTE, EN_PROGRESO, COMPLETADA, CANCELADA };

class Tarea {
    int id;
    std::string titulo;
    Prioridad prioridad;
    Estado estado;
    std::string responsable;
public:
    // Constructor, getters, setters
    void print() const;
};

class GestorTareas {
    std::vector<std::shared_ptr<Tarea>> tareas;
    int proximoId = 1;
public:
    std::shared_ptr<Tarea> agregar(const std::string& titulo,
                                    Prioridad p,
                                    const std::string& responsable);
    
    // Todos los métodos siguientes usan lambdas internamente:
    void imprimirTodas() const;
    void imprimirPorPrioridad(Prioridad p) const;
    void imprimirPorResponsable(const std::string& nombre) const;
    
    std::vector<std::shared_ptr<Tarea>> buscar(
        std::function<bool(const Tarea&)> predicado) const;
    
    void ordenarPor(std::function<bool(const Tarea&, const Tarea&)> comparador);
    
    int contarEnEstado(Estado e) const;
    void cambiarEstado(int id, Estado nuevoEstado);
    
    void exportar(const std::string& archivo) const;
};
```

Requisitos adicionales:
- `buscar` debe aceptar cualquier lambda que reciba `const Tarea&` y retorne `bool`.
- `ordenarPor` debe aceptar cualquier lambda de comparación.
- Use `make_shared` para crear las tareas.
- Implemente un `main` que demuestre todas las funcionalidades.

---

## Ejercicios de Exámenes Anteriores

> Los siguientes ejercicios fueron extraídos de exámenes de años anteriores que evaluaron estos temas.

---

### [2do Parcial 2017] — Templates + STL: función genérica `vPrintMap`

A partir del siguiente `main` (no modificable), reconstituya todos los archivos `.h` y `.cpp` necesarios:

```cpp
int main()
{
    map<string, CTransporte *> mapMoviles;
    map<int, persona *> mapPersonas;

    CMaritimo *CMBarco = new CMaritimo();
    CMaritimo *CMVelero = new CMaritimo("CZX023", "Esperanza", 0x7000FF, 5);
    CTerrestre *CTAuto = new CTerrestre();
    CTerrestre *CTMoto = new CTerrestre();

    CTMoto->SetMatricula("007XYZ");
    CTMoto->SetVelocidad(87.5);
    CTMoto->SetMarca("Honda");

    CMBarco->SetPasajeros(500);
    CMBarco->SetMatricula("AUB734");
    CMBarco->SetNombre("Titanic");
    CTAuto->SetMatricula("AA769DB");
    CTAuto->SetVelocidad(197.8);
    CTAuto->SetColor(0x050FF0FF);
    CMBarco->SetColor(CTAuto->GetColor().GetColor());

    mapMoviles.insert(pair<string, CTransporte*>(CMVelero->GetMatricula(), CMVelero));
    mapMoviles.insert(pair<string, CTransporte*>(CMBarco->GetMatricula(), CMBarco));
    mapMoviles.insert(pair<string, CTransporte*>(CTAuto->GetMatricula(), CTAuto));
    mapMoviles.insert(pair<string, CTransporte*>(CTMoto->GetMatricula(), CTMoto));

    persona *Roberto = new persona("Roberto", 12516857, fecha(26, 10));
    persona *Pedro   = new persona();
    persona *Pablo   = new persona("Pablo", 92736675, fecha(25, 10, 1980));

    mapPersonas.insert(pair<int, persona*>(Roberto->GetDocument(), Roberto));
    mapPersonas.insert(pair<int, persona*>(Pedro->GetDocument(), Pedro));
    mapPersonas.insert(pair<int, persona*>(Pablo->GetDocument(), Pablo));

    vPrintMap(mapMoviles, cout);
    vPrintMap(mapMoviles, "prueba_moviles.txt");
    vPrintMap(mapPersonas, cout);
    vPrintMap(mapPersonas, "prueba_personas.txt");

    // liberar memoria
    return 0;
}
```

Implemente las funciones template `vPrintMap` con manejo de errores al escribir en archivo:

```cpp
template <class K, class V>
void vPrintMap(map<K, V>& mapa, ostream& co);

template <class K, class V>
void vPrintMap(map<K, V>& mapa, string nombre);
```

Implemente todas las clases necesarias con una jerarquía de herencia apropiada. Se evaluará el diseño de clases, uso de templates y STL.

---

### [2do Parcial 2025 — Propuesto] — Smart Pointers + Lambdas + STL

Implemente en C++ un sistema de gestión de biblioteca usando C++ moderno:

```
Publicacion (abstracta)
├── Libro    (autor, páginas)
└── Revista  (número, editorial)
```

Requisitos:
- `Biblioteca` almacena `std::vector<std::unique_ptr<Publicacion>>`.
- No se permite `new` ni `delete` en ningún lugar excepto `make_unique`.
- El método `agregar` toma `std::unique_ptr<Publicacion>` por valor (transfer of ownership).
- Los métodos de búsqueda, filtrado y ordenamiento usan lambdas.
- La impresión se realiza con una función template:

```cpp
template<typename Iter>
void imprimirRango(Iter inicio, Iter fin, std::ostream& os);
```

Operaciones requeridas:
1. Agregar publicación (Libro o Revista).
2. Buscar por título (subcadena).
3. Ordenar por título, por tipo (Libro/Revista), o por fecha.
4. Filtrar: solo libros, solo revistas, publicados en un año dado.
5. Imprimir todo (pantalla y archivo).
6. Calcular estadísticas: cantidad de libros, revistas, promedio de páginas de libros.

---

## Resumen de Conceptos Clave

| Tema | Conceptos esenciales | Guía |
|------|---------------------|------|
| Templates | `template<typename T>`, especialización, non-type params | Sección 1 |
| STL | Contenedores, iteradores, algoritmos, `map`, `vector` | Sección 2 |
| `unique_ptr` | Ownership exclusivo, `make_unique`, no copiable, movible | Sección 3 |
| `shared_ptr` | Ownership compartido, `use_count`, `make_shared` | Sección 3 |
| `weak_ptr` | Sin ownership, rompe ciclos, `lock()`, `expired()` | Sección 3 |
| lvalue/rvalue | Clasificación de expresiones, referencias `&&` | Sección 4 |
| Move semantics | Constructor de movimiento, `noexcept`, Regla de los 5 | Sección 4 |
| `std::move` | Convierte lvalue en rvalue, estado post-move | Sección 4 |
| Lambda básica | `[captura](params){ cuerpo }`, tipos de captura | Sección 5 |
| Lambda + STL | `sort`, `find_if`, `count_if`, `transform`, `for_each` | Sección 5 |
| `std::function` | Type erasure, callbacks, almacenar lambdas | Sección 5 |
