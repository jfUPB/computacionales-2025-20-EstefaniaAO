# Unidad 1

## 🛠 Fase: Apply

### Actividad 02
(ya realizada en la fase set-seek)

---

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

### Actividad 03  

*Escribe un programa que compare el valor almacenado en la dirección de memoria 5 con el valor 10.*
*Si el valor es menor que 10, guarda el valor 1 en la dirección 7.*
*Si el valor es mayor o igual a 10, guarda el valor 0 en la dirección 7.*

---

#### Código del programa

```asm
@5       
D=M       // D = RAM[5]
@10
D=D-A     // D = RAM[5] - 10
@MENOR
D;JLT     // Si D < 0 (RAM[5] < 10) ir a MENOR

@7        
M=0       // Si no saltó, entonces RAM[5] >= 10 → guardar 0
@END
0;JMP     // Saltar al final

(MENOR)
@7
M=1       // RAM[5] < 10 → guardar 1

(END)
```

#### Simulación paso a paso  
##### Usando la metodología de predecir, ejecutar, observar y reflexionar:


```asm
@5
```
**Predicción:** A se carga con la dirección 5  
**Resultado:** Correcto

```asm
D=M
```
**Predicción:** D se carga con el valor almacenado en RAM[5]  
**Resultado:** Correcto

```asm
@10
```
**Predicción:** A se carga con el valor 10  
**Resultado:** Correcto

```asm
D=D-A
```
**Predicción:** D ahora será el resultado de RAM[5] - 10  
**Resultado:** Correcto

```asm
@MENOR
```
**Predicción:** A se carga con la dirección de la etiqueta (MENOR)  
**Resultado:** Correcto

```asm
D;JLT
```
**Predicción:** Si D < 0, salta a MENOR. Es decir, si RAM[5] < 10.  
**Resultado:** Correcto, es una instrucción de salto condicional basada en el resultado de la ALU

```asm
@7
```
**Predicción:** A se carga con la dirección 7  
**Resultado:** Correcto

```asm
M=0
```
**Predicción:** Se guarda un 0 en RAM[7]  
**Resultado:** Correcto

```asm
@END
```
**Predicción:** A se carga con la dirección de la etiqueta END  
**Resultado:** Correcto

```asm
0;JMP
```
**Predicción:** Salta incondicionalmente a END  
**Resultado:** Correcto

```asm
(MENOR)
@7
```
**Predicción:** Si cayó aquí, es porque RAM[5] < 10. A se carga con la dirección 7  
**Resultado:** Correcto

```asm
M=1
```
**Predicción:** Se guarda un 1 en RAM[7]  
**Resultado:** Correcto

```asm
(END)
```
**Predicción:** Etiqueta para el final del programa, no hace nada  
**Resultado:** Correcto

##### Generalidad:

Al cambiar el valor en la dirección 5 y correr el programa, verifiqué que efectivamente:

- Si RAM[5] < 10, se almacena un 1 en RAM[7].
- Si RAM[5] >= 10, se almacena un 0 en RAM[7].

Se usaron instrucciones tipo C (`D=D-A`, `M=0`, etc.) y condicionales como `D;JLT`, que depende del resultado de la operación en la ALU para decidir si salta o no.

---

### Actividad 04  

*“Crea un programa que use un ciclo para sumar los números del 1 al 5 y guarde el resultado en la dirección de memoria 12.”*

---

####  Código del programa

```asm
@1
D=A
@i
M=D       // i = 1

@0
D=A
@sum
M=D       // sum = 0

(LOOP)
@i
D=M
@6
D=D-A
@END
D;JEQ     // Si i == 6, terminar

@i
D=M
@sum
M=M+D     // sum = sum + i

@i
M=M+1     // i++

@LOOP
0;JMP

(END)
@sum
D=M
@12
M=D       // guardar resultado final en RAM[12]
```

#### Simulación paso a paso  

##### Usando la metodología de predecir, ejecutar, observar y reflexionar:

```asm
@1
```
**Predicción:** A se carga con el valor 1  
**Resultado:** Correcto

```asm
D=A
```
**Predicción:** D = 1  
**Resultado:** Correcto

```asm
@i
```
**Predicción:** A se carga con la dirección de la variable `i`
**Resultado:** Correcto

```asm
M=D
```
**Predicción:** RAM[i] = 1  
**Resultado:** Correcto (se inicializa `i` en 1)

```asm
@0
```
**Predicción:** A se carga con el valor 0  
**Resultado:** Correcto

```asm
D=A
```
**Predicción:** D = 0  
**Resultado:** Correcto

```asm
@sum
```
**Predicción:** A se carga con la dirección de la variable `sum` (por ejemplo, RAM[17])  
**Resultado:** Correcto

```asm
M=D
```
**Predicción:** RAM[sum] = 0  
**Resultado:** Correcto (see inicializa `sum` en 0)

---

#### Inicio del ciclo

```asm
(LOOP)
@i
```
**Predicción:** A se carga con la dirección de `i`  
**Resultado:** Correcto

```asm
D=M
```
**Predicción:** D = valor actual de i  
**Resultado:** Correcto

```asm
@6
```
**Predicción:** A = 6  
**Resultado:** Correcto

```asm
D=D-A
```
**Predicción:** D = i - 6. Sirve para saber si ya llegamos a 6  
**Resultado:** Correcto

```asm
@END
```
**Predicción:** A se carga con la dirección de la etiqueta END  
**Resultado:** Correcto

```asm
D;JEQ
```
**Predicción:** Si D == 0 (es decir, i == 6), salta a END  
**Resultado:** Correcto

```asm
@i
```
**Predicción:** A = dirección de `i`  
**Resultado:** Correcto

```asm
D=M
```
**Predicción:** D = valor de i  
**Resultado:** Correcto

```asm
@sum
```
**Predicción:** A = dirección de `sum`  
**Resultado:** Correcto

```asm
M=M+D
```
**Predicción:** sum = sum + i  
**Resultado:** Correcto

```asm
@i
```
**Predicción:** A = dirección de `i`  
**Resultado:** Correcto

```asm
M=M+1
```
**Predicción:** i = i + 1  
**Resultado:** Correcto

```asm
@LOOP
```
**Predicción:** A = dirección de LOOP (vuelve al inicio del ciclo)  
**Resultado:** Correcto

```asm
0;JMP
```
**Predicción:** Salta incondicionalmente a LOOP  
**Resultado:** Correcto

---

#### Al terminar el ciclo

```asm
(END)
@sum
```
**Predicción:** A = dirección de `sum`  
**Resultado:** Correcto

```asm
D=M
```
**Predicción:** D = valor acumulado en `sum`  
**Resultado:** Correcto — debería ser 15 al final

```asm
@12
```
**Predicción:** A = 12  
**Resultado:** Correcto

```asm
M=D
```
**Predicción:** RAM[12] = 15 (resultado final de la suma 1+2+3+4+5)  
**Resultado:** Correcto

##### Generalidad:

- Inicialicé `i` en 1 y `sum` en 0.
- El ciclo (`LOOP`) va sumando `i` a `sum` mientras `i` sea menor que 6.
- Cuando `i == 6`, se rompe el ciclo y se guarda el resultado en la dirección 12.

El bucle funciona gracias a la comparación `D;JEQ`, usando la ALU para restar y decidir si seguir o salir.  
Es una implementación simple pero útil para entender cómo se controlan ciclos en el computador Hack.
