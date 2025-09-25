# Bitácora de aprendizaje de la unidad 6

## Actividad 01:

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


