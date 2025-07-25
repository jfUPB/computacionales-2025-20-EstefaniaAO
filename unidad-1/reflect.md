# Unidad 1

## 🤔 Fase: Reflect

### Actividad 05:
*Autoevaluación.*

---

#### _Parte 1: recuperación de conocimiento (retrieval practice)_

#### Describe con tus palabras las tres fases del ciclo Fetch-Decode-Execute. ¿Qué rol juega el Program Counter (PC) en este ciclo?

1. **Fetch:** El computador toma la instrucción desde la memoria RAM usando la dirección almacenada en el Program Counter (PC).
2. **Decode:** Se interpreta la instrucción: el hardware identifica qué operación debe realizarse y con qué datos.
3. **Execute:** El computador ejecuta la operación: puede ser una suma, mover datos a registros o hacer un salto.

**El PC** tiene la dirección de la instrucción actual. Después del ciclo, se incrementa automáticamente (a menos que haya un salto), apuntando a la siguiente instrucción.

---

#### ¿Cuál es la diferencia fundamental entre una instrucción-A (que empieza con @) y una instrucción-C (que involucra D, M, A, etc.) en el lenguaje ensamblador de Hack? Da un ejemplo de cada una.

- Una **instrucción-A** (por ejemplo `@10`) carga el valor 10 en el registro A. Sirve para acceder a una dirección de memoria o establecer una constante.
- Una **instrucción-C** (por ejemplo `D=M+1`) realiza cálculos o mueve datos entre registros. No comienza con `@`.

---

#### Explica la función de los siguientes componentes del computador Hack: el registro D, el registro A y la ALU.

- **Registro A:** Guarda direcciones de memoria o valores constantes.
- **Registro D:** Guarda datos para operaciones, especialmente con la ALU. Es temporal.
- **ALU:** Hace operaciones como suma, resta, comparaciones, etc., usando los valores de A y D.

---

#### ¿Cómo se implementa un salto condicional en Hack? Describe un ejemplo (p. ej., saltar si el valor de D es mayor que cero).

Se usa una instrucción-C con condición de salto, como `D;JGT`. Lo que sucede es que estas hacen la comparación con 0, no con otro numero elegido.
Con el jemplo puesto:

```asm
@10
D=A
@RESULTADO
D;JGT
```

Este bloque salta a la etiqueta `RESULTADO` **solo si D > 0**.

---

#### ¿Cómo se implementa un loop en el computador Hack? Describe un ejemplo (p. ej., un loop que decremente un valor hasta que llegue a cero).

Usamos etiquetas y saltos condicionales. Ejemplo:

```asm
@i
M=5        // inicia i en 5
(LOOP)
@i
D=M
@END
D;JEQ      // Si i == 0, salta a END
@i
M=M-1      // i = i - 1
@LOOP
0;JMP      // Salta de nuevo al inicio
(END)
```

Este ciclo se repite hasta que `i` llega a 0.

---

#### ¿Cuál es la diferencia entre la instrucción D=M y la instrucción M=D?

- `D=M`: carga en D el valor de la dirección apuntada por A.
- `M=D`: guarda en la dirección de A el valor que hay en D.

---

#### Describe brevemente qué se necesita para leer un valor del teclado (KBD) y para “pintar” un pixel en la pantalla (SCREEN).

- Para *leer el teclado*, se accede a la dirección de memoria `KBD` (24576). Si una tecla está presionada, allí aparecerá su código.
- Para *pintar en pantalla**, se escriben valores binarios en direcciones a partir de `SCREEN` (16384). Cada bit representa un pixel.

---

#### _Parte 2: reflexión sobre tu proceso (metacognición)_

---

#### ¿Cuál fue el concepto o actividad más desafiante de esta unidad para ti y por qué?

Los **saltos condicionales** fueron lo más difícil, especialmente distinguir entre `JEQ`, `JGT`, `JLT`, etc., porque cambian completamente el flujo del programa. Requiere entender bien los valores que tiene D y cómo se comportan los saltos. Además de entender las etiquetas y la lógica detrás

---

#### La metodología de “predecir, ejecutar, observar y reflexionar” fue central en nuestras actividades. ¿En qué momento esta metodología te resultó más útil para entender algo que no tenías claro?

Principalmente cuando se introdujeron los saltos con mayor o menor que, ya que no sabía qué esperar, pero estos cambian completamente el flujo del programa, entonces me permitía reajustar mis otras predicciones y entender mejor cómo funciona el sistema.

---

#### Describe un momento “¡Aha!” que hayas tenido durante estas dos semanas. ¿Qué estabas haciendo cuando ocurrió?

Cuando entendí los saltos y las etiquetas. Al principio, confundí las etiquetas como (LOOP) con comentarios o marcadores como en otros lenguajes (por ejemplo en C#), pero luego entendí que realmente representan una dirección específica en el programa.
Cuando hago @LOOP, no es solo una nota visual, sino que el ensamblador traduce eso a la dirección numérica donde está la etiqueta (LOOP).

También comprendí que las instrucciones de salto condicional como D;JEQ, D;JGT, etc., siempre comparan contra 0, no contra un número arbitrario. Así que si quiero saber si un valor es mayor que 1, tengo que hacer la operación adecuada antes (como restar 1 y luego ver si el resultado es mayor que 0).



---

#### Pensando en la próxima unidad, ¿Qué harás diferente en tu proceso de estudio para aprender de manera más efectiva?

Me voy a enfocar en *hacer predicciones más conscientes* antes de ejecutar el código. También planeo tomarme más tiempo en repasar lo que hace cada instrucción, en vez de memorizarlo. Así evitar más errores por suposición.

### Actividad 06:
*Coevaluación.*

---
