# Unidad 2 ✅ Rúbrica

# Fase set seek

## Rúbrica de Evaluación: Traducción de Conceptos de Alto Nivel a Ensamblador

### **Escala de Desempeño (basada en evidencia verificable)**

| Nivel | Descriptor |
| :---: | :--- |
| **5** | **Completo y Preciso** | Se cumplen la totalidad de los requisitos solicitados para el criterio, de manera correcta, clara y sin omisiones. La evidencia es verificable y precisa. |
| **4** | **Logrado** | Se cumplen la mayoría de los requisitos, pero se presenta **una omisión menor o un error puntual** que es fácil de identificar. |
| **3** | **Suficiente** | Se cumplen los requisitos de forma básica o parcial. Hay omisiones evidentes o las explicaciones y programas presentan errores conceptuales. |
| **2** | **En Desarrollo** | Se aborda el requisito de forma incompleta, con errores importantes o sin seguir las instrucciones clave. La evidencia es insuficiente para demostrar comprensión. |
| **1** | **Inicial** | Se realiza un intento mínimo, pero es extremadamente deficiente, no corresponde a lo solicitado o es incomprensible. |
| **0** | **No Realizado** | No se presenta evidencia alguna para el criterio. La sección correspondiente en la bitácora está en blanco o no se entrega. |

***

### 1. Implementación de Control de Pantalla y Traducción a C++ (Actividad 01)

Evalúa la respuesta a estas preguntas y solicitudes: `Escribir un programa en ensamblador que dibuje un punto negro en la esquina superior izquierda de la pantalla (dirección SCREEN/16384). Traducir el programa a C++. Simular paso a paso aplicando la metodología predice-ejecuta-observa-reflexiona.`

| Nivel | Descripción del Desempeño |
| :---: | :--- |
| **5** | **Completo y Preciso:** La bitácora presenta un programa en ensamblador funcionalmente correcto que dibuja el punto en la posición correcta usando la dirección SCREEN. La traducción a C++ es precisa y establece conexiones claras entre conceptos de alto y bajo nivel. La simulación paso a paso está completa con predicciones, observaciones y reflexiones detalladas. |
| **4** | **Logrado:** Ambos programas son funcionalmente correctos, pero presenta una de las siguientes fallas: la simulación es parcial, omite alguna etapa de la metodología, o la traducción a C++ contiene una imprecisión menor. |
| **3** | **Suficiente:** El programa en ensamblador es correcto, pero la traducción a C++ es superficial o contiene errores conceptuales. La simulación es básica sin aplicar completamente la metodología solicitada. |
| **2** | **En Desarrollo:** El programa en ensamblador funciona parcialmente o tiene errores en el manejo de la dirección de pantalla. La traducción es incorrecta o no se presenta. La simulación es deficiente. |
| **1** | **Inicial:** El programa no funciona correctamente o no utiliza la dirección SCREEN apropiadamente. No hay evidencia de traducción o simulación adecuada. |
| **0** | **No Realizado:** No se presenta evidencia de la actividad en la bitácora. |

**Calificación Obtenida:** `[  ]` / 5

***

### 2. Implementación de Línea Horizontal y Manejo de Words (Actividad 02)

Evalúa la respuesta a estas preguntas y solicitudes: `Modificar el programa anterior para dibujar una línea horizontal de 16 píxeles (1 word completo). Traducir a C++. Simular paso a paso con la metodología predice-ejecuta-observa-reflexiona.`

| Nivel | Descripción del Desempeño |
| :---: | :--- |
| **5** | **Completo y Preciso:** La bitácora presenta una modificación correcta del programa que dibuja una línea de 16 píxeles utilizando apropiadamente el concepto de word (16 bits). La traducción a C++ es precisa. La simulación demuestra comprensión clara del manejo de memoria y bits. |
| **4** | **Logrado:** La modificación es correcta y la traducción adecuada, pero presenta una de las siguientes fallas: la simulación es incompleta, o hay una imprecisión menor en la explicación del concepto de word. |
| **3** | **Suficiente:** El programa dibuja la línea correctamente, pero la explicación del concepto de word es superficial. La traducción a C++ es básica o contiene errores menores. La simulación es parcial. |
| **2** | **En Desarrollo:** La modificación del programa es incorrecta o no comprende el concepto de word. La traducción es deficiente y la simulación no demuestra comprensión. |
| **1** | **Inicial:** El programa no funciona o no está relacionado con los requisitos. No hay evidencia de comprensión de los conceptos. |
| **0** | **No Realizado:** No se presenta evidencia de la actividad en la bitácora. |

**Calificación Obtenida:** `[  ]` / 5

***

### 3. Implementación de Entrada/Salida Interactiva (Actividad 03)

Evalúa la respuesta a estas preguntas y solicitudes: `Modificar el programa para mover la línea horizontal usando las teclas 'd' (derecha) e 'i' (izquierda). Traducir a C++. Simular paso a paso con la metodología completa.`

