# Unidad 2

## 🔎 Fase: Set + Seek

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

---

En proceso

  ``` asm
@CONTADOR
M=0
@SCREEN
M=-1

@KBD
D=M
@100
D=D-A
@DERECHA
D;JEQ

@KBD
D=M
@100
D=D-A
@IZQUIERDA
D;JEQ


(DERECHA)
@CONTADOR
D=M
@SCREEN
A=D+A
M=-1

//
@CONTADOR
M=M+1
D=M
@SCREEN
A=D+A
M=-1

(IZQUIERDA)
@CONTADOR
D
  ```
