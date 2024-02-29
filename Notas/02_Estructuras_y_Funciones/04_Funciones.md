[Contenidos](../Contenidos.md) \| [Anterior (3 Manejo de archivos)](03_Archivos.md) \| [Próximo (5 Tipos y estructuras de datos)](05_TiposDatos.md)

# 2.4 Funciones

A medida que tus programas se vuelven más largos y complejos, vas a necesitar organizarte. En esta sección vamos a introducir brevemente funciones y módulos de la biblioteca así como también la administración de errores y excepciones.


## Funciones a medida

Vamónos brevemente al terreno de la matemática e imaginemos una función lineal sencilla:

$$f(x) = 2x + 3$$

Esta función tiene un parámetro `x` al cual podemos pasarle un valor, realiza una operación `2x + 3` y nos devuelve otro valor. En la siguiente tabla le pasamos un valor a `x` y lo que nos devuelve la función `f(x)`:

| $$x$$ | $$f(x)$$ |
| ----- | -------- |
| 2     | 7        |
| 3     | 9        |
| 5     | 13       |

Volviendo a la programación, las funciones son parecidas: son bloques de código reutilizables a los que se les pueden pasar parámetros para hacer programas flexibles. Son una serie de instrucciones que realizan tareas y devuelven un valor.

En este caso pueden o no aceptar parámetros y pueden o no retornar cosas. Cuando la función no retorna un valor, se le suele llamar **procedimiento** y cuando retorna algo se le llama **función**. Si queremos que la función devuelva un valor, tenemos que colocar `return`. Veamos unos ejemplos:

```python
# Estructura de una función
# Primero escribimos la firma de la función

def componer_cadena(texto1, texto2): # Esto es la firma de la función
    '''Componer cadena
    
    Esta función toma dos strings y devuelve la concatenación de ambas.
    '''# Esto que está entre comillas triples es la documentación de la función

    cadena = texto1 + texto2         # Cuerpo de la función
    return cadena                    # Retorno de valor

# Ahora llamamos a la función en nuestro script
cadena1 = 'Hola'
cadena2 = 'Mundo'
composicion = componer_cadena(cadena1, cadena2) # Guardamos el valor de la función
print(composicion)

# La salida será la siguiente
# HolaMundo
```

En el ejemplo anterior mostramos cómo es la estructura de una función:

1. **Firma:** la firma de la función es lo primero que se coloca: `def nombre_de_funcion(parametros):`. Siempre ponemos la palabra reservada `def` para definir la función, seguido del ``nombre_de_la_función``, luego ``paréntesis`` donde pondremos nuestros ``parámetros`` y finalizamos con `:`. Si nuestra función no utiliza parámetros, colocamos los paréntesis sin nada adentro.
2. **Cuerpo:** el cuerpo de la función es todo lo que sigue **indentado** debajo. Acá colocamos todo el código que vamos a usar. Los `parámetros` son variables internas de la función que van a hacer referencia a valores externos que le pasemos. Notar que nuestros parámetros en el ejemplo son `texto1` y `texto2` y los usamos dentro de la función. El `return` de la función (casi)siempre se coloca a lo último y vamos a especificarle qué valor queremos que retorne. En nuestro caso, le decimos que retorne la variable `cadena` que es el resultado.

Luego, cuando llamamos a nuestra función en la variable `composicion` le estamos pasando dos `argumentos` que son `cadena1` y `cadena2`. Notar que estos ``argumentos`` no tienen por qué llamarse exactamente igual a los `parámetros`.

> [!NOTE]
> Es una buena práctica documentar las funciones justo debajo de la firma. Sin embargo, no te preocupes por hacer eso ahora ya que lo veremos más adelante.

> [!IMPORTANT]
> Por defecto las funciones en python retornan `None` en caso de no especificarle un retorno.

Otro ejemplo:

```python
def sumcount(n):
    '''
    Devuelve la suma de los primeros n enteros
    '''
    total = 0
    while n > 0:
        total += n
        n -= 1
    return total
```

Para llamar a una función:

```python
a = sumcount(100)
```

## Funciones de la biblioteca

Python trae una gran biblioteca estándar.
Los módulos de esta biblioteca se cargan usando `import`.
Por ejemplo:

```python
import math
x = math.sqrt(10)

import urllib.request
u = urllib.request.urlopen('http://www.python.org/')
data = u.read()
```

Vamos a estudiar bibliotecas y módulos en detalle más adelante.

## Errores y excepciones

Las funciones informan los errores como excepciones. Dado que una excepción interrumpe la ejecución de una función, la misma puede generar que todo el programa se detenga si no es administrada adecuadamente.

Probá por ejemplo lo siguiente en tu intérprete:

```python
>>> int('N/A')
Traceback (most recent call last):
File "<stdin>", line 1, in <módulo>
ValueError: invalid literal for int() with base 10: 'N/A'
>>>
```

