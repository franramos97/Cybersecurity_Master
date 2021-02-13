## Fundamentos de NumPy

### Arrays como colecciones homogéneas

Los arrays se pueden construir a partir de otros tipos de datos.


```python
import numpy as np
a = np.array([2, 4, 8, 10, 12, 14, 16], dtype=np.int16)
a
```




    array([ 2,  4,  8, 10, 12, 14, 16], dtype=int16)



Y siempre tienen un tipo homogéneo.


```python
a.dtype
```




    dtype('int16')



Que se puede especificar como parámetro al construir el array. Si no se hace, NumPy buscará el más adecuado.


```python
b = np.array([2, 4, 8, 10, 12, 14, 16])
c = np.array([2.0, 4, 8, 10, 12, 14, 16])

b.dtype, c.dtype
```




    (dtype('int32'), dtype('float64'))



### Los arrays pueden tener diferentes dimensiones


```python
cubo = np.array(range(1, 25))
print(cubo) 
cubo.shape = (3, 4, 2) #es como '3' arrays de 4 filas y 2 columnas
print(cubo.size, cubo.ndim) #cubo.size = 3x4x2 = 24 y cubo.ndim=3 pq tiene como 3 dimensiones (son 3 arrays, o un array tridimensional creo q seria mejor dicho)
cubo
```

    [ 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24]
    24 3
    




    array([[[ 1,  2],
            [ 3,  4],
            [ 5,  6],
            [ 7,  8]],
    
           [[ 9, 10],
            [11, 12],
            [13, 14],
            [15, 16]],
    
           [[17, 18],
            [19, 20],
            [21, 22],
            [23, 24]]])



### Inicialización de arrays simples


```python
zeros = np.zeros(5)
ones = np.ones((4, 3)) #4 filas y 3 columnas rellenas de 1s
print(zeros, zeros.dtype)
print(ones)
print(ones.shape) #el shape es de 4x3
print(ones.size)#4x3=12
```

    [0. 0. 0. 0. 0.] float64
    [[1. 1. 1.]
     [1. 1. 1.]
     [1. 1. 1.]
     [1. 1. 1.]]
    (4, 3)
    12
    

### Inicialización de arrays con valores ordenados


```python
rango = np.arange(20, 30, 0.5)
rango
```




    array([20. , 20.5, 21. , 21.5, 22. , 22.5, 23. , 23.5, 24. , 24.5, 25. ,
           25.5, 26. , 26.5, 27. , 27.5, 28. , 28.5, 29. , 29.5])




```python
espacio = np.linspace(10, 12, 20) #muestras uniformes entre 10 y 12
espacio
```




    array([10.        , 10.10526316, 10.21052632, 10.31578947, 10.42105263,
           10.52631579, 10.63157895, 10.73684211, 10.84210526, 10.94736842,
           11.05263158, 11.15789474, 11.26315789, 11.36842105, 11.47368421,
           11.57894737, 11.68421053, 11.78947368, 11.89473684, 12.        ])



### Inicialización de arrays con muestras aleatorias


```python
aleatorios = np.random.random(20) #crea 20
print(aleatorios)
aleatorios[7:10] #devuelve del 7 al 9 (10 no incluido)
```

    [0.79333571 0.84192325 0.84335194 0.1767349  0.31080065 0.49999915
     0.60604107 0.88461699 0.79713054 0.78329091 0.10732743 0.46415533
     0.06136177 0.51092225 0.81427066 0.69245673 0.13011037 0.90621457
     0.99706095 0.18114068]
    




    array([0.88461699, 0.79713054, 0.78329091])



Si queremos aleatorios enteros podemos utilizar <code>randint()</code>

Lo anterior son aleatorios de una distribución uniforme (continua o discreta). Podemos también utilizar aleatorios generados con otras distribuciones como la distribución normal.


```python
otros = np.random.randn(1000)
%matplotlib inline
import matplotlib.pyplot as plt
_ = plt.hist(otros, bins=50, alpha=0.75)
```


    
![png](output_19_0.png)
    


Pueden consultarse muchas otras formas de generar muestras aleatorias aquí:
    http://docs.scipy.org/doc/numpy/reference/routines.random.html

### Uso de operaciones vectorizadas

Una vez tenemos el array, podemos utilizar una gran cantidad de funciones que se aplican a sus elementos.


```python
vector = np.random.randint(1, 20, 10) #devuelve enteros random del 1 al 20 y 10 en total (10 randoms)
print(vector)
print(vector.sum())
sum(vector) # equivalente.
```

    [ 2  9 14  3  9 12  6  3  4 11]
    73
    




    73



En el caso de más de una dimensión, podemos especificar el eje.


```python
vector = vector.reshape((2, 5)) #al vector de antes le hacemos un 'reshape' y lo hacemos de 2 filas y 5 columnas
vector
```




    array([[ 2,  9, 14,  3,  9],
           [12,  6,  3,  4, 11]])




```python
print(vector.sum(axis=0)) # Suma las filas de una misma columna.
print(vector.sum(axis=1)) # suma las columnas de una misma fila
```

    [14 15 17  7 20]
    [37 36]
    

### Concatenación de arrays


Nunpy tiene funciones para concatenar arrays, 


```python
a = np.random.randint(10, 20, 6)
b = np.random.randint(10, 20, 6)
c = np.random.randint(10, 20, 6)

a, b, np.concatenate([a, c, b]) #muestra a, b y luego la concatenacion de abc
```




    (array([14, 13, 10, 18, 17, 18]),
     array([13, 11, 10, 13, 10, 18]),
     array([14, 13, 10, 18, 17, 18, 18, 15, 14, 14, 17, 16, 13, 11, 10, 13, 10,
            18]))




```python
a = a.reshape(2, 3)
b = b.reshape(2,3)
```


```python
horizontal = np.hstack([a,b])
print(horizontal)
```

    [[14 13 10 13 11 10]
     [18 17 18 13 10 18]]
    


```python
vertical = np.vstack([a,b])
vertical
```




    array([[14, 13, 10],
           [18, 17, 18],
           [13, 11, 10],
           [13, 10, 18]])



También podemos dividirlos.


```python
st = np.vstack([a,b])
uno, otro = np.split(st, 2)
print(uno)
print(otro)
```

    [[14 13 10]
     [18 17 18]]
    [[13 11 10]
     [13 10 18]]
    

### Ordenación de arrays

La función sort nos permite ordenar arrays o matrices, no obstante en el caso de matrices es importante comprender como funciona.


```python
a = np.random.random(400)
a.shape=(200, 2)
plt.scatter(a[:,0], a[:,1], alpha=0.7)
b = np.sort(a, axis=0)
_ = plt.scatter(b[:,0], b[:,1], alpha=0.4, marker="^")
```


    
![png](output_37_0.png)
    


¿Por qué no se superponen los puntos? Es importante entender que np.sort() considera cada fila o columna de manera independiente, por tanto, **se pierde la relación entre las diferentes "filas"**. 


```python
b[:6]
```




    array([[0.00129083, 0.01077164],
           [0.00733372, 0.01481117],
           [0.01299276, 0.02805157],
           [0.0152908 , 0.04667114],
           [0.01623488, 0.05153608],
           [0.02102419, 0.05655183]])


