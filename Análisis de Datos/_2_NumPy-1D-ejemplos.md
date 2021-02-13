# Using NumPy arrays with one-dimensional data

## What is NumPy? 

Numerical Python (NumPy) is a Python library providing a fast and space-efficient data structure: the <code>ndarray</code>, a **homogeneous** array structure, indexed with integers.
The official documentation is <a href="http://www.numpy.org/">here</a>.

## Creating arrays

You can create an empty array. Simply import numpy and use following constructors: <code>array</code>, <code>arange</code>, <code>ones</code>, <code>zeros</code> or <code>empty</code>. 


```python
import numpy as np # Everybody uses np as shorthand...
# Now we can create arrays from any sequence-like object:
L = [10, 5, 6, 8, -2] #Lista
T = (10, 5, 6, 9, -2) #Tupla
a = np.array(L)
b = np.array(T)
print(a)
print(b)
```

    [10  5  6  8 -2]
    [10  5  6  9 -2]
    


```python
# If you create it with empty, the values are arbitrary (not random, not necessarily zero).
nothing = np.empty(10) #el 10 es el shape 
nothing
```




    array([1., 1., 1., 1., 1., 1., 1., 1., 1., 1.])



You can create arrays based on the lenght of other arrays, e.g. with the <code>ones_like</code> function.


```python
#Create an array of ones with the same lenght as this one:
L = [2, 3, 4, 6, 4, 3, 2]
a = np.ones_like(L)
b = np.ones(len(L))
print(a)
print(b)
```

    [1 1 1 1 1 1 1]
    [1. 1. 1. 1. 1. 1. 1.]
    

You can also create ranges with <code>arange</code>.


```python
# Create an array with the even numbers 
# between (and including) 200 and 250.
a = np.arange(200, 251, 2)
a
```




    array([200, 202, 204, 206, 208, 210, 212, 214, 216, 218, 220, 222, 224,
           226, 228, 230, 232, 234, 236, 238, 240, 242, 244, 246, 248, 250])



The function <a href="http://docs.scipy.org/doc/numpy/reference/generated/numpy.linspace.html">linspace</a> allows for controlling the amount of numbers to generate.


```python
np.linspace(2.0, 3.0, num=5)
```




    array([2.  , 2.25, 2.5 , 2.75, 3.  ])



## Arrays have types

The <code>dtype</code> attribut of an array returns the type of its elements.


```python
ones = np.ones(5)
print(type(ones[0]))
print(ones.dtype, ones.itemsize)
```

    <class 'numpy.float64'>
    float64 8
    

NumPy arrays have a single element type for all its values. 


```python
L = [1, 4.6, 5]
a = np.array(L)
a.dtype
```




    dtype('float64')



We can **explicitly** indicate the type we want for a NumPy array.


```python
#What happened with the numbers? 
L = [1, 4.8, 5]
a = np.array(L, dtype="int32")
a.dtype, a #si lo ponemos como 'int32' el 4.8 lo redondea a 4, siempre al mas bajo
```




    (dtype('int32'), array([1, 4, 5]))



NumPy element types are not the same as the standard data types of Python. 


```python
L = ["this", "is", "an", "array555rrrr"]
a = np.array(L) #creo un array a partir de la lista de cadenas
a.dtype
#TODO: Now interpret the type... look in the documentation of NumPy data types. 
```




    dtype('<U12')



You can convert the type of an array using <code>as_type</code> operation.


```python
#Convert form float to int with astype:
a = np.array([5.7, 4.2, 3.0]) #a.dtype = float64
print(a.dtype)
b = a.astype(np.int32)
print(b)
print(b.dtype) #vemos q lo hemos cambiado a int32 el tipo
```

    float64
    [5 4 3]
    int32
    


