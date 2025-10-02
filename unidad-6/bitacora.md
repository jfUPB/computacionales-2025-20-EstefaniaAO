# Bitácora de aprendizaje de la unidad 6

## Actividad 01:

---
<p align="center">
Código:
</p>

- *ofApp.h:*
~~~C++
#pragma once

#include "ofMain.h"
#include <string>
#include <vector>

class Observer {
public:
  virtual ~Observer() = default;
  virtual void onNotify(const std::string & event) = 0;
};

class Subject {
public:
  void addObserver(Observer * observer);
  void removeObserver(Observer * observer);

protected:
  void notify(const std::string & event);

private:
  std::vector<Observer *> observers;
};

class Particle;

class State {
public:
  virtual ~State() = default;
  virtual void update(Particle * particle) = 0;
  virtual void onEnter(Particle * particle) { }
  virtual void onExit(Particle * particle) { }
};

class Particle : public Observer {
public:
  Particle();
  ~Particle() override;

  Particle(const Particle &) = delete;
  Particle & operator=(const Particle &) = delete;

  void update();
  void draw();
  void onNotify(const std::string & event) override;

  void setState(State * newState);

  ofVec2f position;
  ofVec2f velocity;
  float size;
  ofColor color;

private:
  void keepInsideWindow();
  State * state;
};

class NormalState : public State {
public:
  void update(Particle * particle) override;
  void onEnter(Particle * particle) override;
};

class AttractState : public State {
public:
  void update(Particle * particle) override;
};

class RepelState : public State {
public:
  void update(Particle * particle) override;
};

class StopState : public State {
public:
  void update(Particle * particle) override;
};

class ParticleFactory {
public:
  static Particle * createParticle(const std::string & type);
};

class ofApp : public ofBaseApp, public Subject {
public:
  ~ofApp() override;
  void setup() override;
  void update() override;
  void draw() override;
  void keyPressed(int key) override;

private:
  std::vector<Particle *> particles;
};
~~~

- *ofApp.cpp:*
~~~C++
#include "ofApp.h"
#include <algorithm>

void Subject::addObserver(Observer * observer) {
  if (!observer) return;
  if (std::find(observers.begin(), observers.end(), observer) == observers.end()) {
    observers.push_back(observer);
  }
}

void Subject::removeObserver(Observer * observer) {
  if (!observer) return;
  observers.erase(std::remove(observers.begin(), observers.end(), observer), observers.end());
}

void Subject::notify(const std::string & event) {
  for (Observer * observer : observers) {
    observer->onNotify(event);
  }
}

Particle::Particle()
  : state(nullptr) {
  position = ofVec2f(ofRandomWidth(), ofRandomHeight());
  velocity = ofVec2f(ofRandom(-0.5f, 0.5f), ofRandom(-0.5f, 0.5f));
  size = ofRandom(2.0f, 5.0f);
  color = ofColor(255);

  state = new NormalState();
  state->onEnter(this);
}

Particle::~Particle() {
  if (state) {
    state->onExit(this);
    delete state;
    state = nullptr;
  }
}

void Particle::setState(State * newState) {
  if (state) {
    state->onExit(this);
    delete state;
  }
  state = newState;
  if (state) {
    state->onEnter(this);
  }
}

void Particle::update() {
  if (state) {
    state->update(this);
  }
  keepInsideWindow();
}

void Particle::draw() {
  ofPushStyle();
  ofSetColor(color);
  ofDrawCircle(position, size);
  ofPopStyle();
}

void Particle::onNotify(const std::string & event) {
  if (event == "attract") {
    setState(new AttractState());
  } else if (event == "repel") {
    setState(new RepelState());
  } else if (event == "stop") {
    setState(new StopState());
  } else if (event == "normal") {
    setState(new NormalState());
  }
}

void Particle::keepInsideWindow() {
  const float W = static_cast<float>(ofGetWidth());
  const float H = static_cast<float>(ofGetHeight());

  if (position.x < 0.0f) {
    position.x = 0.0f;
    velocity.x *= -1.0f;
  } else if (position.x > W) {
    position.x = W;
    velocity.x *= -1.0f;
  }
  if (position.y < 0.0f) {
    position.y = 0.0f;
    velocity.y *= -1.0f;
  } else if (position.y > H) {
    position.y = H;
    velocity.y *= -1.0f;
  }
}

