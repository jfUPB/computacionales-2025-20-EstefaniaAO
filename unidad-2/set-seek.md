# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01

<img width="1424" height="764" alt="image" src="https://github.com/user-attachments/assets/d5adeb7a-0fd8-4a96-ab69-349774ff2ac7" />

  ``` asm
@SCREEN
M=1
  ```

Predicción:
La instrucción @SCREEN carga en A la dirección 16384 (la primera posición del mapa de pantalla). Luego, con M=1, se va a escribir el valor 1 en esa dirección, lo que debería encender el primer bit del primer pixel en la esquina superior izquierda de la pantalla (ponerlo negro).

Ejecución:
Exactamente como predije, A se vuelve 16384 y se guarda el valor 1 en esa posición de RAM, lo cual enciende el primer pixel.

Reflexión:
Este fue sencillo, pero sirve para entender cómo interactúan las instrucciones @ y M= con la pantalla. Me ayudó a ver cómo un solo bit ya cambia un pixel, y que SCREEN es solo un alias para la dirección 16384.

en C++

  ``` c++
screen = 1;
screen= 0xFFFF;
  ```

### Actividad 02

---
<img width="1419" height="780" alt="image" src="https://github.com/user-attachments/assets/1ffd430b-bbb6-4336-87ec-430ce81808fc" />

  ``` asm
@SCREEN
M=-1
  ```

@SCREEN carga en A la dirección 16384. Con M=-1 se va a escribir -1 en esa posición, que en binario es todos unos (0xFFFF), así que debería encender no solo el primer pixel sino los primeros 16 pixels de la esquina superior izquierda (porque un word controla 16 píxeles horizontales).

Ejecución:
Se escribe -1 en RAM[SCREEN], es decir en la dirección 16384. El primer word de la pantalla queda con todos sus bits en 1, y se ven 16 píxeles negros consecutivos en la parte superior izquierda.

Reflexión:
Comparando con el experimento anterior (M=1), entendí mejor que cada word controla 16 pixeles, y el valor que se escribe en memoria determina exactamente cuáles bits/pixeles se encienden o se apagan. Aquí ya no era un solo punto, sino toda una franja.

en C++:

  ``` c++
screen=-1;
screen= 0xFFFF;
  ```

### Actividad 03

---

  ``` asm
@CONTADOR
M=-1
@SCREEN
M=0

// "d" tiene código 100 y "i" tiene código 105
(LEER)
@KBD
D=M
@100
D=D-A
@DERECHA
D;JEQ

@KBD
D=M
@105
D=D-A
@IZQUIERDA
D;JEQ

@LEER
0;JMP

(DERECHA)
@CONTADOR
D=M
@SCREEN
A=D+A
M=0

//
@CONTADOR
M=M+1
D=M
@SCREEN
A=D+A
M=-1

@LEER
0;JMP

(IZQUIERDA)
@CONTADOR
D=M
@SCREEN
A=D+A
M=-1

@LEER
0;JMP
  ```

  ```c++
int contador = 0;
char tecla;
char* screen = (char*)0x4000; 
char* teclado = (char*)0x6000; // DKBD

while (true) {
    tecla = *teclado;

    if (tecla == 'd') {
        screen[contador] = 0;    // borra posición actual
        contador++;              // bueve a la derecha
        screen[contador] = 0xFF; // lo dibuja
    }

    if (tecla == 'i') {
        screen[contador] = 0;    // borra posición actual
        contador--;              // mueve a la izquierda
        screen[contador] = 0xFF; // lo dibuja
    }
}
  ```


#### Predicciones antes de ejecutar (con base en el código ASM):
- Líneas iniciales (@CONTADOR, M=-1)
Supongo que aquí se está iniciando un contador en -1 para que luego al sumarle 1 quede en 0 y empiece desde el principio de la pantalla. Aunque no estoy del todo segura, porque si empieza en -1, pensé que podría causar problemas

- Líneas @KBD, D=M, @100, D=D-A...
Parece que se está leyendo el teclado. El valor ASCII de "d" es 100, así que si presiono esa tecla debería irse a la etiqueta DERECHA. Lo mismo con "i" que vale 105. Pero no sé si el teclado lee rápido o si se necesita soltar la tecla para que funcione de nuevo.

- Bloque DERECHA y IZQUIERDA
Creo que cada bloque borra la posición actual del pixel y luego lo mueve en la dirección correcta. Me parece curioso que en el bloque de izquierda no esté M=M-1 (aunque en este código sí lo está, menos mal). Me pregunto si el pixel va a salir de la pantalla si sigo presionando muchas veces.

- Pantalla (@SCREEN, A=D+A)
Esto está accediendo a una dirección de memoria que representa la pantalla. No sabía que así se movía la posición del pixel, pero con el A=D+A creo que se está sumando el valor de D al inicio de SCREEN para ir avanzando o retrocediendo según el contador.

#### Ejecución y observación:
Cuando cargo el programa y presiono "d", el pixel se mueve a la derecha, como esperaba.
Si presiono "i", se mueve hacia la izquierda.
Si mantengo presionada una tecla, se mueve muchas veces seguidas. Me hace pensar que sería mejor agregar un pequeño delay o ver si hay una forma de detectar cuándo la tecla se suelta.
El pixel puede desaparecer si me paso de la pantalla (por ejemplo, hacia direcciones negativas del contador), pero como decía el enunciado, no importa para esta actividad.
Si cambio el M=-1 por otro valor, el brillo del pixel cambia, lo cual también podría usarse para hacer que se vea más tenue o más brillante según la dirección o el estado del movimiento.


