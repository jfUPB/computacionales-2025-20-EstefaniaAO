# Bitácora de aprendizaje de la unidad 7

## Actividad 01:

### Incluye una captura de pantalla del ejemplo funcionando en tu máquina.

<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/ed862668-73ce-4d0f-bc31-57a6d73bd012" />

### Observa el proyecto, trata de entenderlo, pero ten presente que lo analizaremos más adelante.

```C++
// Crea un VAO/VBO con los datos de un triángulo
void setupTriangle() {
	float vertices[] = {
		-0.5f, -0.5f, 0.0f,
		 0.5f, -0.5f, 0.0f,
		 0.0f,  0.5f, 0.0f
	};
	glGenVertexArrays(1, &VAO);
	glGenBuffers(1, &VBO);

	glBindVertexArray(VAO);
	glBindBuffer(GL_ARRAY_BUFFER, VBO);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
	glEnableVertexAttribArray(0);
	glBindVertexArray(0);
}
```

Por lo que puedo entender aquí se crea un arreglo con los tres puntos, cada uno con su valor x, y, y el último valor que en los 3 es 0 entonces no estoy muy segura por ahora de que sea.
Se generan los vertices con `glGenVertexArrays(1, &VAO)` y luego no se que pasa. Creo que con `glBindVertexArray(VAO);` se asigna un array a un triangulo pero la verdad no entiendo todo muy bien.

```C++
if (!glfwInit()) {
	std::cerr << "Fallo al inicializar GLFW\n";
	return -1;
}
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4);
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 6);
glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
```

Se inicializa y se manda un mensaje de error en caso de que la inicialización sea false. Luego se activan cosas relacionadas con las pantallas que no estoy muy segura de por que.

```C++
// 2) Crear ventana
GLFWwindow* mainWindow = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "Ventana", nullptr, nullptr);
if (!mainWindow) {
	std::cerr << "Error creando ventana1\n";
	glfwTerminate();
	return -1;
}
```

Se crea una ventana con el ancho y alto de la misma. Se manda un mensaje de error si no se crea.

```C++
	// 3) Lee el tamaño del framebuffer
	int bufferWidth, bufferHeight;
	glfwGetFramebufferSize(mainWindow, &bufferWidth, &bufferHeight);
	
	// 4) Callbacks 
	glfwSetFramebufferSizeCallback(mainWindow, framebuffer_size_callback);


	// 5) Cargar GLAD y recursos en contexto de window1
	glfwMakeContextCurrent(mainWindow);

	if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)) {
		std::cerr << "Fallo al cargar GLAD (contexto1)\n";
		return -1;
	}
```
Siento que estas partes son las que menos entiendo particularmente. Me gustaría quizá ampliar mas en los buffers.

### ¿Qué preguntas te surgen al ver el código?. Anota al menos tres preguntas que te gustaría investigar más adelante (no te preocupes que la idea de esta unidad es que las resuelvas).

- ¿Qué diferencia hay exactamente entre un VAO y un VBO y por qué se necesitan los dos?
- ¿Qué es un framebuffer y por qué se tiene que leer su tamaño al inicio?
- ¿Qué significa eso del contexto de OpenGL y por qué hay que “cargar GLAD” dentro de ese contexto?
- ¿Cómo sabe OpenGL que los datos que pusimos en el arreglo son los que debe dibujar como un triángulo y no como otra cosa?

## Actividad 02

> Necesito que hagas digestión de esta información y que la entiendas. Para ello te voy a pedir un resumen en tus propias palabras de lo que acabas de leer. En tu resumen debes tratar de conectar GLFW, opengl32.lib, GLAD, GLM y los drivers de la GPU. ¿Qué rol cumple cada uno? ¿Cómo se relacionan entre sí? Mira, trata de hacer esto de memoria y como si estuvieras contándole a un amigo que quiere aprender OpenGL. Cuando haces el proceso de memoria tu cerebro hace un esfuerzo adicional y eso te ayuda a aprender. Además, si no recuerdas algo quiere decir que no lo entendiste bien y eso es una buena señal para que vuelvas a leerlo.

