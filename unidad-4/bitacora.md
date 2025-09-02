# Evidencias para la unidad 4

## Código

Código para ofApp.h:

``` cpp
#pragma once
#include "ofMain.h"

// Nodo de la cola
struct Node {
	float x, y;
	float radius;
	ofColor color;
	float opacity;
	Node * next;

	Node(float _x, float _y, float _radius, ofColor _color, float _opacity)
		: x(_x)
		, y(_y)
		, radius(_radius)
		, color(_color)
		, opacity(_opacity)
		, next(nullptr) { }
};

// Implementación manual de una cola (FIFO)
class BrushQueue {
public:
	Node * front;
	Node * rear;
	int size;
	int maxSize;

	BrushQueue(int _maxSize);
	~BrushQueue();

	void enqueue(float x, float y, float radius, ofColor color, float opacity);
	void dequeue();
	void clear();
	bool isEmpty();
};

// Constructor
BrushQueue::BrushQueue(int _maxSize)
	: front(nullptr)
	, rear(nullptr)
	, size(0)
	, maxSize(_maxSize) { }

// Destructor
BrushQueue::~BrushQueue() { clear(); }

// Implementa aquí `enqueue()`
void BrushQueue::enqueue(float x, float y, float radius, ofColor color, float opacity) {
	// TODO: crear un nuevo nodo y agregarlo al final de la cola.
	Node * nuevo = new Node(x, y, radius, color, opacity);
	if (rear == nullptr) {
		front = rear = nuevo;
	} else {
		rear->next = nuevo;
		rear = nuevo;
	}
	size++;
	// Si la cola supera `maxSize`, eliminar el nodo más antiguo con `dequeue()`.
	if (size > maxSize) {
		dequeue();
	}
}

// Implementa aquí `dequeue()`
void BrushQueue::dequeue() {
	// TODO: eliminar el nodo más antiguo si la cola no está vacía.
	if (isEmpty()) return;
	Node * temp = front;
	front = front->next;
	delete temp;
	size--;
	if (size == 0) rear = nullptr;
}

// Implementa aquí `clear()`
void BrushQueue::clear() {
	// TODO: eliminar todos los nodos de la cola.
	while (!isEmpty()) {
		dequeue();
	}
}

// Implementa aquí `isEmpty()`
bool BrushQueue::isEmpty() {
	// TODO: retornar si la cola está vacía.
	return front == nullptr;
}

class ofApp : public ofBaseApp {
public:
	BrushQueue strokes; // Cola de trazos
	float backgroundHue = 0;

	ofApp()
		: strokes(50) { } // Tamaño máximo de la cola

	void setup();
	void update();
	void draw();
	void keyPressed(int key);
};

```

Código para ofApp.cpp:

``` cpp
#include "ofApp.h"

//--------------------------------------------------------------
void ofApp::setup() {
	ofBackground(0);
}

//--------------------------------------------------------------
void ofApp::update() {
	backgroundHue += 0.2;
	if (backgroundHue > 255) backgroundHue = 0;

	// TODO: agregar un nuevo trazo si el mouse está presionado.
	if (ofGetMousePressed()) {
		float x = ofGetMouseX();
		float y = ofGetMouseY();
		float radius = ofRandom(5, 20);
		ofColor color;
		color.setHsb(ofRandom(255), 200, 255);
		float opacity = ofRandom(100, 255);
		// Usa strokes.enqueue(x, y, radius, color, opacity);
		strokes.enqueue(x, y, radius, color, opacity);
	}
}

//--------------------------------------------------------------
void ofApp::draw() {
	// Fondo con gradiente dinámico
	ofColor color1, color2;
	color1.setHsb(backgroundHue, 150, 240);
	color2.setHsb(fmod(backgroundHue + 128, 255), 150, 240);
	ofBackgroundGradient(color1, color2, OF_GRADIENT_LINEAR);

	// TODO: dibujar los trazos almacenados en la cola.
	Node * current = strokes.front;
	// Recorre los nodos desde strokes.front hasta nullptr y usa ofDrawCircle().
	while (current != nullptr) {
		ofSetColor(current->color, current->opacity);
		ofDrawCircle(current->x, current->y, current->radius);
		current = current->next;
	}
}

//--------------------------------------------------------------
void ofApp::keyPressed(int key) {
	if (key == 'c') {
		// TODO: limpiar la cola de trazos.
		strokes.clear();
	}
	if (key == 'a') {
		// TODO: alternar entre 50 y 100 trazos.
		strokes.maxSize = (strokes.maxSize == 50) ? 100 : 50;
	} else if (key == 's') {
		// TODO: guardar el frame actual.
		ofSaveScreen("captura_" + ofGetTimestampString() + ".png");
	}
}


```

Código para main.cpp:
``` cpp
#include "ofApp.h"
#include "ofMain.h"

//========================================================================
int main() {
	ofSetupOpenGL(1024, 768, OF_WINDOW); 
	ofRunApp(new ofApp());
}

```

## Demostración:

[Aquí está el video demostrativo de mi aplicación](url del video no listado en youtube)

