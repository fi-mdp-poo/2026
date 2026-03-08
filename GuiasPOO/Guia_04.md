# Guía de Trabajos Prácticos - Unidad 4
## Materia: Programación III

## Ejercicios de la Práctica

Unidad Nº4: Herencia
Preguntas orientadoras
1) Explique con sus palabras el concepto de Herencia.
2) ¿Cuál es la relación entre los conceptos de clase, subclase y superclase?
3) ¿En el lenguaje C++, la derivación es una forma de expresar la relación ____?
4) Una clase derivada es una ____________ de una clase base.
5) ¿Para qué se utiliza la palabra clave protected?
6) Los datos y miembros protegidos son completamente _________ para las clases derivadas.
7) Las subclases pueden ___________ el comportamiento de su superclase.
8) ¿Puede una clase derivada hacer que la función pública de una clase base sea privada?

Ejercicios
1)  Modele una clase denominada "polar", que represente a los números complejos en forma polar. 
Nota: "polar" deriva de "complejo", la clase que usted programó en el ejercicio 1 de la unidad III. En el proyecto se debe reutilizar la implementación de la clase complejo mediante el archivo binario (extensión .o). La definición de la clase "polar" deberá contener la interfaz y los atributos mostrados en la imagen que sigue:


2) Implemente la clase "becario" vista en teoría. Analice todas las ambigüedades que puedan surgir al escribir el programa que prueba dicha clase.

3) Implemente en lenguaje C++ el siguiente diagrama UML.
4) Encuentre diferentes maneras de corregir el error del siguiente programa para que imprima el valor 10:

5)  Se desea modelar una Empresa con empleados. Una empresa conoce a todos sus empleados, y estos pueden ser de planta permanente o temporaria, además hay gerentes, que también son empleados de planta permanente, pero siguen un régimen salarial particular.
Cuando un empleado es de planta permanente cobra la cantidad de horas trabajadas por $30, más antigüedad ($10 por año de antigüedad), más salario familiar. Cuando es de planta temporaria, no cobra antigüedad y cobra la cantidad de horas trabajadas por $20, más salario familiar. El salario familiar es $20 por cada hijo, los empleados casados además cobran $10 por su esposa/o. Un gerente cobra de manera similar a un empleado de planta permanente pero su hora trabajada vale $40, por antigüedad se le pagan $15 por año, mientras que el salario familiar es el mismo que el de los empleados de planta permanente y temporal.
a) Realizar un diagrama de clases.
b) Implementar el método #montoTotal en la clase Empresa, que retorna el monto total que la empresa debe pagar en concepto de sueldos a sus empleados (Definir e implementar todas las clases y métodos necesarios).

## Ejercicios de Exámenes Anteriores

> Los siguientes ejercicios fueron extraídos de primeros parciales y recuperatorios de años anteriores. Corresponden a los temas de esta unidad.

---

### [1er Parcial 2022 / 2021] Teoría - Herencia múltiple: salida del programa con constructores

Analice el siguiente código e indique el orden exacto de los mensajes en pantalla para la construcción de `obj1` (constructor por defecto) y `obj2` (constructor con parámetro 20). Justifique:

```cpp
class derivada : private base, public otra_base {
public:
    derivada(void) { base::num = 10; otra_base::num = 20; num = 35; }
    derivada(int valor) : base(valor/10), otra_base(valor*10) { num = 45; }
    // ...
};

int main()
{
    derivada obj1, obj2 = 20;
    cout << "\nValor de num en obj1 = " << obj1.GetNum() << endl;
    cout << "\nValor de num en obj2 = " << obj2.GetNum() << endl;
    obj1.SetNum(50);
    obj2.SetNum(100);
    cout << "\nValor de num en obj1 = " << obj1.GetNum() << endl;
    cout << "\nValor de num en obj2 = " << obj2.GetNum() << endl << endl;
    return 0;
}
```

---

### [1er Parcial 2022] Práctica - Clase cRadioReloj con herencia múltiple (ver Guía 2)

Implementar las clases `cReloj`, `cRadio`, `cRadioReloj` y `cTime` tal como se describe en el ejercicio de la Guía 2, haciendo foco en la correcta implementación de la herencia múltiple (`cRadioReloj : public cRadio, public cReloj`) y en la resolución de ambigüedades que puedan surgir.

---

### [1er Parcial 2021] Práctica - Clases con herencia y composición: CPolinomio y cRadioReloj

Implementar dos ejercicios en el mismo examen:

**Ejercicio 1:** Implementar las clases `CTermino` y `CPolinomio` tal como se describe en la Guía 1, asegurando que el programa compile sin errores a partir del `main()` dado. La suma de polinomios debe producir: `5x⁵ + 7x² – x + 5.25`.

**Ejercicio 2:** Implementar las clases `cReloj`, `cRadio`, `cRadioReloj` y `cTime` tal como se describe en la Guía 2.

---

### [1er Parcial 2023] Práctica - CMatriz y CPantalla con herencia (ver Guía 3)

Implementar las clases `CMatriz`, `CPantalla` y `CColor` tal como se describe en el ejercicio de la Guía 3, asegurando que la clase `CPantalla` herede o componga correctamente con `CMatriz`, y que los métodos `ajustarColor`, `borrarVerde` y `reforzarRojo` estén correctamente implementados.

