En este capítulo se explica lo que son los comentarios en programación, cómo usarlos y los tipos que maneja Python.

---
Los comentarios pueden parecer simples anotaciones en el código cuando se empieza en programación; sin embargo, son una parte importante de esta.

**Un comentario es un trozo de texto que se incluye en el código, pero que es ignorado por el intérprete de Python cuando se ejecuta el programa.**

Los comentarios se utilizan para explicar lo que hace el código, cómo funcionan partes de él, porque se escribió de esa forma, etc. Son muy útiles para que otros programadores entiendan el código, y para que quien lo escribió, se acuerde de lo que hizo cuando vuelva a revisarlo más adelante.

## Comentarios de una línea

En Python los comentarios de una línea se escriben utilizando el carácter almohadilla (`#`). Todo lo que sigue a una almohadilla en una línea, se considera un comentario, y es ignorado cuando se ejecuta el código.

Por ejemplo:

```python
# Imprime en la consola el texto ¡Hola mundo!
print("¡Hola mundo!")
```

En este ejemplo, el comentario está explicando lo que hace la línea de código que tiene debajo.

Este es solo un ejemplo para que entiendas los comentarios, no significa que tengas que estar comentando línea por línea el código, sino más bien, las partes más importantes.

La decisión de cuándo escribir un comentario y cuándo no, es algo que aprenderás a decidir con un poco de práctica.

## Desactivar partes del código

Los comentarios también se pueden utilizar para desactivar temporalmente una parte del código. Por ejemplo, si tienes una línea de código que no deseas ejecutar, para hacer pruebas, la puedes comentar para que no tome efecto en el momento de ejecutar todo el código.

Es decir, que el intérprete haga como que no existe. Esto es especialmente útil cuando se está depurando código (buscando y arreglando fallos), y se necesitan probar diferentes partes del mismo, sin tener que borrar, y volver a escribir el código cada vez.

Aquí tienes un ejemplo:

```python
# print("Esta línea no se ejecutará")
print("Esta línea se ejecutará")
```

En este ejemplo, ==la primera línea== contiene la función `print()` comentada, por lo tanto, no se ejecutará, aunque sea código Python.

En cambio, la segunda línea, que no está comentada, se ejecutará con normalidad.

> Con el fin de hacer una distinción entre los comentarios y el resto de código, en los editores de código o IDEs, verás que todo lo que queda comentado, se ve del mismo color, no se colorean las palabras clave, operadores, tipos de datos, etc.

## Dividir secciones en el código

Los comentarios también se pueden usar para dividir el código en secciones más pequeñas y legibles. Por ejemplo, si tenemos una función que es especialmente larga, se pueden utilizar comentarios para dividirla en secciones más pequeñas, y explicar lo que hace cada una. Esto hará que el código sea más fácil de leer, y de entender por parte de las personas.

En el siguiente ejemplo, la función `funcion_larga()`, se divide en tres secciones diferentes. Mediante el uso de comentarios se explica lo que hace cada una de ellas:

```python
def funcion_larga():
    # Primera sección
    # Esta sección hace esto
    . . .
    # Segunda sección
    # Esta sección hace esto otro
    . . .
    # Tercera sección
    # Esta sección hace otra cosa más
    . . .
```

Los tres puntos (`. . .`) en este fragmento de código representan el código Python que pondrías en cada sección; son meramente orientativos.

> Las funciones son elementos que pueden contener cierto código listo para realizar una tarea específica. El código contenido en una función, puede ser reutilizado tantas veces como sea necesario.
> 
>  Hablaré de estos elementos con todo detalle cuando llegue el momento.

## Comentarios a la derecha del código

Algo que encontrarás con mucha frecuencia en el código Python, son pequeños comentarios a la derecha de ciertas líneas de código. A continuación, tienes un ejemplo:

```python
print("Línea 1.")
print("Línea 2.") # Comentario cualquiera
print("Línea 3.")
```

 Resultado en la consola

```
Línea 1.
Línea 2.
Línea 3.
```

Estos comentarios no anulan la línea de código. Siempre que se mantengan después de la instrucción.

