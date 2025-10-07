Las palabras reservadas de los lenguajes de programación son elementos muy importantes para establecer una comunicación adecuada con el intérprete.

---
Al igual que ocurre con un lenguaje humano, debemos aprender por lo menos lo básico en vocabulario, para empezar a desarrollar una comunicación con otra persona.

En programación, se puede interpretar de la misma forma, deberás aprender a comunicarte con el intérprete de Python, de forma que entienda todo lo que le quieres pedir.

> A las palabras reservadas de un lenguaje de programación, se les conoce también como palabras clave.

Las palabras reservadas son elementos reservados en un lenguaje de programación. Estos elementos no pueden ser utilizados en ningún identificador, de ningún elemento que escribas, como por ejemplo, el nombre de una variable, el nombre de una función, una clase, etc.

Por ejemplo, la palabra `global` está reservada en Python. Entonces, no puedes llamar a una variable de esa forma:

```python
global = 10
```

 Error en la consola

```
SyntaxError: invalid syntax
```

> El error de sintaxis en Python, `_SyntaxError: invalid syntax_`, ocurre en este caso porque `global` es una palabra clave reservada del lenguaje, lo que quiere decir que tiene un propósito específico dentro de la estructura del código. No se puede utilizar como nombre de una variable, ya que Python lo reconoce como parte de la sintaxis para declarar variables globales dentro de funciones (tema más avanzado que trataremos en otro momento).  
> Intentar asignarle un valor directamente, como se hace con `global = 10`, crea un conflicto que provoca este error.> 
>  Para solucionarlo, simplemente usa otro nombre que no sea una palabra reservada.

Por supuesto, sí que puedes poner un nombre que la contenga.

Por ejemplo:

```python
variable_global = 10
```

### Listado de palabras reservadas de Python

A continuación tienes el listado completo con todas las palabras reservadas que tiene Python. Por el momento son un total de 35, pero puede que con el tiempo se añada alguna más.

Son bastantes palabras, y por supuesto no te las tienes que aprender de memoria. Con el tiempo las irás memorizando, a medida que las vayas utilizando, y sepas perfectamente cómo aplicarlas al código.

Lo importante es que tengas a mano la siguiente página para futuras referencias, y que sepas que no debes utilizar nunca estas palabras como identificadores (nombres de algún elemento, como puede ser una variable).

Por supuesto, estás empezando con Python, y muchas de las siguientes definiciones carecerán de sentido para ti, en este momento. No obstante, pronto irás entendiéndolo todo.

Estas son las 35 palabras:

#### and

Operador lógico que evalúa dos expresiones y devuelve `True` solo si ambas son verdaderas.

#### as

Palabra reservada utilizada para asignar un alias a un módulo o recurso importado, facilitando su uso dentro del código.

#### assert

Comprueba si una expresión es verdadera; si no lo es, lanza una excepción, generalmente utilizada para depuración.

#### async

Declara una función como asíncrona, lo que permite que su ejecución no bloquee el hilo principal del programa.

#### await

Pausa la ejecución de una función asíncrona hasta que una operación asíncrona previamente iniciada se complete.

#### break

Interrumpe un bucle antes de que se complete, terminando la ejecución de la estructura que lo contiene.

#### class

Palabra clave que define una nueva clase, proporcionando una plantilla para crear objetos en programación orientada a objetos.

#### continue

Hace que el programa salte a la siguiente iteración de un bucle, omitiendo el resto del código en la iteración actual.

#### def

Se utiliza para declarar una nueva función, especificando su nombre, parámetros y el bloque de código que ejecutará.

#### del

Elimina objetos o sus referencias, como claves en un diccionario o variables, liberando así recursos.

#### else

Parte de una estructura condicional que ejecuta un bloque de código si las condiciones anteriores no se cumplen.

#### finally

Bloque de código que se ejecuta siempre al final de una estructura `try`-`except`, sin importar si hubo una excepción.

#### for

Inicia un bucle que itera sobre una secuencia, como una lista o un rango, ejecutando el código interno en cada iteración.

#### global

Declara una variable como global, permitiendo que se acceda y modifique fuera del alcance local de una función.

#### elif

