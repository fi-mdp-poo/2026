# Guía de Trabajos Prácticos - Unidad 2
## Materia: Programación III

## Ejercicios de la Práctica

Unidad Nº2
Preguntas orientadoras
1) Mencione y defina los tipos de ámbitos de las variables que se presentaron en la teoría. ¿Por qué definiría como global una variable de tipo static?.
2) ¿Qué modificador elegiría para indicar que una variable puede cambiar su valor entre accesos aún cuando no pareciera haber sido modificado?¿En qué aplicaciones se utiliza este modificador?
3) ¿Por qué puede ser que el operador new retorna un puntero y no una referencia? (Ayuda: piense en la diferencia entre punteros y referencias para encontrar la respuesta).
4) Indique cuál es la función de los constructores y para qué existen los destructores.
5) Llene los espacios en blanco:
Se tiene acceso a los miembros de clase vía el operador _________ en conjunción con un objeto de clase o vía el operador _________ en conjunción con un apuntador a un objeto de clase.
Los miembros de una clase especificados como _________ son sólo accesibles a las funciones miembro de la clase y amigos de la clase.
Un _________ es una función miembro especial utilizada para inicializar los miembros de datos de una clase.
El acceso por omisión para los miembros de una clase es _________.
Los miembros de una clase especificados como _________ son accesibles en cualquier parte en que un objeto de la clase esté en alcance.
El operador _________ asigna dinámicamente memoria para un objeto de un tipo específico y regresa un _________ a dicho tipo.
Las operaciones de entrada son soportadas por la clase _________.
Las operaciones de salida son soportadas por la clase _________.

6) Encuentre el o los errores y corrija:
void ~Time(int);
Suponga la siguiente definición parcial de la clase Time
class Time
{
public:
//function prototypes
private:
int hour = 0;
int minute = 0;
int second = 0;
};
7) ¿Se pueden utilizar los nombres definidos en un espacio de nombres sin utilizar la palabra reservada using?

Ejercicios
1)  ¿Qué inconveniente presenta el programa que se muestra a continuación? Discuta las posibles soluciones.
int main (int argc, char *argv[])
{
    string cadena;
    cin >> cadena;
    getline(cin, cadena);
    cout << "La cadena ingresada es: " << cadena << endl;
    return 0;
} 
2)  ¿Qué está mal en este programa? Mencione 3 formas de corregirlo.
#include <iostream>
int main()
{
    cout << "¡Hola, mundo!" << endl;
    return 0;
}  
3) Crear la clase Fecha con todos los datos miembro y atributos que se muestran en el diagrama de clase siguiente y luego, crear un programa cliente que demuestre el uso de cada una de las funciones.

4) Crear la clase Tiempo con todos los datos miembro y atributos que se muestran en el diagrama que sigue y luego, crear un programa cliente que muestre el uso de cada una de las funciones.

5) El siguiente diagrama muestra un concepto fundamental de la programación orientada a objetos: La composición. Implementar la clase Empleado y escribir un programa que imprima en pantalla la siguiente información:
Perez, Juan
Contratado el: 1 de Julio de 1999
Fecha de nacimiento: 31 de Diciembre de 1978
Lopez, Pedro
Contratado el: 1 de Julio de 1999
Fecha de nacimiento: 25 de Febrero de 1980
Presione una tecla para continuar . . .


6) En  una  competencia  de  ciclismo  intervienen  un número desconocido de  deportistas,  cada  uno  realiza  dos  pruebas,  una  por tiempo y otra por número de vueltas. Se deben registrar los siguientes datos de cada participante: nombre, fecha de nacimiento, tiempo en la primera prueba y número de vueltas en la segunda prueba, respectivamente. Imprimir en pantalla el nombre del participante que realizó la primera prueba en el menor tiempo, el nombre del participante que hizo la mayor cantidad de vueltas y 5 columnas en las que se muestre para cada participante:
Nombre, edad, tiempo en la primera prueba, número de vueltas en la segunda prueba, diferencia de tiempo respecto del más rápido en la primera prueba y diferencia de vueltas respecto del que hizo más vueltas en la segunda prueba.
Se pide implementar un programa en lenguaje C++, orientado a objetos, que resuelva el problema. Además, se pide el diagrama de clases y de secuencia.
NOTA: En todos los ejercicios, se deben instanciar los objetos en la memoria HEAP.




## Ejercicios de Exámenes Anteriores

> Los siguientes ejercicios fueron extraídos de primeros parciales y recuperatorios de años anteriores. Corresponden a los temas de esta unidad.

---

