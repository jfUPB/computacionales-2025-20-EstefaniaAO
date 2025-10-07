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

```C++	

```

### ¿Qué preguntas te surgen al ver el código?. Anota al menos tres preguntas que te gustaría investigar más adelante (no te preocupes que la idea de esta unidad es que las resuelvas).


