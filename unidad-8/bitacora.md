# Bitácora de aprendizaje de la unidad 8

## **Índice**

- [Actividad 01](#actividad-01)
- [Actividad 02](#actividad-02)
- [Actividad 03](#actividad-03)
- [Actividad 04](#actividad-04)
- [Actividad 05](#actividad-05)
- [Autoevaluación](#autoevaluación)

## Actividad 01:

<img width="1301" height="763" alt="image" src="https://github.com/user-attachments/assets/aa150e94-9fae-44ed-83c2-c4cd9c3e765d" />
<img width="391" height="378" alt="image" src="https://github.com/user-attachments/assets/f3e20f39-b13d-491e-82a7-a72fa262b440" />

Hay un circulo moviendose a través de la pantalla, al hacer click el circulo se queda quieto. Pensé que este era el funcionamiento pero en realidad es porque se freezea el programa. Según el código esperaba que el circulo cambiara de tamaño, pero tengo entendido que al estar manejando un solo hilo y con tanta información entonces se bloquea al usar heavyComputation()

~~~C++
void ofApp::heavyComputation() {
    double result = 0;
    for (int j = 0; j < 1000000000; ++j) {
        result += sqrt(j);
    }
    circleSize = ofRandom(20, 70);
    ofLog() << "Circle size: " << circleSize;
}
~~~

### Al cambiar el código:

ofApp.h
~~~C++
#pragma once
#include "ofMain.h"
#include "ofThread.h"

class ofApp : public ofBaseApp, public ofThread {
public:
	float x = 0;
	float speed = 3;
	float circleSize = 50;

	void setup();
	void draw();
	void mousePressed(int x, int y, int button);
	void heavyComputation();
	void startHeavyComputation();
	void threadedFunction() override;
	void exit();
};

~~~

ofApp.cpp
~~~C++
#include "ofApp.h"

void ofApp::setup() {
	ofSetFrameRate(60);
	ofSetWindowShape(400, 400);
}

void ofApp::draw() {

	ofBackground(220);
	ofSetColor(0);
	lock();
	ofDrawCircle(x, ofGetHeight() / 2, circleSize);
	unlock();
	x = fmod(x + speed, ofGetWidth());
}

void ofApp::mousePressed(int x, int y, int button) {
	if (!isThreadRunning()) {
		std::cout << "Starting heavy computation in a thread" << std::endl;
		startThread();
	} else {
		std::cout << "Thread is already running" << std::endl;
	}
}

void ofApp::threadedFunction() {
	heavyComputation();
	std::cout << "Thread ends" << std::endl;
}

void ofApp::heavyComputation() {
	double result = 0;
	for (int j = 0; j < 1000000000; ++j) {
		result += sqrt(j);
	}
	lock();
	ofSeedRandom();
	circleSize = ofRandom(20, 70);
	unlock();
	std::cout << "Circle size: " << circleSize << std::endl;
}

void ofApp::exit() {
	if (isThreadRunning()) {
		stopThread();
		waitForThread();
	}
}

~~~

<img width="1096" height="603" alt="image" src="https://github.com/user-attachments/assets/a01253b2-90de-4f38-b8b5-a1086cb6e8fb" />

### Ejecuta el programa y haz clic en la ventana. Observa lo que sucede. ¿Qué es lo que ves? ¿Qué es lo que esperabas ver? ¿Por qué crees que sucede esto?

Al hacer click pasan algunos segundos y despues cambia el tamaño del circulo. Es lo que se podía esperar, quizá esperaba que sucediera más rapidamente o que aún se freezeara como en la versión anterior pero el funcionamiento como tal es bueno. Creo que se demora porque, como se indica en consola, el hilo se queda unos momentos procesando la información mientras la aplicación sigue funcionando. Al usar distintos hilos para distintas funciones es posible ejecutar calculos más pesados sin que deje de funcionar la aplicación.

~~~C++
void ofApp::threadedFunction() {
	heavyComputation();
	std::cout << "Thread ends" << std::endl;
}

void ofApp::heavyComputation() {
	double result = 0;
	for (int j = 0; j < 1000000000; ++j) {
		result += sqrt(j);
	}
	lock();
	ofSeedRandom();
	circleSize = ofRandom(20, 70);
	unlock();
	std::cout << "Circle size: " << circleSize << std::endl;
}
~~~

### En tus propias palabras, explica la diferencia entre concurrencia y paralelismo. ¿Por qué es importante entender esta diferencia al trabajar con hilos?

La concurrencia son varias tareas que progresan o parecen progresar al mismo tiempo, sin embargo esto no implica que necesariamente requiera varios nucleos. El paralelismo es cuando en realidad si se están ejecutando en tiempo real simultaneamente, usando varios núcleos al tiempo.

## Actividad 02:

### ¿En que partes del código se está protegiendo la variable circleSize?

Se maneja el circleSize en:

~~~C++
	lock();
	ofDrawCircle(x, ofGetHeight() / 2, circleSize);
	unlock();
~~~
~~~C++
	circleSize = ofRandom(20, 70);
~~~
~~~C++
	std::cout << "Circle size: " << circleSize << std::endl;
~~~
El lock y unlock se hace para implementar el mutual exclusion, por lo que si se estan implementando formas de proteger el acceso a la variable circleSize. Esto se hace porque el programa hace el calculo del tamaño y también dibuja al circulo simultaneamente, entonces entendiblemente se necesita prevenir la condición de carrera. Esto previene que los dos hilos accedan a la variable simultaneamente y que uno lo este modificando mientras el otro lo dibuja.

### Piensa qué ocurre con el paralelismo cuando se sincroniza el acceso a un recurso compartido. ¿Qué ocurre con el rendimiento del programa? ¿Es posible que el rendimiento se vea afectado por el uso de mutex? ¿Por qué?

Cuando usamos mutex para sincronizar el acceso a un recurso compartido, el paralelismo se reduce, porque los hilos ya no pueden acceder al recurso al mismo tiempo, como normalmente lo harían. Entonces el rendimiento sí puede verse afectado, porque en vez de ejecutar todo en paralelo, algunos hilos tienen que esperar a que el recurso esté libre. Básicamente, se vuelve un proceso más secuencial en esa parte del programa, y eso hace que el tiempo total de ejecución pueda aumentar. Igual es necesario para evitar errores, pero sí termina afectando el rendimiento cuando varios hilos compiten por el mismo recurso.

### Ejecuta el código y observa el resultado. ¿Qué ocurre si cambias el valor de la variable useLock? ¿Por qué crees que ocurre esto?

<img width="474" height="242" alt="image" src="https://github.com/user-attachments/assets/682848cb-e68b-4ae3-96af-786b04763025" />
<img width="424" height="133" alt="image" src="https://github.com/user-attachments/assets/cbfc288a-024e-4a54-8504-93f6d7d5234e" />
<img width="414" height="137" alt="image" src="https://github.com/user-attachments/assets/7b8d8e18-5a27-4a14-9b0e-719188bc8a22" />

Cuando corro el programa y cambio la variable useLock, se ve la diferencia de inmediato. Si useLock está en verdadero, el contador llega casi al valor esperado, porque los hilos se ordenan para usar la misma variable y no se pisan entre sí.

Cuando useLock está en falso, el contador no llega al valor que debería. Se pierde un montón de incrementos, porque todos los hilos están escribiendo al mismo tiempo sin control. Entonces el resultado sale mal. Eso pasa por la condición de carrera, y por eso sí es necesario usar lock() para evitar esos errores

### Explica en tus propias palabras ¿Cómo puede presentarse la condición de carrera en este caso? ¿Qué es lo que está pasando? Te pido que propongas un ejemplo.

La condición de carrera es básicamente un error cuando varios hilos intentan acceder a la misma variable en paralelo. La condición de carrera se presenta aquí porque varios hilos están intentando incrementar la misma variable counter al mismo tiempo. Aunque en el código solo vemos ++(*counter), el procesador en realidad no hace esa operación en un solo paso. Lo que pasa internamente es algo como: leer el valor desde memoria, guardarlo en un registro del procesador, incrementarlo en ese registro y luego escribirlo otra vez en memoria.

El problema es que si dos hilos leen el mismo valor antes de que alguno alcance a escribir su actualización, ambos van a terminar escribiendo un valor que ya quedó desactualizado. Entonces uno de los incrementos se pierde. Eso ocurre porque la operación de incremento no es atómica y puede ser interrumpida por otro hilo en cualquiera de esos pasos.

Por ejemplo, si el contador está en 10 y dos hilos lo quieren incrementar al mismo tiempo, los dos pueden leer 10, cada uno lo incrementa a 11, y al final ambos escriben 11. El resultado final es 11, cuando debería ser 12. Ahí es donde se ve la condición de carrera.

## Actividad 03:

<img width="1008" height="786" alt="image" src="https://github.com/user-attachments/assets/39af96e7-8468-489a-b03e-70ef74b2ddda" />
<img width="1006" height="759" alt="image" src="https://github.com/user-attachments/assets/d6c3d05b-fa58-4c5b-b4ed-f88eec536172" />

- Realiza cambios pequeños y específicos.
- Lanza una hipótesis sobre lo que crees que va a pasar.
- Ejecuta el código y observa lo que ocurre.
- ¿Tu hipótesis era correcta? ¿Por qué crees que ocurre esto?

### Experimento 1

Cambio esta línea
~~~
maxIterations = 100;
~~~
por
~~~
maxIterations = 10;
~~~
#### Hipótesis
Si bajo el número de iteraciones, el cálculo será más rápido y la imagen perderá detalle porque muchos puntos escaparán antes.

#### Resultado
La imagen se ve mucho más simple y con áreas planas sin detalle. El cálculo sí termina más rápido.
<img width="1012" height="755" alt="image" src="https://github.com/user-attachments/assets/93d0a339-d89e-418d-bc5e-f1919575257c" />

#### Razón
Menos iteraciones significan menos comprobaciones por píxel. Como el algoritmo para decidir si un punto pertenece o no al conjunto depende de repetir cálculos, detenerse temprano provoca que muchos puntos se clasifiquen como “escapados” sin analizar su comportamiento real. Eso reduce la riqueza del fractal y genera una imagen básica. También mejora el rendimiento porque hay menos trabajo total.

### Experimento 2
Cambio esta línea
~~~
maxIterations = 100;
~~~
por
~~~
maxIterations = 200;
~~~
#### Hipótesis
Si aumento el número de iteraciones, obtendré más detalle en el fractal y el cálculo será más lento porque hay más operaciones.

#### Resultado
La imagen muestra patrones más definidos y transiciones más suaves. El cálculo tarda más tiempo. Aunque la verdad la diferencia no es tan notoria como pensé.
<img width="997" height="748" alt="image" src="https://github.com/user-attachments/assets/1f9053e8-5f29-4145-bf0b-70070ca96db5" />

#### Razón
Al aumentar las iteraciones, se analiza con mayor precisión el comportamiento de cada punto en el plano complejo. Puntos cercanos a la frontera del conjunto necesitan más pasos para determinar si escapan o permanecen. Eso produce mejoras visuales, pero al mismo tiempo el costo computacional crece de forma directa con la cantidad de iteraciones, por lo tanto el render tarda más.

### Experimento 3
Cambio esta línea
~~~
maxIterations += 1;
~~~
por
~~~
maxIterations += 20;
~~~

#### Hipótesis
Cada vez que presione la tecla, el detalle aumentará mucho más rápido pero también la ejecución se volverá pesada enseguida.

#### Resultado
El fractal mejora unas pocas veces, se empieza a ver como más oscuro y también va subiendo el tiempo pero tampoco es demasiado.
<img width="918" height="686" alt="image" src="https://github.com/user-attachments/assets/4ebbba07-243c-4af5-a0ce-014f0a870485" />
<img width="1005" height="751" alt="image" src="https://github.com/user-attachments/assets/30c0e768-c68b-49ef-bb04-cfb11257ca15" />
<img width="1010" height="756" alt="image" src="https://github.com/user-attachments/assets/8503fb91-a060-4a68-bf25-0ba19c7ed89a" />
<img width="992" height="747" alt="image" src="https://github.com/user-attachments/assets/b02cb718-446b-47d6-b859-1444fe241948" />
<img width="1004" height="743" alt="image" src="https://github.com/user-attachments/assets/cc3f6916-94a6-4c77-990f-cb77a6bb3811" />

#### Razón
Incrementar un valor pequeño permite explorar progresivamente el nivel de detalle. Al aumentar en pasos grandes, se provoca un salto inmediato en carga computacional. Como cada pixel necesita recorrer todas las iteraciones nuevas, el programa consume más CPU y el tiempo de render puede crecer hasta niveles incómodos. No es un bloqueo total, pero sí una caída notable en respuesta visual.

### Experimento 4 (colores)
Cambio esta línea
~~~
float hue = ofMap(iterations, 0, maxIterations, 0, 255);
~~~
por
~~~
float hue = (iterations * 10) % 255;
~~~

#### Hipótesis
El color avanzará en saltos más marcados y aparecerán franjas de color más visibles, posiblemente creando patrones repetidos.

#### Resultado
La imagen muestra bandas de color más bruscas y repetitivas. El aspecto fractal se mantiene pero los colores ya no parecen un degradado suave, sino bloques periódicos. Aparte porque se le estan quitando la interpolación

<img width="1006" height="747" alt="image" src="https://github.com/user-attachments/assets/04849bab-d25d-4517-a0dd-087e41bbb0c5" />
<img width="1003" height="746" alt="image" src="https://github.com/user-attachments/assets/d4708c42-1649-4367-ad07-2287750645c3" />


#### Razón
La fórmula original asigna el color proporcionalmente al progreso de escape, creando una transición continua relacionada con la profundidad fractal. Al usar un módulo, el color repite un ciclo y ya no depende de una progresión lineal suave sino de saltos. Esto resalta las regiones con la misma cantidad de iteraciones en patrones visibles, lo cual puede ser estético pero cambia la interpretación visual matemática del conjunto.

## Actividad 04:

Sin hilos:
<img width="1012" height="754" alt="image" src="https://github.com/user-attachments/assets/46e08034-4e60-4465-9c61-f6fa26d76aa1" />
Con hilos:
<img width="1006" height="761" alt="image" src="https://github.com/user-attachments/assets/909c8169-8baa-46d4-a4f6-4ff35ad9cb54" />

### ¿Cuál es la estructura de datos principal que contiene la información de todos los boids y que es accedida por múltiples hilos?

La estructura principal es un `std::vector<Boid> boids` dentro de la clase `Flock`. Ese vector guarda todos los boids y tanto el hilo principal como el hilo secundario lo usan. El hilo principal lo usa para dibujar, y el hilo secundario lo usa para actualizar posiciones y velocidades.


### ¿Qué operaciones realiza el hilo trabajador sobre el vector de boids?

En `Flock::threadedFunction()` el hilo recorre el vector y realiza cálculos de comportamiento sobre cada boid. Lee posiciones y velocidades, calcula fuerzas, y luego actualiza los valores (lectura y escritura). Para eso usa `lock()` y `unlock()` para asegurar acceso exclusivo mientras modifica los datos.


### ¿Qué operación realiza el hilo principal sobre el vector en `ofApp::draw()`?

El hilo principal bloquea el acceso al vector, recorre los boids y los dibuja en pantalla con `boid.draw()`. En esta parte solo lee datos, no escribe, pero igual necesita el lock para evitar que el hilo de actualización modifique el vector al mismo tiempo.


### ¿Qué hacen `Flock::addBoid()` y `ofApp::mouseDragged()`?

`Flock::addBoid()` agrega un nuevo boid al vector.  
`ofApp::mouseDragged()` llama a `addBoid()` cuando arrastro el mouse, lo que hace que se agreguen boids dinámicamente mientras el programa está corriendo. Esa operación también requiere lock para que el vector no sea modificado al mismo tiempo que está siendo leído o escrito por otro hilo.

### Escenario donde la falta de sincronización rompe todo

El hilo que actualiza los boids está recorriendo el vector para calcular separación, cohesión y esas cosas. Está usando el tamaño del vector y moviéndose posición por posición.

Al mismo tiempo, yo arrastro el mouse y `mouseDragged()` llama a `addBoid()` y mete un boid nuevo al vector. Entonces el vector cambia de tamaño justo cuando el otro hilo lo está usando.

¿Qué pasa?  
Puede que el iterador quede apuntando a un lugar que ya no existe, o que el índice se salga de rango porque el tamaño cambió de repente. En ese punto el programa puede crashear o leer cosas que no importan.


### Dónde están los lock() y unlock()

- En `threadedFunction()` antes de recorrer los boids:
  - `lock()`
  - recorrer y actualizar boids
  - `unlock()`

- En `addBoid()`:
  - `lock()`
  - `boids.push_back(...)`
  - `unlock()`

Esos son los dos puntos donde se protege el acceso al vector.


### Cómo lock()/unlock() evita el problema

Cuando el hilo de actualización tiene el lock, nadie más puede tocar el vector. Entonces si `mouseDragged()` intenta meter un nuevo boid, tiene que esperar a que el otro hilo termine.

Eso evita que el vector crezca a mitad del recorrido y que el iterador se vuelva loco. O sea, el lock hace que todo pase en orden y evita que el programa se caiga por accesos raros.


### Por qué muchos hilos esperando el mismo lock no siempre ayuda

Los hilos son rapidos, pero si todos pelean por el mismo lock, terminan esperando y no haciendo nada. O sea, en vez de paralelizar, se vuelve como una fila

Al final gastas tiempo cambiando de hilo y esperando el lock, y el programa no va más rápido. Incluso puede ir más lento. Aquí la idea es que paralelismo sirve, pero si todo depende de un solo recurso compartido, se vuelve cuello de botella.

### ¿Qué pasaría si tuviéramos varios hilos que calculan el movimiento de los boids? ¿Cómo podrías implementar esto? ¿Qué problemas crees que podrían surgir? ¿Cómo podrías solucionarlos?

Si meto varios hilos calculando boids al mismo tiempo, la idea sería dividir los boids en grupos y que cada hilo procese un grupo. Suena chévere porque en teoría debería ser más rápido.

El problema es que todos los boids necesitan leer la posición de los demás para decidir hacia dónde moverse. Entonces cada hilo estaría revisando el mismo vector, y ahí empiezan las peleas por acceso a memoria. Toca usar locks o algo similar y con varios hilos eso se vuelve un atasco y puede incluso hacerlo más lento.

También puede pasar que un hilo lea posiciones viejas porque otro hilo ya las cambió. Eso da comportamientos raros, como boids con movimientos inconsistentes.

Solución que yo usaría:
- Copiar las posiciones de todos los boids al inicio de cada frame.
- Cada hilo lee esa copia (sin pelear).
- Cada hilo actualiza solo su parte del vector original.
- Al final sincronizo todo.

Así evito locks constantes y cada hilo trabaja más tranquilo.


### ¿Qué diferencias encuentras entre el código del Flocking sin hilos y el Flocking con hilos? ¿Por qué crees que es importante la sincronización en el segundo caso?

En la versión sin hilos todo corre en el hilo principal. Normla peero si le meto muchos boids los FPS cae porque todo se hace secuencial.

En la versión con hilos, el cálculo va en un hilo y el dibujo en otro. Eso ayuda a que el render siga fluido mientras los boids piensan en otro lado. Pero como los dos hilos usan el mismo vector, toca usar `lock()` y `unlock()` para que no choquen.

La sincronización es importante porque si los dos hilos tocan el vector al mismo tiempo puedo romper la memoria, obtener posiciones corruptas o hasta crashear.


### ¿Por qué al añadir un nuevo boid la simulación se ralentiza? ¿Qué ocurre si añades muchos boids?

Cada boid revisa a los demás, entonces entre más boids meto, más comparaciones toca hacer. Eso es literalmente más carga cada frame. Si meto demasiados boids, el CPU colapsa y el FPS baja bastante. 


###  Notaste que la versión con hilos tiene un `sleep(5)` en el hilo trabajador. ¿Por qué crees que se ha añadido? ¿Qué pasaría si lo eliminamos?

Ese `sleep(5)` parece estar ahí para controlar el ritmo de ejecución del hilo y evitar que consuma toda la CPU calculando sin descanso. También puede ayudar a que los valores se actualicen de forma más estable visualmente.

Si se elimina, el hilo podría correr sin pausa y saturar el procesador, haciendo que todo se vuelva muy demandante y posiblemente inestable o más difícil de ver fluido.

### Compara el rendimiento de ambos enfoques. ¿Cuál crees que es más eficiente? ¿Por qué?

El enfoque con hilos puede ser más eficiente si está bien sincronizado y hay suficientes boids, porque reparte la carga entre varios núcleos del procesador.

Sin embargo, si hay pocos boids o la sincronización no está bien hecha, puede que el enfoque sin hilos sea más rápido, porque no hay sobrecosto de coordinación entre hilos.


### ¿Qué pasaría si no se usaran `lock` y `unlock` en la versión con hilos? ¿Cómo afectaría esto al comportamiento del programa?

Sin lock y unlock, los hilos podrían modificar datos al mismo tiempo y generar resultados incorrectos. Por ejemplo, un boid podría leer valores “a medias” o calcular su movimiento con información incompleta, provocando movimientos erráticos o resultados impredecibles.

Incluso si el error no se ve inmediatamente, pueden ocurrir fallos difíciles de reproducir o comportamientos que no siguen las reglas del flocking.

### ¿Qué ocurre si mientras el hilo trabajador está calculando el movimiento de los boids, el hilo principal intenta añadir un nuevo boid? ¿Se congelará la aplicación? ¿Por qué?

Si el hilo principal intenta añadir un boid justo cuando el hilo trabajador está calculando, la aplicación no debería congelarse, pero sí podría fallar o generar comportamiento raro si no hay control de acceso a los datos.

El problema es que ambos hilos estarían usando la misma estructura de boids al mismo tiempo: el hilo trabajador procesando el vector y el hilo principal tratando de modificarlo. Eso puede causar cosas como acceder a memoria inválida, índices corruptos o resultados impredecibles.

La app no se congela porque los hilos siguen corriendo, pero sí puede crashear o dejar el flocking actuando raro. Por eso se necesita un lock cuando se agregan boids. Ese lock pausa momentáneamente el acceso para evitar que ambos hilos toquen los datos al mismo tiempo.

## Actividad 05:

### Pega la parte clave de tu función modificada que calcula el píxel para el conjunto de Julia.
~~~
int calculateJuliaPixel(int x, int y) {
    float zx = ofMap(x, 0, imgWidth, -2.0, 1.0);
    float zy = ofMap(y, 0, imgHeight, -1.5, 1.5);
    int iterations = 0;
    while (zx * zx + zy * zy < 4.0 && iterations < maxIterations) {
        float tempX = zx * zx - zy * zy + juliaK.x;
        zy = 2.0 * zx * zy + juliaK.y;
        zx = tempX;
        iterations++;
    }
    return iterations;
}
~~~

En ofApp.cpp al crear los hilos:
~~~
MandelbrotThread* newThread = new MandelbrotThread(startY, endY, imgWidth, imgHeight, maxIterations, pixels, juliaK);
~~~

### Muestra cómo mapeaste la posición del mouse a la constante k.

Para mapear la posición del mouse puse lo siguiente en update():
~~~
juliaK.x = ofMap(mouseX, 0, imgWidth, -1.5f, 1.5f);
juliaK.y = ofMap(mouseY, 0, imgHeight, -1.5f, 1.5f);
~~~

Y después creé una función para que la aplicación siguiera el movimiento del mouse:
~~~
void ofApp::mouseDragged(int x, int y, int button) {
    startCalculation();
}
~~~

### Describe brevemente cómo reutilizaste la estructura de hilos de la versión Mandelbrot. ¿Tuviste que cambiar mucho esa parte?

La verdad no tuve que realizar casi cambios en esta estructura. La lógica de lanzar y esperar los hilos quedó igual, solo agregué una variable nueva en la parte de los cálculos, que es el vector que almacena los valores de la constante k para Julia. Por lo demás, la estructura se pudo reutilizar completamente sin problema.

### ¿Cómo te aseguraste de que la imagen se recalculara cuando el mouse se movía?

Básicamente, cada vez que el mouse cambia de posición, se actualiza la constante juliaK con las coordenadas mapeadas. Luego, al arrastrar el mouse (mouseDragged) se llama a startCalculation(), disparando un recálculo completo de todos los píxeles en paralelo. Así la imagen siempre refleja el valor actualizado de k.

### Incluye al menos dos capturas de pantalla que muestren diferentes fractales de Julia generados al mover el mouse en tu aplicación.

<img width="1017" height="753" alt="image" src="https://github.com/user-attachments/assets/2d91b721-761c-42cd-ad28-beedeeea2475" />
<img width="1008" height="755" alt="image" src="https://github.com/user-attachments/assets/5e4e21f5-cc91-4afa-878e-f10363855ec3" />

esto es masomenos imitando la del ejemplo:

<img width="1010" height="742" alt="image" src="https://github.com/user-attachments/assets/2aa4618b-0d61-46bd-9f6c-5401d95f95e9" />
<img width="1013" height="796" alt="image" src="https://github.com/user-attachments/assets/4a9498e6-fd54-433f-a12b-286a92bca8cf" />


### ¿Encontraste algún desafío particular al implementar la interacción o modificar el cálculo?

Siento que lo más complicado no fue entender los cálculos o la estructura de los hilos, sino integrar la nueva variable juliaK y asegurar que el recálculo se disparara correctamente con el movimiento del mouse. En general, la lógica la comprendí bien, pero a veces me costó aplicarla de forma práctica sin errores.

## Autoevaluación:
