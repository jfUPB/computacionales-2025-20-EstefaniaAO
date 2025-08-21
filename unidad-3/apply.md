# Unidad 3


## 🛠 Fase: Apply

  ```C++
#include <iostream>
#include <string>

class Personaje {
public:
    std::string nombre;
    int* estadisticas;

    Personaje(std::string n, int vida, int ataque, int defensa) {
        nombre = n;
        estadisticas = new int[3];
        estadisticas[0] = vida;
        estadisticas[1] = ataque;
        estadisticas[2] = defensa;
        std::cout << "Constructor: nace " << nombre << std::endl;
    }

    void imprimir() {
        std::cout << "Personaje " << nombre
            << " [Vida: " << estadisticas[0]
            << ", ATK: " << estadisticas[1]
            << ", DEF: " << estadisticas[2]
            << "]" << std::endl;
    }
};

void simularEncuentro() {
    std::cout << "\n--- Iniciando encuentro ---" << std::endl;
    Personaje heroe("Aragorn", 100, 20, 15);

    Personaje copiaHeroe = heroe;
    copiaHeroe.nombre = "Copia de Aragorn";

    std::cout << "Saliendo del encuentro..." << std::endl;
}

int main() {
    simularEncuentro();
    std::cout << "\nSimulación terminada." << std::endl;
    return 0;
}
  ```

### Diagnóstico del problema (análisis):

#### Compila y ejecuta el código.

<img width="1919" height="1005" alt="image" src="https://github.com/user-attachments/assets/c318a62d-bb02-4529-b89e-8ef7d2a2ce7d" />


#### Identifica y explica con detalle al menos dos errores críticos de gestión de memoria en la clase Personaje y su uso en simularEncuentro.
_Para cada error, responde:_

1. Fuga de memoria (no liberar lo que se reserva)
- **¿Cuál es el error?**
No hay destructor que libere la memoria reservada con new int[3].
  
- **¿Por qué ocurre? Explica el mecanismo a nivel de memoria (stack, heap, punteros)**
Lo identifiqué igual que en la Actividad 07, donde aprendí que al reservar en el heap debo usar delete[]. Aquí nunca se hace, entonces la memoria queda ocupada.
  
- **¿Cuál es su consecuencia?**
Cada vez que se crean objetos, la memoria no se devuelve al sistema. Esto hace que el programa consuma cada vez más RAM.

2. Copia superficial del objeto
- **¿Cuál es el error?**
Cuando se ejecuta Personaje copiaHeroe = heroe;, los dos objetos comparten el mismo bloque de memoria para estadisticas.
  
- **¿Por qué ocurre? Explica el mecanismo a nivel de memoria (stack, heap, punteros)**
Lo relaciono con la Actividad 08, donde vi que copiar un objeto con punteros copia solo la dirección, no los valores. Entonces, cuando se destruyen, ambos intentan liberar el mismo bloque de memoria.
  
- **¿Cuál es su consecuencia?**
Esto puede provocar corrupción de memoria, errores aleatorios o cierres inesperados del programa.
  
#### Solución y refactorización (síntesis y creación):

Para corregirlo, utilicé la técnica de la Actividad 09, donde aprendí que se pueden usar estructuras seguras como std::array para que la memoria se gestione automáticamente. Así evité punteros crudos, fugas y la copia superficial.

*En la actividad 9: En lugar de reservar memoria manualmente con new y luego tener que liberarla con delete, dejamos que C++ lo haga solo. Así, cuando se copia un objeto, sus datos se copian correctamente sin compartir la misma dirección de memoria y cuando el objeto se destruye, la memoria se libera sola*

```C++
#include <iostream>
#include <string>
#include <array>

class Personaje {
public:
    std::string nombre;
    std::array<int, 3> estadisticas;

    Personaje(std::string n, int vida, int ataque, int defensa)
        : nombre(n)
    {
        estadisticas[0] = vida;
        estadisticas[1] = ataque;
        estadisticas[2] = defensa;
        std::cout << "Constructor: nace " << nombre << std::endl;
    }

    ~Personaje() {
        std::cout << "Destructor: muere " << nombre << std::endl;
    }

    void imprimir() {
        std::cout << "Personaje " << nombre
            << " [Vida: " << estadisticas[0]
            << ", ATK: " << estadisticas[1]
            << ", DEF: " << estadisticas[2]
            << "]" << std::endl;
    }
};

void simularEncuentro() {
    std::cout << "\n--- Iniciando encuentro ---" << std::endl;
    Personaje heroe("Aragorn", 100, 20, 15);

    Personaje copiaHeroe = heroe;
    copiaHeroe.nombre = "Copia de Aragorn";

    std::cout << "Saliendo del encuentro..." << std::endl;
}

int main() {
    simularEncuentro();
    std::cout << "\nSimulación terminada." << std::endl;
    return 0;
}
  ```
  <img width="1919" height="1027" alt="image" src="https://github.com/user-attachments/assets/0a884eaf-da82-475b-8b4e-d6046a8f5381" />

Justificación de la Solución:

- Uso de std::array en lugar de punteros dinámicos
Para manejar colecciones de valores de manera segura, evitando fugas y errores de memoria al utilizar estructuras que gestionan automáticamente su contenido (Actividad 09).
Gracias a esto, cada copia del objeto tiene sus propios valores y no se comparten direcciones.

- Destructor simple para depuración (Actividad 10)
Mantengo el destructor para imprimir mensajes, pero no para liberar memoria, porque ya no es necesario.
Los destructores sirven para ver el ciclo de vida del objeto. Aquí ya no necesito liberar nada manualmente, pero mantengo el mensaje para seguir la pista en la ejecución.

- Evitar punteros sin gestión automática
El uso directo de new y delete puede ser mala idea, es preferible estructuras más seguras para manejar memoria (Actividad 07). Reemplazarlos por estructuras seguras hace la clase más segura y evita fugas y errores difíciles de rastrear.


