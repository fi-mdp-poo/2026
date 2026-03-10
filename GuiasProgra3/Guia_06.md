# Guía de Trabajos Prácticos - Unidad 6
## Materia: Programación III

## Ejercicios de la Práctica

Unidad Nº6: Flujos y excepciones

### Preguntas orientadoras

1) ¿Uno de los principales objetivos de los flujos (streams) es: _____________________________________________?.
2) Escribir en el disco (y en la pantalla, aunque en menor extensión) es muy "costoso". Lleva mucho tiempo (relativamente hablando) escribir información en el disco o leer información del disco, y la ejecución del programa por lo general se bloquea debido a las lecturas y escrituras de disco. ¿Cómo se soluciona este problema?
3) Como es de esperarse, C++ se basa en el método orientado a objetos para implementar los flujos y los búffers. Explique el objetivo de las siguientes clases:
   - `streambuf`
   - `ios`
   - `istream` y `ostream`
   - `iostream`
   - `fstream`
4) ¿Qué es el operador de extracción, y qué hace?
5) ¿Cuáles son las tres formas de utilizar `cin.get()`, y cuáles son sus diferencias?
6) ¿Cuál es la diferencia entre `cin.read()` y `cin.getline()`?
7) ¿Cuál es el ancho predeterminado para enviar como salida un entero largo mediante el operador de inserción?
8) ¿Cuál es el valor de retorno del operador de inserción?
9) ¿Qué parámetro lleva el constructor para un objeto `ofstream`?
10) ¿Qué hace el argumento `ios::ate`?
11) Cuando inicia un programa de C++ que incluye la clase `iostream`, se crean e inicializan cuatro objetos de E/S estándar. ¿Cuáles son?
12) ¿Qué es una excepción?
13) ¿Qué es un bloque `try`?
14) ¿Qué es una instrucción `catch`?
15) ¿Qué información puede contener una excepción?
16) ¿Cuándo se crean los objetos de excepción?
17) ¿Se deben pasar las excepciones por valor o por referencia?
18) ¿Atrapará una instrucción `catch` una excepción derivada si está buscando la clase base?
19) ¿Qué significa `catch(...)`?
20) ¿Por qué preocuparse por producir excepciones? ¿Por qué no manejar el error donde ocurre?
21) ¿Por qué generar un objeto? ¿Por qué no sólo pasar un código de error?
22) ¿Se tiene que atrapar una excepción en el mismo lugar en el que el bloque `try` la creó?

### Ejercicios

1) Explique cada línea del siguiente fragmento de código:

```cpp
try {
    throw runtime_error("algo salió mal");
} catch (runtime_error& e) {
    cout << e.what() << endl;
}
```
2) Cree un bloque `try`, una instrucción `catch` y una excepción simple.

3) Modifique la respuesta del ejercicio 2, coloque datos en la excepción junto con una función getter, y utilícela en el bloque `catch`.

4) Modifique la clase del ejercicio 3 para que sea una jerarquía de excepciones. Cambie el bloque `catch` para utilizar los objetos derivados y los objetos base.

5) Escriba un programa que tome un nombre de archivo como parámetro y que abra el archivo para lectura. Lea todos los caracteres del archivo y despliegue en la pantalla sólo las letras y los signos de puntuación. (Ignore todos los caracteres no imprimibles).

6) Escriba un programa que tome un nombre de archivo como parámetro y que abra el archivo para lectura. Lea todos los bytes y presente en pantalla la información en formato Hex, como se muestra a continuación. Además, genere un nuevo archivo de texto y guarde la información presentada en pantalla, respetando el mismo formato.

7) Realizar un programa que permita crear un archivo nuevo, abrir uno existente, agregar, buscar, modificar y borrar registros. El nombre del archivo será ingresado por teclado. Cada registro del archivo será un objeto persona con los atributos nombre, dirección y teléfono. Así mismo, para que el usuario pueda elegir cualquiera de las opciones mencionadas, el programa visualizará en pantalla un menú similar al siguiente:

