# Guía de Trabajos Prácticos - Unidad 3
## Materia: Programación III

## Ejercicios de la Práctica

Unidad Nº3
Preguntas orientadoras
1) ¿Qué significa el modificador const situado a la derecha de los paréntesis, en la declaración de un método?
2) ¿ Cómo se declaran dos clases mutuamente amigas?
3) ¿Qué diferencia hay entre un constructor de copia y el operador de asignación (=)?
4) ¿Cuándo se llama al constructor de copia? 
5) Al sobrecargar funciones miembro de una clase, ¿de qué manera deben diferir?
6) ¿Por qué utilizar valores predeterminados si se puede sobrecargar una función?
7) ¿Qué operadores no se pueden sobrecargar en C++?

8) ¿Para qué sirve el operador const_cast<type>?

Ejercicios
1) Escribir un programa en C++ que permite realizar operaciones con números complejos: suma, resta, multiplicación y división. Además, a cualquier número complejo se le puede pedir su módulo y ángulo. Este último deberá pertenecer al intervalo [0, 2pi].

2) Observe el programa que sigue y piense cual será su salida. Luego, escriba el código en su IDE favorito, compile y ejecute para verificar lo que usted pensó.
 
3) El siguiente programa usa las funciones insert, find y replace del objeto string. Estudie las sentencias assert y verifique su funcionamiento. Piense las diferencias entre insertar y reemplazar una cadena en otra. ¿Cómo trabaja la función replace?


4) Modifique el programa del ejercicio 3 para poder reemplazar un string ingresado por teclado dentro del string s. Se debe controlar que la cadena ingresada por teclado exista dentro de la cadena s. ¿Qué sucede con el tamaño del string s si la cadena a reemplazar tiene un tamaño igual o menor que el de la cadena reemplazada? ¿ Aumenta, disminuye o se mantiene el tamaño de s? ¿Se le ocurre un caso en el que el reemplazo haga que el tamaño de s pueda aumentar?
5) El siguiente listado es un programa de prueba llamado test.cpp para probar el funcionamiento de una clase denominada CRacional. Copie el código en su IDE favorito y a continuación, haga que compile sin advertencias y ejecute.
#include <iostream>
#include "racional.h"
using namespace std;

int main()
{
  CRacional a, b(3, 7), c;
  int d = 3;

  cout << "a: "; cin >> a;
  c = a + b;
 
  c += b;
  cout << c << endl;
  c = a + b; // equivale a: c = a.operator+(b)
  cout << c << endl;
  c = b + a; // equivale a: c = b.operator+(a)
  cout << c << endl;
  c = a + static_cast<CRacional>(d);
  cout << c << endl;
  c = static_cast<CRacional>(d) + a;
  cout << c << endl;
 
  double x = 2.0;
  c = x + a;
  cout << c << endl;
 
  if (a == b) cout << "a es igual a b\n";
  if (a < b) cout << "a es menor que b\n";
  if (a > b) cout << "a es mayor que b\n";
  if (!a) cout << "racional nulo\n";

  c = ++a;
  cout << "c = " << c << endl;
  cout << "a = " << a << endl;
  c = a++;
  cout << "c = " << c << endl;
  cout << "a = " << a << endl;
 
  c = --a;
  c = a--;

  c = -a + b;
  cout << c << endl;

  c = -a - b;
  cout << c << endl;

  c = a * b;
  cout << c << endl;

  c = a / b;
  cout << c << endl;
 
  x = c;
  cout << x << endl;

  system("pause"); 
  return 0;
}

6) La imagen que sigue muestra la declaración de una clase string básica. Se pide el código que implemente esa clase y un programa de prueba que demuestre su funcionamiento.



## Ejercicios de Exámenes Anteriores

> Los siguientes ejercicios fueron extraídos de primeros parciales y recuperatorios de años anteriores. Corresponden a los temas de esta unidad.

---

### [1er Parcial 2023] Práctica - Clase CMatriz con sobrecarga de operadores y clase CPantalla

Escriba un programa en lenguaje C++ que permita ejecutar el siguiente código:

