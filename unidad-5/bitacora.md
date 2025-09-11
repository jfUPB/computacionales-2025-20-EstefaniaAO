# Bitácora de aprendizaje de la unidad 5

## 1.  **Diagnóstico inicial**
---
##### *Tus reflexiones sobre la actividad de diagnóstico.*
---

> ### Parte 1:

- ¿Qué es el encapsulamiento para ti? Describe una situación en la que te haya sido útil o donde hayas visto su importancia.

  El encapsulamiento es un principio de POO donde se "encapsulan" o "encierran" los métodos y atributos en las clases para darles cierto grado de seguridad o privacidad dentro del programa, restringiendo el acceso a ellas o a sus componentes. En el proyecto final de POO, donde debiamos desarrollar un sistema de veterinaria, fue muy util para poder manejar y definir los componentes que iban a ser privados ya que no queríamos que fueran cambiando o que se modificaran a lo largo de la ejecución, siento que nos dio más manejo general de las clases. También en general para el desarrollo en unity el uso de encapsulamiento en C# es muy útil cuando creamos programas o juegos donde es tan recomendable que un atributo pueda ser manejado por cualquier código o parte del código, y para poder controlarlo mejor.

- ¿Qué es la herencia? ¿Por qué un programador decidiría usarla? Da un ejemplo simple.

  La herencia es un principio de POO donde una clase padre le hereda sus atributos y métodos a una clase hijo, existen distintos tipos de herencia y además una clase hijo puede ser la clase padre de otra clase. En general es demasiado útil para la optimización del código, nuevamente con nuesto trabajo de sistema de veterinaria fue muy útil porque aunque manejabamos distintos animales podiamos tener la clase "Animal" que tenía las caracteristicas generales que compartian todos los pacientes y ya eso lo heredaban clases más especificas entonces no teniamos que estar constantemente declarando nombre, edad, color, peso y otros atributos a todas las especies. También un caso que se mostró en nuestro programa, ya que lo desarrollamos en Unity, el uso de MonoBehaviour es un caso de herencia que Unity hace automaticamente y mucha gente no se da cuenta. Básicamente eso nos permitió acceder a todas las funcionalidades (métodos más que nada) del Monobehavior en todas las clases, como Awake(), Start(), Update(), entre otros. Esto hace el desarrollo en Unity muchisimo más facil.

  Un ejemplo de herencia tener la clase padre "Humano", que tiene las caracteristicas principales que necesitamos para el código, y aparte tengo algunos atributos que le quiero asignar a las mujeres y otros a los hombres. Entonces creo la clase Mujer y la clase Hombre, y cada una hereda de humano.

~~~ C#
class Humano { }
class Mujer : Humano { }
class Hombre : Humano { }
~~~

- ¿Qué es el polimorfismo? Describe con tus palabras qué significa que un código sea “polimórfico”.

El polimorfismo es el que menos tengo claro. Diría que el polimorfismo, desde un entendimiento muy superficial que tengo, es un principio en POO que se refiere a la capacidad de un atributo o método de tomar caracteristicas o acciones distintas o de distintas cosas, como ir cambiando, según como se llame o ejecute. Entonces asumiría que un código polimórfico es un código optimizado para que estos componentes puedan ejecutar distintas acciones en el código, un código flexible.