Para poder entender qué pasó (debuguear), el mensaje describe cuál fue el problema, dónde ocurrió y un poco de la historia (traceback) de los llamados que terminaron en este error.

### Atrapar y administrar excepciones

Las excepciones pueden ser atrapadas y administradas.
Para atrapar una excepción, se usan los comandos `try - except`. Podés probar el siguiente fragmento de código pegándolo en un archivo [foo.py](https://es.wikipedia.org/wiki/Foo) :

```python
numero_valido=False
while not numero_valido:
    try:
        a = input('Ingresá un número entero: ')
        n = int(a)
        numero_valido = True
    except ValueError:
        print('No es válido. Intentá de nuevo.')
print(f'Ingresaste {n}.')
```

Si en este ejemplo le usuarie ingresa por ejemplo una letra, el comando `n = int(a)` genera una excepción de tipo `ValueError`: el comando `numero_valido = True` no se ejecuta, la excepción es atrapada por el `except ValueError` y el ciclo se repite. Probalo ingresando letras, números con decimales y números enteros. Probá también qué ocurre si querés salir sin ingresar nada generando una excepción presionando las teclas `Ctrl+C`. Leé el mensaje que describe lo ocurrido:  `Ctrl+C` genera una excepción de tipo `KeyboardInterrupt` que no es atrapada.

Si no especificamos el tipo de excepción que queremos atrapar, vamos a terminar atrapando todas la excepciones. Probá lo mismo que antes pero con este código.

```python
numero_valido=False
while not numero_valido:
    try:
        a = input('Ingresá un número entero: ')
        n = int(a)
        numero_valido = True
    except:
        print('No es válido. Intentá de nuevo.')
print(f'Ingresaste {n}.')
```

Deberías observar una diferencia: al presionar las teclas `Ctrl+C` la excepción `KeyboardInterrupt` sí es atrapada y no se termina el ciclo hasta no ingresar un número entero.

En general es difícil saber exactamente qué tipo de errores pueden ocurrir por adelantado. Para bien o para mal, la administración de excepciones suele ir creciendo a medida que un programa va generando errores inesperados (al mejor estilo: "Uh! Me olvidé de que podía pasar esto. Deberíamos preverlo y administrarlo adecuadamente para la próxima").

### Generar excepciones

Para generar una excepción (también diremos *levantar* una excepción, porque es más cercano al término inglés "raise"), se usa el comando `raise`. Por ejemplo, si tenemos el siguiente código en el archivo `foo.py`:

```python
raise RuntimeError('¡Qué moco!')
```

Al correrlo va a detener la ejecución y permite rastrear la excepción leyendo el mensaje de error que imprime.

```bash
bash $ python3 foo.py
Traceback (most recent call last):
  File "foo.py", line 21, in <módulo>
    raise RuntimeError("¡Qué moco!")
RuntimeError: ¡Qué moco!
```

Alternativamente, esa excepción puede ser atrapada por un bloque `try-except`, pudiendo de esta forma evitar que el programa termine.

## Ejercicios

### Ejercicio 2.5: Definir una función
Probá primero definir una función simple:

```python
>>> def saludar(nombre):
        'Saluda a alguien'
        print('Hola', nombre)

>>> saludar('Guido')
Hola Guido
>>> saludar('Paula')
Hola Paula
>>>
```

Si la primera instrucción de una función es una cadena, sirve como documentación de la función. Probalo escribiendo `help(saludar)` para ver cómo la muestra.

### Ejercicio 2.6: Transformar un script en una función
Transformá el programa `costo_camion.py`  (que escribiste en el [Ejercicio 2.2](../02_Estructuras_y_Funciones/03_Archivos.md#ejercicio-22-lectura-de-un-archivo-de-datos) de la sección anterior) en una función `costo_camion(nombre_archivo)`. Esta función recibe un nombre de archivo como entrada, lee la información sobre los cajones que cargó el camión y devuelve el costo de la carga de frutas como una variable de punto flotante.

Para usar tu función, cambiá el programa de forma que se parezca a esto:

```python
def costo_camion(nombre_archivo):
    ...
    # Tu código
    ...

costo = costo_camion('../Data/camion.csv')
print('Costo total:', costo)
```

Cuando ejecutás tu programa, deberías ver la misma salida impresa que antes. Una vez que lo hayas corrido, podés llamar interactivamente a la función escribiendo esto:

```bash
bash $ python3 -i costo_camion.py
```

Esto va a ejecutar el código en el programa y dejar abierto el intérprete interactivo.

```python
>>> costo_camion('../Data/camion.csv')
47671.15
>>>
```

Es útil para testear y debuguear poder interactuar interactivamente con tu código.


### Ejercicio 2.7: Buscar precios
A partir de lo que hiciste en el [Ejercicio 2.3](../02_Estructuras_y_Funciones/03_Archivos.md#ejercicio-23-precio-de-la-naranja), escribí una función `buscar_precio(fruta)` que busque en archivo `../Data/precios.csv` el precio de determinada fruta (o verdura) y lo imprima en pantalla. Si la fruta no figura en el listado de precios, debe imprimir un mensaje que lo indique.


```python
>>> buscar_precio('Frambuesa')
El precio de un cajón de Frambuesa es: 34.35
>>> buscar_precio('Kale')
Kale no figura en el listado de precios.
```

Guardá este código en un archivo `buscar_precios.py` para entregar al final de la clase.


### Ejercicio 2.8: Administración de errores
Este ejercicio introduce el tema de administración de errores. Es un tema un poco avanzado. No te inquietes si no entendés aún en profundidad estos conceptos, los vamos a retomar más adelante. Simplemente nos parece que está bueno empezar desde temprano a hablar de estos temas.

Probá correr la siguiente función ingresando tu edad real, una edad escrita con letras (como "ocho") y una edad negativa (-3):

```python
def preguntar_edad(nombre):
    edad = int(input(f'ingresá tu edad {nombre}: '))
    if edad<0:
        raise ValueError('La edad no puede ser negativa.')
    return edad
```

Ahora probá este ejemplo que atrapa la excepción generada con `raise` y continúa la ejecución con la siguiente persona.

```python
for nombre in ['Pedro','Juan','Caballero']:
    try:
        edad = preguntar_edad(nombre)
        print(f'{nombre} tiene {edad} años.')
    except ValueError:
        print(f'{nombre} no ingresó una edad válida.')
```

Vamos a usar estas ideas aplicadas al procesamiento de un archivo CSV. ¿Qué pasa si intentás usar la función `costo_camion()` con un archivo que tiene datos faltantes?

```python
>>> costo_camion('../Data/missing.csv')
Traceback (most recent call last):
    File "<stdin>", line 1, in <módulo>
    File "costo_camion.py", line 11, in costo_camion
    ncajones = int(fields[1])
ValueError: invalid literal for int() with base 10: ''
>>>
```

El programa termina con un error. A esta altura tenés que tomar una decisión. Para que el programa funcione podés editar el archivo CSV de entrada de manera de corregirlo (borrando líneas o adecuando la información) o podés modificar el código para que maneje las líneas *incorrectas* de alguna manera.

Modificá el programa `costo_camion.py` para que atrape la excepción con un bloque `try - except`, imprima un mensaje de aviso (warning) y continúe procesando el resto del archivo.

Vamos a retomar el tema de administración de errores en las próximas clases. Es normal que te quede una sensación de que no se entiende del todo. Nos faltan varias piezas para armar el rompecabezas.

### Ejercicio 2.9: Funciones de la biblioteca
Python viene con una gran biblioteca estándar de funciones útiles. En este caso el módulo `csv` podría venirnos muy bien. Podés usarlo cada vez que tengas que leer archivos CSV. Acá va un ejemplo de cómo funciona.

```python
>>> import csv
>>> f = open('../Data/camion.csv')
>>> rows = csv.reader(f)
>>> headers = next(rows)
>>> headers
['nombre', 'cajones', 'precio']
>>> for row in rows:
        print(row)

['Lima', '100', '32.2']
['Naranja', '50', '91.1']
['Caqui', '150', '103.44']
['Mandarina', '200', '51.23']
['Durazno', '95', '40.37']
['Mandarina', '50', '65.1']
['Naranja', '100', '70.44']
>>> f.close()
>>>
```

Una cosa buena que tiene el módulo `csv` es que maneja solo una gran variedad de detalles de bajo nivel como el problema de las comillas, o la separación con comas de los datos. Es posible que en versiones nuevas de python el problema de las comillas ya esté resuelto, pero sucedía esto cuando se leía un archivo y se usaba el método ``split()``: ``['"Lima"', '100', '32.2']``.

> [!NOTE]
>Si no pudiste visualizar este error, acá te mostramos qué pasaba. Si tuvieras una línea de dato en el csv que sea así:
>
> ```csv
> "Naranja",50,91.1
> ```
>
> Y usaras el método ``split()`` obtendrías esto:
>
> ```py
> ['"""Naranja""', '50', '91.1"\n']
> ```
>
> Por eso está bueno usar el módulo ``csv``.


Modificá tu programa `costo_camion.py` para que use el módulo `csv` para leer los archivos CSV y probalo en un par de los ejemplos anteriores.




[Contenidos](../Contenidos.md) \| [Anterior (3 Manejo de archivos)](03_Archivos.md) \| [Próximo (5 Tipos y estructuras de datos)](05_TiposDatos.md)

