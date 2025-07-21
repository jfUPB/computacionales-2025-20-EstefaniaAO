# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01 
---

*Experimento 01: Crea un archivo llamado program.asm y copia el código del programa anterior. Ejecuta el programa en el simulador de la CPU Hack y observa cómo se comporta. ¿Qué sucede? ¿Qué valor se almacena en la dirección de memoria 16? ¿Por qué crees que es ese valor? ¿Qué instrucciones se ejecutan en cada ciclo Fetch-Decode-Execute? ¿Qué cambios observas en el contenido de la memoria y los registros?*

*Experimento 02: Escribe un programa en lenguaje ensablador que sume los números 5 y 10, y almacene el resultado en la dirección de memoria 20. Utiliza el simulador de la CPU Hack para ejecutar tu programa y verifica que el resultado es correcto.*

---

#### Reporta tus observaciones para cada experimento en tu bitácora de aprendizaje.

Al ejecutarlo en la página salen los registros con la nomenclatura:

- **PC (Program Counter):** mantiene la dirección de la instrucción que se va a ejecutar.
- **A (registro A):** puede guardar una constante o una dirección de memoria.
- **D (registro D):** es un registro, no es una dirección de memoria.
- **M:** se refiere a la memoria RAM en la dirección guardada en el registro A.

En el experimento 01: 

El programa carga los valores 1 y 2, los suma y guarda el resultado (3) en la dirección de memoria 16. Después entra en un ciclo infinito con 0;JMP para detener la ejecución del programa en esa misma línea.

```asm
@1        // A = 1
D=A       // D = 1
@2        // A = 2
D=D+A     // D = 1 + 2 = 3
@16       // A = 16
M=D       // RAM[16] = 3
@END      // A = dirección de la etiqueta END
(END)     // etiqueta END
0;JMP     // salta a END ya que esta es la dirección de memoria que A registró.
```

**¿Qué valor se almacena en la dirección de memoria 16 y por qué?** Se almacena el número 3 porque es el resultado de sumar 1 y 2, y luego ese valor se guarda directamente en `RAM[16]` mediante la instrucción `M=D`.

**¿Qué instrucciones se ejecutan en cada ciclo Fetch-Decode-Execute?** Cada instrucción sigue el mismo ciclo. Primero se hace el *fetch*, donde la CPU lee la instrucción desde la ROM según el valor del PC. Luego se hace el *decode*, donde la CPU interpreta si es una instrucción de tipo A (por ejemplo `@5`) o de tipo C (como `D=D+A`). Finalmente se ejecuta la instrucción, ya sea cargando un valor, haciendo una suma o guardando en memoria. Este ciclo se repite con cada línea del programa.

Es decir, cada instrucción pasa por los siguientes pasos:

1. **Fetch:** la CPU lee la instrucción desde la memoria ROM usando el valor del PC.
2. **Decode:** la CPU interpreta la instrucción y que es lo que debe realizar.
3. **Execute:** se realiza la operación indicada, como mover datos o hacer una suma.

En este experimento se puede ver de la siguiente forma:

- `@1` → se carga el número 1 en A.
- `D=A` → el valor de A (1) pasa al registro D.
- `@2` → se carga el número 2 en A.
- `D=D+A` → se suma D (1) + A (2) y se guarda el resultado (3) en D.
- `@16` → se apunta a la dirección de memoria 16.
- `M=D` → se guarda el valor 3 en la RAM[16].

**¿Qué cambios se observan en la memoria y en los registros?** Durante la ejecución se ve cómo los valores de los registros cambian paso a paso. A y D se actualizan constantemente. Por ejemplo, D primero guarda el 1 y luego el 3. La memoria RAM cambia solo en la dirección 16, donde se almacena el valor final de la operación. El contador de programa (PC) se incrementa normalmente con cada instrucción, pero al final se queda atrapado en la etiqueta `(END)` por el salto infinito.

Experimento 02: suma de 5 + 10 y almacenamiento en RAM[20]

```asm
@5        // A = 5
D=A       // D = 5
@10       // A = 10
D=D+A     // D = 5 + 10 = 15
@20       // A = 20
M=D       // RAM[20] = 15
(END)
@END
0;JMP     // salto infinito
```

