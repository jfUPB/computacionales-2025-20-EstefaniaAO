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

### B. Verificación con el depurador

#### 3. Salida real observada

--- Experimento con paso de parámetros ---
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
  
<img width="585" height="375" alt="image" src="https://github.com/user-attachments/assets/5fc8313e-9f26-46c7-abb9-361ab5468ea9" />


#### 4. 
4.1 Paso por valor

Diferencia: en mi predicción puse que val_A quedaba en 30, pero en realidad sigue en 20.
Esto confirma que al pasar por valor se crea una copia del dato en el stack, y cualquier cambio se queda dentro de la función.

Captura aquí: ventana de variables mostrando val_A = 20 después de sumaPorValor.

4.2 Paso por referencia

Aquí sí coincidió mi predicción: val_B pasó a 30.
Esto es porque la función recibe una referencia, es decir, una especie de alias de la misma variable. No hay copia.

Captura aquí: ventana de variables mostrando val_B = 30 después de sumaPorReferencia.

4.3 Paso por puntero

También coincidió: val_C quedó en 30.
Con punteros, la función recibe una dirección de memoria, y al desreferenciar (*a) se puede modificar directamente la variable original.

Captura aquí: ventana de variables mostrando val_C = 30 después de sumaPorPuntero.

4.4 Variable estática

En cada llamada a ejecutarContador, contador_estatico fue aumentando: 1, luego 2, luego 3.
A diferencia de una variable local normal, no se destruye al salir de la función: se guarda en memoria estática y “recuerda” su valor entre llamadas.

Captura aquí: ventana de variables mostrando contador_estatico cambiando en cada llamada.


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