| Nivel | Descripción del Desempeño |
| :---: | :--- |
| **5** | **Completo y Preciso:** La bitácora presenta un programa funcionalmente correcto que implementa el control interactivo con las teclas especificadas. Utiliza apropiadamente la dirección de teclado (KEYBOARD). La traducción a C++ maneja correctamente la entrada/salida. La simulación demuestra comprensión completa del flujo interactivo. |
| **4** | **Logrado:** El programa funciona correctamente con control interactivo, pero presenta una falla menor: la simulación es parcial, o la traducción a C++ tiene una imprecisión en el manejo de E/S. |
| **3** | **Suficiente:** El programa implementa parcialmente la interactividad, pero puede tener problemas con el reconocimiento de teclas o el movimiento. La traducción es básica y la simulación superficial. |
| **2** | **En Desarrollo:** El programa no maneja correctamente la entrada de teclado o el movimiento de la línea. La traducción es incorrecta y la simulación deficiente. |
| **1** | **Inicial:** El programa no implementa interactividad o no funciona. No hay evidencia de comprensión de entrada/salida. |
| **0** | **No Realizado:** No se presenta evidencia de la actividad en la bitácora. |

**Calificación Obtenida:** `[  ]` / 5

***


### 4. Conversión y Análisis de Estructuras de Control (Actividad 04)

Evalúa la respuesta a estas preguntas y solicitudes: `Convertir el programa con ciclo for (suma 1+...+100) a ensamblador. Comparar las versiones while y for en ensamblador. Presentar conclusiones sobre las diferencias y similitudes.`

| Nivel | Descripción del Desempeño |
| :---: | :--- |
| **5** | **Completo y Preciso:** La bitácora presenta una conversión correcta del ciclo for a ensamblador. Incluye un análisis comparativo exhaustivo entre ambas versiones identificando similitudes y diferencias. Las conclusiones son sólidas y demuestran comprensión profunda de las estructuras de control. |
| **4** | **Logrado:** La conversión es correcta y funcional, pero el análisis comparativo es básico, omitiendo algunos aspectos importantes, o las conclusiones son correctas pero superficiales. |
| **3** | **Suficiente:** La conversión del for es mayoritariamente correcta, pero contiene errores menores. El análisis comparativo es superficial y las conclusiones son vagas o incompletas. |
| **2** | **En Desarrollo:** La conversión contiene errores significativos que afectan la funcionalidad. El análisis comparativo es deficiente y las conclusiones son incorrectas o no se presentan. |
| **1** | **Inicial:** La conversión es incorrecta o no se presenta. No hay evidencia de análisis comparativo o comprensión de las estructuras de control. |
| **0** | **No Realizado:** No se presenta evidencia de la actividad en la bitácora. |

**Calificación Obtenida:** `[  ]` / 5

***

### 5. Implementación y Comprensión de Punteros (Actividad 05)

Evalúa la respuesta a estas preguntas y solicitudes: `Convertir a ensamblador los dos programas con punteros dados. Simular paso a paso aplicando la metodología predice-ejecuta-observa-reflexiona para demostrar comprensión del concepto de punteros.`

| Nivel | Descripción del Desempeño |
| :---: | :--- |
| **5** | **Completo y Preciso:** La bitácora presenta conversiones correctas de ambos programas con punteros a ensamblador. La simulación paso a paso demuestra comprensión clara del concepto de direccionamiento indirecto y manejo de punteros. Las predicciones, observaciones y reflexiones son detalladas y precisas. |
| **4** | **Logrado:** Las conversiones son correctas, pero la simulación es parcial, omitiendo alguna etapa de la metodología, o hay una imprecisión menor en la explicación del concepto de punteros. |
| **3** | **Suficiente:** Una de las conversiones es correcta, pero la otra contiene errores. La simulación es básica y la comprensión de punteros es superficial. |
| **2** | **En Desarrollo:** Las conversiones contienen errores significativos en el manejo de direccionamiento indirecto. La simulación es deficiente y no demuestra comprensión del concepto. |
| **1** | **Inicial:** Las conversiones son incorrectas o no comprenden el concepto de punteros. No hay evidencia de simulación adecuada. |
| **0** | **No Realizado:** No se presenta evidencia de la actividad en la bitácora. |

**Calificación Obtenida:** `[  ]` / 5

***

# Fase apply

### 6. Implementación de Arreglos con Punteros (Actividad 06)

Evalúa la respuesta a estas preguntas y solicitudes: `Implementar el programa de suma de arreglo en ensamblador usando punteros. Inicializar el arreglo desde la dirección 16. Construir paso a paso mediante pruebas documentadas. Mostrar programa final y pruebas.`

| Nivel | Descripción del Desempeño |
| :---: | :--- |
| **5** | **Completo y Preciso:** La bitácora presenta una implementación completa y correcta del programa con arreglos usando punteros. Inicializa correctamente los datos desde la dirección 16. Documenta el proceso de construcción paso a paso con pruebas específicas para cada característica. Incluye simulación completa y programa final funcional. |
| **4** | **Logrado:** La implementación es correcta y funcional, pero presenta una de las siguientes fallas: la documentación de pruebas es incompleta, o falta algún detalle en el proceso de construcción paso a paso. |
| **3** | **Suficiente:** El programa funciona básicamente, pero contiene errores menores en el manejo de arreglos o punteros. La documentación de pruebas es superficial o el proceso paso a paso es incompleto. |
| **2** | **En Desarrollo:** El programa contiene errores significativos en la implementación de arreglos o punteros. Las pruebas son deficientes y no hay evidencia clara de construcción paso a paso. |
| **1** | **Inicial:** El programa no funciona correctamente o no implementa los conceptos requeridos. No hay evidencia de pruebas o metodología paso a paso. |
| **0** | **No Realizado:** No se presenta evidencia de la actividad en la bitácora. |

***

