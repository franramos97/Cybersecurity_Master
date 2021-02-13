# Introducción a NumPy (indexación)

## Miguel-Angel Sicilia, msicilia@uah.es

## Objetivos

* Comprender los subarrays que provienen de la indexación como copias.
* Entender el concepto y uso del boolean indexing y el fancy indexing. 
* Utilizar indexaciones para manipular arrays.

## Indexación y copia

* Los arrays (de una o varias dimensiones) pueden indexarse y trocearse (*slice*) igual que las listas de Python.
* Pero la diferencia es que los *slices* son **vistas**, no copias.
    * La idea es evitar las copias en arrays de gran tamaño.


```python
import numpy as np
a = np.random.randint(10, 20,(4,5))
a
```




    array([[16, 12, 13, 17, 16],
           [17, 19, 17, 16, 13],
           [12, 19, 12, 13, 15],
           [18, 11, 11, 13, 17]])




```python
b = a[1, 1:]
b[2] = 0
a, b #imprimo el array a y el b
```




    (array([[16, 12, 13, 17, 16],
            [17, 19, 17,  0, 13],
            [12, 19, 12, 13, 15],
            [18, 11, 11, 13, 17]]),
     array([19, 17,  0, 13]))



### La copia es explícita

* En general, tenemos que indicar como programadores cuándo queremos una copia de un array.


```python
c = a.copy()
c[2,2]=-99
print(c)
a #vemos q 'a' no cambia
```

    [[ 16  12  13  17  16]
     [ 17  19  17   0  13]
     [ 12  19 -99  13  15]
     [ 18  11  11  13  17]]
    




    array([[16, 12, 13, 17, 16],
           [17, 19, 17,  0, 13],
           [12, 19, 12, 13, 15],
           [18, 11, 11, 13, 17]])



## Un ejemplo de indexación

* Tomamos una muestra de la función  $y=x^2$ (la derivada sera $y=2x$)
* Calcular una aproximación de la derivada de manera gráfica requiere calcular los valores $s_i=\frac{\Delta y}{\Delta x}$.
* Luego tenemos que calcular las $s_i = y_i - y_{i-1} \forall i >1$


```python
%matplotlib inline
import matplotlib.pyplot as plt
x = np.linspace(0,50, 20) # Aumentar el último número nos acerca al límite.
y = x**2
dy = y[1:] - y[:-1]
dx = x[1:] - x[:-1]
plt.plot(np.arange(1, 20), dy/dx, label="s")
plt.plot(2*x, label="$2x$")
_ = plt.legend()
```


    
![png](output_8_0.png)
    


### Diferencias (sutiles) entre arrays de una o varias dimensiones

<img src="https://www.oreilly.com/library/view/python-for-data/9781449323592/httpatomoreillycomsourceoreillyimages2172114.png" width="40%">

## Boolean indexing

* Comparar arrays produce arrays de valores booleanos (*broadcasting*)
* Esto se puede utilizar para indexar con esos arrays, que actúan filtrando datos.


```python
a = np.random.randint(10, 20,(4,5)) 
a > 13 #compara en cada posicion si es > 13
```




    array([[False,  True, False,  True, False],
           [False,  True, False,  True,  True],
           [ True, False, False, False,  True],
           [ True,  True,  True,  True, False]])




```python
a[a > 13] #donde pone TRUE me quedo con esa casilla, donde pone FALSE lo quita. ESTO NO EXISTE EN PYTHON
```




    array([19, 18, 17, 17, 14, 17, 14, 18, 17, 16, 19])



## Expresiones booleanas complejas

* Cuando hacemos boolean indexing, podemos necesitar utilizar expresiones con operadores lógicos. 
* NumPy nos proporciona la versión vectorizada de los mismos. 
* Por ejemplo:


```python
import numpy as np
a = np.random.randint(10, 20, 30)
a[(a>15) & (a % 2) ==0]  #misma op q lo de abajo
a[np.logical_and(a>15, np.mod(a,2)==0)] #mismo q lo de arriba
```




    array([18, 18, 18, 18, 18, 16])



## Fancy indexing

* Otra posibilidad interesante es indexar valores no contiguos.
* Una utilidad directa es poder seleccionar ciertos índices donde hay datos particulares, y extraerlos como un subarray.


```python
sample = np.random.randint(1, 20, 300)
print(sample)
sample[[7, 14, 123]]
```

    [17  9 19  4 12  8 11 16 15  8  3  4 19 19 10  4 19 17 19  2 17  5 15 17
      7 14 14  3 10 19 13 19 19  2  7  5  7  7  8 14 10 16 13 14 15 13 14  1
      8 12 16 16 11 11 17  6  5  4  2 13 12  9  4  9  8 14  6  3 16  2 18 11
     10 13 19  8 15 12  3 15  2 14 10 12  9  1 14 12 16 18  5  9 14 12 19 18
      3 12  7 13  6  4 14  5 12 18  3 15 14 14  5  2  2  3  4  7  3  3  1 15
      4 18 11 14  3  8 12  7 11 18 17  2 12 18 10 18 10 14  2  5  5  1 16 19
     14  9  3  5  4  2 12 18 18  9  7  8  3  6  5  6 16  8 18  3 14 15 16  7
     19  1  1  3  9 11 18  7 19 13  6 16  7 14  8  5  9  2  8  2 19  6 11  2
     12  3  5  8  8 18 17  9 11 13 11  8 11 10  7 12  8  4  5 15 13 12 11  8
     12  6 17  2  8 11 13  1  7  2  5  6 16 12 15 11 17  5 19 14 12 10 12  4
      4 12  3  6 13  2  6 18  3  5  6 13  1  5 18  6 13 13  2 12 15 13  5 10
      9  1 13  2  7  6 18  2 12  4 14 11 16 13 17  7 12  8 17 13  5 17 19  7
     18  1  8 19 19 17 18  6 14 10  8 15]
    




    array([16, 10, 14])




```python
indices = np.where(sample >17) #los indices donde estén los valores > 17
indices
```




    (array([  2,  12,  13,  16,  18,  29,  31,  32,  70,  74,  89,  94,  95,
            105, 121, 129, 133, 135, 143, 151, 152, 162, 168, 174, 176, 188,
            197, 234, 247, 254, 270, 286, 288, 291, 292, 294], dtype=int64),)




```python
plt.scatter(range(sample.size), sample, alpha=0.75) #muestra todos los valores creados originalmente (puntos azules)
_ = plt.scatter(indices, sample[indices], marker="*") #esta muestra solamente "indices", me los pinta en naranja
```


    
![png](output_18_0.png)
    


## Más ejemplos

* `3_3_NumPy-1D-ejemplos.ipynb`
* `3_4_Numpy-2D-ejemplos.ipynb`

## Ejercicios
* `numpy_indexacion.pdf`


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
