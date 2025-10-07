Antes de empezar a programar, necesitas leer este capítulo. Se trata de un capítulo dedicado a las normas y convenciones para los nombres de variable, no todas, ya que hay muchas en Python, pero sí algunas de las más importantes.

---
Empezando con este tema, tenemos que tener en cuenta que aparte de reglas obligatorias, contamos con una serie de convenciones. Estas convenciones son buenas prácticas que deberás seguir. Con ellas te adaptarás al estilo de código de la comunidad Python.

Para que nos entendamos bien. **Si hablo de regla, estoy hablando de una norma obligatoria. Si no la cumples tendrás errores en tus programas Python**.

En cambio, **cuando hable de convenciones, me estaré refiriendo a esas buenas prácticas que todos deberíamos seguir para hacer un código limpio, y muy fácil de leer por las personas.**

### Reglas en los nombres de variables

Empecemos por las reglas obligatorias sobre los nombres de variables en Python.

#### Sensible a mayúsculas y minúsculas

Python es un lenguaje sensible a mayúsculas y minúsculas. Esto quiere decir, que deberás escribir los nombres de los elementos respetándolas.

Aquí tienes tres variables que el intérprete de Python considerará como diferentes:

```python
nombre_variable
NOMBRE_VARIABLE
Nombre_Variable
```

> Hay lenguajes de programación donde no importan las mayúsculas y minúsculas.
> 
>  En Python, siempre importan.

Para nombrar a las variables, utiliza únicamente letras, números y el carácter guion bajo (`_`), ya que los nombres de estas solo pueden estar formados por estos caracteres.

Los siguientes ejemplos son nombres de variables válidos en Python:

```python
numero1
fecha_actual
resultado
```

En cambio, estos tres ejemplos, son incorrectos:

```python
1numero
$resultado
nombre-usuario
```

Quizás te extrañe que sea incorrecto el primer nombre de variable, del último ejemplo (`1numero`). Puesto que he dicho que se admiten nombres con números y letras. Esto es debido a una norma más, la cual viene a continuación.

#### Empezar por un número

Los nombres de variables en Python, no pueden empezar por un valor numérico. A partir del segundo carácter del nombre, puedes poner los que quieras, pero el primero, debe ser sí o sí, un guion bajo o una letra.

Aquí tienes un par de ejemplos más de nombres correctos a nivel de regla:

```python
_n0c3e
a1_b2_c3
```

Y digo “correctos a nivel de regla”, porque son nombres poco correctos para llevar unas buenas prácticas, ya que no representan un nombre descriptivo con su cometido.

¿Se te ocurre qué dato guardar en una variable como `a1_b2_c3`?

#### Palabras reservadas en los nombres

El término “palabra reservada”, viene del cometido de estas palabras.

Tales palabras hacen funcionar partes muy importantes del lenguaje de programación, de modo que se reservan para que no las puedas utilizar en tus propios elementos del código. Acción que crearía múltiples problemas.

Recuerda que tienes las palabras reservadas en el capítulo anterior. Si ves que en algún momento tienes algún error, fíjate en que no hayas puesto una de esas palabras, por ejemplo, como nombre de variable.

### Convenciones en los nombres de las variables

Pasemos a ver diferentes convenciones que tiene Python a la hora de crear nombres para las variables.

> Tal y como he indicado al principio de este capítulo, las convenciones son buenas prácticas que deberás utilizar, aunque no sean estrictamente obligatorias.

#### Utiliza nombres de variables descriptivos

Una de las cosas más importantes a la hora de escribir código limpio, es escribir nombres de variables descriptivos con los valores que van a almacenar.

Por ejemplo, mira el poco sentido que tienen las siguientes variables:

```python
fecha = "Hola"
nombre = 7
a = 10034.5657
```

Estas variables no van a dar error, pero constituyen una mala práctica, ya que en un futuro, cuando leas de nuevo tu programa, para modificar algo, o lo hagan otras personas, va a ser un completo caos.

Imagínate entender la lógica de quizás un centenar, o varios millares de variables, en un programa con nombres de este tipo.

¿No te parece mejor así?

```python
saludo = "Hola"
numero = 7
numero_preciso = 7.5657
```