void NormalState::onEnter(Particle * particle) {
  particle->velocity.set(ofRandom(-0.5f, 0.5f), ofRandom(-0.5f, 0.5f));
}

void NormalState::update(Particle * particle) {
  particle->position += particle->velocity;
}

static void steer(Particle * particle, const ofVec2f & toward, float accel, float vmax, float posScale) {
  ofVec2f dir = toward - particle->position;
  float len = dir.length();
  if (len > 1e-6f) {
    dir /= len;
    particle->velocity += dir * accel;
  }
  particle->velocity.limit(vmax);
  particle->position += particle->velocity * posScale;
}

void AttractState::update(Particle * particle) {
  ofVec2f mouse(ofGetMouseX(), ofGetMouseY());
  steer(particle, mouse, /*accel*/ 0.05f, /*vmax*/ 3.0f, /*posScale*/ 0.2f);
}

void RepelState::update(Particle * particle) {
  ofVec2f mouse(ofGetMouseX(), ofGetMouseY());
  ofVec2f away = particle->position - mouse;
  float len = away.length();
  if (len > 1e-6f) {
    away /= len;
    particle->velocity += away * 0.05f;
  }
  particle->velocity.limit(3.0f);
  particle->position += particle->velocity * 0.2f;
}

void StopState::update(Particle * particle) {
  particle->velocity *= 0.80f;
  if (particle->velocity.lengthSquared() < 1e-4f) {
    particle->velocity.set(0.0f, 0.0f);
  }
  particle->position += particle->velocity;
}

Particle * ParticleFactory::createParticle(const std::string & type) {
  Particle * particle = new Particle();

  if (type == "star") {
    particle->size = ofRandom(2.0f, 4.0f);
    particle->color = ofColor(255, 0, 0);
  } else if (type == "shooting_star") {
    particle->size = ofRandom(3.0f, 6.0f);
    particle->color = ofColor(0, 255, 0);
    particle->velocity *= 3.0f;
  } else if (type == "planet") {
    particle->size = ofRandom(5.0f, 8.0f);
    particle->color = ofColor(0, 0, 255);
  }
  return particle;
}

ofApp::~ofApp() {
  for (Particle * p : particles) {
    removeObserver(p);
    delete p;
  }
  particles.clear();
}

void ofApp::setup() {
  ofBackground(0);
  particles.reserve(100 + 5 + 10);

  for (int i = 0; i < 100; ++i) {
    Particle * p = ParticleFactory::createParticle("star");
    particles.push_back(p);
    addObserver(p);
  }
  for (int i = 0; i < 5; ++i) {
    Particle * p = ParticleFactory::createParticle("shooting_star");
    particles.push_back(p);
    addObserver(p);
  }
  for (int i = 0; i < 10; ++i) {
    Particle * p = ParticleFactory::createParticle("planet");
    particles.push_back(p);
    addObserver(p);
  }
}

void ofApp::update() {
  for (Particle * p : particles) {
    p->update();
  }
}

void ofApp::draw() {
  for (Particle * p : particles) {
    p->draw();
  }
}

void ofApp::keyPressed(int key) {
  switch (key) {
  case 's':
    notify("stop");
    break;
  case 'a':
    notify("attract");
    break;
  case 'r':
    notify("repel");
    break;
  case 'n':
    notify("normal");
    break;
  default:
    break;
  }
}
~~~

---

### ¿Cómo puedes interactuar con la aplicación? Menciona específicamente las teclas y qué efecto parecen tener sobre las partículas.

