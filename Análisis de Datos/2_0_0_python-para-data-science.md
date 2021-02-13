# Data science con Python, una introducción
### Miguel-Angel Sicilia, msicilia@uah.es

## Objetivos:
* Contextualizar el uso del stack científico de Python en el trabajo del *data scientist*.
* Valorar los puntos fuertes de Python como lenguaje de *data science* en contraposición a otros.
* Entender las diferentes partes del stack científico de Python.
* Comprender el concepto de distribución del stack científico de Python.

<img src="http://i.imgur.com/aoz1BJy.jpg">  

## ¿Un data scientist o varios?

* “More than 40% of data science tasks will be automated by 2020, resulting in increased productivity and broader usage of data and analytics by citizen data scientists, according to Gartner


* […] a **citizen data scientist as a person who creates or generates models that use advanced diagnostic analytics or predictive and prescriptive capabilities**, but whose primary job function is **outside the field of statistics and analytics**.

* […] citizen data scientists can bridge the gap between mainstream self-service analytics by business users and the advanced analytics techniques of data scientists”

## La ciencia en data science

* “Ironically, the greatest value from predictive analytics typically comes **more from their unexpected failures than their anticipated success**. In other words, the real influence and insight come from learning exactly how and why your predictions failed. […] it means the assumptions, the data, the model and/or the analyses were wrong in some meaningfully measurable way”.

⎯ Michael Schrage, research fellow at MIT Sloan School’s Center for Digital Business


## Machine learning en contexto

<img src="https://cdn-images-1.medium.com/max/1600/0*N1_npW--J0N1d_3o.">

<img src="https://blogs.gartner.com/smarterwithgartner/files/2015/10/EmergingTech_Graphic.png">

## La realidad de las tareas del data scientist

* "Data preparation accounts for about 80% of the work of data scientists"

* "76% of data scientists view data preparation as the least enjoyable part of their work"
    * https://whatsthebigdata.com/2016/05/01/data-scientists-spend-most-of-their-time-cleaning-data/

## ¿Por qué Python?

* Comienzo en 1994, Python 2 en 2000, Python 3 en 2008. Comunidad amplia y activa.
* Resuelve el problema de “two languages”.
* Lenguaje multiparadigma.
* Python puede utilizarse para invocar bibliotecas en C o Fortran, o utilizar lenguajes derivados como Cython.
* El entorno de edición ahora convertido en Jupyter (Julia+Python+R... y otros)


## El stack científico de Python

* Un lenguaje abierto de propósito general: https://www.python.org 
* Un conjunto de bibliotecas para computación científica: https://www.scipy.org
  * Extensibles con el concepto de `scikit`: https://www.scipy.org/scikits.html
* Un entorno de computación interactiva: http://jupyter.org/

* El stack de SciPy (ahora obsoleto) se definió formalmente aquí: https://www.scipy.org/stackspec.html
* Pero realmente es la base de un conjunto de bibliotecas en evolución.


<img width="60%" src="https://devopedia.org/images/article/60/7938.1587985662.jpg">

## Elementos fundamentales

* Basado en <code>numpy</code>: arrays y matrices <br>
* Algunas bibliotecas utilizan <code>pandas</code> que nos proprociona <code>DataFrames</code>.<br>
  * Ambas bibliotecas descritas por McKinney (segunda ed.)
  * Más recientemente, descrito por VanderPlas (2017)
  
<table border="0">
<tr><td><img width="200" src="https://covers.oreillystatic.com/images/0636920050896/lrg.jpg"></td>
<td><img width="200" src="https://jakevdp.github.io/PythonDataScienceHandbook/figures/PDSH-cover.png"></td>
</tr>
</table>

## ¿Qué es la programación científica?

* Estilo de programación centrado en la manipulación de vectores y matrices.
  * La mayoría de las operaciones comunes están ya encapsuladas como funciones y **vectorizadas**.
  * El código suele ser conciso y hacer un uso limitado de estructuras de control.
  * La eficiencia de operaciones importa, cuando el tamaño de los datos crece.
* Orientado a la fase analítica, no a producir código profesional para entornos de operaciones.


# ¿Qué tipo de datos?
* Normalmente tratamos datos tabulares o bien series temporales.
* Datos como texto o imágenes se "traducen" a ese formato tabular.


* Algunas bibliotecas asumen que tenemos "tidy data", que puede definirse como:
    * Each variable you measure should be in one column.
    * Each different observation of that variable should be in a different row.
    * There should be one table for each "kind" of variable.
    * If you have multiple tables, they should include a column in the table that allows them to be linked.

<img src="http://garrettgman.github.io/images/tidy-4.png">

<img src="https://r4ds.had.co.nz/images/tidy-8.png">

## Ubicando las diferentes tareas

<img src="http://www.havlena.net/wp-content/uploads/5-crisp-dm.png" width="800">

## ¿Qué es Anaconda?

* Es una distribución de SciPy, del mismo modo que Ubuntu es una distribución de Linux, e incluye a Jupyter.
* Incorpora un gestor de paquetes propio, denominado `conda`, que puede utilizarse en sustitución o como complemento a `pip`.

### Instalación de Anaconda

* https://docs.anaconda.com/anaconda/install/ 
