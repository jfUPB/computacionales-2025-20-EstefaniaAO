# Unidad 2

## 🔎 Fase: Set + Seek

<img width="1424" height="764" alt="image" src="https://github.com/user-attachments/assets/d5adeb7a-0fd8-4a96-ab69-349774ff2ac7" />

@SCREEN
M=1

en++

screen = 1;
screen= 0xFFFF;

---
<img width="1419" height="780" alt="image" src="https://github.com/user-attachments/assets/1ffd430b-bbb6-4336-87ec-430ce81808fc" />

@SCREEN
M=-1

en C++:

screen=-1;
screen= 0xFFFF;

---

En proceso

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