### Resumen:

Para crear un proyecto OpenGL en Windows, lo primero es hacer un proyecto vacío en C++ en Visual Studio. Después de eso, toca agregar las librerías que necesita para funcionar: GLFW, GLAD, y opcionalmente GLM. Cada una tiene su rol, pero todas se conectan entre sí para que el programa pueda mostrar gráficos.

GLFW es como la que se encarga de crear la ventana y manejar el teclado o el mouse. Sin ella, básicamente no habría dónde mostrar nada. Tiene un archivo .lib, que sirve cuando el programa se compila, y un .dll, que se necesita al ejecutar. Si ese .dll no está en la carpeta del proyecto, el programa compila pero no corre.

Luego está opengl32.lib, que ya viene con Windows. Es como la base que permite iniciar OpenGL, pero solo tiene funciones viejas (hasta la versión 1.1). Entonces, para poder usar las funciones modernas, entra GLAD.

GLAD lo que hace es cargar esas funciones nuevas desde los drivers de la tarjeta gráfica. Es como un intermediario que busca en los drivers las funciones de OpenGL moderno y las deja listas para que el código las use. Sin GLAD no podríamos usar casi nada avanzado.

Y GLM es otra biblioteca que no es obligatoria, pero ayuda un montón. Es como una caja de herramientas para hacer matemáticas con vectores, matrices, rotaciones y todo eso que se usa en 3D.

- opengl32.lib inicia el contexto de OpenGL,
- GLFW crea la ventana y maneja los eventos,
- GLAD carga las funciones modernas desde el driver,
y 
- GLM ayuda con las operaciones matemáticas.

## Actividad 03

### Cambiando los valores 

Cuando dejo `glViewport(0, 0, bufferWidth, bufferHeight);`, el triángulo ocupa toda la ventana, o sea se ve normal, centrado.
<img width="845" height="482" alt="image" src="https://github.com/user-attachments/assets/0b4c86d4-4e90-46cd-bdb0-91451c864ada" />

Probé cambiando la línea del viewport y sí se nota bastante la diferencia.

Pero cuando pongo `glViewport(0, bufferHeight/2, bufferWidth/2, bufferHeight/2);`, el triángulo solo aparece en la parte superior izquierda de la ventana, y además se ve más pequeño, como si OpenGL solo estuviera dibujando dentro de una parte del espacio total.
```C++
glViewport(0, bufferHeight/2, bufferWidth/2, bufferHeight/2);
```
<img width="1096" height="504" alt="image" src="https://github.com/user-attachments/assets/ba7998e0-fdee-411e-814d-96450938d85e" />

 Si los multiplico (por 2 o 4), el triángulo se “sale” de los bordes o se corta, porque el área de dibujo queda más grande que la ventana.
```C++
glViewport(0, bufferHeight*2, bufferWidth*2, bufferHeight*2);
```
<img width="1011" height="444" alt="image" src="https://github.com/user-attachments/assets/0e764be7-c18d-4633-bb9a-3fcc70c257d6" />

Si divido más los valores (por ejemplo entre 4), el área visible se reduce todavía más, el triángulo se ve en una esquina y más chiquito.
```C++
glViewport(0, bufferHeight / 4, bufferWidth / 4, bufferHeight / 4);
```
<img width="972" height="437" alt="image" src="https://github.com/user-attachments/assets/960fd723-f51b-4d1a-8d81-4fb28c8129b9" />

Entonces lo que creo que está pasando es que glViewport le dice a OpenGL en qué parte de la ventana puede dibujar y con qué tamaño. Es como si definiera una “ventana dentro de la ventana”, un espacio donde realmente se renderiza la escena.

### 🧪

Estaba leyendo la documentación de OpenGL, que también tenía cosas muy interesantes:

**OpenGL: máquina de estados** 