Parte de una estructura condicional que permite evaluar múltiples condiciones si las anteriores no se cumplen.

#### except

Captura y maneja excepciones en un bloque `try`, permitiendo que el programa continúe sin interrumpirse.

#### False

Valor booleano que representa la falsedad lógica en Python.

#### from

Se usa para importar elementos específicos de un módulo, como funciones o clases, sin necesidad de importar todo el módulo.

#### if

Inicia una estructura condicional que evalúa una expresión, ejecutando el bloque de código correspondiente si es verdadera.

#### import

Incorpora módulos o bibliotecas externas en el código, habilitando el uso de sus funciones, clases u objetos.

#### in

Comprueba si un elemento está presente dentro de una secuencia, como una lista, tupla o cadena de caracteres.

#### is

Compara la identidad de dos objetos, verificando si son exactamente el mismo objeto en memoria.

#### lambda

Define funciones anónimas y de una sola línea, que pueden ser utilizadas en cualquier parte del código donde se necesite una función simple.

#### nonlocal

Permite modificar variables dentro de funciones anidadas que no pertenecen a la función local, pero tampoco son globales.

#### None

Representa la ausencia de valor o un valor nulo en Python.

#### not

Operador lógico de negación, que invierte el valor de una expresión booleana.

#### or

Operador lógico que evalúa dos expresiones y devuelve `True` si al menos una de ellas es verdadera.

#### pass

Permite dejar bloques de código vacíos sin generar errores, útil como marcador de posición en funciones o bucles.

#### raise

Genera una excepción manualmente, permitiendo que el programador indique situaciones de error específicas.

#### return

Finaliza la ejecución de una función y devuelve un valor al llamador de la función.

#### True

Valor booleano que representa la verdad lógica en Python.

#### try

Bloque de código que intenta ejecutar operaciones que podrían generar excepciones, permitiendo el manejo adecuado de errores.

#### while

Inicia un bucle que se ejecuta mientras una condición dada sea verdadera.

#### with

Palabra clave que facilita la gestión de recursos, como archivos, asegurando que se cierren o liberen adecuadamente después de su uso.

#### yield

Se utiliza dentro de funciones generadoras para devolver valores de manera pausada, permitiendo que se reanude su ejecución posteriormente.

### Ver el listado de palabras reservadas de Python en la consola

Algo muy práctico que puedes hacer, es escribir el siguiente código en un archivo de Python. Este pequeño código, lo que hace, es mostrar en la consola el listado de las palabras clave de Python.

```python
import keyword
print(keyword.kwlist)
```

   Resultado en la consola

```
['False', 'None', 'True', 'and', 'as', 'assert', 'async','await', 'break', 'class', 'continue',
'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in',
'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

Gracias a esto, siempre que tengas dudas sobre si una palabra está reservada o no, puedes consultarlo.

### Palabras blandas

Algunas palabras son determinadas en la referencia oficial de Python, como soft keywords (palabras clave blandas).

Estas palabras son las siguientes:

- `match` (versión 3.10)
- `case` (versión 3.10)
- `type` (versión 3.12)
- `_` (versión 3.10)

Lo que aparece entre paréntesis es la versión en la que se añadieron estas palabras blandas. Al ser bastante recientes, no podemos descartar que vaya a haber más, en un tiempo no muy lejano.

Estos identificadores están definidos en el propio lenguaje Python, pero no están reservados, por eso, no aparecen en el listado de palabras reservadas.

El motivo de que haya estas palabras blandas, es para establecer retrocompatibilidad. Con el tiempo, se van añadiendo estructuras nuevas, como la de `match`. Esto quiere decir, que es posible que mucho software anterior a esa versión, escrito con Python, utilice una palabra tan común como esta, que significa en español, “coincidencia”. Hay muchos programas antiguos que llevan variables o funciones, llamadas de esa forma.

En conclusión, la decisión de no forzar estas palabras, como palabras reservadas, fue una decisión tomada para permitir la migración de programas antiguos sin grandes cambios.

Con una palabra blanda puedes hacer esto, aunque no es recomendable:

```python
match = 10
```

Pero con una palabra reservada, aunque quieras, no puedes hacer esto, recuerda el ejemplo de `global` del principio del capítulo.