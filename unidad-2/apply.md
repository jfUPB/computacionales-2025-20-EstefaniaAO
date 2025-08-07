# Unidad 2


## 🛠 Fase: Apply

### Actividad 06

  ``` c++
int arr[] = {1,2,3,4,5,6,7,8,9,10};
int sum = 0;

for (int j = 0; j < 10; j++) {
    sum = sum + arr[j];
}
  ```

Según mi entendimiento, tengo primero que iniciar todas los valores del array del 1 al 10 desde la posición 16.

Los datos del arreglo están almacenados desde la dirección 16.

---

#### Paso 1:
Primero quiero crear el arreglo.

  ``` asm
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
  ```

#### Objetivo
Almacenar los valores 1 a 10 desde la dirección de memoria 16.

#### Predicción
Si cargo los valores directamente y los asigno a @16, @17, ... @25, puedo verificar que el arreglo quedó bien guardado.

#### Prueba
Verifiqué en el simulador que las direcciones de la 16 a la 25 tenían los valores correctos.

<img width="1229" height="528" alt="image" src="https://github.com/user-attachments/assets/d017afeb-21e4-45df-a8d2-f8d4ddb1a304" />

#### Reflexión
Todo funciona.

#### Paso 2:
Realizar la suma

  ``` asm
@sum
M=0        // sum = 0
@0
D=A
@j
M=D        // j = 0
(LOOP)
@j
D=M
@10
D=D-A
@END
D;JGE      // Si j >= 10, salta al final
@j
D=M
@16
A=D+A      
D=M
@sum
M=M+D
@j
M=M+1
@LOOP
0;JMP
(END)
@END
0;JMP
  ```

#### Objetivo
Recorrer el arreglo con un índice j y sumar los elementos.

#### Predicción
Voy a crear una variable j e intentar hacer un bucle. Es probable que falle porque no estoy manejando bien los punteros.

#### Prueba
El código no se ejecuta, no suma bien los valores del arreglo.

#### Reflexión
El problema está en que no tengo una base definida para el arreglo (@16 + j no se puede hacer así directamente). Además, la suma de direcciones con @16 y @j no es válida así.

#### Paso 2:
Realizar la suma

  ``` asm
@sum
M=0        // sum = 0
@0
D=A
@j
M=D        // j = 0
(LOOP)
@j
D=M
@10
D=D-A
@END
D;JGE      // Si j >= 10, salta al final
@j
D=M
@16
A=D+A      
D=M
@sum
M=M+D
@j
M=M+1
@LOOP
0;JMP
(END)
@END
0;JMP
  ```

#### Objetivo
Recorrer el arreglo con un índice j y sumar los elementos.

#### Predicción
Voy a crear una variable j e intentar hacer un bucle. Es probable que falle porque no estoy manejando bien los punteros.

#### Prueba
El código no se ejecuta, no suma bien los valores del arreglo.

#### Reflexión
El problema está en que no tengo una base definida para el arreglo (@16 + j no se puede hacer así directamente). Además, la suma de direcciones con @16 y @j no es válida así.


#### Paso 3:
Definir el puntero y arreglar lo de la suma, para ello agrego:

  ``` asm
@16
D=A
@BASE
M=D      // BASE = 16
  ```

Y reemplazo:

  ``` asm
@j
D=M
@BASE
A=M+D     // arr[j]
D=M
  ```

#### Objetivo
Crear una variable BASE que contenga la dirección 16 y usarla para acceder a arr[j] correctamente.

#### Predicción
Si uso A=M+D después de cargar BASE, puedo acceder a la posición correcta del arreglo.

#### Prueba
Hice una prueba sumando solo los primeros tres elementos, y ahora sí da la suma correcta.

#### Reflexión
Ya estoy accediendo bien al arreglo. Solo me falta completar el programa para recorrer todo.

#### Paso 4;
Pensé que este código final funcionaría pero no cargo en el simulador:

  ``` asm
// sum = 0
@sum
M=0
// j = 0
@0
D=A
@j
M=D
(LOOP)
@j
D=M
@10
D=D-A
@END
D;JGE         // if j >= 10, jump to END
// D = j
@j
D=M
@16
A=D+A         // Esto NO funciona como se espera, es el error que estamos evaluando
D=M           // D = arr[j]
// sum = sum + arr[j]
@sum
M=M+D
// j++
@j
D=M
D=D+1
@j
M=D
@LOOP
0;JMP
(END)
@END
0;JMP
  ```

Después de consultar con IA: 

"Ese error es muy común cuando pasas del pseudo-código a Hack. El problema está en estas líneas:
  ``` asm
@BASE
A=M+D        // ❌ ESTO NO EXISTE EN HACK
  ```
En el lenguaje ensamblador Hack no puedes hacer A=M+D. Solo puedes cargar un valor constante con @valor, o mover D, A o M, y hacer operaciones como D=D+A, pero no puedes sumar registros directamente en una sola instrucción A."

Además:
"Error loading memory: Line 82: Invalid ASM value: M+D. Did you mean D+M?"

Las correcciones a realizar en un futuro:

Obtener BASE (la dirección inicial del arreglo).
Obtener j (el índice).
Sumar BASE + j en D.
UsarA=D para apuntar a la dirección BASE + j.

#### FINAL:

  ``` asm
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
// --- sum = 0
@sum
M=0
// --- j = 0
@0
D=A
@j
M=D
// --- BASE = 16
@16
D=A
@BASE
M=D
// --- LOOP para sumar los elementos del arreglo
(LOOP)
    // if j >= 10 goto END
    @j
    D=M
    @10
    D=D-A
    @END
    D;JGE
    // addr = BASE + j
    @j
    D=M
    @BASE
    A=M
    D=D+A
    @addr
    M=D
    // D = *addr = arr[j]
    @addr
    A=M
    D=M
    // sum += arr[j]
    @sum
    M=D+M
    // j++
    @j
    M=M+1
    // Volver al inicio del LOOP
    @LOOP
    0;JMP
(END)
@END
0;JMP
  ```

<img width="1257" height="858" alt="image" src="https://github.com/user-attachments/assets/4e165dcb-0cfd-45e9-8f2e-edc5f80c965d" />

- Al principio me costó entender cómo usar punteros, especialmente con la parte de @BASE / A=M y luego D=D+A para sumar posiciones.
- También me equivoqué escribiendo M=M+D, pero recordé que Hack solo acepta ciertas combinaciones exactas como D+M.
- Tuve más en cuenta que A guarda direcciones y D guarda valores, y eso es clave para moverse con punteros.
- Ver cómo se construye todo paso a paso me ayudó a entender mucho mejor cómo funciona el acceso a memoria en bajo nivel.


Defino a sum y el int j

``` asm

```

Defino el loop

``` asm

``` 

