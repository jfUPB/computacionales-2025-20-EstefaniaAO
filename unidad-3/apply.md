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

CAPTURA

#### Identifica y explica con detalle al menos dos errores críticos de gestión de memoria en la clase Personaje y su uso en simularEncuentro.
_Para cada error, responde:_

1. 
- **¿Cuál es el error?**

  
- **¿Por qué ocurre? Explica el mecanismo a nivel de memoria (stack, heap, punteros)**

  
- **¿Cuál es su consecuencia?**

  
- **Solución y refactorización (síntesis y creación):**
  

Re-escribe la clase Personaje para que sea segura en cuanto a memoria. Debes utilizar los conocimientos adquiridos en esta unidad y por tanto tu solución no debería usar la Regla de los tres que probablemente sea la solución que te ofrezca una IA.
Presenta el código completo de tu clase Personaje corregida.
Justificación de la Solución:
