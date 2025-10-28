# Bitácora de aprendizaje de la unidad 8

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


### Ejecuta el código y observa el resultado. ¿Qué ocurre si cambias el valor de la variable useLock? ¿Por qué crees que ocurre esto?

<img width="474" height="242" alt="image" src="https://github.com/user-attachments/assets/682848cb-e68b-4ae3-96af-786b04763025" />
<img width="424" height="133" alt="image" src="https://github.com/user-attachments/assets/cbfc288a-024e-4a54-8504-93f6d7d5234e" />
<img width="414" height="137" alt="image" src="https://github.com/user-attachments/assets/7b8d8e18-5a27-4a14-9b0e-719188bc8a22" />



### Explica en tus propias palabras ¿Cómo puede presentarse la condición de carrera en este caso? ¿Qué es lo que está pasando? Te pido que propongas un ejemplo.

La condición de carrera es básicamente un error cuando varios hilos intentan acceder a la misma variable en paralelo.