```python
#Try to convert this array of strings to some kind of integer array:
a = np.array(["23", "12", "nan", "32", "10"])
print(a.astype(np.float))
b = a.astype(np.int32) #NI IDEA ESTO
```

    [23. 12. nan 32. 10.]
    


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-14-ba0b8c95ffa6> in <module>
          2 a = np.array(["23", "12", "nan", "32", "10"])
          3 print(a.astype(np.float))
    ----> 4 b = a.astype(np.int32) #NI IDEA ESTO
    

    ValueError: invalid literal for int() with base 10: 'nan'


From NumPy 1.8 there are "Nan"-safe versions of many functions.


```python
problematic = np.array([1,2, np.nan, 3])
```


```python
problematic.sum() #como hay un nan se ralla y devuelve nan
```




    nan




```python
np.nansum(problematic) #'nansum' trata los 'nan' como 0s
```




    6.0



## Vectorization!

In general, operations of arrays with scalars are **vectorized**, i.e. they are applied to each element.


```python
x = np.ones(10)
print(x)
x = x*2
x
```

    [1. 1. 1. 1. 1. 1. 1. 1. 1. 1.]
    




    array([2., 2., 2., 2., 2., 2., 2., 2., 2., 2.])



And if we apply operation between arrays of the same shape (size) they are applied element-wise.


```python
y = np.ones(10) #ops elemento a elemento
y + x
```




    array([3., 3., 3., 3., 3., 3., 3., 3., 3., 3.])




```python
# Arrays need to be same size:
z = np.ones(5)
print(x.shape,z.shape) #no se puede operar, distinto shape
x+z
```

    (10,) (5,)
    


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-20-0bcf2f620d26> in <module>
          2 z = np.ones(5)
          3 print(x.shape,z.shape) #no se puede operar, distinto shape
    ----> 4 x+z
    

    ValueError: operands could not be broadcast together with shapes (10,) (5,) 


Now an exercise!


```python
# create an array of 20 numbers between 5 and 10 using a gamma distribution, print them 
# and later increase them in a 20% (you have to find the numpy function for that in the docs)
shape, scale = 2., 2. # mean and dispersion
r = np.random.gamma(shape, scale, 20)
print(r)
r = r*1.2
print(r)
```

    [2.69806257 4.94511904 1.78028457 1.01464683 6.04980248 4.5481334
     4.7906703  2.76593372 2.65877295 0.69722372 3.30382674 2.17922518
     0.77525633 8.44073017 8.67221269 0.28203485 0.90517932 4.78333931
     9.32064932 5.64077762]
    [ 3.23767508  5.93414285  2.13634149  1.2175762   7.25976297  5.45776007
      5.74880436  3.31912046  3.19052754  0.83666846  3.96459209  2.61507022
      0.9303076  10.1288762  10.40665522  0.33844182  1.08621518  5.74000717
     11.18477919  6.76893315]
    

## Basic graphing

You can use matplotlib as usual. Now let's generate more random numbers.


```python
import matplotlib.pyplot as plt
%matplotlib inline
#Re-generate the previous array but with 2000 elements and plot it
r = np.random.gamma(shape, scale, 2000)
plt.plot(r)
```




    [<matplotlib.lines.Line2D at 0x1cf8a284d00>]




    
![png](output_39_1.png)
    


But for plotting the histogram, you need to do it other way...


```python
# TODO: Search in the docs of the pyplot object a way to plot a histogram. 
# You should particularly read what "bins" are as this influence the plotting of histograms. 
_ = plt.hist(r, bins=100)
```


    
![png](output_41_0.png)
    


Check if this looks like a Gamma distribution with the given parameters :-)

## Indexing and slicing

You can index and slice numpy arrays just as you do with Python lists. If you assign a value to an slice, it gets propagated to all the cells.


```python
#Create an array of ten ones, then slice the second half and set all the elements
#in the slice to zero.
o = np.ones(10)
print(o)
o[5:]=0
print(o)
```

    [1. 1. 1. 1. 1. 1. 1. 1. 1. 1.]
    [1. 1. 1. 1. 1. 0. 0. 0. 0. 0.]
    

A very important aspect of slicing is that **it creates views, not copies**