> ### **Parte 2:**
> *análisis de código (en C#)*

~~~C#
using System;
using System.Collections.Generic;

public abstract class Figura
{
    private string nombre;

    public string Nombre
    {
        get { return nombre; }
        protected set { nombre = value; }
    }

    public Figura(string nombre)
    {
        this.Nombre = nombre;
    }

    public abstract void Dibujar();
}

public class Circulo : Figura
{
    public double Radio { get; private set; }

    public Circulo(double radio) : base("Círculo")
    {
        this.Radio = radio;
    }

    public override void Dibujar()
    {
        Console.WriteLine($"Dibujando un {Nombre} de radio {Radio}.");
    }
}

public class Rectangulo : Figura
{
    public double Base { get; private set; }
    public double Altura { get; private set; }

    public Rectangulo(double b, double h) : base("Rectángulo")
    {
        this.Base = b;
        this.Altura = h;
    }

    public override void Dibujar()
    {
        Console.WriteLine($"Dibujando un {Nombre} de {Base}x{Altura}.");
    }
}

public class Programa
{
    public static void Main()
    {
        List<Figura> misFiguras = new List<Figura>();

        misFiguras.Add(new Circulo(5.0));
        misFiguras.Add(new Rectangulo(4.0, 6.0));
        misFiguras.Add(new Circulo(10.0));

        foreach (Figura fig in misFiguras)
        {
            fig.Dibujar();
        }
    }
}
~~~

#### Encapsulamiento:

##### Señala una línea de código que sea un ejemplo claro de encapsulamiento y explica por qué lo es.

~~~C#
    private string nombre;
~~~

Es encapsulamiento ya que se pone al atributo string nombre como private (solo puede ser editado o accedido si está en la misma clase o como parte del código), este creo que es modificador que es como mas protegido o más rigido, menos modificable. Entonces es más controlado tanto el atributo como la forma en la que se modifica

##### ¿Por qué crees que el campo nombre es private pero la propiedad Nombre es public? ¿Qué problema se evita con esto?

Que sea más manejable, que no se ingresen valores incorrectos directamente al atributo privado. El público accede al privado y se lee directamente el publico, entonces hay mucho más control de las modificaciones, tengo entendido que por eso se encapsula el dato nombre.

#### Herencia:

##### ¿Cómo se evidencia la herencia en la clase Circulo?

Circulo hereda de figura, como se muestra en la línea:

~~~C#
public class Circulo : Figura
~~~

Esto quiere decir que Circulo esta heredando métodos y atributos de figura.

##### Un objeto de tipo Circulo, además de Radio, ¿Qué otros datos almacena en su interior gracias a la herencia?

nombre, Nombre y quizá Dibujar().

#### Polimorfismo:

##### Observa el bucle foreach. La variable fig es de tipo Figura, pero a veces contiene un Circulo y otras un Rectangulo. Cuando se llama a fig.Dibujar(), el programa ejecuta la versión correcta. En tu opinión, ¿Cómo crees que funciona esto “por debajo”? No necesitas saber la respuesta correcta, solo quiero que intentes razonar cómo podría ser.

No estoy segura pero asumo que este método aunque ya esta llamado las propias clases hacen Override del método entonces se ejecuta realmente según la figura que lo llame. Entonces por debajo lee quién lo llama y el método se transforma según la definición que tenga cada clase del método, se sobrescribe a la clase que lo llame.

> ### Parte 3:
> *hipótesis sobre la implementación*

---

##### *Esta es la parte más importante. Imagina que eres un diseñador de lenguajes de programación. Tienes que decidir cómo implementar estos conceptos en la memoria y en el procesador. No hay respuestas incorrectas, solo ideas. Dibuja si te ayuda.*

---

#### Memoria y herencia: cuando creas un objeto Rectangulo, este tiene Base, Altura y también Nombre. ¿Cómo te imaginas que se organizan esos tres datos en la memoria del computador para formar un solo objeto?

En direcciones de memoria distintas seguidas para cada uno creado, incluyendo los atributos heredados.

#### El mecanismo del polimorfismo: pensemos de nuevo en la llamada fig.Dibujar(). El compilador solo sabe que fig es una Figura. ¿Cómo decide el programa, mientras se está ejecutando, si debe llamar al Dibujar del Circulo o al del Rectangulo? Lanza algunas ideas o hipótesis.

El valor de la dirección de figura cambia según la figura que lo llame, entonces lo que lee el programa cuando se ejecuta Dibujar() es directamente el circulo o el rectangulo.

#### La barrera del encapsulamiento: ¿Cómo crees que el compilador logra que no puedas acceder a un miembro private desde fuera de la clase? ¿Es algo que se revisa cuando escribes el código, o es una protección que existe mientras el programa se ejecuta? ¿Por qué piensas eso?

Una protección mientras el programa se ejecuta o se compila, o también podría no estar declarado según quién lo llame. No sé la verdad.

## 2.  **La pregunta inicial**

---
### ¿Es posible acceder a atributos privados de una clase mediante punteros o manipulación directa de memoria?
---

Escogí esta pregunta ya que me interesa entender los límites del programa y que tan privada o protegida es realmente una aplicación.

## 3.  **Registro de exploración:** 

Ya sé que puedo leer los datos usando los punteros, pero
### ¿Puedo modificar un campo privado aunque la clase no proporcione ningún método para hacerlo?



### ¿Existen otras formas para acceder a la memoria y copiar datos privados antes de compilar en C++?



### ¿Cómo se pueden leer campos privados heredados?



Pero para esto primero quiero investigar y entender
#### ¿Cómo funciona la herencia en términos de memoria?



### ¿



### ¿Cómo puedo acceder a datos privados mientras el programa se está ejecutando?



> "Existen diversas técnicas que permiten a un atacante acceder a estos datos mientras o poco después de que el programa se está ejecutando. Una de las más comunes es el volcado de memoria en tiempo de ejecución, donde el atacante utiliza herramientas como gcore, ProcDump, gdb o debuggers para capturar el contenido de la RAM asociada al proceso activo. En ese volcado, pueden buscarse directamente cadenas de texto o patrones que revelen contraseñas almacenadas en variables como std::string o buffers de caracteres. Otra técnica es el análisis post-mortem, que aprovecha el hecho de que, al cerrar un programa, la memoria no se borra instantáneamente, por lo que fragmentos de datos sensibles podrían permanecer accesibles hasta que el sistema los sobrescriba. En casos extremos, se han documentado ataques físicos como el Cold Boot Attack, donde se extrae la memoria RAM de un equipo recién apagado y se congela para mantener temporalmente su contenido, permitiendo su lectura en otro sistema."



> Aquí documentas cada ciclo de pregunta -> hipótesis -> experimento -> hallazgo -> reflexión.
> Debe ser rico en evidencia visual (código, capturas del depurador con anotaciones, diagramas).

Conclusiones:

Sin tener acceso al código fuente, un atacante puede obtener contraseñas si el programa las guarda en memoria sin protección. El modificador private no protege en tiempo de ejecución, solo evita el acceso desde otras clases en tiempo de compilación. 

Incluso cuando las contraseñas no están presentes de forma explícita en el código fuente, su paso por memoria RAM representa un riesgo. A menos que se usen prácticas activas de limpieza y protección de memoria, es posible que una contraseña sea extraída mediante técnicas de análisis en tiempo de ejecución.

## 4.  **Consolidación, autoevaluación y cierre:**
> [!CAUTION]
> Esta sección es OBLIGATORIA y central para tu evaluación