- `@5` → carga el número 5 en el registro A.  
- `D=A` → copia el valor de A (5) al registro D.  
- `@10` → carga el número 10 en A.  
- `D=D+A` → suma el valor que ya tenía D (5) con el nuevo valor de A (10); ahora D = 15.  
- `@20` → apunta a la dirección de memoria 20 (RAM[20]).  
- `M=D` → guarda el contenido de D (15) en la RAM[20].  
- `(END)` → define una etiqueta llamada END (no hace nada por sí sola).  
- `@END` → carga la dirección de la etiqueta END en A.  
- `0;JMP` → salta a la dirección END, creando un bucle infinito para detener la ejecución del programa.

Este fue el primer programa que escribí desde cero en el simulador del computador Hack. Al principio me costó un poco entender cómo se combinaban las instrucciones para hacer una operación simple como sumar, pero después de observar el experimento anterior, me di cuenta que lo importante es seguir el orden lógico: cargar un valor en D, luego sumarlo con otro y guardar el resultado en memoria.

Algo que descubrí al probarlo fue que si cambiaba el orden de las instrucciones, por ejemplo tratando de hacer `D=D+A` sin haber cargado antes un valor en D, el resultado era incorrecto o simplemente no se almacenaba nada útil en la RAM. También entendí mejor cómo funciona el acceso a la memoria: la instrucción `@20` no pone el número 20 en la RAM, sino que apunta el registro A a esa dirección, y recién con `M=D` se guarda el dato allí.

Otra cosa que me pareció interesante fue ver cómo el simulador actualiza en tiempo real los valores de A, D y M. Me ayudó a visualizar lo que está pasando en cada línea. Por ejemplo, después de ejecutar `D=D+A`, el valor de D pasó a ser 15, y luego pude verificar que efectivamente `RAM[20]` tenía ese valor. 


#### ¿Qué diferencia hay entre los datos almancenados en la memoria ROM y en la RAM?

La ROM contiene el programa como tal: todas las instrucciones que se escribieron en ensamblador, como `@5`, `D=A`, `D=D+A`, etc. Es de solo lectura, así que el programa no puede cambiar su contenido mientras se ejecuta. En cambio, la RAM es donde se almacenan los datos que cambian durante la ejecución, como resultados de operaciones, valores temporales y cualquier cosa que se guarde usando `M=D`. La CPU puede leer y escribir en la RAM en tiempo real. Por ejemplo, en estos experimentos, los valores 3 y 15 que se guardaron en las direcciones 16 y 20 respectivamente, están en la RAM, no en la ROM.

### Actividad 02

Inicialmente en este experimento pude conocer que hay valores predefinidos, por ejemplo `@SCREEN = 16384`. Luego procedí a hacer las predicciones, despues de ejecutarlo todo consulte con inteligencias artificiales el funcionamiento real de cada linea y si estaba en lo correcto:

0: @SCREEN  
Predicción: Pone el valor de A como SCREEN  
→ Correcto.

1: D=A  
Predicción: D=A por lo que los dos valen SCREEN  
→ Correcto.

2: @i  
Predicción: A igual a 16  
→ Correcto.

3: M=D  
Predicción: asigna el valor de D a la posición de A en la RAM  
→ Correcto.

4: @KBD  
Predicción: convierte a A en kbd (24576)  
→ Correcto.

5: D=M  
Predicción: creo que le da a D el valor almacenado en la línea KBD de la RAM pero como es 0, D se vuelve 0  
→ Correcto.

6: @KEYPRESSED  
Predicción: A = 20  
→ Correcto (el número exacto depende del compilador, pero está bien asumir que es la dirección de esa etiqueta).

7: D;JNE  
Predicción: ni idea, no sabría qué hace D;JNE  
→ **Explicación**: Salta solo si D ≠ 0 (JNE = "Jump if Not Equal"). Es una condición que se cumple si hay una tecla presionada.

8: @i  
Predicción: A=16  
→ Correcto.

9: D=M  
Predicción: D toma el valor de RAM[16], que era SCREEN  
→ Correcto.

10: @SCREEN  
Predicción: A vuelve a ser 16384  
→ Correcto.

11: D=D-A  
Predicción: Le resta SCREEN al índice, o sea, D pasa a ser cuántas posiciones lleva avanzadas  
→ Correcto.

12: @READKEYBOARD  
Predicción: A = posición de la etiqueta READKEYBOARD  
→ Correcto.

