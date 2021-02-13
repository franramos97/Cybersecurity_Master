# Timing 

In the context of data science, sometimes the use of some particular form of code, or some data structure matters. This is important when dealing with large data.

## Example


```python
import random
L = [random.random() for i in range(100000)] #genero una lista de 100mil nºs aleat
%time L.sort() #sort ordena L de modo ascendente, "%time" te dice el tiempo en el q hace esto
```

Looking at an operation with the time magic, we can get different times.


```python
L = [random.random() for i in range(100000)]
%timeit L.sort() #timeit ejecuta repetidamente hasta q el tº de ejecucion converga, es una medida más fiable q con "%time"
```

Re-implementing functionality may lead to different performance.


```python
%timeit sum(range(1000)) #esto y la de abajo es lo mismo, pero aqui es mas eficiente
```


```python
%%timeit
total = 0
for i in range(1000):
   total += i
```

## Lists versus NumPy arrays


```python
import numpy as np
L = range(1,1000000) #esto es una lista
a = np.array(L) #esto es un array de numpy
```


```python
%timeit a*a #la operacion de mult los elementos de un 'array' de numpy tarda 2.04 ms de media
```

    2.04 ms ± 126 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
    


```python
%timeit [e*e for e in L] #la operacion de mult los elementos de una 'lista' tarda 114 ms. MUCHO MÁS EFICIENTE NumPy
```

    114 ms ± 6.15 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
    


```python

```
