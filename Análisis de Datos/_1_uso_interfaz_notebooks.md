# Cinco minutos con la interfaz de Notebooks

## Qué hay en cada sitio

Utiliza *"Help|User interface tour"*... 

## Los tipos de celda

Básicamente, podemos tener celdas de "markdown" para escribir, o de código (en este caso para escribir y ejecutar Python). Esto puede hacerse:
* Con el menú *"Cell | Cell type"*
* Con el desplegable de la barra de herramientas.
* Con atajos de teclado. Véase *"Help | Keyboard shortcuts"*

Cámbiame el tipo!

## Un poco de markdown

En una celda de markdown se puede utilizar el formato estilo Github que se describe aquí:
https://help.github.com/articles/markdown-basics/

Por ejemplo, un enlace: [Visit GitHub!](https://www.github.com)

## Un poco de HTML

<img src="https://jupyter.org/assets/main-logo.svg" width="50%" height="50%">

<table>
<tr><td>Grupo</td><td>proporción</td></tr>
<tr><td>A</td><td>0.2</td></tr>
<tr><td>B</td><td>0.3</td></tr>
</table>

## Un poco de Latex

\begin{equation*}
P(E)   = {n \choose k} p^k (1-p)^{ n-k} 
\end{equation*}



Esto es un subconjunto de Latex denominado <a href="https://www.mathjax.org/">MathJax</a>. 

## Y algunas cosas más...


```python
from IPython.display import YouTubeVideo
YouTubeVideo("Rc4JQWowG5I")
```

## Fragmentos de código


```python
a = [x**2 for x in range(1,10)]
a[3:]
```

Ahora elimina la salida del comando que ha quedado en el Notebook... --> Click derecho y le doy a "Clear outputs"

## El kernel 

Observa arriba a la derecha...


```python
import time
time.sleep(10) #aqui lo q ocurre es q arriba a la derecha al lado de Python3 el circulo se pone negro, diciendo q algo se está ejecutando
```

## Ejecutar código

Podemos ejecutar código en una celda de tipo "Code" de diferentes formas:
* Con atajos de teclado, p.ej. Ctrl+Intro.
* Con el menú "Cell"
* Con la barra de herramientas.


```python
a = range(1, 20, 2)
a = list(a) + [1] #a será la lista de a pero con un 1 al final
a
```

Nótese que el prompt In[X] indica un número, que es la secuencia de ejecución. Podemos limpiar los resultados de una celda con el menú *Cell| Current Output*. 

## La magia del tabulador

El tabulador permite autocompletar.


```python
L = ["imagen", "video", "texto"] 
%whos  #lo q empieza por % es un comando de ipython (comandos mágicos)
#TRUCO: ctrl+enter ejecuta el cuadro donde estamos IMPORTANTE!!!!!!!!!!! shift+enter ejecuta y salta a la siguiente celda
```


```python
#Prueba con el tabulador:
L.append
import pandas._config
```

## Ayuda con funciones


```python
import pandas as pd
pd.concat()   #SHIFT+TAB entre los parentesis para ver descripcion de lo q hace la clase 'concat' en este caso
```

Python tiene la función built-in help() que se puede también utilizar aquí.


```python
help(pd.compat)
```

Aunque IPython nos da formas alternativas de accederlo. 


Pulsa Shift|Tab entre los paréntesis:


```python
datos = pd.DataFrame()
```

## Pidiendo más ayuda --> con '?' y '??' se pide por pantalla lo anterior


```python
pd.read_csv?
```


```python
pd.read_csv??
```

O bien utiliza el menú "Help" para diferentes bibliotecas.

Pueden utilizarse todos los *"magics"* de IPython. Una referencia aquí: http://ipython.org/ipython-doc/stable/interactive/magics.html 

Los que afectan a la linea están prefijados por %, los que afectan a toda la celda por %% 


```python
%quickref
```


```python
%lsmagic #List currently available magic functions
```

## Referencia

http://nbviewer.ipython.org/github/ipython/ipython/blob/3.x/examples/Notebook/Index.ipynb

## Apéndice: Galileo fue el primer creador de Notebooks

Galileo cuestionó la teoría Aristotélica del geocentrismo, basado en las primeras observaciones con telescopio. Concretamente, observando los satélites del planeta Jupiter.

En sus observaciones, galileo incluía datos, reflexiones y dibujos. ¿Se parece bastante a algo?

Aquí hay un ejemplo de sus notas en la obra *Siderius Nuncius*, página 75.
<img src="http://galileo.rice.edu/lib/student_work/astronomy95/astrogrobs/snsmall.gif">


```python

```


```python

```
