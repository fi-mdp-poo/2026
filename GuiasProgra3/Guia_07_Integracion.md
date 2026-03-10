# Guía de Integración - Exámenes Completos
## Materia: Programación 3

> Esta guía contiene los exámenes completos de años anteriores organizados cronológicamente. Está pensada para la preparación integral antes de cada instancia evaluativa.
>
> **Cronograma de temas:**
> - **1er Parcial:** Unidades 1–4 (intro C++/clases/UML, constructores/destructores/composición, encapsulamiento/sobrecarga de operadores, herencia simple y múltiple)
> - **2do Parcial:** Unidades 5–6b (polimorfismo/clases abstractas, streams/excepciones, templates/STL, smart pointers, move semantics, lambdas)

---

## Año 2017

### [1er Parcial 2017]

#### Parte Teórica

Analice el siguiente código en C++. Asuma que compila y ejecuta sin errores:

```cpp
#include <iostream>
using namespace std;

int x_static = 0;

class base {
public:
    static int s;
    int x;
    base(int _x = 1) : x(_x) { s++; cout << "base(" << x << ")" << endl; }
    ~base() { s--; cout << "~base(" << x << ")" << endl; }
};
int base::s = 0;

class otra_base {
public:
    int y;
    otra_base(int _y = 2) : y(_y) { cout << "otra_base(" << y << ")" << endl; }
    ~otra_base() { cout << "~otra_base(" << y << ")" << endl; }
};

class derivada : public base, public otra_base {
public:
    derivada(int _x, int _y) : base(_x), otra_base(_y) {
        cout << "derivada(" << _x << "," << _y << ")" << endl;
    }
    ~derivada() { cout << "~derivada" << endl; }
};

int main() {
    base b1(10);
    {
        base b2(20);
        derivada d(30, 40);
        cout << "base::s = " << base::s << endl;
    }
    cout << "base::s = " << base::s << endl;
    return 0;
}
```

a) ¿Qué imprime el programa? Justifique el orden de construcción y destrucción.

b) ¿Qué significa el miembro estático `base::s`? ¿Dónde se almacena? ¿Cuántas instancias existen?

c) ¿Cuál es la diferencia entre una variable `static` dentro de una función y un atributo `static` de una clase?

d) ¿Qué significa `volatile` en la declaración de una variable? ¿Y `mutable`?

e) Si se declara `const base cb(5)`, ¿qué restricciones implica para el uso de `cb`?

f) ¿Qué es una referencia en C++? ¿En qué se diferencia de un puntero?

g) ¿Para qué sirven los namespaces? Dé un ejemplo de uso.

#### Parte Práctica

Implemente las clases `CTermino` y `CPolinomio` en C++ que permitan manipular polinomios algebraicos. El programa principal (no modificable) es:

```cpp
int main() {
    CPolinomio p1, p2, p3;

    // Carga p1 = 3x^2 + 2x + 1
    p1 = p1 + CTermino(3, 2);
    p1 = p1 + CTermino(2, 1);
    p1 = p1 + CTermino(1, 0);

    // Carga p2 = x^3 + 4x
    p2 = p2 + CTermino(1, 3);
    p2 = p2 + CTermino(4, 1);

    p3 = p1 + p2;
    cout << "p1 = " << p1 << endl;
    cout << "p2 = " << p2 << endl;
    cout << "p3 = p1+p2 = " << p3 << endl;
    cout << "p1*p2 = " << p1 * p2 << endl;
    cout << "p1(2) = " << p1(2) << endl;  // evaluacion en x=2

    return 0;
}
```

La salida esperada es:
```
p1 = 3x^2 + 2x^1 + 1x^0
p2 = 1x^3 + 4x^1
p3 = p1+p2 = 1x^3 + 3x^2 + 6x^1 + 1x^0
p1*p2 = 3x^5 + 2x^4 + 13x^3 + 8x^2 + 4x^1
p1(2) = 17
```

Deberá sobrecargar los operadores `+` (suma de polinomios y suma de término a polinomio), `*` (producto de polinomios), `<<` (impresión), y el operador de llamada `()` (evaluación en un punto).

