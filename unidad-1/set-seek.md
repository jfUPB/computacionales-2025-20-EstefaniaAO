# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01 
---

*Experimento 01: Crea un archivo llamado program.asm y copia el código del programa anterior. Ejecuta el programa en el simulador de la CPU Hack y observa cómo se comporta. ¿Qué sucede? ¿Qué valor se almacena en la dirección de memoria 16? ¿Por qué crees que es ese valor? ¿Qué instrucciones se ejecutan en cada ciclo Fetch-Decode-Execute? ¿Qué cambios observas en el contenido de la memoria y los registros?*

*Experimento 02: Escribe un programa en lenguaje ensablador que sume los números 5 y 10, y almacene el resultado en la dirección de memoria 20. Utiliza el simulador de la CPU Hack para ejecutar tu programa y verifica que el resultado es correcto.*

---

#### Reporta tus observaciones para cada experimento en tu bitácora de aprendizaje.

Hay valores predefinidos, por ejemplo @SCREEN=16384

Al ejecutarlo en la página salen los registros con la nomenclatura:

- **PC *(Program Counter)*:** mantiene la dirección de la instrucción que se va a ejecutar.

- **A *(registro A)*:** puede guardar una constante o una dirección de memoria.

- **D *(registro D)*:** es un registro, no es una dirección de memoria.

- **M:** se refiere a la memoria RAM en la dirección guardada en el registro A.

En el experimento 01: 

~~~
@1        // A = 1
D=A       // D = 1
@2        // A = 2
D=D+A     // D = 1 + 2 = 3
@16       // A = 16
M=D       // RAM[16] = 3
@END      // A = dirección de la etiqueta END
(END)     // etiqueta END
0;JMP     // salta a END ya que esta es la dirección de memoria que A registró.
~~~


#### ¿Qué diferencia hay entre los datos almancenados en la memoria ROM y en la RAM?

### Actividad 02

#### Identifica una instrucción que use la ALU y explica qué hace.


#### ¿Para qué sirve el registro PC?


#### ¿Cuál es la diferencia entre @i y @READKEYBOARD?


#### Describe qué se necesita para leer el teclado y mostrar información en la pantalla.


#### Identifica un bucle en el programa y explica su funcionamiento.


#### Identifica una condición en el programa y explica su funcionamiento.

