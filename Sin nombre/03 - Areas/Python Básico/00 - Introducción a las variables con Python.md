En este capítulo se explica lo que son las variables, las partes que las forman, los datos que pueden contener, cómo declararlas, inicializarlas, reasignarlas, etc.

---
Empecemos en Python con las variables, uno de los temas más básicos de cualquier lenguaje de programación.

## Crear una hoja para código Python

Primero, vamos a crear un archivo de Python con extensión `.py`. En él, escribirás el código de todo lo que vaya explicando a partir de ahora. Así podrás ir probando todo el código, de una forma práctica.

![[crear-hoja-python.png]]

Otra cosa que puedes hacer, en lugar de tener un solo archivo para todos los capítulos, es crear uno expresamente para cada capítulo, por si quieres tener una serie de apuntes y el código de cada explicación.

Incluso, te recomiendo que utilices _Jupyter Notebook_ o _Google Colab_, para guardar todos los apuntes con código. Así no necesitas ni un IDE o editor de código instalado en tú PC, smartphone o tableta, ya que te permiten ejecutar el código de tus apuntes, desde el propio navegador.

Una vez tengas un archivo donde escribir código Python, ya puedes empezar a escribir los ejemplos que te pondré a lo largo de todo el curso.

## ¿Qué es una variable?

**Una variable en programación es una forma de almacenar datos que pueden cambiar de valor, de ahí proviene su nombre, puede variar.** Se trata de un espacio en la memoria del ordenador donde se guarda un valor. Imagina este espacio como una caja, y la memoria RAM como un gran contenedor para miles de cajas.

![[caja-variable.png]]
Pues bien, estas cajas se van guardando en la memoria, mientras los programas están abiertos. De esta forma, un programa cualquiera carga todas las cajas (variables) que necesita en la memoria y así puede utilizarlas cuando las necesite. Es decir, disponer de la información que necesita el programa en todo momento.

## Partes de las variables

Estas cajas, que llamaremos variables a partir de ahora, están formadas normalmente por **cuatro partes**, aunque puede variar un poco según el lenguaje de programación.

La primera parte es el **tipo de dato**. Este tipo de dato no se tiene que especificar en todos los lenguajes de programación. Un ejemplo de esto es Python, que omite esta parte y lo hace de forma implícita, según el valor que tenga una variable.

La segunda parte es el **nombre**, conocido en programación como identificador. Este nombre debe ser descriptivo con la información que se quiere guardar en la variable. Por ejemplo, si queremos guardar la edad de un usuario, un buen identificador para esta variable sería `edad`.

La tercera parte es el **operador de asignación**, el cual se utiliza para vincular el nombre de la variable con el dato que va a guardar. Normalmente, es el símbolo igual (`=`) en todos los lenguajes de programación.

La cuarta parte es el **valor** que queremos guardar en la variable. Por ejemplo, queremos guardar la edad de 32 años. En ese caso, a la variable `edad` le daremos un valor de `32`.

A continuación, tienes un ejemplo de como se podría ver esta variable:

```java
int edad = 28;
```


> ¡Importante! Esta sintaxis no es válida en Python, solo es para explicarte las variables de forma general.

El punto y coma del final de la variable, es una **finalización de instrucción**. Esto tampoco lo tienen todos los lenguajes de programación.

**En Python, la sintaxis de variable es la siguiente:**

```python
edad = 28
```

Podemos apreciar que no se especifica ni el tipo de dato, ni el punto y coma. El intérprete de Python se encargará de detectar automáticamente el tipo de dato. Este sabrá si es un número, texto, etc.

Python nos simplifica mucho el trabajo en este aspecto, y hace más fácil manejar variables para quien empieza en programación. Es uno de los motivos por los que se dice mucho que la curva de aprendizaje es más fácil que la de muchos otros lenguajes de programación.

> Recuerda que el intérprete de Python, es el encargado de ejecutar el código Python.

## Las variables y los tipos de datos

En algunos lenguajes de programación como Java, tenemos que indicar que tipo de dato vamos a almacenar en una variable. Al especificar el tipo de dato en la variable, esta tendrá que contener siempre valores de ese tipo. A esto se le conoce como _tipado estático_.

En cambio, las variables de Python pueden ir cambiando a cualquier tipo de dato, en cualquier momento. El lenguaje es muy flexible en este aspecto. A esta forma de trabajar con las variables se le conoce como _**tipado dinámico**_.

Las variables pueden guardar valores de diferentes tipos de datos, como por ejemplo, números, texto, valores booleanos, fechas, etc.

El intérprete de Python sabe perfectamente de qué tipo es una variable, basándose en el dato que guardemos en ella. Si la variable lleva un valor numérico entero como es el valor `10`, el intérprete identifica este valor y la variable pasa a ser de ese tipo.

```python
numero_1 = 10
```

Con esto hemos declarado e inicializado una variable de tipo `int` en Python.

> En Python el tipo de dato que sirve para guardar los valores numéricos enteros, como puede ser `10`, se llama `int`.