---

### [2do Parcial 2017]

#### Parte Práctica

A partir del siguiente `main.cpp` (no modificable), reconstituya todos los archivos `.h` y `.cpp` necesarios para que el programa compile y produzca la salida indicada.

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

Implemente las funciones template `vPrintMap` (con manejo de errores al escribir en archivo):

```cpp
template <class K, class V>
void vPrintMap(map<K, V> &mapa, ostream &co);

template <class K, class V>
void vPrintMap(map<K, V> &mapa, string nombre);
```

Implemente todas las clases necesarias con sus getters/setters, constructores y una jerarquía de herencia apropiada. Se evaluará el diseño de clases, uso de templates y STL.

---

## Año 2021

### [1er Parcial 2021]

#### Parte Práctica (Ejercicio 1 — CPolinomio)

Mismas especificaciones que el [1er Parcial 2017 Parte Práctica]. Ver sección correspondiente.

#### Parte Práctica (Ejercicio 2 — cRadioReloj)

A partir del siguiente fragmento de `main` (no modificable), implemente todas las clases necesarias:

```cpp
int main()
{
    cReloj reloj1;
    cReloj reloj2(12, 30, 0);
    cRadio radio1;
    cRadio radio2(99.9, 0);  // frecuencia, encendido
    cRadioReloj rr1;
    cRadioReloj rr2(reloj2, radio2);

    cout << "Reloj1: ";     reloj1.mostrar();
    cout << "Reloj2: ";     reloj2.mostrar();
    cout << "Radio1: ";     radio1.mostrar();
    cout << "Radio2: ";     radio2.mostrar();
    cout << "RR1: ";        rr1.mostrar();
    cout << "RR2: ";        rr2.mostrar();

    rr2.avanzarHora(1);
    rr2.cambiarFrecuencia(107.5);
    cout << "RR2 modificado: "; rr2.mostrar();

    return 0;
}
```

La clase `cRadioReloj` debe heredar de `cReloj` y de `cRadio`. Incluir también `cTime` con getters/setters de hora, minuto y segundo. El diseño de constructores y métodos queda a criterio del alumno (se evaluará).

---

### [2do Parcial 2021]

#### Parte Teórica

1. ¿Puede un constructor de una clase abstracta ser llamado directamente? ¿Para qué sirve?

2. Explique los tres tipos de binding en C++. ¿Cuál se usa para lograr polimorfismo?

3. ¿Qué es un template en C++? Dé un ejemplo de función template.

4. ¿Qué es una excepción? ¿Cuándo es preferible usar excepciones en lugar de códigos de retorno de error?

5. ¿Cuál es la diferencia entre una clase abstracta y una clase concreta en C++? ¿Puede instanciarse una clase abstracta?

#### Parte Práctica

Implemente en C++ un sistema de gestión de figuras geométricas que cumpla con los siguientes requisitos:

- Clase abstracta `CFigura` con método virtual puro `area()` y `perimetro()`, y un nombre descriptivo.
- Clases concretas `CCirculo` (radio) y `CRectangulo` (base, altura) que hereden de `CFigura`.
- Clase `CColor` para representar color RGB.
- Clase `ManejadorFiguras` que almacene un vector de punteros a `CFigura` y provea:
  - `agregarFigura(CFigura*)`: agrega una figura.
  - `imprimirFiguras(ostream&)`: imprime todas las figuras con su área y perímetro.
  - `ordenarPorArea()`: ordena las figuras de menor a mayor área.
  - `guardarEnArchivo(string nombre)`: guarda las figuras en un archivo de texto.

El programa principal debe cargar figuras desde un archivo de texto (formato a elección del alumno), mostrarlas, ordenarlas y guardarlas en otro archivo.

---

## Año 2022

### [1er Parcial 2022]

#### Parte Teórica

Analice el mismo código del [1er Parcial 2017 Parte Teórica] (herencia múltiple con `base`, `otra_base`, `derivada`).

#### Parte Práctica

