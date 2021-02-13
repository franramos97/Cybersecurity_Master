## Algunas utilidades de IPython

### La historia

Lo primero que tenemos que notar es que las celdas se marcan con **In[..]** y las salidas de expresiones con **Out[...]** y estas son variables. Hay "atajos" como utilizar "_" para referirnos a la última salida.


```python
2+2
```


```python
_+2 #"_" significa operacion anterior y ya le suma 2
```

Las entradas son cadenas.


```python
In[3] #con esto le indicas la op q le metes [2] es 2+2 [3] es _ +2 etc...
```


```python
Out[1]+10 #la salida del [1] + 10
```


```python
_*4 +1
```


```python
print("penúltima:", __)
```

Para un acceso a la historia en general tenemos el magic %history (tiene parámetros, por ejemplo -f para grabar a fichero)


```python
%history
```

## Uso del sistema operativo

Cuando vamos haciendo programas más complejos, suele ser útil hacerlos en un script de Python y ejecutarlos desde el Notebook.


```python
%run script.py  #con esto puedo ejecutar el script "script.py"
```

Y la memoria lógicamente se actualiza


```python
z + 3
```

Tenemos además posibilidad de utilizar comandos del sistema operativo. Tiene dos variantes:
* shell capture %sc o abreviado con "!"
* shell execute %sx o abreviado con "!!"


```python
%ls -la *.ipynb  #esto es linux
```


```python
lista= %ls -la *.ipynb #estoy metiendo el comando en una variable "lista"
lista
```


```python
%ls -la *.ipynb
```

Las listas pueden procesarse.


```python
donde_estoy = %pwd
print(type(donde_estoy))
donde_estoy
%whos
```

Y se pueden utilizar variables de Python en los magics.


```python
nombre = "carpetaNueva"
%mkdir {nombre} #creo una "carpetaNueva" con este nombre, en el dir q estoy
%ls -l c* #listo todo lo q empiece por 'c'
```

## Más magia

Para ver todos los comandos mágicos:


```python
%lsmagic #List currently available magic functions
```


```python
procesos = !ps
print(type(procesos))
print(procesos[2])
```

Una referencia más completa en:
    http://ipython.org/ipython-doc/stable/interactive/reference.html


### Por último, veamos el kernel como un proceso!

Con *connect_info* podemos ver los detalles de la conexión del Notebook al kernel. Normalmente esto no es necesario, y podemos obviarlo completamente. 

No obstante, si queremos entender cómo se puede utilizar Jupyter para hacer programación paralela, es importante tener claro que **cada kernel es un proceso separado**. 


```python
%connect_info
```

Vemos que el Notebook está asociado a un proceso que es accesible mediante una configuración de red. Una forma de conectarnos desde una consola de IPython a este mismo proceso es:

<code>ipython console --existing</code> (se conecta al último kernel arrancado)

Podemos conectar a este mismo kernel desde una consola y probar a definir nuevas variables, que se verán tanto desde el Notebook como desde la consola. Es una forma de compartir un mismo proceso (aunque esto no es muy útil por sí mismo).


NOTA: Al salir de la consola se cerrará el kernel, a menos que salgamos indicando lo contrario:

<code>exit(keep_kernel=True)</code>


```python
%whos??
```

### Algún truco más interesante...

http://blog.rtwilson.com/ipython-tips-tricks-notes-part-1/
