# Montecarlo-racing

En este proyecto implemento un entorno de **aprendizaje por refuerzo** basado en un simulador de carreras en 2D y aplico el **método de Monte Carlo** para la evaluación y mejora de políticas.

## Descripción
El objetivo es que un agente (un coche) aprenda a recorrer un circuito desde la posición inicial hasta la meta evitando chocar contra obstáculos.  
He creado dos pistas diferentes (track1 y track2) y he implementado políticas de actualización que permiten al agente mejorar su rendimiento a lo largo de los episodios.
Dichas pistas son matrices en las que cada casilla puede ser representada por:
- 0: representa a una casilla de carretera por la que puede circular el coche
- 1: Representa una pared o límite del circuito
- 2: Casilla de meta
- 3: Casillas de salida

## Explicación del código
La clase **RacingEnvironment** se inicializa con el track y consta de estos métodos principalmente:
- **reset**: Este método devuelve al coche a una de las casillas de salida de manera aleatoria. Se llama al método reset cuando el coche llega a la meta o cuando se choca con los límites del circuito.
- **step**: El método step es el que hace que el vehículo se desplace, calcula las nuevas posiciones y velocidades en función de una acción y devuelve una recompensa para que aprenda el sistema lo que está bien y lo que está mal (Aquí está la clave del aprendizaje).
- **is_within_bounds**: Este método comprueba que el coche está dentro de los límites del circuito en cada paso.


Además del entorno, se definen las siguientes funciones principales:

- **monte_carlo_control()**:  
  Implementa el control por **Monte Carlo** para estimar la función `Q` y derivar una política.  
  - Entrada: un objeto `env` (instancia de `RacingEnvironment`), número de episodios, factor de descuento `gamma` y `max_steps` por episodio.  
  - Espacio de acciones: combinaciones `(dx, dy)` con `dx` en `[1, -1, 0]` y `dy` en `[-1, 0, 1]`. Esto son combinaciones de 
  - Salida: `policy` (diccionario `state -> action`), `steps_per_episode` (lista con nº de pasos por episodio) y `logs` (trayectorias guardadas para visualización).  
  - Observaciones: durante el bucle se almacenan retornos por pares `(estado,acción)` y se calcula la media para actualizar `Q`. Además guarda algunos episodios finales en `logs` para crear GIFs.
 
- **epsilon_greedy()**:  
  Política ε-greedy que devuelve la acción a tomar en un estado dado, priorizando la mejor acción conocida pero con cierta probabilidad de elegir aleatoriamente para explorar nuevos caminos y encontrar soluciones más eficientes.

- **plot_policy()**:  
  Muestra la política sobre la pista usando flechas (`quiver`) indicando la acción recomendada en cada estado (útil para inspeccionar visualmente la política).


## Recompensas (comportamiento del entorno)
- La función `step` devuelve recompensa `+100` al alcanzar la meta y `-1` por cada paso normal.  
- Si se detecta colisión con un límite/pared, el episodio finaliza y la recompensa es de `-10` (el método retorna `done=True`).

## Requisitos
- Python 3.8+
- Dependencias: `numpy`, `pandas`, `matplotlib`, `imageio`  

## Visualización
El proyecto genera animaciones en formato **GIF** que muestran el recorrido del agente en la pista.  
Estas animaciones son útiles para evaluar visualmente cómo mejora el agente a lo largo del entrenamiento, reduciendo errores y optimizando el camino hacia la meta.

## Ejecución

1. Clona el repositorio y asegúrate de tener track1.csv y track2.csv en la raíz del proyecto.
2. Abre Práctica 2 parte 1. Montecarlo Racing.ipynb y ejecuta todas las celdas.
3. Se habrán generado los gifs y las gráficas con las que podrás ver cómo de efectivo ha sido tu entrenamiento en este entorno.
4. Modifica parámetros como el número de episodios para ver cómo se comporta el coche en diferentes escenarios. Puedes crear tu propio circuito también.



