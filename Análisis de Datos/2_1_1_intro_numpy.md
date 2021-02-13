# Introducción a NumPy
## Miguel-Angel Sicilia, msicilia@uah.es

## Objetivos:

* Entender las diferencia entre los arrays de NumPy y las listas en Python.
* Ser capaz de crear arrays de NumPy y muestras aleatorias de distribuciones conocidas.
* Ser capaz de buscar funciones de manipulación de arrays y matrices.


* Arrays (y matrices) homogéneos **eficientes** en Python.
* Estructura básica para todo el stack científico de Python.
* “Batteries included” para data science:
http://scipy.github.io/old-wiki/pages/Numpy_Example_List_With_Doc.html 
  * ejemplo: buscar `allclose` ¿para qué sirve? # determina si en dos arrays de nºs todos son cercanos o iguales
  #busco esta clase dentro del enlace anterior y veo como funciona
  * ejemplo: buscar ordenación de arrays.
* La lista por categorías puede encontrarse aquí:
https://docs.scipy.org/doc/numpy/genindex.html#A


<img src="numpystack.png" width="50%">

* `NDArray` son los objetos NumPy.
* *UFuncs* son operaciones vectorizadas sobre arrays
* La idea básica es reemplazar operaciones costosas sobre secuencias en Python como:


```python
a = [1, 2, 3, 4]
b = [5, 6, 7, 8]
c = []
for i in range(len(a)): 
    c.append(a[i] *b[i])
```

Por código C optimizado e independiente del número de dimensiones, de manera que se *vectorizan* las operaciones y se usan con la sintaxis:



```python
import numpy as np
a = np.array([1,2,1]) #son datos distintos
b = np.array([2,2,2]) #son datos distintos
c = a * b #este codigo es el mismo q el del recuadro anterior y mas efectiva
c
```




    array([2, 4, 2])