```cpp
int main()
{
    CMatriz original;
    original.cargar("colores.bin");
    cout << "Matriz original:" << endl << original << endl;

    CMatriz copia(original), suma;
    suma = original;
    suma += (copia + 5);
    cout << "Matriz suma:" << endl << suma << endl;

    CPantalla pantalla(original);
    cout << "Pantalla original:" << endl << pantalla << endl;

    pantalla.ajustarColor(3);
    cout << "Pantalla con ajuste de color:" << endl << pantalla << endl;

    pantalla.borrarVerde();
    cout << "Pantalla sin componente verde:" << endl << pantalla << endl;

    pantalla.reforzarRojo(0.35);
    cout << "Pantalla modificada (rojo reforzado):" << endl << pantalla << endl;

    // generar la misma información en un archivo de salida: pantalla.txt

    CColor color(pantalla.getPtr()[pantalla.getFilas() * pantalla.getColumnas() - 1]);
    cout << "El color del ultimo punto de la pantalla es:" << endl << color << endl;

    return 0;
}
```

Consignas:

1. El método `cargar` de `CMatriz` recibe el nombre del archivo binario con el formato: `| cantidad_filas | cantidad_columnas | todos_los_valores |`. Los valores se imprimen en hexadecimal con prefijo `0x`.

2. El objeto `pantalla` es una matriz de colores en formato `0xAARRGGBB`. Métodos:
   - `ajustarColor(n)`: suma `n` a la componente alpha de todos los colores.
   - `borrarVerde()`: establece la componente green a 0.
   - `reforzarRojo(p)`: aumenta la componente red en el porcentaje indicado (máximo `0xFF`).

3. Todos los datos deben poder persistirse en un archivo de texto.

4. `CColor` imprime un color con el formato:
   ```
   El color del ultimo punto de la pantalla es:
   componente azul: valor_azul
   componente verde: valor_verde
   componente rojo: valor_rojo
   componente alpha: valor_alpha
   ```

5. Correcta modularización. Sin errores ni warnings. Gestionar adecuadamente la memoria dinámica.

---

### [1er Recuperatorio 2023] Práctica - Clase CVentana con redimensionado y manejo de bits

Se debe implementar una clase `CVentana` que:

1. Cargue una matriz de colores a partir de un archivo binario.
2. Sobrecargue el constructor de copia, el operador `=` y admita el producto por un número entero. En la posmultiplicación se modifican las componentes verdes; en la premultiplicación, las componentes azules.
3. Implemente un método `redim(new_rows, new_cols, index)` que permita redimensionar la matriz obteniendo una submatriz cuyos colores son los mismos que los de la original a partir de un determinado índice.
4. Sepa imprimirse y almacenarse.
5. En un archivo de texto se tienen almacenados N botones que son ventanas con: dimensiones, coordenadas (x, y), índice para copiar colores, título (string), y un byte de estado donde los bits pares indican el estado de 4 medidores. Los medidores deben poder setearse o leerse sin afectar los otros bits.

---

### [1er Parcial 2017] Práctica - Clase CPolinomio con operadores sobrecargados

Se requiere escribir un programa para manipular ecuaciones algebraicas dependientes de una variable. Ejemplo:

```
2x³ – x + 8.25  +  5x⁵ – 2x³ + 7x² – 3  =  5x⁵ + 7x² – x + 5.25
```

Cada término del polinomio será representado por una clase `CTermino` (atributos privados: `coeficiente` float, `exponente` int) y cada polinomio por una clase `CPolinomio`.

La clase `CTermino` debe permitir al menos:
- Construir un término iniciado a 0 por omisión.
- Acceder al coeficiente y al exponente.
- Sobrecargar `==`, `>` y `<` para comparar por grado.
- Sobrecargar el operador `<<` para mostrar en formato `{+|-}ax^exp` (ej: `-7x^3`).

La clase `CPolinomio` debe permitir al menos:
- Construir un polinomio con cero términos.
- Obtener el número de términos.
- Agregar términos ordenados por exponente ascendente. Si el término existe, sumar coeficientes. Si el coeficiente es nulo, no realizar operación.
- Sobrecargar el operador `+`.
- Sobrecargar `<<` para mostrar en formato: `+ 5x^5 – 1x^1 + 5.25`.
- Sobrecargar el operador `()` para evaluar el polinomio en un valor x (retorna `double`).
- Sobrecargar el operador de conversión a `double` (evalúa en x=1).
- Sobrecargar el operador `*` para pre y post multiplicar por un `float`.

La salida esperada del programa de prueba es:
```
Polinomio A:  - 3x^2 + 2x^1 + 6
Polinomio B:  + 8x^2 - 2x^1 - 6
Polinomio R:  + 5x^2
valor del polinomio para x = 5: 125
valor del polinomio para x = 1: 5
PolinomioA * 2.5 = - 7.5x^2 + 5x^1 + 15
-1.5 * PolinomioR = -7.5x^2
```
