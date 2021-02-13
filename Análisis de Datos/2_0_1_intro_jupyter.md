# Introducción a Jupyter (antes IPython)
## Miguel-Angel Sicilia, msicilia@uah.es

## Objetivos

* Entender qué es Jupyter como entorno de computación interactiva.
* Saber manejar el entorno de Notebooks de Jupyter.
* Saber utilizar las características de entorno mejorado de IPython.
* Comprender cómo Jupyter puede utilizarse para compartir trabajo.

## ¿Qué es Jupyter?

* Comienza como un proyecto denominado **IPython** a modo de *intérprete de Python mejorado*.
* Idea: Combinar HTML, markdown, Python (u otros lenguajes), Latex, javascript, etc. en un **"Notebook"**.
    * Ideas de *literate programming* y "reproducible science".
* Paradigma REPL (*read-eval-print loop*) distribuido soportado por un protocolo independiente del lenguaje.
  * Comenzó con (I-)Python, ahora cubre muchos otros.
* Programación paralela “DIY” integrada.


## Jupyter es un servidor de kernels

<img src="http://res.cloudinary.com/dyd911kmh/image/upload/f_auto,q_auto:best/v1508152648/Jupyter-notebook-Definitive-Guide_ul01sa.png">

%Un kernel es un intérprete de Python, cada vez q abro una hoja se abre un intérprete de python, por lo que tendremos un intérprete por cada hoja

## Un poco de práctica

* Uso de IPython en consola.
* Notebooks:
    * `1_uso_interfaz`
    * `2_ipython_magics`
    * `3_timing`


## (de nuevo)¿Qué es Jupyter?

* Un protocolo de computación interactiva.
* Que soporta múltiples lenguajes o *kernels*:
  * https://github.com/jupyter/jupyter/wiki/Jupyter-kernels


## Compartir Notebooks

* En Internet
* En mi organización

## nbviewer

* `nbconvert` como servicio en la Web.
    * http://nbviewer.jupyter.org/
* Permite publicar desde Git o Gist.
* No guarda los Notebooks, solo los muestra.

## jupyterhub

* Para grupos de trabajo: https://jupyterhub.readthedocs.io/en/latest/
* Funcionamiento:
    * Autenticación de usuarios
    * Servidores Jupyter separados para cada usuario.
    * Se puede distribuir en clúster con diferentes servidores, y compartir datos vía NFS. 



<img width="400" src="https://jupyterhub.readthedocs.io/en/latest/_images/jhub-parts.png">