“Dibujamos” al cambiar variables de contexto de OpenGL que le indican cómo hacerlo. 

Usamos muchas funciones que: 
- cambian el estado
- usan el estado

Los objetos (colección de opciones que representan una parte del estado de OpenGL) se visualizan como estructuras. De ahí se crea una referencia a este como un ID. 

Librerías populares: *GLUT, SDL, SFML y GLFW.*

***GLFW***
Librería escrita en C para OpenGL. Permite crear un contexto para OpenGL, definir parámetros de ventanas, input del usuario, etc…

- **Construir GLFW:**
    * *CMake*: herramienta para generar proyectos y soluciones de la elección del usuario. 

Para usarlo en un proyecto tenemos que vincular la librería con el proyecto. 

***GLAD***
OpenGL es solo una especificación, por lo que depende del fabricante de la tarjeta gráfica (y drivers) implementar funciones específicas. Muchas funciones no están en tiempo de compilación y hay que obtenerlas en tiempo de ejecución. Entonces el desarrollador tendría que buscar la ubicación de cada función usando métodos específicos del sistema operativo y guardar las direcciones. Librerías como GLAD facilitan y automatizan la carga de estas funciones. 

—
> En los includes hay que añadir GLAD antes que GLFW. El archivo include para GLAD incluye los headers de OpenGL necesarios detrás de escenas.

### 🧪: Cambiar `GL_TRIANGLES`

<img width="750" height="458" alt="image" src="https://github.com/user-attachments/assets/f5de7699-f463-41f8-9b78-f1970fa0fbe7" />
Normal.

Con `GL_LINES`
<img width="843" height="445" alt="image" src="https://github.com/user-attachments/assets/4a0092e6-04a9-4836-a515-f00b444abcd5" />

Con `GL_POINTS`
<img width="721" height="467" alt="image" src="https://github.com/user-attachments/assets/ec547c5b-4aaf-43f2-b52b-9637251f24aa" />

Con `GL_TRIANGLES` pero cambiando el tercer parámetro a 
- 2:
<img width="922" height="444" alt="image" src="https://github.com/user-attachments/assets/aa395bd9-b41a-4af2-aeb7-ff63b98d5f5b" />

- 5:
<img width="786" height="442" alt="image" src="https://github.com/user-attachments/assets/17caeb41-bd13-4a22-b155-055ac09dc221" />


> Vamos a terminar esta actividad con un nuevo momento de consolidación parcial. Hay algunos conceptos relacionados con los shaders y el pipeline de OpenGL que no hemos visto en detalle, pero no te preocupes, los vamos a trabajar en la siguiente actividad. Por ahora, quiero que te concentres en lo que has aprendido hasta aquí. Explica con tus propias palabras los siguientes conceptos. Puedes usar ejemplos, analogías o diagramas para ilustrar tus respuestas. Es importante que intentes responder estos conceptos sin ver inicialmente tus notas. Trata de ejercitar tu memoria y tu comprensión. Luego, puedes revisar tus notas para completar o corregir lo que hayas escrito.

- ¿Qué es el contexto OpenGL?

El contexto OpenGL es como el “taller” donde OpenGL trabaja. Es un espacio que guarda todo el estado actual del sistema gráfico: qué color se usa, qué objetos están cargados, qué shaders están activos, etc.
Sin este contexto, OpenGL no tendría dónde guardar o aplicar los cambios. Cada vez que usamos funciones de OpenGL, en realidad estamos modificando este “taller”, diciéndole cómo dibujar.
Por eso se dice que OpenGL es una máquina de estados, porque todo lo que hace depende del estado actual del contexto.

- ¿Cuál es el rol de la biblioteca GLFW y qué ventaja tiene usarla?

GLFW es una librería que nos ayuda a crear ese “taller” (el contexto OpenGL), además de manejar cosas como la ventana, el teclado, el mouse y el tiempo del juego.
Sin GLFW, crear un contexto gráfico dependería del sistema operativo (Windows, Linux, macOS) y sería mucho más complicado.
La ventaja de usarla es que nos abstrae de los detalles del sistema operativo, permitiendo que nuestro código sea más limpio, portable y enfocado en OpenGL.