### [1er Parcial 2022 / 2021] Teoría - Constructores en herencia múltiple: salida del programa

Analice el siguiente código e indique el orden exacto de los mensajes que se imprimirán en pantalla. Justifique el orden de llamada a constructores y destructores:

```cpp
class base {
public:
    base(void) { num = 1; cout << num << endl; }
    base(int val) { num = val; cout << num << endl; }
    ~base(void) { cout << "Destructor clase base" << endl; }
protected:
    int num;
};
class miembro {
public:
    miembro(void) { cout << "Constructor miembro" << endl; }
    ~miembro(void) { cout << "Destructor miembro" << endl; }
};
class otra_base {
public:
    otra_base(void) { num = 10; cout << num << endl; }
    otra_base(int val) { num = val; cout << num << endl; }
    ~otra_base(void) { cout << "Destructor otra_base" << endl; }
private:
    int num;
    miembro obj;
};
class derivada : private base, public otra_base {
public:
    derivada(void) { base::num = 10; otra_base::num = 20; num = 35; }
    derivada(int valor) : base(valor/10), otra_base(valor*10) { num = 45; }
    ~derivada(void) { cout << "Destructor derivada" << endl; }
    void SetNum(int val) { num = val; }
    int GetNum() { return num; }
private:
    int num;
};
int main()
{
    derivada obj1, obj2 = 20;
    // ... SetNum y GetNum
    return 0;
}
```

---

### [1er Parcial 2021] Teoría - Constructor de copia, operador de asignación y funciones amigas

Preguntas sobre la implementación de `CPolinomio` y `CTermino`:

a) ¿Qué métodos de las clases `CTermino` y `CPolinomio` se invocan y en qué orden cuando se ejecuta: `PolinomioR = PolinomioA + PolinomioB;`?

b) ¿Es necesario un constructor de copia para `CPolinomio`? ¿Por qué? En caso afirmativo, escríbalo.

c) ¿Es necesario sobrecargar el operador de asignación para `CPolinomio`? ¿Por qué? En caso afirmativo, escríbalo.

d) Si los operadores de relación de la clase `CTermino` se declaran privados, ¿qué ocurre?

e) ¿Se podría resolver el problema del punto d) declarando a la clase `CPolinomio` amiga de `CTermino`? ¿Por qué?

---

### [1er Parcial 2022] Práctica - Clase cRadioReloj con herencia múltiple

Escriba un programa en lenguaje C++ que permita ejecutar el conjunto de sentencias mostradas en el siguiente `main()` (almacenado en `main.cpp`):

```cpp
#include "cRadioReloj.h"

int main()
{
    cReloj clock1,           // por defecto: 0 0 0
           clock2(3, 4, 50),
           clock3(clock2);

    clock1.setTiempo(23, 58, 59);
    clock1.setMarca("primero");
    clock2.setMarca("segundo");
    clock3.setMarca("reloj copiado");

    cRadio radio1,           // por defecto: 95.5 FM false
           radio2(103.3),
           radio3(860.0, AM),
           radio4(radio3);

    radio4.setPrendido(true);

    cRadioReloj alarma1(cTime(23, 59, 59)),
                alarma2(clock1.getTiempo(), cTime(8, 29, 58));

    alarma1.setPrendido(true);
    alarma1.setAlarma(12, 59, 59);
    alarma2.setBanda(AM);

    alarma1.incrementarTiempo();
    cTime tiempo = alarma1.getTiempo();
    cout << "Hora mostrada en la radio_alarma1: " << tiempo << '\n';

    alarma2.incrementarTiempo();
    tiempo = alarma2.getTiempo();
    cout << "Hora mostrada en la radio_alarma2: " << tiempo << '\n';

    alarma2.incrementarTiempo();
    tiempo = alarma2.getTiempo();
    cout << "Hora mostrada en la radio_alarma2: " << tiempo << '\n';

    if(alarma1.verificarAlarma())
        cout << "La alarma 1 esta prendida" << endl;
    else
        cout << "La alarma 1 esta apagada" << endl;

    return 0;
}
```

Las clases involucradas tienen los siguientes atributos privados/protected:

```cpp
class cReloj { cTime time; char *marca; };
class cRadio { float frecuencia; TipoDeBanda banda; bool prendido; };
class cRadioReloj : public cRadio, public cReloj { cTime alarma; TipoAlarma tipo; bool prendido; };
class cTime { unsigned int hora, minuto, segundo; };
```

Se debe realizar correcta modularización. El programa debe compilar sin errores ni warnings. Gestionar adecuadamente el uso de la memoria dinámica.
