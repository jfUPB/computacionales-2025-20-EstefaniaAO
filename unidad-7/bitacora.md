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