- ¿Por qué crees que OpenGL necesita un contexto (recuerda la analogía del taller de arte)?

Imagina que OpenGL es un artista. Para pintar necesita un taller con sus herramientas, colores, y lienzos.
Ese taller es el contexto OpenGL.
Si no existiera, el artista no sabría qué pinceles usar, dónde pintar ni qué cuadro está trabajando.
El contexto le da orden a todo lo que hace: es donde se guarda el estado de sus materiales, sus instrucciones y sus resultados.

- ¿En últimas qué será el framebuffer y a qué te recuerda de las dos primeras unidades del curso?

El framebuffer es el “lienzo” donde OpenGL pinta la imagen antes de mostrarla en pantalla.
Cuando termina de dibujar, esa imagen se envía a la pantalla.
Me recuerda a las primeras unidades del curso, donde hablábamos de cómo los pixeles se almacenan en memoria para formar una imagen.
El framebuffer es justamente esa memoria intermedia donde se guarda el resultado final del dibujo.

- ¿Qué relación entre en el viewport y el framebuffer?

El framebuffer es toda la superficie donde se dibuja, pero el viewport define qué parte de esa superficie se usa para mostrar la imagen.
Por ejemplo, si el framebuffer es una hoja de papel completa, el viewport sería un recuadro dentro de ella donde decides dibujar.
En código, lo configuramos con glViewport() para ajustar el área visible.

- ¿En todo la analizado hasta ahora qué rol juega los drivers de la GPU y la GPU misma?

Los drivers de la GPU son los que traducen las instrucciones de OpenGL a algo que la GPU realmente pueda entender y ejecutar.
La GPU (tarjeta gráfica) es quien hace los cálculos pesados: renderiza triángulos, aplica texturas, luces, etc.
OpenGL solo define el “lenguaje” y las reglas, pero cada fabricante implementa esas funciones a su manera por medio de los drivers.

- ¿Por qué crees que sea necesario activar el VSync? ¿Si no lo activas y la imagen es estática qué crees que pase, y si es dinámica?

El VSync (Vertical Synchronization) sincroniza la velocidad de dibujo de los frames con la frecuencia del monitor (por ejemplo, 60 Hz).
Si no se activa y la imagen es estática, puede que no notes diferencia, pero si es dinámica, puede aparecer el efecto tearing (la imagen se corta o desincroniza al moverse).
Activar el VSync evita eso, aunque a veces puede reducir ligeramente la velocidad de respuesta si el rendimiento es muy alto.

- En esta unidad estamos usando OpenGL moderno, pero ¿Qué es OpenGL Legacy? ¿Qué diferencias hay entre ambos?

OpenGL Legacy es la versión antigua (hasta la 2.0 aprox.) que usaba funciones fijas como glBegin() o glVertex3f() para dibujar.
El programador no tenía tanto control: la “tubería de renderizado” era fija y hacía todo automáticamente.
El OpenGL moderno (3.0 en adelante) elimina esa forma y nos obliga a crear nuestros propios shaders y manejar los datos con VBO y VAO, dándonos mucho más control y eficiencia.

- ¿Qué es el shader program? ¿Por qué es importante en OpenGL moderno?

El shader program es un pequeño programa que corre en la GPU. Está compuesto por dos partes principales:

-- Vertex Shader: se encarga de procesar la posición de los vértices.

-- Fragment Shader: se encarga del color de cada píxel.
Es esencial porque en OpenGL moderno ya no hay una pipeline fija, así que todo lo visual pasa por estos shaders.
Nos permiten definir exactamente cómo se verá cada punto, textura o luz.

- Trata de revisar el código setupTriangle(), intuitivamente ¿Qué crees que hace? ¿Qué crees que es el VAO y el VBO?

