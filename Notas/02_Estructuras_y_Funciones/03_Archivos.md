[Contenidos](../Contenidos.md) \| [Anterior (2 Entorno de desarrollo integrado)](02_IDE.md) \| [Próximo (4 Funciones)](04_Funciones.md)

# 2.3 Manejo de archivos

La mayoría de los programas usa alguna fuente de datos. En esta sección discutimos el acceso a archivos.

## Archivos de entrada y salida

Con estos comandos podés abrir dos archivos, una para lectura y otro para escritura:

```python
f = open('foo.txt', 'rt')     # Abrir para lectura ('r' de read, 't' de text)
g = open('bar.txt', 'wt')     # Abrir para escritura ('w' de write, 't' de text)
```

> [!NOTE]
> Los nombres [foo, bar, foobar](https://es.wikipedia.org/wiki/Foo) son genéricos que se usan usualmente para aludir a un nombre que se ignora, son los análogos informáticos a fulano, mengano y zutano.

Para leer el archivo completo, o una parte:

```python
data = f.read()

# Leer 'maxbytes' bytes
data = f.read([maxbytes])
```

Para escribir un texto en el archivo:

```python
g.write('un texto')
```

Finalmente, hay que cerrar los archivos cuando terminamos de usarlos.

```python
f.close()
g.close()
```

**Es importante cerrar adecuadamente** los archivos y es bastante fácil olvidarse (puede que el programa termine y no se termine de guardar bien). Por eso, preferimos abrir los archivos con el comando `with` de la siguiente forma.

```python
nombre_archivo = 'foo.txt'

with open(nombre_archivo, 'rt') as file:
    # Usá el archivo `file`
    ...comandos que usan el archivo
    # No hace falta cerrarlo explícitamente
...comandos que no usan el archivo
```

El comando `with` se lo utiliza muchísimo ya que permite cerrar automáticamente el archivo cuando lo terminamos de usar y también en caso que ocurra alguna excepción. Esto previene que ocurran bugs en el código o algunas fugas de memoria.

> [!WARNING]
> A partir de ahora, cuando te pidamos abrir archivos hacelo con el comando `with`.

> [!NOTE]
> En algunos sistemas operativos es probable que le tengas que especificar el_ encoding _agregando `encoding='utf8'` como parámetro al comando `open`. Algo tipo `open('foo.txt', 'rt', encoding='utf8')`.

## Comandos usuales para leer un archivo

Para leer un archivo entero, todo de una, como cadena:

```python
with open('foo.txt', 'rt') as file:
    data = file.read()
    # `data` es una cadena con *todo* el texto en `foo.txt`
```

Para leer línea por línea iterativamente:

```python
nombre_archivo = 'foo.txt'

with open(nombre_archivo, 'rt') as file:
    for line in file:
        print(line)
        # Procesar la línea
```

## Comandos usuales para escribir un archivo

Para escribir cadenas:

```python
with open('outfile', 'wt') as out:
    out.write('Hello World\n')
    ...
```

También podés simplemente redireccionar la salida del print (de la pantalla a un archivo).

```python
with open('outfile', 'wt') as out:
    print('Hello World', file=out)
    ...
```

## Ejercicios

Estos ejercicios usan el archivo `../Data/camion.csv`.  El archivo contiene una lista de líneas con información sobre los cajones de fruta cargados en un camión. Suponemos que estás trabajando en el directorio `ejercicios_python/Clase02` del curso. Si no estás segure, podés pedirle a Python que te diga dónde está trabajando con este comando:

```python
>>> import os
>>> os.getcwd()
'D:\\ejercicios_python\\Clase02' # La salida va a cambiar
>>>
```

### Ejercicio 2.1: Preliminares sobre lectura de archivos
Primero, tratá de leer el archivo entero de una en una larga cadena:

```python
>>> with open('../Data/camion.csv', 'rt') as f:
        data = f.read()

>>> data
'nombre,cajones,precio\n"Lima",100,32.20\n"Naranja",50,91.10\n"Caqui",150,83.44\n"Mandarina",200,51.23\n"Durazno",95,40.37\n"Mandarina",50,65.10\n"Naranja",100,70.44\n'
>>> print(data)
nombre,cajones,precio
"Lima",100,32.20
"Naranja",50,91.10
"Caqui",150,83.44
"Mandarina",200,51.23
"Durazno",95,40.37
"Mandarina",50,65.10
"Naranja",100,70.44
>>>
```

En el ejemplo de arriba podrás observar que Python tiene dos modos de salida. En el primero escribiste `data` en el intérprete y Python mostró la representación *cruda* de la cadena, incluyendo comillas y códigos de escape (`\n`). Cuando escribiste `print(data)`, en cambio, obtuviste la salida formateada de la cadena.

Leer un archivo entero y cargarlo en memoria todo de una vez parece simple, pero sólo tiene ventajas si el archivo es pequeño. Si estás trabajando con archivos enormes es mejor procesar las líneas de tu archivo una a una.

Para leer un archivo línea por línea, usá un ciclo ``for`` como este:

```python
>>> with open('../Data/camion.csv', 'rt') as f:
        for line in f:
            print(line, end = '')

nombre,cajones,precio
"Lima",100,32.2
"Naranja",50,91.1
...
>>>
```

En ese código, las líneas son leídas una por una hasta el final del archivo, cuando el ciclo se termina.

En ciertas ocasiones, puede pasar que quieras leer una sola línea de texto (por ejemplo, querés saltearte la primera línea del archivo que contiene los nombres de las columnas).

```python
>>> f = open('../Data/camion.csv', 'rt')
>>> headers = next(f)
>>> headers
'nombre,cajones,precio\n'
>>> for line in f:
        print(line, end = '')

"Lima",100,32.20
"Naranja",50,91.10
...
>>> f.close()
>>>
```

El comando `next()` devuelve la siguiente línea de texto en el archivo. Sin embargo, sólo para que sepas, los ciclos `for` usan el método `next()` para obtener sus datos. Por lo tanto, típicamente no forzás un llamado extra a `next()` salvo que explícitamente quieras saltear o leer una línea particular como en nuestro caso de acá abajo.

Una vez que estés leyendo un archivo línea a línea, podés hacer otras operaciones, como separar los datos dentro de una línea con el método `split()`. Por ejemplo, probá esto:

```python
>>> f = open('../Data/camion.csv', 'rt')
>>> headers = next(f).split(',')
>>> headers
['nombre', 'cajones', 'precio\n']
>>> for line in f:
        row = line.split(',')
        print(row)

['Lima', '100', '32.20\n']
['Naranja', '50', '91.10\n']
...
>>> f.close()
```

> [!NOTE]
> 1. En estos ejemplos tuvimos que llamar  a `f.close()` explícitamente porque no estamos trabajando con el comando `with`. Probá hacerlo en el intérprete de esta forma, pero recordá trabajar con el comando `with`.
> 2. Usamos `../Data` para acceder a la carpeta "Data" porque ésta se encuentra dentro de la carpeta "ejercicios_python", al igual que la carpeta actual de trabajo, que es "Clase02". Con los dos puntos del inicio del path nos referimos a la carpeta "madre", es decir, a la carpeta que contiene a la actual.

### Ejercicio 2.2: Lectura de un archivo de datos
Ahora que sabés leer un archivo, escribamos un programa que haga un cálculo simple con los datos leídos.

Las columnas en `camion.csv` corresponden a un nombre de fruta, una cantidad de cajones cargados en el camión, y un precio de compra por cada cajón de ese grupo. Escribí un programa llamado `costo_camion.py`  que abra el archivo, lea las líneas, y calcule el precio pagado por los cajones cargados en el camión.

> [!IMPORTANT]
> Para interpretar un string `s` como un número entero, usá `int(s)`. Para leerlo como punto flotante, usá `float(s)`.

Tu programa debería imprimir una salida como la siguiente:

```bash
Costo total 47671.15
```

Acordate de guardar tu archivo en el directorio `Clase02`; vamos a volver a trabajar sobre él.

### Ejercicio 2.3: Precio de la naranja
El archivo `../Data/precios.csv` contiene una serie de líneas con precios de venta de cajones en el mercado al que va el camión. El archivo se ve así:

```py
"Lima",40.22
"Uva",24.85
"Ciruela",44.85
"Cereza",11.27
"Frutilla",53.72
...
```

Escribí un código que abra el archivo `../Data/precios.csv`, busque el precio de la naranja y lo imprima en pantalla.

```python
>>> with open('../Data/precios.csv', 'rt') as f:
...
>>>

El precio de la naranja es:  106.28
```

### Ejercicio 2.4: Archivos comprimidos
¿Qué pasaría si quisiéramos leer un archivo comprimido con gzip, por ejemplo? La función primitiva de Python  `open()` no hace esa tarea. Pero hay un módulo de la biblioteca de Python llamado `gzip` que lee archivos comprimidos.

Probalo:

```python
>>> import gzip
>>> with gzip.open('../Data/camion.csv.gz', 'rt') as f:
        for line in f:
            print(line, end = '')

... mirá la salida ...
>>>
```

> [!WARNING]
> La inclusión del modo  `'rt'` es crítica acá. Si te lo olvidás, vas a estar leyendo cadenas de bytes en lugar de cadenas de caracteres.

### Comentario: ¿No deberíamos estar usando Pandas para esto?

Es frecuente que les estudiantes que conocen un poco más de Python rápidamente señalen que hay módulos como [Pandas](https://pandas.pydata.org) que tienen, entre muchas otras funcionalidades, la posibilidad de leer archivos CSV en una sola instrucción. Es verdad, y funcionan muy bien. Sin embargo, este no es un curso sobre Pandas. Si bien más adelante veremos algo de esta biblioteca, lo que nos interesa en este momento es aprender a manejar archivos directamente. Estamos trabajando con archivos CSV porque es un formato sencillo que es muy útil conocer, pero es principalmente una excusa para mostrar cómo Python maneja archivos de texto. 

En resumen, cuando tengas que trabajar con datos, definitivamente usá Pandas. Pero para aprender a manejar archivos vamos a seguir usando las funciones básicas de Python.


[Contenidos](../Contenidos.md) \| [Anterior (2 Entorno de desarrollo integrado)](02_IDE.md) \| [Próximo (4 Funciones)](04_Funciones.md)

