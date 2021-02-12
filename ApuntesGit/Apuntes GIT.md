# APUNTES GIT

## 1. Partes de un proyecto Git

1. Archivos y directorios que creamos nosotros
2. ".git" en el directorio root del repositorio que contiene información sobre la historia del proyecto.

## 2. Comprobar estado de un repositorio

- **`git status`**: Muestra la lista de archivos que han sido modificados desde el último guardado
- **``git diff``**: Cambios en el repositorio
- **`git diff 'directory'`**: Cambios en los ficheros del 'directory' 
- **`diff --git a/rep.txt b/rep.txt`**: Diferencias entre 2 ficheros. "a" 1ª versión, "b" 2ª versión. "-" muestra borrados, "+" muestra adiciones.
  
## 3. Git Workflow

DirectorioDeTrabajo --- Staging Area --- RepoLocal --- RepoRemoto

   git add --------------->     git commit ------->   git push --->
                                                    <--- git pull

   <----------------------------------------- git checkout

## 4. Ver cambios 

- **`git diff -r HEAD`**: '-r' significa comparar con una revisión y 'HEAD' con el commit más reciente
- **"`git diff HEAD~1..HEAD~3`**: Diferencia entre commit HEAD~1 y commit HEAD~3.

## 5. "Commit" cambios. Guardar en StagingArea

**`Commit guarda`** el autor, un mensaje y el tiempo del commit.

- Cuando hacemos "commit" se requiere un log message (comentario). Usando **`git commit -m "Comentario"`**
- **`git commit --amend -m "NuevoComent"`**: Esto es para enmendar un comentario anterior.
- **`git commit`**: Sin '-m' lanza visual estudio (hay que configurarlo para ello) y puedes añadir mas información sobre el commit.

## 6. Ver historia del repositorio

- **`git log`**: Muestra el log de la historia del projecto, primero salen los más recientes.
- **`git log -3 report.txt`**
- **`git annotate file`**: Muestra quién hizo cambios a cada linea del archivo y cuando.

## 7. Ver un commit especifico ("git show")

- **"`git show 'hashDelCommit`"**: Muestra la entrada del log y los cambios (como 'diff').
- **`git show HEAD~'nº del commit por debajo del primero"`**: Usamos HEAD para referirnos al commit más reciente y '~1' sería el anterior (debajo).

## 8. Ignorar files `.gitignore`

Crear un file en el directorio root llamado ".gitignore".

Si .gitignore contiene: "build" "*.py" tenemos que Git ignora los ficheros/directorios nombrados "build" y las extensiones ".py"

## 9. Borrar archivos

- **`git clean -n`**: Muestra los archivos que están en el repo pero no están trackeados
- **`git clean -f`**: Se borran los archivos

## 10. Configuración de Git

- **`git config --list`**: Muestra los ajustes. Opciones: --system: Ajustes para cada usuario del orde. --global: Ajustes para cada uno de tus proyectos. --local: Ajustes para un proyecto específico.

En cada ordenador, al configurar Git debemos añadir el **usuario** (user.name) y el **correo** (user.email).

- **`git config --global user.name Fran`**
- **`git config --global user.email Fran@hotmail.es`**

## 11. Subir selectivamente al repoLocal ('commits' selectivos)

- **`git add path/to/file`**. Si nos equivocamos con el path podemos hacer un **`git reset HEAD`** para borrar el ultimo commit

## 12. Revertir cambios a archivos 

- NO añadidos (add) al stage area: **`git checkout -- ruta/al/archivo `**
- SÍ añadidos (add) al stage area: **`git reset HEAD path/to/file`** + **`git checkout -- path/to/file`**

## 13. Restaurar versiones antiguas de un fichero

- **`git checkout 2242bd report.txt`** Reemplaza la version actual de 'report.txt' con la versión que tiene el hash '2242bd' para ver esto podemos hacer **`git log`**. Hacer esto crea un nuevo 'commit'