setupTriangle() probablemente crea los datos para un triángulo y los envía a la GPU.

-- El VBO (Vertex Buffer Object) guarda los vértices (posiciones, colores, coordenadas, etc.).

-- El VAO (Vertex Array Object) guarda las referencias de cómo usar esos datos: cuál buffer, qué atributos, y cómo leerlos.

  el VBO tiene los datos, y el VAO sabe cómo interpretarlos para dibujar.

- En el ciclo principal (game loop) de OpenGL, notaste que en cada frame (cuadro) le decimos a openGL que use el shader program y el VAO. Si le indicas esto antes del game loop ¿Será necesario seguirlo haciendo en cada loop? Si no es necesario ¿En qué casos crees que esto puede ser útil?

Técnicamente, si no cambias de objeto ni de shader, no sería necesario volver a decirlo cada frame.
Pero hacerlo garantiza que el estado sea consistente: si en algún momento cambias de VAO o shader, luego puedes volver a este sin problemas.
También es útil si en el futuro dibujas más de un objeto o escena con distintos shaders.

- Finalmente, recuerda lo que hace glfwSwapBuffers(mainWindow); ¿Por qué crees que es importante? ¿Qué pasaría si no lo llamas? ¿Cómo explicas lo que pasa si no lo llamas? (experimenta)

glfwSwapBuffers() intercambia el framebuffer que OpenGL acaba de dibujar con el que se está mostrando en pantalla.
Esto se llama doble buffering: mientras uno se muestra, el otro se prepara.
Si no llamas a esta función, la ventana nunca actualizará la imagen, así que parecerá congelada.

## Actividad 04

### ¿Cuál es la diferencia entre una CPU y una GPU?

La CPU es el procesador principal del computador, el que se encarga de ejecutar la mayoría de las instrucciones y coordinar todos los procesos. Está hecha para manejar muchas tareas diferentes, pero una a la vez o en pocos núcleos.

La GPU, en cambio, está diseñada para procesar muchos datos al mismo tiempo, especialmente los relacionados con gráficos e imágenes. Tiene muchos núcleos más pequeños y simples que trabajan en paralelo para realizar operaciones repetitivas muy rápido.

- ¿Cuáles son los tres pasos claves del pipeline de OpenGL? Explica en tus propias palabras cuál es el objetivo de cada paso.

Los tres pasos principales del pipeline de OpenGL son:

1. Procesamiento de vértices: aquí se toman los vértices de los objetos (sus coordenadas, colores, texturas, etc.) y se transforman para ubicarlos correctamente en la escena.

2. Rasterización: convierte esos vértices (que forman triángulos u otras figuras) en fragmentos, que son como los datos de cada posible píxel en pantalla.

3. Procesamiento de fragmentos: en este paso se calcula el color final de cada fragmento, aplicando texturas, luces y efectos antes de que se conviertan en píxeles visibles.

- La gran novedad que introduce OpenGL moderno es el pipeline programable. ¿Qué significa esto? ¿Qué diferencia hay entre el pipeline fijo y el programable? ¿Qué ventajas le ves a esto? y si el pipeline es programable, ¿Qué tengo que programar?

En las versiones antiguas (OpenGL Legacy) el pipeline era fijo, lo que significa que las etapas funcionaban de una sola forma establecida por OpenGL. El programador no podía cambiar cómo se procesaban los vértices o los colores.

El pipeline programable permite escribir pequeños programas llamados shaders, que controlan directamente cómo se procesan los vértices (con el vertex shader) y cómo se calculan los colores y luces (con el fragment shader).

Si el pipeline es programable, lo que se programa son precisamente esos shaders que reemplazan las funciones fijas que antes hacía OpenGL automáticamente.

- Si fueras a describir el proceso de rasterización ¿Qué dirías?

La rasterización es el proceso donde OpenGL toma las formas geométricas (como triángulos) y las convierte en fragmentos que corresponden a posiciones específicas en la pantalla.
Es básicamente el paso que traduce la geometría del mundo 3D a una imagen 2D formada por pequeños puntos (fragmentos) que luego se colorean.