| **Acción**               | **Resultado** |
|--------------------------|---------------|
| **Inicial**              | Diferentes "partículas" en forma de círculo flotan libremente por la ventana. Tienen colores y tamaños distintos, y cada una se mueve en una dirección única. Al chocar con los bordes o esquinas del canvas, rebotan y cambian de dirección. |
| **Presionar `s`**        | Todas las partículas se detienen en el lugar exacto donde estaban cuando se presionó la tecla. Permanecen completamente estáticas. Solo hace falta presionar una vez para congelar el estado. |
| **Presionar `a`**        | Las partículas comienzan a sentirse atraídas por el cursor. Se mueven directamente hacia él de forma continua. Al llegar, no se detienen, sino que oscilan de un lado al otro atravesando el cursor, superponiéndose entre ellas. |
| **Presionar `r`**        | Las partículas se repelen constantemente del cursor. Se alejan en dirección contraria a la posición del puntero, incluso mientras están en movimiento, como si el cursor emitiera una fuerza que las empuja. |
| **Presionar `n`**        | Las partículas reinician su comportamiento al estado inicial: vuelven a moverse en direcciones aleatorias, rebotan al chocar con los bordes, y cada vez que se presiona la tecla, las direcciones cambian, generando un nuevo patrón. También siento que se reseteen porque algunas empiezan a moverse mas lento que en el estado inicial. |

### ¿Observas los diferentes tipos de “partículas”? ¿Se comportan todas igual inicialmente?

Hay 3 tipos distintos, sin ver el código su mayor diferencia es el color y el tamaño. Hay rojas, azules y verdes. Generalmente, parece que las rojas y verdes son de tamaños similares, aunque las verdes parecen tener un rango mas alto (en el sentido de que pueden llegar a ser más grandes que las rojas) y las azules son las mas grandes de todas.

### Toma algunas capturas de pantalla de la aplicación en diferentes momentos (estado inicial, después de presionar ‘a’, ‘r’, ‘s’, ‘n’) y añádelas a tu bitácora.