```python
#Create a slice of the four elements in the middle of the above array 
# and use a new variable to reference it.
# Then assign the value 2 to the new reference and print the original.
sl = o[3:7]
print(sl)
sl[:] = 4 #toda la lista 'sl' ahora es = 4
print(sl)
z = sl.copy() 
print(o)
print(z)
```

    [1. 1. 0. 0.]
    [4. 4. 4. 4.]
    [1. 1. 1. 4. 4. 4. 4. 0. 0. 0.]
    [4. 4. 4. 4.]
    

## Fancy indexing

There are special cases of indexing that may be useful in some situations.


```python
data = np.random.normal(180, 20, 12)
data
```




    array([154.84322534, 183.08080751, 163.95961208, 140.4115198 ,
           196.04743247, 156.87790438, 194.94476577, 187.97904764,
           173.47263253, 187.85297891, 197.20851014, 155.48590807])




```python
data[[3,5]] #muestra la posicion 3 y la 5 (la 4ª y la 6ª)
```




    array([140.4115198 , 156.87790438])




```python
index = np.array([ [2, 4], [1, 8]])
print(index)
data[index]
```

    [[2 4]
     [1 8]]
    




    array([[163.95961208, 196.04743247],
           [183.08080751, 173.47263253]])



## Boolean arrays

You can use boolean expressions to get boolean arrays.


```python
# Assign scores in a class of 15 students for two assignments.
# Create two arrays with the scores of the students using a normal distribution.
assig1 = np.random.normal(5, 2, 15)
print(assig1)
assig2 = np.random.normal(5, 2, 15)
np.ones(10)
print(o)
o[5:]=0
print(o)
#TODO Round all the scores to one decimal. 
assig1 = assig1.round(1)
assig2 = assig2.round(1)
print(assig1)
print(assig2)
```

    [4.18257693 9.32566667 5.3281898  7.17534263 1.08587145 3.53882917
     4.12701669 8.46399767 3.89446092 4.78233519 6.86659967 4.07176604
     4.77194376 6.48319071 7.63205478]
    [1. 1. 1. 4. 4. 4. 4. 0. 0. 0.]
    [1. 1. 1. 4. 4. 0. 0. 0. 0. 0.]
    [4.2 9.3 5.3 7.2 1.1 3.5 4.1 8.5 3.9 4.8 6.9 4.1 4.8 6.5 7.6]
    [3.9 0.9 4.1 3.4 6.3 5.2 5.3 4.6 4.3 9.5 5.8 5.3 6.3 7.  8. ]
    


```python
assig1 >= 5 #si es >5 algun valor de esa lista me devuelve 'True'
```




    array([False,  True,  True,  True, False, False, False,  True, False,
           False,  True, False, False,  True,  True])




```python
# Now let's think that only the students that passed the first assignment will 
# have the second considered.
# Get a boolean array for the students that passed the first assignement. 
passed1 = assig1 >= 5.0
print(passed1)

# Now get the mean of the two assignments per each of the students that passed the first one.
((assig2+assig1)/2)[passed1]
```

    [False  True  True  True False False False  True False False  True False
     False  True  True]
    




    array([5.1 , 4.7 , 5.3 , 6.55, 6.35, 6.75, 7.8 ])



We can also use boolean arrays for counting.


```python
np.sum(assig1<5)
```




    8




```python
np.sum((assig1>=5) & (assig1<7))
```




    3



## Playing with ufuncs

Universal functions (ufuncs) are vectorized operations on NumPy arrays. Let's plot some functions implemented with ufuncs. The complete list is here: http://docs.scipy.org/doc/numpy/reference/ufuncs.html 


```python
import numpy as np
#TODO: Create an array of 50 elements between 1 and 100 using linspace. Look
# at the documentation of this function.
x = np.linspace(1, 100)#x defecto son 50 elementos, x eso no lo ponemos
print(len(x))

# Get the sin of each of the generated points and plot them.
y = np.sin(x)
plt.plot(x,y)
```

    50
    




    [<matplotlib.lines.Line2D at 0x1cf8a4e3d00>]




    