- ¿Qué son los fragmentos? ¿Es lo mismo un fragmento que un pixel? ¿Por qué?

Un fragmento no es exactamente un píxel, aunque están muy relacionados.
El fragmento es la información que podría llegar a ser un píxel en la pantalla, pero todavía no lo es.
Cada fragmento contiene datos como su color, profundidad, coordenadas y otras propiedades. Solo después de pasar ciertas pruebas e convierte en un píxel visible.

- Explica qué problema resuelve el Z-buffer y ¿Qué es el depth test?

El Z-buffer guarda la información de profundidad de cada fragmento, es decir, qué tan cerca o lejos está respecto a la cámara.
El depth test usa esa información para decidir qué fragmentos se deben ver y cuáles deben quedar ocultos detrás de otros objetos.

- ¿Por qué se presenta el problema de la aliasing? ¿Qué es el anti-aliasing?

El aliasing aparece cuando las líneas o bordes diagonales se ven con dientes o escalonadas porque la resolución de la pantalla es limitada.
El anti-aliasing es una técnica para suavizar esos bordes, mezclando los colores de los píxeles cercanos para dar una apariencia más suave y continua.

- ¿Qué relación hay entre la iluminación y el fragment shader? Siempre es necesario tener en cuenta la iluminación en un fragment shader? o puedo hacer un fragment shader sin iluminación? Explica que implicaciones tiene esto.

La iluminación se calcula normalmente dentro del fragment shader, porque ahí se determina el color final de cada punto en pantalla y cómo le afectan las luces.
No siempre es obligatorio incluir iluminación: se puede hacer un fragment shader sin ella, por ejemplo, si solo quieres mostrar colores planos o efectos estilizados.
Pero si la ignoras, la escena se verá sin sombras ni realismo.

- ¿Qué implica para la GPU que una aplicación tenga múltiples fuentes de iluminación?

Cada fuente de luz agrega más cálculos al fragment shader (dirección, intensidad, color, sombras, etc.).
Tener muchas luces significa que la GPU debe hacer más operaciones por fragmento, lo que puede disminuir el rendimiento si hay miles de píxeles en pantalla.

### 🧪

- ¿Qué se necesita para dibujar un triángulo en OpenGL?

Para dibujar un triángulo en OpenGL primero se necesita enviar los datos de los vértices a la GPU. Esos datos (las posiciones de los tres puntos del triángulo) se guardan en un VBO (Vertex Buffer Object), que es un objeto que almacena los vértices en la memoria de la tarjeta gráfica.

Luego se crea un VAO (Vertex Array Object), que guarda la configuración de cómo se leen esos datos: por ejemplo, cuántos valores tiene cada vértice y de qué tipo son. El VAO actúa como una especie de “plantilla” que le dice a OpenGL cómo interpretar la información del VBO.

Después de crear y configurar estos objetos, se hace un binding (se activan), y OpenGL ya sabe qué datos usar cuando le pidamos dibujar. Finalmente, se llama la función glDrawArrays(GL_TRIANGLES, 0, 3), que indica que se deben dibujar tres vértices en forma de triángulo.

- ¿Qué se necesita para poder usar un shader en OpenGL?

Para usar un shader en OpenGL moderno hay que crear, compilar y enlazar programas de shader que se ejecutan directamente en la GPU.

Primero se escriben los códigos fuente de los shaders: al menos un vertex shader (para procesar los vértices) y un fragment shader (para calcular el color de los píxeles o fragmentos).

Después, se crean objetos de shader con OpenGL, se les asigna su código fuente y se compilan. Si no hay errores, se linkean ambos dentro de un shader program, que es el objeto final que contiene el pipeline programable.

Una vez el programa está listo, se activa con glUseProgram(shaderProg). A partir de ahí, todos los dibujos que hagamos usarán esos shaders hasta que se desactive o se cambie el programa.