```
Archivo actual: ninguno
----------------------------------------------------------
1. Nuevo archivo
2. Abrir archivo
3. Agregar registro
4. Buscar un registro
5. Buscar siguiente
6. Modificar un registro
7. Eliminar un registro
8. Salir
----------------------------------------------------------
Opción (1 - 8): 1
Nombre del archivo: telefonos.dat
```

La opción Nuevo abrirá un archivo para agregar registros; si el archivo existe, preguntará si se desea sobreescribir. La opción Abrir permitirá abrir un archivo para leer y escribir o para agregar; estas dos opciones se elegirán de un menú. La opción Buscar permitirá buscar un registro por el campo nombre; se permitirá introducir una subcadena de nombre, incluso vacía. La opción Buscar siguiente buscará el siguiente registro que cumpla con las mismas condiciones que el anteriormente buscado. Finalmente, la opción Eliminar permitirá marcar un registro para borrar. Se deberá realizar al menos un método para cada una de las opciones, excepto para las opciones Buscar, que compartirán ambas el mismo método, y para Salir.

> **Nota:** En todos los programas, use excepciones para manejar situaciones anómalas.


## Ejercicios de Exámenes Anteriores

> Los siguientes ejercicios fueron extraídos de segundos parciales y recuperatorios de años anteriores. Corresponden a los temas de esta unidad.

---

### [2do Parcial 2022] Teoría - Excepciones en C++

Analice el siguiente código C++ y responda sin ejecutarlo:

```cpp
#include <iostream>
#include <stdexcept>
using namespace std;

void funcion2() {
    throw out_of_range("indice fuera de rango");
}

void funcion1() {
    try {
        cout << "Paso B" << endl;
        funcion2();
        cout << "Paso C" << endl;
    }
    catch (logic_error& e) {
        cout << "Paso D: " << e.what() << endl;
    }
    cout << "Paso E" << endl;
}

int main() {
    try {
        funcion1();
    }
    catch (exception& e) {
        cout << "Paso A: " << e.what() << endl;
    }
    return 0;
}
```

a) ¿Cuál es la salida exacta del programa? Tenga en cuenta que `out_of_range` hereda de `logic_error`. Justifique.

b) Indicar si es cierto: *"Si en un bloque try/catch existen varias sentencias catch para capturar excepciones, es obligatorio ordenarlas poniendo antes las excepciones más particulares y después las más generales"*. Justifique con un ejemplo.

c) ¿Qué diferencia hay entre `catch(exception e)` y `catch(exception& e)`? ¿Cuál es preferible y por qué?

d) ¿Qué sucede si se lanza una excepción dentro de un destructor? ¿Es recomendable?

---

### [2do Parcial 2022] Práctica - Gestión de biblioteca (polimorfismo + excepciones)

Ver ejercicio completo en la Guía 5. En este examen se enfatiza además el **manejo de excepciones**: el programa no debe dejar de funcionar ante errores en tiempo de ejecución. Agregar bloques `try/catch` adecuados en todas las operaciones críticas (apertura de archivos, asignación de memoria, operaciones sobre volúmenes).

---

### [2do Parcial 2023] Práctica - Empresa de mantenimiento (polimorfismo + excepciones + archivos)

Ver ejercicio completo en la Guía 5. En este examen el énfasis está en el **manejo de excepciones** y en la **lectura correcta del archivo binario** con el formato detallado:

- `int` = cantidad de trabajos.
- `0xAA` (1 byte) = identifica trabajo de pintura.
- `0x55` (1 byte) = identifica revisión de alarma.

Datos de prueba:
```
Pintura:   Trabajador: "Pintor1", 25/01/2010, código 100
           Fecha: 10/01/2023, Cliente: "Cliente1" (200), Superficie: 60.0, Precio: 5.0

Alarma:    Fecha: 01/03/2023, Cliente: "Cliente3" (210), Alarmas: 10

Alarma:    Fecha: 15/04/2023, Cliente: "Cliente2" (220), Alarmas: 5

Pintura:   Trabajador: "Pintor2", 25/03/2022, código 150
           Fecha: 01/02/2023, Cliente: "Cliente4" (230), Superficie: 30.0, Precio: 4.5
```
