## Proyecto final de curso Computación Evolutiva- Asignación de vuelos y Gates al llegar en aeropuerto

La problemática elegida para el presente trabajo es una preocupación real por la que atraviesan todos los aeropuertos del mundo y que repercute económicamente a las aerolíneas. Esto no solo afecta directamente a los ingresos de las empresas, sino que también genera un deterioro de la reputación de los aeropuertos y de las aerolíneas, causando malestar en los pasajeros debido a  la pérdida de tiempo o cambios de itinerario. 

En tanto, debido al alcance del problema, por un largo tiempo se ha intentado darle solución de diversas formas. Una de ellas, y quizá la más antigua, fue asignar dicha responsabilidad sobre los operadores de las torres de control  quienes llegaban a la solución usando como único criterio el expertis dentro de su campo laboral. Esto último, a pesar de funcionar en un principio, dejó de hacerlo debido al crecimiento del uso del avión como medio de transporte aumentando de forma drástica el tráfico aéreo y con ello la cantidad de pasajeros. 

Por ello, con el desarrollo de las tecnologías y nuevos modelos, ha sido esperable el intento de diversas formas de lograr una solución viable y automatizada  haciendo uso de búsqueda de profundidad o adversarial [1][2]. Ello con el objetivo de optimizar los tiempos de atención del desembarque y asignación de gates, para de esa forma maximizar el tráfico de pasajeros en el aeropuerto.

En ese sentido el presente trabajo busca implementar una solución basada en NSGA-II (Elitist Non-Dominated Sorting Genetic Algorithm), para de esa forma poder encontrar una frontera de Pareto con los resultados que reduzcan el tiempo de desembarque y maximizando la cantidad de pasajeros desembarcados. 


![EjemploIndividuoAvion](https://user-images.githubusercontent.com/94587191/160244760-95fa90e1-8542-415a-8c3d-eae3270b0e5f.jpg)

# Descripción de las variables del problema de asignación de vuelos

Para la implementación del algoritmo genético en este problema un individuo esta representada por una determinada asignación de un subconjunto de vuelos (almacenado en list_of_flights) a un conjunto de gates (almacenado en list_of_gates). La lista de gates disponibles que se pueden asignar a los vuelos se almacena en la variable flights_pool del individuo asi como también la lista de gates se almacenará en la variable gates_pool. Ver el siguiente gráfico ilustrativo:


Para el modelamiento del algoritmo se implementó una representación de clases de Python tanto para los gates( puertas de desembarque) como para los fligths(Vuelos a designar).  Lo cual nos permitirá describir sus atributos de la siguiente manera:
La clase Gate será una clase padre y tendrá dos sub clases hijas ( Sleeve y Zone) y tendrá los siguientes atributos:
identifier: identificador del Gate
distance: en el caso de Sleeve tomará el valor de la longitud de la manda . En el caso de Zone es la distancia a la puerta de desembarque.
potencial_of_speed: en el caso de Sleeve esto es la velocidad de pasajeros en manga. En el caso de Zone almacena la velocidad del bus
number_of_persons_every_10m: en el caso de Sleeve esto almacena el número de personas cada 10 metros. En el caso de Zone es la capacidad del bus.
Para los vuelos se definió una clase padre llamada “flights” que tendrá los siguientes atributos:
identifier = identificador de vuelo
maximum_capacity = máxima capacidad del vuelo
number_of_passengers = número de pasajeros
parking_time = tiempo de parqueo
length_wings = longitud de las alas
inspection_time = tiempo de inspección
arriving_time = tiempo de llegada al aeropuerto
leaving_time = tiempo de partida del aeropuerto

Se partió de un algoritmo base que tenia operadores y funciones tanto de cruzamiento como mutación incompletos ya que estos no permitían discriminar que los Gates y flights estén repetidos en una misma solución.

Por lo que primero se procedió a modificar los operadores de cruzamiento y mutación de tal manera que su operación asegure que la dependencia no tenga flights o gates repetidos.  
 
Posteriormente se implementaron las funciones utilitarias para desplegar un algoritmo multi objetivo.
get_crowding_distances : Esta función permite calcular la distancia crowding de un individuo con relación a la diferencia del fitness mas próximo hacia arriba menos el fitness más próximo hacia abajo. Esta función recibe una lista de fitness.
select_by_crowding: Selecciona una población de individuos basado en torneos de pares de individuos: dos individuos se escoge al azar  y se selecciona el mejor según la distancia crowding. Se repite hasta obtener el número máximo de individuos. Parámetros de entrada : población y límite de individuos a seleccionar de la población.
get_paretofront_population : Obtiene de population la poblacion de individups de la frontera de Pareto
build_next_population : Construye la población de la siguiente generación añadiendo sucesivas fronteras de Pareto hasta tener una población de al menos min_pop_size individuos. Reduce la frontera de Pareto con el método de crowding distance si al agregar la frontera excede el tamaño máximo de la poblacion (max_pop_size).
build_offspring_population : Construye una población hija con los operadores de cruzamiento y mutación pasados
 
# Experimentación y Resultados

En el caso de mono objetivo, realizamos 10 corridas para comparar los métodos de “cruzamiento uniforme” y “onepoint”




Luego realizamos 10 pruebas, respectivamente,  para obtener la mejor tasa de mutación (0.2 ó 0.8) de la mutación  “position”.


Con el mejor método de cruzamiento y la tasa óptima implementamos el algoritmo multi objetivo, donde los objetivos son minimizar el tiempo de desembarco de pasajeros y maximizar la cantidad de pasajeros a desembarcar

Resultados y Discusión:

En el caso del mono objetivo obtenemos que el mejor cruzamiento es el uniforme y la mejor tasa de mutación es 0.2


Al comparar el mejor individuo del mono objetivo con los óptimos de pareto del multi objetivo vemos que el mejor individuo del mono objetivo está muy dominada por las soluciones el multi objetivo

Creemos que esta situación se da por el hecho que el fitnnes en el algoritmo monoobjetivo mezcla dos variables independientes forzando a optimizar una nueva variable (Eficiencia ) que se aleja del problema “real” que es minimizar el tiempo de desembarco y maximizar la cantidad de pasajeros a desembarcar.