Si en otra línea del código especificamos otro tipo de valor para la variable, esta se adaptará y cambiará su tipo de dato. Por ejemplo, si guardamos en ella un valor de tipo texto, la variable pasará a ser de tipo `str`.

Esto es posible en Python:

```python
numero_1 = 10
numero_1 = "Hola"
```

El tipado dinámico puede ser visto como algo ventajoso debido a su mayor flexibilidad en el código, pero también presenta una serie de desventajas.

Por un lado, el tipado dinámico permite escribir código más rápidamente y de forma más flexible, ya que las variables pueden cambiar de tipo de dato incluso durante la ejecución del programa. Esto puede hacer el código más legible y sencillo en ciertas situaciones.

Sin embargo, esta flexibilidad puede resultar en un arma de doble filo. El tipado dinámico dificulta la solución de errores y de diversos problemas (depuración), porque los errores de tipo de dato, puede que aparezcan en ciertos casos, solo en tiempo de ejecución (cuando se está ejecutando el programa), en lugar de ser detectados en tiempo de compilación, como ocurre en los lenguajes de tipado estático y compilados.

Además, el tipado dinámico puede aumentar la probabilidad de producir errores humanos y problemas de seguridad. Los lenguajes de tipado estático son estrictos sobre los tipos de datos que maneja el programa, lo que ayuda a evitar una serie de errores típicos.

Entonces, ¿es mal lenguaje Python por implementar un tipado dinámico? La respuesta corta es que no. Si implementas el código de la forma que te enseñaré en este curso, no tendrás ningún problema. Verás que puedes utilizar a tu favor, este tipado sin problema alguno. No necesitarás que un compilador te fuerce a “hacer las cosas correctamente”, si ya las sabes hacer tú.

## Nomenclatura de las variables

El nombre de una variable sigue ciertas reglas de sintaxis y convenciones de nomenclatura especificadas con cada lenguaje de programación. Por ejemplo, en Python no podemos utilizar el símbolo `$` para el nombre de una variable, sin embargo, en PHP es requisito empezar los nombres de variable con este símbolo.

## Reasignación de valores a las variables

Las variables solo pueden llevar un valor o un conjunto de valores a la vez. Esto significa, que si en un hipotético programa, le damos en diferentes líneas de código, varios valores a una variable, esta se quedará siempre con el último valor asignado. Desechando siempre el anterior.

Aquí tienes un ejemplo:

```python
edad = 20
edad = 32
edad = 26
```

En este caso el valor `32` reemplaza al `20` en la segunda línea, y el `26` al `32` en la última línea, con lo que la variable se quedaría finalmente solo con el valor `26`.

A esta acción de ir reemplazando valores se le conoce en programación como **reasignación de valores**.

Empecemos a practicar con las variables en Python. A continuación, empezarás a aprender a manejar las variables de forma básica.

## Declaración e inicialización de variables

Declarar o definir una variable, significa crearla.

Inicializar una variable, significa darle un valor cuando no tiene uno previamente. Es decir, darle un valor inicial.

### Declarar sin inicializar variables

En Python, declarar variables sin inicializarlas, no es posible.

Hay otros lenguajes de programación como Java, que sí lo permiten. Así que nos encontramos con una de las cosas que caracterizan a Python.

Si intentas ejecutar el siguiente código, recibirás este error:

```python
numero_1
numero_1 = 10
```

   Error en la consola

```
NameError: name 'numero_1' is not defined.
```

> El error ocurre en la ==primera línea==. Nos indica que el nombre de la variable no está definido. Claro, eso es lo que intentamos hacer al escribir su nombre, definir la variable, y luego, en la segunda línea, se intenta inicializar con un valor. No obstante, el intérprete ni siquiera llega a ejecutar la segunda línea. Ha fallado en la primera.

De cualquier modo, esto no funciona así en Python. La forma correcta es siempre inicializar las variables con un valor en su declaración. Con eso evitamos el fallo y usamos la forma correcta de hacer las cosas en Python.

Del código anterior, quédate solo con la segunda línea:

```python
numero_1 = 10
```

Ahora, si ejecutas el código, no verás ningún resultado en la consola, pero ya no recibirás el error.

En Python hay un elemento especial llamado `None`. Este elemento se utiliza normalmente para dejar una variable declarada, “sin inicializar”.

Con `None`, indicaremos que queremos que la variable contenga un **valor nulo o “vacío”**. No obstante, aunque sea un valor nulo, realmente estamos inicializándola con un valor.

```python
numero_1 = None
```

Por el momento, no vas a necesitar este elemento, así que no le des gran importancia. Aprenderás su uso con temas un poco más avanzados.

### Inicializar variables con tipo de dato

Python no deja tener las variables vacías, pero podemos dejarlas inicializadas con un tipo de dato concreto. Por ejemplo, que queremos una variable para guardar valores numéricos enteros (`int`), pero aún no queremos poner ningún valor concreto. Entonces, se puede hacer esto:

