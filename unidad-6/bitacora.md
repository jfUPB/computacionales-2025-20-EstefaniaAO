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


