En este capítulo, te mostraré los cuatro tipos de datos básicos de Python, `str`, `int`,`float` y `bool`, con algún pequeño ejemplo de uso de cada uno, pero no será hasta los siguientes capítulos que empecemos a utilizarlos plenamente.

---
Los datos de tipo `str` se utilizan para representar texto.

Una cadena de caracteres es un conjunto de letras, números o símbolos que comúnmente llamamos “texto”.

Puedes crear una variable de este tipo asignando texto entrecomillado.

Por ejemplo:

```python
saludo = "Hola alumnos"
```

Al entrecomillar el texto le indicamos al intérprete de Python donde empieza y donde acaba. Las primeras comillas indican la apertura de la cadena de caracteres, y las otras, indican el cierre. Si el intérprete de Python no encuentra el cierre, se produce un error.

> De ahora en adelante, me referiré a este tipo de dato como `str`, cadena de texto, cadena de caracteres o _string_.
> 
>  Todos estos nombres son para referirse a lo mismo, y se emplean muy a menudo por la comunidad de programadores.

Las cadenas de caracteres se pueden escribir tanto con las comillas simples (`''`), como con comillas dobles (`""`).

Aquí tienes el ejemplo anterior, escrito con comillas simples. Perfectamente válido, también.

```python
saludo = 'Hola alumnos'
```

Si empiezas una cadena de caracteres con un tipo de comilla, debes acabarlo con el mismo. No es válido hacer esto:

```python
saludo = 'Hola alumnos"
```

 Error en la consola

```
SyntaxError: unterminated string literal (detected at line 1)
```

> **El error lo indica en las primeras comillas.**  
> El intérprete sabe que ahí empieza una cadena de caracteres, pero no puede encontrar el cierre correspondiente, al haber utilizado otro tipo de comillas.

## El tipo de dato numérico entero (int)

El tipo de dato `int` se utiliza para almacenar números enteros. Es decir, números positivos, negativos y cero. Sin decimales.

Puedes crear una variable de tipo int, simplemente asignando sin comillas, un valor numérico entero a una variable. Por ejemplo:

```python
edad = 32
```

Para representar un número negativo, lo haremos añadiendo el símbolo menos (`-`) junto al número, y sin espacios:

```python
temperatura = -10
```

> Recuerda poner siempre los valores numéricos sin comillas, si no, serán simple texto con el que no podremos hacer operaciones matemáticas con normalidad.

## El tipo de dato decimal (float)

Los números de punto flotante, conocidos como `float`, se utilizan para representar números con decimales.

Puedes crear una variable de este tipo, asignando, sin comillas, un valor numérico con decimales a una variable.  
Pueden ser números positivos, cero o negativos, pero siempre con parte decimal. Si los pones sin parte decimal, serán interpretados por el intérprete de Python, como tipo de dato `int`.

Este tipo de dato se conoce como número de punto flotante, número de coma flotante o simplemente, decimales.

Aquí tienes un ejemplo de un tipo de dato `float`:

```python
PI = 3.14159
```

> ¡Importante! La parte decimal de los datos de tipo `float` debe ir con un punto, no una coma.

Si quieres crear un `float` negativo, añádele el símbolo menos (`-`) antes del número:

```python
valor_negativo = -17.6743
```

## El tipo de dato booleano (bool)

Los datos de tipo `bool` de Python, se utilizan para representar valores booleanos.

Estos valores booleanos únicamente tienen dos posibles estados: `True` y `False` (verdadero y falso).

Para crear una variable de este tipo, solo tienes que asignarle uno de estos dos valores **sin comillas**, para evitar que sean tratados como tipo de dato `str`.

Aquí tienes un ejemplo con las dos posibilidades de este tipo de dato:

```python
valor_booleano = True
valor_booleano = False
```

> ¡Importante! El intérprete de Python distingue mayúsculas de minúsculas. Por lo tanto, ten en cuenta que la T y la F, de `True` y `False`, respectivamente, deben ir siempre en letra mayúscula. El resto, en letra minúscula.

> Los booleanos vienen del _álgebra de Boole_, un sistema algebraico desarrollado por el matemático británico _George Boole_ en el siglo XIX.
