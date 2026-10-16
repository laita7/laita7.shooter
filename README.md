## Práctica con clases, booleanos, cadenas y manejo de eventos


En esta práctica vamos a desarrollar un juego completo utilizando HTML, CSS y JavaScript. El juego es un clásico juego de disparos en el que controlamos a nuestro personaje (un cuadrado) mediante las teclas de dirección o una pantalla táctil. El objetivo del juego es disparar a una serie de figuras que aparecerán en la pantalla y convertirlas en estrellas, mientras esquivamos sus disparos. Para comenzar el desarrollo utilizaremos una versión básica del juego, cuyo código se proporciona.

![game](assets/game.png)

## Elementos del juego

El proyecto debe descargarse o clonarse en el ordenador desde el que se esté trabajando. Para ello, puede descargarse el paquete con el código desde el archivo adjunto.
En este código modelamos cada uno de los elementos del juego mediante una clase de JavaScript con sus métodos y atributos:

- Entity: Cada uno de los elementos dibujados en el juego.
- Character: Cada uno de los personajes del juego, es decir, aquellos elementos que tienen "vida". Hereda de la clase ENTITY.
- Player: El personaje principal del juego. Hereda de la clase CHARACTER.
- Opponent: La figura que debemos convertir en una estrella. Hereda de la clase CHARACTER.
- Shot: Un disparo de un CHARACTER. Hereda de la clase ENTITY.
- Game: El propio juego.

En el código proporcionado, todos los atributos y métodos de estas clases están documentados detalladamente. El siguiente diagrama muestra la jerarquía de herencia entre las clases:

![class](assets/class.png)


## Inicio y actualización del juego

En el archivo index.html se importan todos los scripts necesarios para el funcionamiento del juego, incluidas todas las clases requeridas y el archivo main.js. En este archivo se definen una serie de constantes necesarias para el juego, se crea una instancia de la clase GAME y se llama a su método START para iniciar el juego.

El método START crea los personajes, dibuja el juego según el tamaño de la pantalla e inicializa los detectores de eventos (que veremos en la siguiente sección). Además, este método inicia un temporizador que llama a la función UPDATE cada 50 ms para actualizar y representar el estado actual del juego según las acciones del usuario, los movimientos del oponente y la posición de los disparos. Este intervalo de tiempo equivale a 20 fotogramas por segundo, lo que significa que actualizamos el juego 20 veces por segundo, más que suficiente para crear la ilusión de movimiento.


## Manejador de eventos

Para controlar al personaje principal del juego mediante las teclas de dirección o una pantalla táctil, necesitamos utilizar los eventos proporcionados por el navegador para este propósito. En el método START de la clase GAME inicializamos los detectores de eventos necesarios:

- keydown: Se ejecuta cuando el usuario pulsa una tecla. Almacena la tecla pulsada en el atributo KEYPRESSED de GAME.
- keyup: Se ejecuta cuando el usuario suelta una tecla. Vacía el contenido del atributo KEYPRESSED de GAME.
- touchstart: Se ejecuta cuando el usuario toca la pantalla. Almacena la posición horizontal (x) donde el usuario tocó en el atributo XDOWN de GAME.
- touchmove: Se ejecuta cuando el usuario arrastra el dedo por la pantalla. Vacía el contenido del atributo XDOWN de GAME.

Como se ha mencionado anteriormente, cada 50 ms se llama al método UPDATE de GAME. Este método comprueba el valor de XDOWN y KEYPRESSED para actualizar la posición del personaje principal según las acciones del usuario.



## TAREAS

Se requiere modificar el código proporcionado para implementar tres nuevas funcionalidades:

- [TAREA 1] Registrar la puntuación del usuario: Cada vez que un oponente se convierta en una estrella, la puntuación deberá aumentar en un punto.
- [TAREA 2] El personaje principal debe tener tres vidas: Si recibe el impacto de un disparo, en lugar de perder inmediatamente, el número de vidas disminuirá en una unidad, dando al jugador otra oportunidad para ganar. Si el número de vidas llega a cero, la partida terminará.
- [TAREA 3] Si el jugador golpea al oponente (triángulo) y lo convierte en una estrella, aparecerá un jefe final más poderoso (pentágono). Este jefe se moverá al doble de velocidad que el triángulo.

Para implementar estas tres funcionalidades, siga los siguientes pasos:

- [TAREA 1]
1- Añada un nuevo atributo SCORE a la clase GAME que represente la puntuación (inicialmente 0).
2- Modifique el código en el método COLLIDE de la clase OPPONENT para que incremente SCORE en una unidad cada vez que un triángulo sea alcanzado por un disparo.


- [TAREA 2]
1- Añada un nuevo atributo LIVES a la clase PLAYER, inicializado con el valor 3. Puede definir el número inicial de vidas en una constante dentro de main.js.
2- Modifique el código en el método COLLIDE de la clase PLAYER para que reduzca una vida cada vez que el jugador sea alcanzado por un disparo mientras siga vivo.
Si al jugador le quedan vidas, deberá "morir" durante dos segundos (llamando al método collide de la superclase CHARACTER) y revivir después de ese tiempo. Para ello, el atributo src de this.image deberá recuperar su valor original (almacenado en this.myImage) y this.dead deberá establecerse en false.
Si al jugador no le quedan vidas, deberá morir permanentemente (llamando al método collide de la superclase CHARACTER) y el juego deberá finalizar llamando al método endGame.

3- Añada el código necesario para mostrar en todo momento la puntuación y las vidas en la pantalla del juego. Para ello, cree una lista (etiqueta HTML ul) con dos elementos (etiqueta li). El primero, con id="scoreli", mostrará la puntuación con el siguiente formato: Score: x, donde x es el valor del atributo SCORE del juego. El segundo, con id="livesli", mostrará el número de vidas con el formato: Lives: y, donde y es el valor del atributo LIVES del jugador. Para actualizar el HTML con los valores de puntuación y vidas, utilice el método innerHTML del elemento HTML correspondiente.


- [TAREA 3]
1- Cree una nueva clase llamada BOSS en un nuevo archivo denominado Boss.js (no olvide importarlo en index.html). Esta clase deberá heredar los métodos y atributos necesarios de la clase OPPONENT, sobrescribiendo aquellos que sean necesarios para conseguir la funcionalidad requerida. Para representar al jefe final puede utilizar las imágenes boss.png y boss_dead.png de la carpeta assets.

2- Modifique el código necesario en el método removeOpponent de GAME para que, cuando el jugador consiga derrotar al triángulo, aparezca el desafío final. Es decir, el atributo opponent de la instancia de GAME deberá contener un objeto Boss cuando el jugador derrote al oponente inicial.

3- Modifique el código de la función endGame (sin cambiar su cabecera) para que, si el jugador derrota al jefe final, es decir, gana la partida con más de 0 vidas, aparezca la imagen you_win.png de la carpeta assets en lugar de game_over.png.




## RÚBRICA

La práctica se calificará sumando a la nota total el porcentaje indicado si la parte especificada es correcta:

- 25%: Se muestran correctamente las vidas del usuario.
- 25%: Se muestra correctamente la puntuación del usuario.
- 50%: La funcionalidad del oponente final está implementada correctamente.
