# Unidad 3

## 🔎 Fase: Set + Seek

Valor inicial de val_A: 20
  -> Dentro de sumaPorValor, 'a' ahora es: 30
Valor final de val_A: 30   
   // (1) pensé que el cambio sí se reflejaba en val_A, porque sumaron 10 dentro de la función.

Valor inicial de val_B: 20
  -> Dentro de sumaPorReferencia, 'a' ahora es: 30
Valor final de val_B: 20   
   // (2) supuse que al salir de la función volvía a ser 20, como si se “reiniciara”.

Valor inicial de val_C: 20
  -> Dentro de sumaPorPuntero, '*a' ahora es: 30
Valor final de val_C: 20   
   // (3) pensé que era igual al paso por valor, que no se modificaba el original.

--- Experimento con variables estáticas ---
  -> Llamada a ejecutarContador. Valor de contador_estatico: 1
  -> Llamada a ejecutarContador. Valor de contador_estatico: 1
  -> Llamada a ejecutarContador. Valor de contador_estatico: 1   
   // (4) asumí que se reiniciaba cada vez que entraba a la función.


Mi predicción inicial es que solo el primer valor de val_A cambia, y que las otras variables no se ven afectadas realmente. También supuse que contador_estatico se reinicia cada vez.

#### Diagrama de memoria

+-------------------------------+
|       Segmento de código      |
|   main(), ejecutarContador(), |
|   sumaPorValor(),             |
|   sumaPorReferencia(),        |
|   sumaPorPuntero()            |
+-------------------------------+
| Variables globales y estáticas|
| - contador_global (global)    |
| - contador_estatico (estática)|
+-------------------------------+
|           Heap                | 
|   (No se usa en este programa)|
+-------------------------------+
|           Stack               |
| - val_A, val_B, val_C (main)  |
| - a (parámetro en cada función|
|   sumaPorValor, sumaPorRef,   |
|   sumaPorPuntero)             |
+-------------------------------+

#### Autoexplicación :)  :

Segmento de código:
Aquí están todas las instrucciones del programa, incluidas las funciones main, ejecutarContador, sumaPorValor, sumaPorReferencia y sumaPorPuntero. En este punto, la función main sigue existiendo en este segmento porque aún no termina su ejecución.

Variables globales y estáticas:
En esta región se encuentran contador_global y contador_estatico. Ambas persisten durante toda la ejecución del programa. La diferencia es que contador_global se declaró fuera de cualquier función, mientras que contador_estatico está dentro de ejecutarContador() pero, al ser estática, también pertenece a este segmento y conserva su valor entre llamadas.

Heap:
En este programa no uso asignación dinámica (new o malloc), así que este segmento vacío.

Stack:
Aquí están las variables locales de main: val_A, val_B y val_C. Como estoy justo antes de que main termine, estas todavía existen en el stack.
Por otro lado, el parámetro a de la función sumaPorValor ya no está en el stack, porque esa función terminó su ejecución y al salir se liberó la memoria que ocupaba.

### B. Verificación con el depurador

#### 3. Salida real observada

  ``` --- Experimento con paso de parámetros ---
Valor inicial de val_A: 20
  -> Dentro de sumaPorValor, 'a' ahora es: 30
Valor final de val_A: 20

Valor inicial de val_B: 20
  -> Dentro de sumaPorReferencia, 'a' ahora es: 30
Valor final de val_B: 30

Valor inicial de val_C: 20
  -> Dentro de sumaPorPuntero, '*a' ahora es: 30
Valor final de val_C: 30

--- Experimento con variables estáticas ---
  -> Llamada a ejecutarContador. Valor de contador_estatico: 1
  -> Llamada a ejecutarContador. Valor de contador_estatico: 2
  -> Llamada a ejecutarContador. Valor de contador_estatico: 3
``` 
  
<img width="585" height="375" alt="image" src="https://github.com/user-attachments/assets/5fc8313e-9f26-46c7-abb9-361ab5468ea9" />


#### 4. 
4.1 Paso por valor

Diferencia: en mi predicción puse que val_A quedaba en 30, pero en realidad sigue en 20.
Esto confirma que al pasar por valor se crea una copia del dato en el stack, y cualquier cambio se queda dentro de la función.

<img width="1865" height="711" alt="image" src="https://github.com/user-attachments/assets/47225240-c87d-425f-b0ee-b836a22e4997" />


4.2 Paso por referencia

Aquí sí coincidió mi predicción: val_B pasó a 30.
Esto es porque la función recibe una referencia, es decir, una especie de alias de la misma variable. No hay copia.

<img width="1919" height="918" alt="image" src="https://github.com/user-attachments/assets/628d5704-723a-473a-a444-2899567f122b" />


4.3 Paso por puntero

También coincidió: val_C quedó en 30.
Con punteros, la función recibe una dirección de memoria, y al desreferenciar (*a) se puede modificar directamente la variable original.

<img width="1915" height="913" alt="image" src="https://github.com/user-attachments/assets/232b5f2d-0031-44f3-9408-d12bbe5af55b" />


4.4 Variable estática

En cada llamada a ejecutarContador, contador_estatico fue aumentando: 1, luego 2, luego 3.
A diferencia de una variable local normal, no se destruye al salir de la función: se guarda en memoria estática y “recuerda” su valor entre llamadas.

<img width="946" height="242" alt="image" src="https://github.com/user-attachments/assets/2cad44bc-9f1e-4be2-8deb-7f155a8aa7b3" />
<img width="944" height="272" alt="image" src="https://github.com/user-attachments/assets/8cf27e8a-a93b-47bb-ad92-8bd9d8bba9a1" />
<img width="947" height="254" alt="image" src="https://github.com/user-attachments/assets/d2ae9dc0-9b57-42c9-9f06-70c4be381ef0" />



#### 5. Diferencias observadas:

Al revisar el programa en depuración me di cuenta de estas diferencias clave respecto a lo que había predicho:

Paso por valor: confirmé que la variable original no cambia, porque se crea una copia en el stack. Mi error inicial fue asumir que porque dentro de la función sí aumentaba, ese cambio se quedaba guardado.

Paso por referencia: observé que la referencia actúa como un alias de la variable original, de manera que cualquier modificación dentro de la función se refleja en la variable fuera de ella.

Paso por puntero: funciona parecido a la referencia, pero con la diferencia de que explícitamente se trabaja con direcciones de memoria y con el operador *. Verlo en el depurador ayuda porque se pueden ver las direcciones apuntadas.

Variable estática: lo más interesante fue notar que contador_estatico se queda en el segmento de datos estáticos y no en el stack. Eso explica que “recuerde” el valor de la ejecución anterior. Una variable local normal se destruye cada vez que la función termina, pero la estática persiste hasta que acaba todo el programa.

#### Explica con tus propias palabras el comportamiento de contador_estatico. ¿Por qué “recuerda” su valor entre llamadas a la función ejecutarContador? ¿En qué se diferencia de una variable local normal?

Una variable local normal se crea y destruye cada vez que la función termina.
En cambio, una variable estática local como contador_estatico se guarda en el segmento de datos estáticos (igual que las globales), por eso solo se inicializa una vez y conserva su valor en todas las llamadas, aunque no sea visible desde main.

Eso significa que: 
- Se inicializa una sola vez.
- Persiste en memoria toda la ejecución del programa.
- Aunque no es visible fuera de la función, “recuerda” su valor cada vez que lo llamo.