Puedes ver que la variable `saludo` contiene un valor con un saludo, que la variable `numero` lleva un valor numérico y que `numero_preciso`, tiene un número con decimales, el cual da cierta precisión decimal. Son nombres que se correlacionan con su contenido almacenado.

Si que es cierto que en algunos ejemplos del curso, utilizaré variables con nombres simples como `a`, `b`, `c`, etc., con el fin de simplificar las explicaciones. En ejemplos pequeños de código, ayudan a ver mejor el flujo de la lógica y a comprender los conceptos básicos. Pero recuerda siempre que, en proyectos más grandes y aplicaciones reales, es importante utilizar nombres de variables más descriptivos para que el código sea fácil de leer por las personas.

Por ejemplo, cuando te explique de qué forma funciona una sencilla función de suma, podría hacerlo con variables como `a` y `b`, con el fin de simplificar el código y las explicaciones.

Por ejemplo:

```python
def sumar(a, b):
    return a + b
```

La frase que te podría dar para explicarte esta función, sería algo como “La función `sumar()` devuelve el resultado de la suma entre los valores de `a` y `b`.”

La especificación de nombres en cada programa que hagas en el futuro, quedará de tu parte. Con la experiencia adquirida, cada día lo harás mejor y te saldrá con más facilidad, sin detenerte demasiado a pensar nombres de variables.

#### Nombres en minúsculas

Por convención, los nombres de las variables deberán escribirse completamente en minúsculas, sin excepción.

#### Nombres compuestos y el guion bajo

Otra convención muy característica de Python, es la de separar los nombres compuestos de las variables con la convención _snake_case_, que se basa en que cada palabra de un nombre de variable (que tiene más de una), debe ser separado con un guion bajo.

Aquí tienes unos ejemplos:

```python
nombre_completo
hora_actual
variable_con_varias_palabras
```

> El nombre _snake_case_ proviene de la forma en que quedan los nombres, como el zigzagueo de una serpiente. Algo que encaja genial con el nombre Python.

#### No escribas los nombres en mayúsculas

He explicado que los nombres de variable deben ir por convención en minúsculas. Sin embargo, tenemos otro tipo de “variables”, que se deben escribir completamente en mayúsculas. Se trata de las constantes.

Las constantes de Python, en realidad son solo variables con el nombre escrito en mayúsculas, es un tema que explicaré en otro capítulo.

```python
PI = 3.14159 # Constante
numero = 10 # Variable
```

La forma de distinguir un tipo de contenedor del otro, es siguiendo las convenciones de nombres.

#### No escribas acentos u otros caracteres especiales

Algo que no deberías hacer, es poner acentos, diéresis u otros símbolos, en los nombres de variable.

A nivel de sintaxis, van a funcionar, pero no se recomienda por problemas de compatibilidad sobre diferentes sistemas, problemas en cuanto a trabajo en equipo internacional, etc.

Imagina que algún desarrollador, te envía código como el de siguiente imagen:

![Python escrito en Japonés](https://www.programacionfacil.org/assets/images/cursos/python-basico/python-japones.png)

En Python funciona, pero no es legible para ti (si no conoces este idioma).

Por otro lado, tendrás que ir copiando y pegando los nombres de variable, si no tienes a mano todos esos símbolos.

Imagina esto mismo, pero con un programa complejo. Se puede volver algo tedioso.

Entonces, en Python se recomienda escribir siempre en inglés, seas de donde seas, y tengas el idioma materno que tengas. Por eso, he decidido incluir un montón de notas de traducción, para que te vayas acostumbrando.

Si no sabes todavía nada, o casi nada de inglés, no te preocupes, empieza con los nombres en español, sin utilizar acentos u otros caracteres especiales como la letra ñ, y con el tiempo, ya aprenderás a escribir código completamente en inglés.

También se recomienda dejar de usar acentos en los comentarios, pero esto ya no afecta de la misma forma, ya que los comentarios, son ignorados por el intérprete, no los tendrá que usar para escribir código otra persona, aunque sí para entenderlo. Entonces, insisto en utilizar el idioma inglés cuando sepas lo suficiente.

![Código básico de Python](https://www.programacionfacil.org/assets/images/cursos/python-basico/codigo-python-basico.png)

En este curso, no voy a dar por hecho que sepas inglés, de modo, que todos los ejemplos serán en español, sin símbolos en el propio código, pero con acentos en los comentarios.