13: D;JLE  
Predicción: No entiendo qué hace eso  
→ **Explicación**: JLE = "Jump if Less or Equal". Salta si D ≤ 0, o sea, si el índice llegó al principio de la pantalla o antes.

14: @i  
Predicción: A = 16 otra vez  
→ Correcto.

15: M=M-1  
Predicción: Le resta 1 al valor guardado en RAM[16], o sea, retrocede el índice  
→ Correcto.

16: A=M  
Predicción: A ahora vale el contenido de RAM[16], o sea, el índice  
→ Correcto.

17: M=0  
Predicción: Apaga ese píxel, porque escribe un 0 en pantalla  
→ Correcto.

18: @READKEYBOARD  
Predicción: A = línea de esa etiqueta  
→ Correcto.

19: 0;JMP  
Predicción: Salta incondicionalmente a READKEYBOARD  
→ Correcto.

20: (KEYPRESSED)  
Predicción: Es una etiqueta, no hace nada directamente  
→ Correcto.

21: @i  
Predicción: A = 16  
→ Correcto.

22: D=M  
Predicción: D = RAM[16], o sea, la dirección del píxel donde se va a escribir  
→ Correcto.

23: @KBD  
Predicción: A = 24576  
→ Correcto.

24: D=D-A  
Predicción: D ahora es el resultado de RAM[16] - 24576  
→ Correcto.

25: @READKEYBOARD  
Predicción: A = etiqueta  
→ Correcto.

26: D;JGE  
Predicción: No sé bien qué hace eso  
→ **Explicación**: JGE = "Jump if Greater or Equal". Si D ≥ 0 (es decir, si ya se pasó del área de pantalla), vuelve a leer teclado sin escribir.

27: @16  
Predicción: A = posición de la variable i  
→ Correcto.

28: A=M  
Predicción: A se vuelve el valor contenido en RAM[16]  
→ Correcto.

29: M=-1  
Predicción: Escribe -1 en esa posición, o sea, prende ese píxel  
→ Correcto.

30: @i  
Predicción: A = 16  
→ Correcto.

31: M=M+1  
Predicción: Aumenta el valor de i, o sea, avanza el índice  
→ Correcto.

32: @READKEYBOARD  
Predicción: A = esa etiqueta  
→ Correcto.

33: 0;JMP  
Predicción: Salta incondicionalmente otra vez  
→ Correcto.


**Una instrucción que usa la ALU** (Unidad Aritmético-Lógica, por sus siglas en inglés: Arithmetic Logic Unit) sería `D=D-A`. En esa línea, la ALU toma los valores de los registros D y A, los resta, y guarda el resultado en D. Es una operación de tipo aritmético. También se usan comparaciones como `D;JLE`, que en el fondo también dependen de los resultados de la ALU para saber si se cumple una condición.

**El registro PC (Program Counter)** es el que indica qué instrucción se va a ejecutar en el siguiente ciclo. Va aumentando línea por línea, pero también puede saltar a otra posición si hay una instrucción de salto como `@LABEL` seguido de `JMP`.

**La diferencia entre `@i` y `@READKEYBOARD`** es que `@i` hace referencia a una dirección de memoria RAM variable (en este caso el índice que apunta a la pantalla), mientras que `@READKEYBOARD` es una etiqueta que apunta a una posición en el código, es decir, una instrucción. `@i` se usa para acceder/modificar datos; `@READKEYBOARD` para saltar dentro del programa.

**Para leer el teclado y mostrar información en la pantalla**, primero se accede a la dirección de memoria especial `@KBD`, que guarda el código de la tecla presionada (si no hay nada, es 0). Luego se hace una comparación para ver si ese valor cambia. Para mostrar información, se escribe un valor en la dirección de memoria correspondiente a la pantalla, que empieza en `@SCREEN`. Por ejemplo, `M=-1` prende un píxel y `M=0` lo apaga.

**Un bucle del programa** es todo lo que está bajo la etiqueta `(READKEYBOARD)`. El programa vuelve a esta línea usando saltos incondicionales (`0;JMP`) o condicionales, de modo que queda en un ciclo constante donde revisa el teclado, actualiza la pantalla y repite.

**Una condición en el programa** está en la instrucción `D;JNE`. Esa línea pregunta: “¿el valor en D es distinto de cero?”. Si sí, salta a otra parte del código; si no, sigue normal. Este tipo de condiciones sirven para tomar decisiones según lo que esté pasando en memoria o en los registros.
