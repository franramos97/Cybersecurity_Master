## Tipos de posts en Tumblr

La bibliotecas pytumblr es la "oficial" de la red social para el lenguaje Python. Más información aquí: https://github.com/tumblr/pytumblr


```python
import pytumblr #pip install pytumblr en el directorio dnde este esto con powershell
```

Como solo vamos a extraer posts taggeados, no hace falta autenticación, basta con una consumer key que tendremos que obtener creando antes una cuenta en Tumblr y solicitando una aplicación.


```python
apikey = "FEe2Qt1CS2Z6ZSFsU4KUgGfHNgcGIXfUOoELZjyxpZJFtjS203"
client = pytumblr.TumblrRestClient(consumer_key=apikey)
```

Probamos el tipo de datos que nos devuelve la recuperación por tags. 


```python
# Prueba, imprimimos el primero de la lista que nos devuelve.
post = client.tagged("random")[0]
print(type(post))
post
```

Ahora vamos a sacar muestras de posts sucesivas para analizar los tipos.

Y almacenaremos los tipos de los posts en una matriz.


```python
import numpy as np

def get_sample(chunks, trace=False):
    data = None
    before = None # Para controlar que sacamos grupos diferentes de posts.
    for i in range(0, chunks):
        if before is None:
            posts = client.tagged("random")
        else:
            posts = client.tagged("random", before=before)
        # Tomamos los tipos de post y las marcas temporales:
        types = []
        timestamps = []
        for p in posts:
            types.append(p["type"])
            timestamps.append(p["timestamp"])
        if trace: 
            print(types)
        # Sacamos la marca temporal más antigua:
        before = min(timestamps)
        if data is None:
            data = np.array(types)
        else:
            data = np.vstack([data, np.array(types)])  
    return data
```


```python
data = get_sample(10, trace=True)
```


```python
data.shape
```

Podemos ahora con <code>unique()</code> obtener las diferentes etiquetas que han salido en la muestra (no necesariamente todos los valores de Tumblr)


```python
labels = np.unique(np.array(data))
labels
```

### Contando los diferentes tipos


```python
def get_counts(data, labels):
    counts = {}
    for t in labels:
        valores = data[data == t] # Selecciona las ocurrencias del tipo.
        cuenta = valores.size  # Cuidado, no utilizar len()
        counts[t]= cuenta
    return counts

c = get_counts(data, labels)
c, list(c.values())
```

### Repitiendo el proceso

Tomamos una muestra más grande.


```python
data = get_sample(200, trace=False)
```

Obtenemos las proporciones de imágenes frente al total de posts, tomando sub-muestras.


```python
prop = []
for i in range(20000):
    indexes = np.random.choice(data.shape[0], 30, replace=False)
    sample = data[indexes]
    labels = np.unique(np.array(sample))
    counts = get_counts(sample, labels)
    prop.append(counts["photo"]/np.sum(list(counts.values())))
```

Dibujamos la distribución de las proporciones obtenidas. Como era de esperar por el teorema central del límite, esa proporción sigue una distribución normal. 


```python
%matplotlib inline
import matplotlib.pyplot as plt
import seaborn as sns

_ = sns.histplot(prop);
```

No obstante, las muestras que tomamos en un momento reflejan el uso actual de Tumblr, pero es importante entender que ese uso, con el tiempo, puede cambiar.


```python

```