| Acción                  | Imagen                                                                                                                                   |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| **Estado inicial**      | ![Estado inicial](https://github.com/user-attachments/assets/9bb86e98-831e-43b5-b9ed-ae93ccdf6dfe)                                       |
| **Al presionar `s`**    | ![Al presionar s](https://github.com/user-attachments/assets/94b60c0d-7d21-4293-8d83-654fbe1e28d6)                                       |
| **Al presionar `a`**    | ![Al presionar a](https://github.com/user-attachments/assets/f90da3dd-d7b5-4762-8fa1-a72ec91730ff)                                       |
| **Al presionar `r`**    | ![Al presionar r](https://github.com/user-attachments/assets/7fd3852d-c490-4703-8519-b839190d0d2f)                                       |
| **Al presionar `n`**    | ![Al presionar n](https://github.com/user-attachments/assets/1c2298c0-6552-422e-87fa-e2f4da99ef76)                                       |


### ¿Qué crees que está pasando “detrás de cámaras” cuando presionas las teclas? Formula una hipótesis inicial sobre cómo la aplicación cambia el comportamiento de las partículas.

Creo que lo que pasa internamente es que cada partícula tiene un estado que define cómo se mueve. Cuando presiono una tecla, la aplicación envía un evento a todas las partículas (como una señal) y ellas cambian de estado según el evento recibido.

- En el estado Normal se mueven aleatoriamente.
- En Attract calculan una fuerza de atracción hacia el mouse.
- En Repel hacen lo mismo pero en dirección contraria.
- En Stop van frenando hasta quedar quietas.

## Actividad 02

1. Explica con tus propias palabras el propósito del patrón Observer. ¿Qué problema resuelve?

El patrón Observer sirve para que un objeto pueda notificar automáticamente a varios otros cuando ocurre un evento o cambia su estado, sin necesidad de estar directamente acoplados entre sí.
El problema que resuelve es la comunicación uno-a-muchos: en lugar de que un objeto tenga que conocer y actualizar manualmente a todos sus dependientes, simplemente envía una notificación general y cada observador decide cómo reaccionar.

En este caso, ofApp no necesita preocuparse por cómo cambian las partículas, solo envía el evento; y cada Particle sabe qué hacer con él.

2. Dibuja un diagrama que muestre la relación entre Subject, Observer, ofApp y Particle en el caso de estudio, indicando quién es el Sujeto y quiénes los Observadores.



3. Construye un diagrama de secuencia que muestre cómo funciona el patrón Observer al presionar una tecla.



4. ¿Qué ventajas crees que ofrece usar el patrón Observer en esta aplicación en comparación con, por ejemplo, que ofApp::update recorriera todas las partículas y les dijera directamente que cambien su comportamiento basado en una variable global? Piensa en términos de acoplamiento y extensibilidad.

- Menor acoplamiento: ofApp no necesita recorrer las partículas una por una ni conocer sus detalles internos; solo envía un evento.
- Extensibilidad: se pueden añadir fácilmente nuevos tipos de observadores (por ejemplo, un sistema de logging o un HUD que muestre estadísticas) sin modificar ofApp.
- Reutilización y claridad: cada partícula maneja de forma independiente cómo cambia su comportamiento cuando recibe un evento.
- Mantenimiento más sencillo: evita el uso de variables globales o condicionales dentro de ofApp::update, manteniendo el código modular y organizado.

## Actividad 03:

1. Explica con tus propias palabras el propósito del patrón Factory Method (o Simple Factory, en este caso). ¿Qué problema principal aborda en la creación de objetos?

El Factory Method es un patrón de diseño creacional que abstrae el proceso de creación de objetos. En vez de que el código cliente tenga que decidir qué clase instanciar (new StarParticle(), new PlanetParticle()), delega esa decisión a una fábrica que sabe cómo construir el objeto adecuado.

El problema que resuelve es el acoplamiento en la creación: evita que el código cliente (por ejemplo, ofApp) esté lleno de llamadas a constructores concretos, lo que complicaría la lectura, el mantenimiento y la extensión del sistema cuando se agreguen nuevos tipos.

En este caso, se usa una versión simplificada del patrón, llamada Simple Factory (o Static Factory Method), porque el método de creación (createParticle) es estático.

2. ¿Qué ventajas aporta el uso de ParticleFactory en ofApp::setup en comparación con instanciar y configurar las partículas directamente allí? Piensa en términos de organización del código (SRP - Single Responsibility Principle), legibilidad y facilidad para añadir nuevos tipos de partículas en el futuro.

- Organización (SRP): ofApp se encarga solo de la lógica de la aplicación, y la fábrica de crear/configurar partículas. Así cada clase cumple una sola responsabilidad.
- Legibilidad: el código en setup es mucho más limpio, solo muestra qué partículas se crean, no cómo se configuran internamente.
- Extensibilidad: si se quiere añadir un nuevo tipo de partícula, no es necesario modificar setup, basta con ampliar la lógica de la fábrica.
- Mantenimiento: la configuración está centralizada, no duplicada en diferentes partes del código.

3. Imagina que quieres añadir un nuevo tipo de partícula llamada "black_hole" que tiene tamaño grande, color negro y velocidad muy lenta. Describe los pasos que necesitarías seguir para implementar esto utilizando la ParticleFactory existente. ¿Tendrías que modificar ofApp::setup? ¿Por qué sí o por qué no?

Si quisiera añadir un nuevo tipo de partícula llamado "black_hole", los pasos serían muy simples. Primero iría a ParticleFactory::createParticle y le agregaría un caso nuevo donde configure esa partícula con tamaño grande, color negro y velocidad lenta. Después, en ofApp::setup, solo tendría que llamar a ParticleFactory::createParticle("black_hole") y ya funcionaría. No tendría que modificar nada más en setup, porque la lógica de cómo se configura la partícula está completamente aislada en la fábrica.

~~~
else if(type == "black_hole") {
    Particle* p = new Particle();
    p->setSize(20);              // grande
    p->setColor(ofColor::black); // negro
    p->setVelocity(ofVec2f(0.1, 0.1)); // muy lenta
    return p;
}
~~~

Y luego:

~~~
particles.push_back(ParticleFactory::createParticle("black_hole"));
~~~

4. El método createParticle en el ejemplo es estático. ¿Qué implicaciones (ventajas/desventajas) tiene esto comparado con tener una instancia de ParticleFactory y un método de instancia createParticle()?.

Ventajas:

- Simplicidad: no hay que crear un objeto ParticleFactory.
- Acceso directo: ParticleFactory::createParticle("tipo").
- Ideal si no necesita guardar estado ni configuraciones.

Desventajas:

- Menos flexible: no se puede cambiar el comportamiento de la fábrica en tiempo de ejecución.
- Más difícil de testear/mokear en pruebas unitarias.
- Si en el futuro se quiere tener una fábrica configurable (por ejemplo, con parámetros globales), habría que reestructurar el diseño para pasar de método estático a instancia.