# Ejes, forma y tipo
![](http://pages.physics.cornell.edu/~myers/teaching/ComputationalMethods/python/anatomyarray.png)

#shape son las dimensiones: 8x3


## Construcción a partir de tipos Python

* Pueden construirse arrays a partir de tipos Python.
* Pero es importante resaltar que lo que se hace es una copia a un *ndtype*, dado que los tipos básicos de Python tienen información adicional que les hace menos eficientes. 


```python
import numpy as np 
a = np.array([0, 1, 2, 3]) 
a
```




    array([0, 1, 2, 3])




```python
a = np.arange(2, 5) #range(2,5) genera una lista entre 2 y 4 
print(a)
b = np.array([2.0, 0.0, 2.0]) 
print(b)
c = a * b # c es un nuevo array
c
```

    [2 3 4]
    [2. 0. 2.]
    




    array([4., 0., 8.])




```python
b *= a  # a *=b no es válido, porque requiere cambiar el tipo. no se pueden meter floats en un entero pero si al revés
b
```




    array([4., 0., 8.])



Pero podemos forzar el tipo al construir.


```python
d = np.arange(1, 10, dtype=np.float)  # forzarlo a q sea de tipo float
print(d)
d.dtype
```

    [1. 2. 3. 4. 5. 6. 7. 8. 9.]
    




    dtype('float64')



# Un array de NumPy no es una lista de Python

<img src="http://jakevdp.github.io/images/array_vs_list.png">

## Otros constructores

* Rellenar con unos o ceros: `np.ones`, `np.zeros`.
* Reservar memoria sin inicializar: `np.empty`
* Datos igualmente espaciados.


```python
np.linspace(1, np.pi, 10)
```




    array([1.        , 1.23795474, 1.47590948, 1.71386422, 1.95181896,
           2.1897737 , 2.42772844, 2.66568318, 2.90363791, 3.14159265])



## Métodos versus funciones

* Nos encontramos a veces un método en el array que también tiene una variante funcional.
* Por ejemplo.



```python
x = np.array(np.arange(0,10000))
print(x)
x.reshape(100,100) #cogiendo el array de mil y lo convierto en una matriz de 100x100, esto no cuesta nada en memoria
%timeit x.mean()
%timeit np.mean(x) #son dos clases distintas pero q hacen lo mismo
```

    [   0    1    2 ... 9997 9998 9999]
    15.2 µs ± 1.03 µs per loop (mean ± std. dev. of 7 runs, 100000 loops each)
    16 µs ± 400 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
    

## Interludio: uso de `matplotlib`

* Podemos dibujar arrays de forma simple con `plot`.
* El eje de abcisas se toma de los índices del array (o se puede pasar como otro array).


```python
%matplotlib inline 
import matplotlib.pyplot as plt #si estoy en un notebook, la salida del gráfico se pega en el html (lo de antes) #pyplot es el lienzo donde voy a dibujar
plt.plot(np.sin(np.linspace(0, 4*np.pi, 100))) #plt.bar para barras, plt.hist para histograma...
plt.plot(np.cos(np.linspace(0, 4*np.pi, 100)), "--")
```




    [<matplotlib.lines.Line2D at 0x2261bf91bb0>]




    
![png](output_22_1.png)
    


## Muestras aleatorias

* En muchos estudios, es necesario crear muestras aleatorias de distribuciones conocidas.
* El paquete `numpy.random` proporciona un gran número de distribuciones.
  * https://docs.scipy.org/doc/numpy-1.16.0/reference/routines.random.html

* La distribución de Poisson:

$f(k,\lambda)=\frac{e^{-\lambda} \lambda^k}{k!}$

* k es el número de ocurrencias del evento (la función nos da la probabilidad de que el evento suceda precisamente k veces).
* $\lambda$ es un parámetro positivo que representa el número de veces que se espera que ocurra el fenómeno durante un intervalo dado. 
    * Ejemplo: si el suceso estudiado (fallos en una máquina) tiene lugar en promedio 2 veces por minuto y estamos interesados en la probabilidad de que ocurra k veces dentro de un intervalo de 3 minutos, usaremos un modelo de distribución de Poisson con λ = 2×3 = 6.


```python
from numpy.random import poisson
muestra = poisson(lam=6, size=30000)
_ = plt.hist(muestra, bins=200) # La asignación elimina la salida (bins)
```


    
![png](output_25_0.png)
    


## Broadcasting, casos especiales
#de esto no vemos nada
* El comportamiento "elemento a elemento" de las operaciones (que se extiende a más de una dimensión) se denomina *broadcasting*. 
* Funciona sobre arrays con las mismas dimensiones y también en casos con dimensiones distintas en las que se considera no hay ambigüedad.



![](http://www.scipy-lectures.org/_images/numpy_broadcasting.png)

## Redimensionamiento

* Los datos de un array se pueden reorganizar en más de una dimensión.


```python
alturas = np.random.normal(180, 20, 20)
print(alturas)
```

    [183.56397394 177.36942808 161.36996746 162.44425594 206.14631107
     209.33125552 171.26191268 179.93995762 158.25719882 187.45565286
     205.01573374 170.25359382 182.22457198 154.52693206 194.28139393
     197.23036596 166.79263979 215.01323885 151.40457658 213.27644785]
    


```python
nueva = alturas.reshape((5, 4))
print(nueva)
```

    [[160.77826397 164.47157895 188.91225046 186.42169283]
     [133.72629309 186.36145805 150.48292634 215.82869059]
     [186.00850985 208.28002075 182.09027291 172.11806874]
     [206.17796055 179.63730044 231.48440156 198.77721208]
     [169.11316865 195.45149764 139.27425077 171.26310659]]
    

* Y muchas funciones permiten especificar también las dimensiones.


```python
np.ones((2,3))
```




    array([[1., 1., 1.],
           [1., 1., 1.]])



* También es usual convertir un array a una matriz de una columna, se puede hacer con <code>reshape()</code> pero hay otra forma.


```python
arr = np.empty(5, dtype=np.int32)
m = arr[:, np.newaxis] #cojo el array y creo una nueva dimension, 
print(arr, arr.shape, arr.ndim)
print(m, m.shape, m.ndim)
```

    [16843009 16843009 16843009 16843009 16843009] (5,) 1
    [[16843009]
     [16843009]
     [16843009]
     [16843009]
     [16843009]] (5, 1) 2
    

## Uso de los ejes

* Las funciones hacen *broadcasting* a todos los valores por defecto.


```python
x = np.random.rand(15)
print(x)
x = x.reshape((3,5))
x
```

    [0.99986487 0.53751676 0.41922609 0.62870455 0.35912463 0.27301789
     0.33923611 0.65776503 0.34956107 0.27104609 0.4937175  0.76905385
     0.8633581  0.74466889 0.87752651]
    




    array([[0.99986487, 0.53751676, 0.41922609, 0.62870455, 0.35912463],
           [0.27301789, 0.33923611, 0.65776503, 0.34956107, 0.27104609],
           [0.4937175 , 0.76905385, 0.8633581 , 0.74466889, 0.87752651]])




```python
x.mean()
```




    0.5722258627255054



* Si queremos limitar ese comportamiento, tenemos el parámetro `axis` en muchas de ellas.


```python
x.mean(axis=0) #antes le hemos dado 3x5 osea q tiene 5 columnas
```




    array([0.58886675, 0.54860224, 0.64678307, 0.5743115 , 0.50256574])



* El slicing se puede utilizar igual que en Python "normal".
<img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.rwAx4Hc44Bod1EE8CPCOPwHaEO%26pid%3DApi&f=1">

## Vectorización de funciones

La vectorización permite código más conciso, pero también es una oportunidad para aprovechar arquitecturas de computadora paralelas (multicore o CPU) u otras técnicas.


```python
def mifuncion(x):
    if x>0.5:
        return x
    else:
        return 0.0
    
mayorque0_5 = np.vectorize(mifuncion)
mayorque0_5(x)
```




    array([[0.99986487, 0.53751676, 0.        , 0.62870455, 0.        ],
           [0.        , 0.        , 0.65776503, 0.        , 0.        ],
           [0.        , 0.76905385, 0.8633581 , 0.74466889, 0.87752651]])




```python
%timeit mayorque0_5(x) 
```

    19.9 µs ± 2.77 µs per loop (mean ± std. dev. of 7 runs, 100000 loops each)
    

No obstante, la simple vectorización no necesariamente hace el código más eficiente.


```python
x = np.random.rand(15)
```


```python
%%timeit
y = np.empty(x.size)
for i in np.arange(x.size):
    y[i] = mifuncion(x[i])

```

    14.5 µs ± 860 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
    

### Opciones para una mejor eficiencia
* **Cython** permite añadir comprobación de tipos a Python, o invocar funciones escritas en C.
* **Numexpr** toma expresiones de NumPy y las intenta optimizar internamente, básicamente para intentar no copiar resultados intermedios.
* **Numba** permite compilar código Python "just-in-time" en su caso aprovechando características del hardware, incluyendo las bibliotecas CUDA para GPUs.

Un ejemplo de Cython:
http://notes-on-cython.readthedocs.io/en/latest/std_dev.html

<img src="http://notes-on-cython.readthedocs.io/en/latest/_images/Results.png">


```python
cdef extern from "math.h":
    double sqrt(double m)

from numpy cimport ndarray
cimport numpy as np
cimport cython

@cython.boundscheck(False)
def cyOptStdDev(ndarray[np.float64_t, ndim=1] a not None):
    cdef Py_ssize_t i
    cdef Py_ssize_t n = a.shape[0]
    cdef double m = 0.0
    for i in range(n):
        m += a[i]
    m /= n
    cdef double v = 0.0
    for i in range(n):
        v += (a[i] - m)**2
    return sqrt(v / n)

```


      File "<ipython-input-12-7f95cda53ee7>", line 1
        cdef extern from "math.h":
             ^
    SyntaxError: invalid syntax
    



```python
import numexpr as ne
def sin_numexpr(x, y):
 return ne.evaluate("y / sqrt(x**2 + y**2)")
```


```python
from numba import vectorize
@vectorize([int32(int32, int32),
            int64(int64, int64),
            float32(float32, float32),
            float64(float64, float64)], target="cuda")
def f(x, y):
    return x + y

```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-14-fb7622e9f75f> in <module>
          1 from numba import vectorize
    ----> 2 @vectorize([int32(int32, int32),
          3             int64(int64, int64),
          4             float32(float32, float32),
          5             float64(float64, float64)], target="cuda")
    

    NameError: name 'int32' is not defined


## Un poco de práctica

* `1_basics_numpy`

## Ejercicios
* `numpy_basico.pdf`

