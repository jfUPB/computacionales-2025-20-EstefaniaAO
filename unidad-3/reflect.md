# Unidad 3


## 🤔 Fase: Reflect.

### Parte 1: recuperación de conocimiento (Retrieval Practice)

#### Explica con tus propias palabras qué es el stack y qué es el heap en C++.

Stack es la zona de memoria donde se guardan variables locales y parámetros, se maneja automáticamente y es más segura pero limitada. El heap es una zona de memoria que debe ser controlada con new y delete manualmente, es mas grande y flexible pero puede producir fallos de memoria si no se es cuidadoso.

#### Describe las tres formas de pasar parámetros a una función en C++ (valor, referencia y puntero). Para cada una, explica qué sucede en memoria y cuándo usarías cada método.

- Valor: Se crea una copia en el stack, los cambios a esa copia no afectan la variable original.
- Referencia (&): Se pasa una referencia al valor original, si afecta a la variable original.
- Puntero (*): Se pasa la dirección de memoria de la variable, se accede al valor original directamente a través de este puntero.

#### ¿Qué diferencia hay entre una variable local, una variable global y una variable local estática? ¿En qué segmento del mapa de memoria se almacena cada una?

- Local: Se declara y funciona dentro del mismo bloque, funciona en el stack y se crea al entrar al bloque y se destruye al salir sola.
- Global: Declarada fuera de las funciones, existe en toda la ejecución del programa y está en la memoria estática.
- Estática: Declarada dentro de un bloque o clase con "static", mantiene su valor y está en la memoria estática. Pero a diferencia de global se comparte entre objetos (En caso de estar dentro de una clase).

#### Explica qué es un objeto en C++ desde la perspectiva de memoria. ¿Dónde se almacenan los miembros de instancia y dónde los miembros estáticos?

Un objeto es una instancia de una clase que se crea en una parte de la memoria, tiene miembros de instancia que se almacenan en su memoria, usualmente en el stack pero también puede ser en el heap. Los miembros estáticos se almacenan en la memoria estática. Tiene una dirección, tiene atributos (almacena valores) y puede tener punteros.

### Parte 2: transferencia y análisis de situación nueva

#### Imagina que trabajas en un equipo de desarrollo de videojuegos y tu compañero te muestra este código problemático que está causando crashes en el juego:

~~~ C++
#include <iostream>
using namespace std;

class Enemigo {
public:
    static int totalEnemigos;
    int vida;
    int* armas;

    Enemigo(int v) : vida(v) {
        totalEnemigos++;
        armas = new int[3];
        armas[0] = 10; armas[1] = 15; armas[2] = 20;
    }
};

int Enemigo::totalEnemigos = 0;

void crearEscuadron() {
    for(int i = 0; i < 5; i++) {
        Enemigo soldado(100);
        soldado.vida -= 10;
    }
    cout << "Escuadrón creado. Total enemigos: " << Enemigo::totalEnemigos << endl;
}

int main() {
    crearEscuadron();
    crearEscuadron();
    return 0;
}
~~~

#### Análisis de problemas: identifica al menos dos problemas serios en este código relacionados con el manejo de memoria. Explica por qué cada uno es problemático.

No hay destructor para Enemigo, también se esta usando un puntero crudo (como sin auto gestión y además al crear una copia por ejemplo todos llevarian al mismo punto de memoria) para las armas en vez de un std:Array en el stack para cada objeto.

#### Predicción de comportamiento: ¿Qué valor mostrará totalEnemigos después de ejecutar el programa? ¿Por qué ocurre esto?

10 ya que el static se comparte en toda la clase y en todas las instancias de enemigos a diferencia de un public por ejemplo.

#### Propuesta de solución: escribe una versión corregida de la clase Enemigo que solucione los problemas identificados. Explica brevemente cada cambio que hiciste.

~~~ C++
#include <iostream>
using namespace std;

class Enemigo {
public:
    static int totalEnemigos;
    int vida;
    std::array<int, 3> armas;

    Enemigo(int v) : vida(v) {
        totalEnemigos++;
        armas[0] = 10;
        armas[1] = 15;
        armas[2] = 20;
    }
};

~Enemigo() {
    std::cout << "Enemigo destruido. Vida restante: " << vida << std::endl;
}


int Enemigo::totalEnemigos = 0;

void crearEscuadron() {
    for(int i = 0; i < 5; i++) {
        Enemigo soldado(100);
        soldado.vida -= 10;
    }
    cout << "Escuadrón creado. Total enemigos: " << Enemigo::totalEnemigos << endl;
}

int main() {
    crearEscuadron();
    crearEscuadron();
    return 0;
}
~~~

Cambios:

Array dentro de cada objeto enemigo para cambiar el puntero anterior, de forma que no se guarden en la misma dirección de memoria y además se libere. Se evita la fuga de memoria.
~~~ C++
    std::array<int, 3> armas;

    Enemigo(int v) : vida(v) {
        totalEnemigos++;
        armas[0] = 10;
        armas[1] = 15;
        armas[2] = 20;
~~~

Agrego destructor a Enemigo para manejar el objeto en el heap.
~~~ C++
~Enemigo() {
    std::cout << "Enemigo destruido. Vida restante: " << vida << std::endl;
}
~~~

### Parte 3: reflexión metacognitiva

#### De todos los conceptos que exploraste en esta unidad (stack vs heap, paso de parámetros, ciclo de vida de objetos, etc.), ¿Cuál consideras que es el más crítico para evitar errores en programas reales? ¿Por qué?

Ser muy consciente con el manejo del heap, que en proyectos pasados no tenía en cuenta y también tener cuidado con el uso de punteros crudos e una clase.

#### ¿Cómo cambió tu comprensión sobre lo que realmente es un “objeto” después de comparar C++ con C#? ¿Qué implicaciones prácticas tiene esta diferencia?

No conocía que habían otras formas de crear instancias o valores más allá del heap, conociendo ahora el stack también se vuelve mucho más completo y facilita más el entendimiento de como gestionar ambos.

#### Si tuvieras que explicar a un compañero de semestres anteriores por qué es importante entender la gestión de memoria en programación, ¿Qué le dirías en máximo 3 oraciones?

Porque es importante optimizar el programa más allá de su funcionamiento, no es bueno tener fugas de memorias y entre más rápido apliques buenas prácticas de programación mejor.
