# Unidad 2


## 🤔 Fase: Reflect


~~~ asm
// Carga los valores del 1 al 10 en RAM[16] a RAM[25]
@1
D=A
@16
M=D
@2
D=A
@17
M=D
@3
D=A
@18
M=D
@4
D=A
@19
M=D
@5
D=A
@20
M=D
@6
D=A
@21
M=D
@7
D=A
@22
M=D
@8
D=A
@23
M=D
@9
D=A
@24
M=D
@10
D=A
@25
M=D

// sum = 0 (R13)
@0
D=A
@R13
M=D

// j = 0 (R14)
@0
D=A
@R14
M=D

// BASE = 16 (R15)
@16
D=A
@R15
M=D

// LOOP para sumar los elementos
(LOOP)
  // if j >= 10 goto END
  @R14
  D=M
  @10
  D=D-A
  @END
  D;JGE

  // D = j + BASE
  @R14
  D=M
  @R15
  D=D+M

  // D = M[BASE + j]
  A=D
  D=M

  // sum += D
  @R13
  M=D+M

  // j++
  @R14
  M=M+1

  @LOOP
  0;JMP

(END)
@END
0;JMP
~~~