```python
numero_1 = int()
```

Con esto creamos una variable de tipo `int`, pero no la inicializamos con un número concreto.

No obstante, esto lo que hace es inicializar con un valor `0`. Si imprimes la variable en la consola, lo verás:

```python
numero_1 = int()

print(numero_1)
```

   Resultado en la consola

```
0
```

Este valor `0` será sustituido después en al reasignar un valor diferente a la variable.

## ¿Existe el punto y coma en Python?

La mayoría de los lenguajes de programación finalizan cada instrucción con un punto y coma ( ; ). En Python, es posible hacer eso, pero no es lo más recomendable. Es más bien raro que alguien lo utilice. Además, se desaconseja su uso.

> Una instrucción es una línea de código que le da una orden a un intérprete o compilador. Por ejemplo `print("¡Hola mundo!")` le da la orden al intérprete de imprimir una frase en la consola. Es como decirle en su idioma lo siguiente:
> 
>  _¡Python! imprime la frase “¡Hola mundo!” en la consola_.

Si ejecutas con Python el siguiente código utilizando un punto y coma no te dará error:

```python
numero_1 = 10;
```

En un lenguaje de programación como C#, para indicar al compilador que finaliza la instrucción, le tenemos que poner punto y coma al final de esta. Este separa las instrucciones de código. Si no lo ponemos, da error.

En cambio, en Python las instrucciones se separan haciendo un salto de línea al final de la instrucción (tecla ). No es necesario, ni recomendable utilizar ese punto y coma.

Si quieres poner varias instrucciones en la misma línea, lo puedes hacer con dicho punto y coma, pero no queda muy legible ni elegante, así que mejor no lo hagas.

Aquí tienes un ejemplo de como queda:

```python
numero_1 = 10; print(numero1)
```

   Resultado en la consola

```
10
```

En la última instrucción, no es necesario poner punto y coma, aunque lo puedes poner.

Si probamos este mismo código, sin el punto y coma, escrito todo en la misma línea, recibimos el siguiente error:

```python
numero_1 = 10 print(numero_1)
```

 Error en la consola

```
SyntaxError: invalid syntax
```

## Asignación múltiple

En Python puedes declarar varias variables de una sola vez, y asignarles valores en una misma instrucción o línea de código. La sintaxis es la siguiente:

```sintaxis
a, b, c = valor_a, valor_b, valor_c
```

Sintaxis

Aquí tienes un ejemplo de uso:

```python
numero_1, numero_2, numero_3 = 10, 12, 17
```

En este ejemplo se va a asignar el valor `10` a la variable `numero_1`, el valor `12` a la variable `numero_2`, y el valor `17` a la variable `numero_3`.

Los valores se asignan en el orden de declaración, y deben coincidir el número de variables con el número de datos suministrados. Si son tres variables, le tienes que dar tres valores exactamente.

En caso contrario recibirás el siguiente error:

```python
numero_1, numero_2, numero_3 = 10
```

 Error en la consola

```
TypeError: cannot unpack non-iterable int object
```

> De forma esencial, el error indica que no es posible asignar un valor simple como `10`, a varios elementos.

## Imprimir valores de variables en la consola

Para imprimir el valor de una variable en la consola, en Python lo hacemos con la función predefinida `print()`.

Entre sus paréntesis, llamamos a la variable con su nombre:

```python
numero_1 = 10
print(numero_1)
```

   Resultado en la consola

```
10
```

## Reasignar valores a variables

Para darle un valor nuevo a una variable que ya tiene otro (reasignar), lo hacemos con la misma sintaxis que la declaración.

En el siguiente ejemplo, primero declaro la variable `numero_1` con un valor de `10`, y en la siguiente línea le reasigno un valor de `20`.

```python
numero_1 = 10
numero_1 = 20
print(numero_1)
```

   Resultado en la consola

```
20
```

Como he explicado, las variables solo pueden tener un valor a la vez. Sin embargo, es posible usar el valor que tiene una variable tantas veces como se desee antes de reasignarle un nuevo valor.

Por ejemplo:

```python
numero_1 = 10
print(numero_1)

numero_1 = 20
print(numero_1)
```

   Resultado en la consola

```
10
20
```

En la primera línea se declara e inicializa la variable `numero_1` con un valor `10`.  
En la ==segunda línea== se imprime (utiliza) el valor de dicha variable. Aparecerá en la consola el valor `10`.  
En cuarta línea se reasigna la variable con el valor `20`. Esta vez, en el último `print()` (==línea 5==), aparecerá el valor `20`.

## El flujo de ejecución del código

Puedes interpretar el flujo de ejecución del código (orden en que se ejecuta el código) como una línea temporal. En esa línea, se narran los pasos que van a ocurrir y en qué orden

La primera línea se ejecuta primero, así en orden, hasta llegar a la última. Así es como se ejecutan las cosas normalmente en un lenguaje de programación.

Sin embargo, más adelante aprenderás que este orden se puede alterar con las estructuras de control de flujo.