En cambio, si el comentario está antes de la instrucción (a su izquierda), esta es anulada. Pasa a ser parte del propio comentario:

```python
print("Línea 1.")
# Comentario cualquiera print("Línea 2.")
print("Línea 3.")
```

   Resultado en la consola

```
Línea 1.
Línea 3.
```

## ¿Cuándo usar cada tipo de comentario?

Normalmente, los comentarios que están exclusivamente ocupando su línea, los vamos a utilizar para describir lo que hace un bloque de código entero. Aquí tienes un ejemplo:

```python
# Bucle que itera en sentido decreciente desde 10 hasta 1
for i in range(10, 0, -1):
    print(i)
```

Este comentario no anula ninguna instrucción y se utiliza para indicar el funcionamiento de un conjunto de instrucciones relacionadas.

Por otro lado, los comentarios de una línea que van a la derecha, se suelen utilizar para describir lo que hace esa sola línea.

Por ejemplo:

```python
print("hola") # Imprime hola en la consola
```

Crea comentarios “inteligentes”. Colócalos solo donde creas que son necesarios. Al principio te servirán para tener unos apuntes de todo lo que vayas probando, pero en programas finales (los que utilizarán los usuarios), debes dar explicaciones para programadores.

Piensa que los programadores de Python ya saben qué hace, por ejemplo, un `print()` Entonces no hace falta que lo expliques en un comentario, para los posibles programadores que trabajen con tu código.

```python
# La función print() imprime un mensaje en la consola. Concretamente, el texto entrecomillado
print("hola")
```

Por ahora, pon todos los comentarios que quieras, para aprender te vendrá genial. No te preocupes por saber si debes ponerlo o no. Esto lo aprenderás con el tiempo.

## Los comentarios multilínea

Los comentarios multilínea se usan para documentar secciones más extensas de código. Estos pueden ocupar desde una sola línea, hasta las que necesites.

> Realmente, los comentarios multilínea de Python no son más que cadenas de caracteres literales, conocidas como _docstrings_, que podemos utilizar a modo de comentario.
> 
>  Con el fin de simplificarte el aprendizaje, llamaré por el momento a las _docstrings_, como comentarios multilínea, basándome en su funcionalidad más evidente.

### Sintaxis de comentario multilínea

Los comentarios multilínea se definen utilizando tres comillas dobles (`"""`) al principio, y tres más al final del comentario.

Las primeras sirven para abrir el comentario, y las otras, para cerrarlo.

Por ejemplo:

```python
"""
Este es un comentario
multilínea de Python.
Con él, podemos ir
escribiendo múltiples líneas.
"""
```

También es posible escribirlo con comillas simples (`'''`):

```python
'''
Este es un comentario
multilínea de Python.
Con él, podemos ir
escribiendo múltiples líneas.
'''
```

Además, es posible utilizar los comentarios multilínea para poder anular trozos enteros de código, en lugar de anular una sola línea. Esto hará que podamos anular trozos grandes de código, para hacer pruebas de funcionamiento del mismo, sin esa parte concreta habilitada.

Aquí tienes un ejemplo:

```python
# Este código imprime los números del 1 al 10
for i in range(1, 11):
    print(i)
"""
# Este código está anulado y no se ejecuta
for i in range(10, 0, -1):
    print(i)
"""
```

En este caso, se ejecutará el primer bucle `for`, y el segundo será ignorado.

### Docstrings

Las _docstrings_ normalmente se utilizan para documentar elementos del código. Elementos como módulos, clases, funciones, etc.

Aquí tienes un ejemplo:

```python
""" docstring del módulo """

class Clase:
    """ docstring de la clase """
    def metodo(self):
        """ docstring del método """

    def funcion():
        """ docstring de la función """
```

Cuando empieces a desarrollar software final, convendrá que añadas siempre estas anotaciones, que formarán parte de la documentación.

Si ya programas en otros lenguajes de programación, sabrás la importancia que tiene la documentación del software.

Si no has programado nunca, aprende al menos lo básico de programación, y poco a poco irás profundizando en temas como el de la documentación. No tengas prisa.