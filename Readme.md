#Algoritmos: Análisis de rendimiento y clasificación

## Contenido
- [Algoritmos: Análisis de rendimiento y clasificación](#algoritmos-anlisis-de-rendimiento-y-clasificacin)
	- [Introducción](#introduccin)
		- [Ejemplo](#ejemplo)
	- [Análisis de algoritmos](#anlisis-de-algoritmos)
		- [Definición:](#definicin)
			- [Orden de crecimiento](#orden-de-crecimiento)
			- [Clasificación del orden de crecimiento](#clasificacin-del-orden-de-crecimiento)
		- [Diseñando algoritmos mas rapidos](#diseando-algoritmos-mas-rapidos)
- [Glosario](#glosario)


##Introducción

**_Algoritmo:_** Un método finito, determinista y efectivo para solucionar un problema. [^1]

El diseño de un algoritmo eficiente juega el rol más importante cuando se quiere atacar un problema complejo, ya que por lo general se estarán procesando cantidades enormes de datos y se hará un uso extensivo de los recursos dentro del ordenador. Es por esto que es de vital importancia hacer eficiente este proceso.

El análisis adecuado de los algoritmos, utilizando los metodos aqui presentes, nos ayuda a encontrar potenciales mejoras que se traducen en un mayor rendimiento de los recursos.

Cabe mencionar que cada algoritmo requiere diferentes metodos de analisis ya que no existe una formula unica y definitiva.

En el siguiente ejemplo veremos un primer enfoque en el analisis de un algoritmo.

###Ejemplo
Tomemos el siguiente programa en Java que suma 3 números y cuenta si el resultado da 0. Este programa lo llamaremos *ThreeSum*:

``` java

public class ThreeSum{
	public static int count(int[] a){
		// Count triples that sum to 0.
		int N = a.length;
		int cnt = 0;
		for (int i = 0; i < N; i++)
			for (int j = i+1; j < N; j++)
				for (int k = j+1; k < N; k++)
					if (a[i] + a[j] + a[k] == 0)
						cnt++;
		return cnt;
	}

}

```

Y como entrada tomaremos archivos que contienen números enteros aleatorios:

* [1Kints.txt](http://algs4.cs.princeton.edu/14analysis/1Kints.txt) *1000 números aleatorios*
* [2Kints.txt](http://algs4.cs.princeton.edu/14analysis/2Kints.txt) *2000 números aleatorios*
* [4Kints.txt](http://algs4.cs.princeton.edu/14analysis/4Kints.txt) *4000 números aleatorios*
* [8Kints.txt](http://algs4.cs.princeton.edu/14analysis/8Kints.txt) *8000 números aleatorios*

Adicionalmente implementaremos un *Cronometro* que nos permitira determinar el tiempo de ejecucion de cualquier programa.

```java
public class Stopwatch{
	private final long start;
	public Stopwatch(){
		start = System.currentTimeMillis();
	}
	public double elapsedTime(){
		long now = System.currentTimeMillis();
		return (now - start) / 1000.0;
	}
}
```
Descripción:

| `public class StopWatch`  | Descripción |
|--------------|---------|
| `Stopwatch()` | Crear un cronometro |
| `double elapsedTime()` | Desplegar el tiempo desde la creación |

La ejecucion de ambas clases se realiza con el siguiente codigo.

```java
public static void main(String[] args)  {
	int[] a = In.readInts(args[0]);
	Stopwatch timer = new Stopwatch();
	int cnt = count(a);
	StdOut.println("elapsed time = " + timer.elapsedTime());
	StdOut.println(cnt);
}
```

El código completo se encuentra [aquí](https://raw.githubusercontent.com/monkeythecoder/AlgoritmosLISDER/master/1-Fundamentales/1-4-AnalysisOfAlgorithms/ThreeSum.java).

Ejecutamos el programa y observamos los distintos tiempos para cada uno de los archivos. 

![Tiempo](https://raw.githubusercontent.com/monkeythecoder/AlgoritmosLISDER/master/img/Captura%20de%20pantalla%20de%202015-04-07%2013%3A31%3A26.png)

### Análisis

Para ayudarnos a encontrar el tiempo de ejecución de nuestro programa, construiremos dos graficas en las cuales el eje *X* sea el numero de elementos analizados y el eje de *Y* sea el tiempo de ejecucion, con la unica diferencia de que la primera es una representación estandar y en la segunda se aplica una escala [log-log](http://es.wikipedia.org/wiki/Representaci%C3%B3n_logar%C3%ADtmica) en ambos lados.

![Grafica](https://raw.githubusercontent.com/monkeythecoder/AlgorithmsSedgewick/master/img/Captura%20de%20pantalla%202015-02-23%20a%20las%2016.48.29.png)

Lo siguiente es hacer un breve analisis sobre el resultado: Podemos observar, en la grafica log-log, una pendiente con valor de ```3``, la ecuacion de dicha linea es $$lg(T(N)) = lg N + lg a$$

donde a es una constante. Ahora, esta ecuación es equivalente a
$$ T(N) = aN^3 $$

el tiempo de ejecución. Podemos utilizar uno de los datos graficados para encontrar ``a``, por ejemplo, de acuerdo a la grafica
$$T(8000) = 51.1 = a8000^3$$
de tal manera `` a = 9.98x10^11 ``, finalmente con esto obtenemos la ecuación $$ T(N) = 9.98X10^11 N^3 $$ que nos ayuda a predecir tiempos de ejecución mas grandes para ``N``.

Existen otros metodos estadisticos para hacer un analisis mas profundo y encontrar ``à`` y el exponente ``b``, pero como ejemplo introductorio es suficiente el metodo ilustrado aqui.

### Hipótesis del orden de crecimiento

El análisis de los algoritmos busca responder preguntas del tipo:

	¿Cual es el tiempo de ejecución de mi programa?
	¿En que punto se desbordara la memoria en mi programa?

Durante los primeros días de las Ciencias de la Computación D. E. Knuth postulo que, independientemente del tipo de programa y los factores que involucran, es posible construir un modelo matemático para describir el Tiempo de ejecución de cualquier programa. El enfoque básico de Knoth es simple: el tiempo total de ejecución es determinado por dos factores principales:

* El costo de cada instrucción
* La frecuencia de ejecución de dicha instrucción

El primero esta en función del ordenador, el compilador y el sistema operativo en conjunto y el segundo depende del programa y de los datos de entrada. Si conocemos estos dos para todas las instrucciones del programa, podemos multiplicarlos juntos y sumar para todas las instrucciones y asi obtener el tiempo de ejecución.

En la siguiente imagen podemos visualizar la frecuencia de ejecución del programa ``ThreeSum``

![Frecuencia](https://raw.githubusercontent.com/monkeythecoder/AlgorithmsSedgewick/master/img/Captura%20de%20pantalla%202015-04-02%20a%20las%2011.35.41.png)

Realizando un analisis de la frecuencia de las instrucciones tenemos la siguiente tabla

| Bloque de instrucciones | Tiempo en segundos | Frecuencia                | Tiempo total    |
|-------------------------|--------------------|---------------------------|-----------------|
| E                       | t₀                 | x (depende de la entrada) | t₀X             |
| D                       | t₁ 			       | N^3 /6 - N^2 /2 + N/3     | t₁(N^3 /6 - N^2/2 + N/3)|
| C                       | t₂                 | N^2 /2 - N/2              | t₂(N^2 /2 - N/2)|
| B                       | t₃                 | N                         | t₃N             |
| A                       | t₄                 | 1                         | t₄              |


Finalmente y simplificando, obtenemos

| -     | -    |
|-------------------------|-----------------|
| **Total**                     | (t₁/6)N^3 + (t₂/2 - t₁/2)N^2 + ( t₁/3 - t₂/2 + t₃)N + t₄ + t₀x            |
| **Aproximación**              |  ~(t₁/6)N^3 (asumiendo x es pequeña)            |
| **Orden de crecimiento**      |   N^3           |


###Definición:
Se escribe `~f(N)` para representar cualquier función que, cuando se divide por `f(N)`, se acerca a 1 mientras `N` crece, y se escribe `g(N) ~  f(N)` para indicar `g(N)/f(N)` se acerca a 1 cuando `N` crece.


####Orden de crecimiento

| Descripción  | Función |
|--------------|---------|
| Constante    | `1`       |
| Logarítmica  | `log N`    |
| Linear       | `N`       |
| Linearítmica | `N log N`   |
| Cuadrática   | `N²`      |
| Cubica       | `N³`      |
| Exponencial  | `2ᴺ`       |

#### Clasificación del orden de crecimiento

**Constante** Los algoritmos cuyo tiempo de ejecucion es constante son aquellos que ejecutan un numero fijo de operaciones para terminar su trabajo, no depende del tamaño de *N*. La mayoria de los operadores en Java tienen tiempo constante.

**Logaritmico** Un algoritmos cuyo tiempo de ejecución crece de forma logaritmica y el cual es muy similiar a uno de tiempo constantante. Un ejemplo clásico es el algoritmo de busqueda binaria.

**Lineal** Algoritmos que tienen un orden de crecimeinto que es directamente proporcional a *N*

**Linearítmica** Auqellos cuyo tiempo de ejecución para un problema de tamaño *N* tienen un crecimiento de *N log N*. La base del logaritmo no es relevante para determinar el crecimiento. ``Mege sort`` tiene este orden

**Cuadratico** Un algoritmo tipico que tiene un orden de *N^2* tiene dos ciclos ``for`` anidados. Los algoritmos ``Selection sort`` e ``Insertion sort`` tienen este orden

**Cubico** Un algoritmo tipico que tiene un orden de N^3 tiene tres ciclos ``for`` anidados. El ejemplo de la sección anterior, ``threesum`` tiene orden *N^3*

**Exponencial** Son aquellos cuyo tiempo de ejecución es proporcional a *2^N* o mayor. Generalmente se usa el termino *exponencial* para referirse a algoritmos cuyo orden es *b^N* para cualquier constante *b>1*. Estos algoritmos son extremadamente lentos, pero juegan un rol muy importante ya que existen problemas en los cuales un algoritmo exponencial es la unica solucion.

###Diseñando algoritmos mas rapidos
Uno de nuestras principales razones para estudiar el orden de los algoritmos es para poder encontrar un algortimo mas rapido que resuelva el mismo problema. Sin embargo existen otros factores que debemos tener en mente al diseñar un algoritmo.

**Grandes contantes** Usualmente al obtener el orden de un algoritmo se suele ignorar los coeficientes constantes de ordenes mas pequeños. Por ejemplo, cuando aproximamos la funcion *2N^2 + cN* a *~2N^2* estamos asumiendo que *c* es pequeña. Si ese no es el caso (por ejemplo *c* siendo *10^3* o *10^6*) la aproximación es incorrecta. Es por eso que debemos ser cuidadosos de la posibilidad de constantes grandes.

**Tiempo de las instrucciones** La creencia de que cada instrucción siempre toma el mismo tiempo de ejecución no siempre es correcta. Por ejemplo, muchos sistemas de computo modernos usan una tecnologia conocida como *caching* para organizar la memoria, en cuyo caso accesar elementos de un arreglo de gran tamaño puede tomar mas tiempo si estos no estan cerca dentro del arreglo.

**Multiples parametros** Se suele medir el desempeño del algoritmo a traves de un solo parametro, pero en la realidad la mayoria de los algoritmos tienen multiples parametros. Un ejemplo clasico es cuando un algoritmo involucra contruir una estructura de datos. Tanto el tamaño de la estructura de datos como el numero de operaciones son parametros para dicha aplicación.

Existen dos estudios posibles sobre el tiempo:

- Uno que proporciona una medida *teorica* (a priori), que que consiste en obtener una función que acote (por arriba o por abajo) el tiempo de ejecución del algoritmo para unos valores de entrada dados.
- Y otro que ofrece una medida real (a posteriori), consistente en medir el tiempo de ejecución del algoritmo para unos valores de entrada dados y en un ordenador concreto
## Arboles de cobertura mínima: *Un algoritmo veloz*

La Teora de Grafos es en sí misma es una fuente rica de problemas que representan un reto para resolverlas utilizando una computadora. No muchos de los problemas de esta teoría pareen tener algun algoritmo que los resuelva en *tiempo polinomial*.

De entre los problemas bien conoidos y ya resueltos, se encuentra la busqueda de un árbol de obertura mínima para una grafo. Especilamente, dado una grafo *G*, on aristas de varias longitudes, el problema es encontrar un arbol *T* en *G* tal que:

- *T* "cubra" *G*, es deir, todos los vertices de *G* se encuentren en *T*.
- *T* tiene la longitud mínima total, sujeto a la ondiion anterior.

En la siguiente imagen se muestra un árbol

# Glosario

- **Ensayo y error** : Consiste en actuar hasta que algo funcione. Puede tomar mucho tiempo y no es seguro que se llegue a una solución. Es una estrategia apropiada cuando las soluciones posibles son pocas y se pueden probar todas, empezando por la que ofrece mayor probabilidad de resolver el problema.
Por ejemplo, una bombilla que no prende: revisar la bombilla, verificar la corriente eléctrica, verificar el interruptor.

- **Iluminación** : Implica la súbita conciencia de una solución que sea viable. Es muy utilizado el modelo de cuatro pasos formulado por Wallas (1921):
preparación, incubación, iluminación y verificación.
Estos cuatro momentos también se conocen como proceso creativo. Algunas investigaciones han determinado que cuando en el periodo de incubación se incluye una interrupción en el trabajo sobre un problema se logran mejores resultados desde el punto de vista de la creatividad. La incubación ayuda a "olvidar" falsas pistas, mientras que no hacer interrupciones o descansos puede hacer que la persona que trata de encontrar una solución creativa se estanque en estrategias inapropiadas.

- **Heurística** : Se basa en la utilización de reglas empíricas para llegar a una solución. El método heurístico conocido como “IDEAL”, formulado por Bransford y Stein (1984), incluye cinco pasos:

	- Identificar el problema
	- Definir y presentar el problema
	- Explorar las estrategias viables
	- Avanzar en las estrategias
	- Lograr la solución
	- Volver para evaluar los efectos de las actividades (Bransford & Stein, 1984).

- **Algoritmos** :  Consiste en aplicar adecuadamente una serie de pasos detallados que aseguran una solución correcta. Por lo general, cada algoritmo es específico de un dominio del conocimiento.

- **Razonamiento analógico** : Se apoya en el establecimiento de una analogía entre una situación que resulte familiar y la situación problema. Requiere conocimientos suficientes de ambas situaciones.  

[^1]: Algoritms (Four edition). Sedgewick, Wayne.