Mismas especificaciones que el [1er Parcial 2021 Ejercicio 2 — cRadioReloj].

---

### [2do Parcial 2022]

#### Parte Teórica

1. ¿Qué es el polimorfismo en C++? ¿Qué mecanismo lo hace posible?

2. ¿Qué diferencia hay entre un método `virtual` y un método `virtual` puro? ¿Puede una clase tener ambos?

3. Indicar si es cierto: *"Si en un bloque try/catch existen varias sentencias catch para capturar excepciones, es obligatorio ordenarlas poniendo antes las excepciones más particulares y después las más generales"*. Justifique.

4. ¿Qué es un `ifstream`? ¿Qué ocurre si se intenta abrir un archivo que no existe? ¿Cómo se verifica que la apertura fue exitosa?

5. ¿Qué ventaja tiene usar `try/catch` en operaciones de apertura de archivos en lugar de chequear el valor de retorno?

#### Parte Práctica

Implemente en C++ un sistema de gestión de biblioteca. El sistema debe manejar:

- Clase `Biblioteca` que contiene un vector de punteros a `Volumen`.
- Clase abstracta `Volumen` con atributos comunes (título, código, fecha, idioma).
- Clases concretas `Libro` (autores, cantidad de páginas) y `Revista` (número, editorial).
- Los datos se persisten en un archivo binario con el formato:
  - `unsigned short`: cantidad de ejemplares.
  - Por cada ejemplar: identificador de tipo (1 byte), luego los datos del volumen.

El menú debe permitir:
```
1 – Agregar volumen.
2 – Ordenar por título.
3 – Ordenar por código.
4 – Imprimir biblioteca.
5 – Guardar en archivo.
0 – Salir (con confirmación).
```

**Requisito adicional:** El programa no debe dejar de funcionar ante errores en tiempo de ejecución. Agregar bloques `try/catch` adecuados en todas las operaciones críticas (apertura de archivos, asignación de memoria, operaciones sobre volúmenes).

---

## Año 2023

### [1er Parcial 2023]

#### Parte Práctica

Implemente en C++ las clases `CMatriz`, `CPantalla` y `CColor` a partir del siguiente `main` (no modificable):

```cpp
int main()
{
    CPantalla pantalla("datos.bin");

    // Ajustar canal alpha de todos los píxeles
    pantalla.ajustarAlpha(0.8);

    // Limpiar canal verde
    pantalla.limpiarVerde();

    // Incrementar componente roja (saturar a 255)
    pantalla.potenciarRojo(50);

    // Imprimir en pantalla
    pantalla.imprimir(cout);

    // Guardar resultado en archivo de texto
    pantalla.guardar("resultado.txt");

    return 0;
}
```

El archivo binario `datos.bin` contiene una matriz de píxeles. Cada píxel se almacena como 4 `unsigned char` (R, G, B, Alpha). Al inicio del archivo se guardan las dimensiones (filas y columnas como `unsigned short`).

`CColor` representa un color RGBA con operaciones de acceso a componentes. `CMatriz` es la clase base que maneja la memoria de la matriz. `CPantalla` hereda de `CMatriz` e implementa las operaciones de procesamiento.

Se evaluará: correcta implementación de constructores/destructores, uso de `new`/`delete`, sobrecarga de operadores donde corresponda, encapsulamiento y diseño de interfaces.

---

### [2do Parcial 2023]

#### Parte Teórica

Analice el siguiente código C++ y responda:

```cpp
#include <iostream>
#include <stdexcept>
using namespace std;

class Base {
public:
    virtual void proceso() {
        throw runtime_error("Error en Base::proceso");
    }
    virtual ~Base() {}
};

class Derivada : public Base {
public:
    void proceso() override {
        cout << "Derivada::proceso - inicio" << endl;
        Base::proceso();
        cout << "Derivada::proceso - fin" << endl;
    }
};

int main() {
    Base* obj = new Derivada();
    try {
        obj->proceso();
        cout << "Sin excepciones" << endl;
    }
    catch (const runtime_error& e) {
        cout << "runtime_error: " << e.what() << endl;
    }
    catch (...) {
        cout << "Excepción desconocida" << endl;
    }
    delete obj;
    return 0;
}
```