## 14. Deshacer todos los cambios hechos 

- **`git reset HEAD 'directorio'`** quita del stage area los archivos del 'directorio' del HEAD (ultimo commit)
- **`git reset HEAD`** quita todo del HEAD commit (ultimo commit)
- **`git reset`** quita TODO de todos los commits
- **`git checkout -- 'directorio'`** Restaura los archivos del 'directorio' a su estado anterior
- **`git checkout -- . `** Restaura los archivos del directorio actual (el punto lo indica '.')

## 15. **Ramas**

Sirve para tener múltiples versiones de tu trabajo y permite llevar un tracking de cada versión sistematicamente. Lo que hagas en cada rama no afecta a las demás (hasta que hagas **`merge`**)

Por defecto cada repositorio Git tiene una rama llamada **master**

- **`Ver ramas`**: Comando **`git branch`** muestra las ramas existentes, en la que estamos actualmente aparece con un '*'
- **`Diferencias entre ramas`**: Comando **`git diff branch1..branch2`** muestra la diferencia entre esas ramas
- **`Cambiar de rama`**: Comando **`git checkout 'nombreRama'`** cambia a la rama 'nombreRama'
- **`Crear rama y moverme a ella (2en1)`**: Comando **`git checkout -b 'nombreRama'`** me crea la rama y me mueve a ella. Los contenidos de la rama son identicos al original, cuando metes cambios en la rama, solo estarán ahí.
- **`Unir dos ramas (merge)`**: Comando **`git merge 'origen' 'destino'`** esto mete los cambios del origen en la rama 'destino' y se crea un commit

## 16. **Crear repositorios**

NO CREAR REPOSITORIOS DENTRO DE OTROS EXISTENTES
- **`git init project-name`**: Crea un repositorio llamado 'project-name' (nombre que tendrá su directorio raíz)
- **`git init`**: Para crear un repositorio bajo el 'path' en el que estás, o usar **`git init /path/to/project`** para ir donde el path y crearlo bajo ese

## 17. **Clonar**/copiar un repositorio existentes

Clonar un repo hace que se copie todo el repositorio (incluida la historia) en un nuevo directorio

- **`git clone 'URLdelRepo'`**: Clona el repo ubicado en 'URLdelRepo'
- **`git clone /proyecto/existente 'nombreProyectoNuevo`**: Clona un **`repo local`**, le pone de nombre al directorio raíz 'nombreProyectoNuevo'. Si no se especifíca le pone el mismo nombre que el que tiene el 'repo local'
  
## 18. Saber de donde viene un repositorio clonado (ver donde está el original)

Cuando se hace un **`clone`** Git guarda un 'remote' apuntando al repo original.
Si clonamos un repo de GitHub a nuestro ordenador local, entonces la copia de GitHub es nuestro 'remoto' (sería el 'origin')

- **`git remote`**: Estando en un directorio, lista los nombres de los remotos

## 19. Definir (remotos) **`remotes`**. Uso de 'origin'

Cuando clonamos un repo, Git crea un remoto llamado 'origin' que apunta al repo original

- **`git remote add nombre-remoto URL`**: Añade otro remoto
- **`git remote rm nombre-remoto`**: Borra el remoto

## 20. **`Pull`**. Traer cambios de un repo remoto a nuestro repo local

- **`git pull remote branch`**: Trae todo de la rama 'branch' del repo remoto identificado como 'remote' y hace un 'merge' para unirlo con la rama en la que estés actualmente

## 21. **`Push`**. Subir cambios de nuestro repo local a un repo remoto

Solo se puede hacer push a un repo remoto cuando hagamos un 'merge' de los contenidos del repositorio remoto a nuestro trabajo en local

- **`git push remote-name branch-name`**: Sube los cambios de mi rama 'branch-name' a una rama con el mismo nombre asociado con el 'remote-name'
