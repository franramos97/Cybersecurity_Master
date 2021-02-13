# Using NumPy arrays with two-dimensional data

## Creating multi-dimensional data

You can create bi-dimensional arrays passing a **list of lists** (works also with some other sequence types).


```python
import numpy as np
m = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]) #esto ya es un array
print(m)
```

    [[ 1  2  3  4]
     [ 5  6  7  8]
     [ 9 10 11 12]]
    

You can get the dimensions of the array with <code>shape</code>, and the number of dimensions with <code>ndim</code>.


```python
print (m.shape) # = (3,4) q son 3 arrays de 4 columnas (3 filas y 4 columnas)
print (m.ndim) #la lista es de 2 dimensiones
```

    (3, 4)
    2
    

In the case of using creation functions, you have to pass a tuple with the **shape** you want the array to have.


```python
ones = np.ones((3, 7))
print(ones)
prueba = np.ones(4)
print(prueba)
```

    [[1. 1. 1. 1. 1. 1. 1.]
     [1. 1. 1. 1. 1. 1. 1.]
     [1. 1. 1. 1. 1. 1. 1.]]
    [1. 1. 1. 1.]
    

And you can use the same functions that with one-dimensional arrays.


```python
ones.fill(2) #llena el array con una funcion escalar
print(ones)
print(ones.sum())
```

    [[2. 2. 2. 2. 2. 2. 2.]
     [2. 2. 2. 2. 2. 2. 2.]
     [2. 2. 2. 2. 2. 2. 2.]]
    42.0
    

## Indexing and slicing

If you use a single index, you are getting a row. If you index two times, you are indexing a concrete element.


```python
print(m[2])
print(m[1][3])
print(m[1,3]) #esta forma y la anterior es lo mismo
```

    [ 9 10 11 12]
    8
    8
    

You can slice the array in one or several of the axis.


```python
# We create a view of the data for all the rows (:) and the columns in positions 5 and 6.
ones[:,5:7].fill(8) #lleno todas las filas (:) y las columnas de la 5 a la 7(sin incluir la 7) de 8s
ones
```




    array([[2., 2., 2., 2., 2., 8., 8.],
           [2., 2., 2., 2., 2., 8., 8.],
           [2., 2., 2., 2., 2., 8., 8.]])




```python
# Slice array m to get only the third column and multiply the values of that column by 10.
m[:, 2] = m[:,2]*10
print(m)

# slice the middle row of m and set it to zeros
m[1,:] = 0
m
```

    [[  1   2  30   4]
     [  5   6  70   8]
     [  9  10 110  12]]
    




    array([[  1,   2,  30,   4],
           [  0,   0,   0,   0],
           [  9,  10, 110,  12]])



You can also index matrices with booleans.


```python
#Generate a boolean matrix with the numbers above 10 in m
sel = m >10 
print(sel)

# Use the boolean matrix to get an array with the values that are below or equal than 10 in the matrix.
a = m[sel == False]
a
```

    [[False False  True False]
     [False False False False]
     [False False  True  True]]
    




    array([ 1,  2,  4,  0,  0,  0,  0,  9, 10])



## Operations in different axis

Statistical and other operations can be applied by rows or columns by indicating the **axis**.


```python
mat = np.random.randint(1, 20, 18)
mat
```




    array([12, 13, 19,  5,  4,  1,  9, 10,  1,  1,  6, 18, 13, 13, 11,  9,  6,
            4])




```python
mat = mat.reshape((6,3))
mat
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-1-eeb291fe21c2> in <module>
    ----> 1 mat = mat.reshape((6,3))
          2 mat
    

    NameError: name 'mat' is not defined



```python
mat.sum()
```




    155




```python
mat.sum(axis=1) #aqui suma por filas
```




    array([44, 10, 20, 25, 37, 19])




```python
mat.sum(axis=0) #aqui suma lo de cada columna (3 columnas)
```




    array([49, 52, 54])



## Transposing

NumPy arrays can be reshaped easily.


```python
m1 = np.linspace(20,30,15)
print(m1.shape)
print(m1)
m1 = m1.reshape((5, 3)) # You have to pass a tuple
print(m1)
print(m1.shape)
```

    (15,)
    [20.         20.71428571 21.42857143 22.14285714 22.85714286 23.57142857
     24.28571429 25.         25.71428571 26.42857143 27.14285714 27.85714286
     28.57142857 29.28571429 30.        ]
    [[20.         20.71428571 21.42857143]
     [22.14285714 22.85714286 23.57142857]
     [24.28571429 25.         25.71428571]
     [26.42857143 27.14285714 27.85714286]
     [28.57142857 29.28571429 30.        ]]
    (5, 3)
    

You can also transpose a matrix.


```python
m1.T
```




    array([[20.        , 22.14285714, 24.28571429, 26.42857143, 28.57142857],
           [20.71428571, 22.85714286, 25.        , 27.14285714, 29.28571429],
           [21.42857143, 23.57142857, 25.71428571, 27.85714286, 30.        ]])



## Plotting

Plotting by default takes each column of a matrix as an independent series of data.


```python
import matplotlib.pyplot as plt
%matplotlib inline
_= plt.boxplot(m1)
```


    
![png](output_32_0.png)
    



```python

```