a) ¿Cuál es la salida exacta del programa? Justifique paso a paso.

b) ¿Qué sucedería si se elimina el bloque `catch(const runtime_error& e)` y se deja solo `catch(...)`?

c) ¿Por qué se declara `virtual ~Base()`? ¿Qué ocurriría si el destructor no fuera virtual?

d) ¿Qué significa `override`? ¿Es obligatorio usarlo?

e) ¿Qué diferencia hay entre atrapar excepciones por valor (`catch(runtime_error e)`) y por referencia constante (`catch(const runtime_error& e)`)?

#### Parte Práctica

Implemente en C++ un sistema de gestión de servicios de mantenimiento. El sistema debe manejar:

- Clase abstracta `Servicio` con atributos: fecha, cliente (nombre + código), costo base.
- Clase `TrabajoPintura` (hereda de `Servicio`) con: nombre del trabajador, fecha de contratación, código de trabajador, superficie (m²), precio por m².
- Clase `RevisionAlarma` (hereda de `Servicio`) con: cantidad de alarmas revisadas.

El archivo binario de entrada tiene el siguiente formato:
- `int`: cantidad de trabajos.
- Por cada trabajo: `0xAA` (1 byte) = pintura, `0x55` (1 byte) = revisión de alarma, seguido de los datos del objeto.

El menú debe permitir:
```
1 – Leer archivo.
2 – Imprimir todos los servicios.
3 – Ordenar por fecha.
4 – Ordenar por nombre de cliente.
5 – Calcular costo total.
0 – Salir.
```

Datos de prueba:
```
Pintura:  Trabajador "Pintor1", contratado 25/01/2010, código 100
          Fecha: 10/01/2023, Cliente "Cliente1" (200), Superficie 60.0, Precio 5.0
Alarma:   Fecha: 01/03/2023, Cliente "Cliente3" (210), Alarmas 10
Alarma:   Fecha: 15/04/2023, Cliente "Cliente2" (220), Alarmas 5
Pintura:  Trabajador "Pintor2", contratado 25/03/2022, código 150
          Fecha: 01/02/2023, Cliente "Cliente4" (230), Superficie 30.0, Precio 4.5
```

**Requisito:** El programa no debe terminar ante errores en tiempo de ejecución. Usar bloques `try/catch` en operaciones críticas (apertura de archivos, lectura del binario, impresión).

---

## Índice de Temas por Examen

| Examen | Teoría | Práctica |
|--------|--------|----------|
| 1er Parcial 2017 | U1: herencia múltiple, static de clase, volatile/mutable/const, referencias, namespaces | U3: CPolinomio con sobrecarga de operadores (+, *, <<, ()) |
| 2do Parcial 2017 | — | U6: STL map, templates vPrintMap, jerarquía CTransporte/CMaritimo/CTerrestre |
| 1er Parcial 2021 | — | U3: CPolinomio; U4: cReloj/cRadio/cRadioReloj (herencia múltiple) |
| 2do Parcial 2021 | U5: constructores abstractos, tipos de binding, templates, excepciones, clase abstracta vs concreta | U5: ManejadorFiguras con CFigura/CCirculo/CRectangulo, polimorfismo, archivos |
| 1er Parcial 2022 | U1: análisis de código herencia múltiple | U4: cReloj/cRadio/cRadioReloj (herencia múltiple) |
| 2do Parcial 2022 | U5+U6: polimorfismo, virtual puro, excepciones, archivos | U5+U6: Biblioteca/Volumen/Libro/Revista con polimorfismo + excepciones |
| 1er Parcial 2023 | — | U2+U3: CMatriz/CPantalla/CColor, constructores, operaciones sobre píxeles |
| 2do Parcial 2023 | U5+U6: análisis de código C++ con polimorfismo y excepciones | U5+U6: TrabajoPintura/RevisionAlarma, polimorfismo, archivos binarios, excepciones |