![png](output_63_2.png)
    


You can plot several arrays in the same plot. 


```python
# TODO: Plot the same as above, plus the square of the sin values. 
# Look in the docs how you can pass more than one array to plot, and how to draw the second 
# with a different color, e.g. red.
plt.plot(x, y, x, y**2, 'r-^')
```




    [<matplotlib.lines.Line2D at 0x1cf8a54a1c0>,
     <matplotlib.lines.Line2D at 0x1cf8a54a2b0>]




    
![png](output_65_1.png)
    


You can also generate random points and do scaterplotting


```python
#TODO: generate two arrays of the same size using a random generator with gamma dstribution.
a = np.random.gamma(3,2, 100)
b = np.random.gamma(3,2, 100)

# Plot the two arrays as a scatterplot. #scatterplot: gráfico de dispersion
plt.scatter(a,b)
```




    <matplotlib.collections.PathCollection at 0x1cf8a5ac1c0>




    
![png](output_67_1.png)
    


Summarization operations are implemented also.


```python
b.mean()
```




    6.40519979646772




```python
np.sqrt(b)
```




    array([1.0645848 , 1.87922906, 3.30665371, 3.11754846, 3.61883276,
           1.83454518, 1.23902679, 1.77899632, 1.81976102, 1.88713656,
           2.16491436, 1.82002082, 2.92961051, 2.02641022, 1.97320244,
           2.60335672, 1.73158265, 1.77706527, 3.48669385, 2.72398243,
           1.97958969, 3.80547476, 3.85126039, 3.59533207, 2.76484935,
           1.67892445, 3.11117165, 1.93827702, 1.89837015, 2.76556945,
           3.05801406, 2.30398283, 2.26404694, 1.81741263, 3.44814223,
           2.35357753, 3.26003047, 3.38800701, 3.68499352, 2.82295951,
           2.54411438, 4.17780664, 1.7392793 , 2.00667611, 2.25717529,
           2.55430399, 2.92548745, 2.03337783, 1.38624701, 1.71298584,
           2.99473502, 3.67508969, 1.78840946, 1.23654373, 1.28012989,
           1.57501673, 1.19734837, 2.64935212, 3.12308738, 3.72153165,
           3.38868042, 2.25226321, 2.61294387, 1.4859737 , 2.01079411,
           2.57651866, 1.8994961 , 2.75375013, 2.05233751, 2.00326998,
           3.01042568, 2.96688943, 1.41100165, 1.72820793, 2.17347124,
           2.67133024, 1.42525366, 1.83247269, 1.42943476, 1.26593551,
           2.08386447, 2.72361639, 2.81627764, 2.2870122 , 2.29370148,
           2.49531836, 2.72247645, 3.09599891, 3.5403611 , 2.32239877,
           2.59990737, 2.69268117, 2.72951288, 3.36203496, 1.36096089,
           3.29555071, 3.19057232, 2.39149801, 1.37687771, 2.65211799])



This is a key reference:
http://wiki.scipy.org/Numpy_Example_List

## Example: selecting random points

Let's generate multivariate normal data:


```python
mean = (0,0)
cov = ((3, 2), (2, 5))
print(cov)
X = np.random.multivariate_normal(mean, cov, 100)
X.shape
```

    ((3, 2), (2, 5))
    




    (100, 2)




```python
plt.scatter(X[:,0], X[:,1]);
```


    
![png](output_75_0.png)
    


Now we can get the indices of randomly chosen points:


```python
# X.shape[0] is the number of points.

indices = np.random.choice(X.shape[0], 10, 
                           replace=False)
indices
```




    array([55, 59, 67, 54, 80,  0, 87, 11,  4, 81])



And then we can plot the selected points.


```python
plt.scatter(X[:,0], X[:,1], alpha=0.2)

selected = X[indices] # selects the rows with the given indices
plt.scatter(selected[:,0], selected[:,1]);

```


    
![png](output_79_0.png)
    



```python

```
