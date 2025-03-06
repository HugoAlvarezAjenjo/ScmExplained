# Trabajando con Git

Para iniciar un repositorio de Git (Carpeta en la que se va a aplicar el control de versiones) ejecutaremos:

```bash
git init
```

Este comando creará una carpeta oculta `.git` en la que se guardará toda la información, esta carpeta no debemos tocarla.

Una vez inicializado el repositorio podremos ya empezar a guardar las versiones del proyecto.

## Commits

Un "commit" en Git es un registro de cambios en un repositorio de código.
Se usa para guardar los cambios que has realizado en los archivos del proyecto, junto con un mensaje descriptivo que explica qué cambios se han realizado.

>Los mensajes deben de ser descriptivos. De nada sirve un mensaje que ponga `gfhfgh` a la hora de buscar un error en el código.

Para hacer un commit se pasan por dos etapas, una etapa de `stage`. En esta etapa se marcan los archivos que se quieran commitear.

Para esto se utiliza el commando:

```bash
git add path/to/the/file.java
```

Si quieres añadir todos los cambios, puedes hacerlo mediante el uso de `.` o `*`

```bash
git add .
```

Con `git status` podremos ver que archivos estan en stage,  los que no, los que rastrea o no el repositorio...

Para commitear los cambios marcados en etapa stage usaremos:

```bash
git commit -m "Mensaje del commit"
```

Con el parametro `-m` le inicamos el mensaje, si no lo incluimos se abrirá el editor que tengamos y ahí pondremos el mensaje.

Para commitear todos los cambios y saltarnos la etapa de stage podremos usar el parametro `-a`:

```bash
git commit -am "Ejemplo"
```

Este `-a` viene de decir quiero comitear todo, hay gente que se salta el `git add` indicando que archivos quiero comitear en el commit:

```bash
git commit index.html -m "Mensaje del commit"
```

Advertencia: Si un archivo no está siendo rastreado (por ejemplo, recién creado), git commit -a no lo incluirá, por lo que es importante usar git add para rastrear esos archivos nuevos.

Y simplemente utilizan el git add para indicar que el repo rastre los archivos recien creados:

```bash
git add -A # A mayuscula los archivos que todavia no se estan rastreando, eliminados, modificados... Todos los archivos presentes
```

Personalmente prefiero usar git add por coherencia.

Para quitar un archivo de stage simplemente ejecute:

```powershell
git rm --cached archivo/a/quitar.md
```

## Historial de Commits

Si queremos ver el historial de commits que hemos hecho a lo largo del desarrollo podemos usar el comando `git log`. Este nos muestra el historial junto con los ids de los commits que utilizaremos más tarde. Mi forma favorita de hacerlo es mediante los flags de:

```bash
git log --oneline --graph
```

El cual te los muestra en forma de grafo de las diferentes ramas y el commit te lo muestra en una sola linea.

## Diferencias entre los Commits

El comando `git diff` es muy útil, permite ver las diferencias entre distintos estados de un repositorio.

Sirve para visualizar diferencias entre distintas cosas, por ejemplo si ejecutas `git diff` ves la diferencia entre lo que hay en el directorio de trabajo y lo que hay en stage. También sirve para ver la diferencia entre el stage y el último commit `git diff --cached`, esto puede ser útil para revisar los cambios antes de commitearlos.

Los más útiles bajo mi opinión son:

```bash
git diff # El equivalente a git diff HEAD
```

Que muestra las diferencias entre el directorio de trabajo y el último commit. Si le añades el flag `--cached` muestra solamente los que estan en stage.

Para ver la diferencia de dos commits:

```bash
git diff <commit1> <commit2>
```

Este muestra la diferencia entre dos commits. (Tienes que pasar el identificador del commit, `git log`). Si solo añades 1 lo hace con el directorio actual.

Para ver un archivo concreto, a esto, usa:

```bash
git diff <commit1> <commit2> -- ruta/del/archivo
```

Si simplemente usas `git diff` sinn ningun id de commit, puedes omitirte las dos `--`.

## Corrección de Commits

Si estas trabajando en una serie de cambios y haces commit pero te das cuenta que tienes que realizar cambios, siempre puedes alterar el último commit con `ammend`.
Tras enviar a stage el archivo o los archivos que quieres cambiar, ejecutarás:

```bash
git commit --amend
```

Esto abrirá tu editor de texto predeterminado con el mensaje del commit anterior. Puedes editar el mensaje si lo deseas. Guarda los cambios y cierra el editor.

Si solo quieres cambiar el mensaje del commit, ejecuta esto último sin añadir ningun archivo a stage.

<u>**Advertencia:**</u> Hacer esto si todavía no has hecho un `push`, si estas trabajando en remoto. Si lo haces cuando ya lo has hecho push, es posible que tendras que hacer:

```bash
git push --force
```

Pero ten en cuenta que hacer esto reescribirá la historia del repositorio, lo que podría causar problemas si otros colaboradores ya han basado su trabajo en el commit anterior.

Una alternativa más segura es:

```bash
git push --force-with-lease
```

Esto es más seguro ya que comprueba si el repositorio ha cambiado, si lo ha hecho el comando falla.

## Commits Temporales

Git stash es una herramienta útil en Git que te permite guardar temporalmente cambios que aún no deseas comprometer o deseas apartar temporalmente del área de trabajo.

Es especialmente útil cuando estas trabajando en una rama y sin hacer un commit quieres cambiarte a otra ya que si realizas un cambio de rama perderás todos los cambios realizados en ella.

Tras añadir los cambios a stage (`git add`) ejecutas:

```bash
git stash
```

Para volver a aplicar los cambios seleccionados puedes usar:

```bash
git stash apply
```

Esto aplicará el último conjunto de cambios almacenados en el stash. Si tienes múltiples sets de cambios guardados, puedes aplicar uno en particular especificando el identificador del stash. `git stash apply stash@{2}` 

Una vez que ya no necesites los cambios puedes borrarlos

```bash
git stash drop
```

Si tienes varios funciona igual usano el identificador. `git stash drop stash@{2}`.

El yo uso es:

```bash
git stash pop
```

Esto aplicará el último set de cambios almacenados en el stash y luego lo eliminará automáticamente del stash.

Puedes ver una lista de todos los sets de cambios almacenados en el stash.

```bash
git stash list
```

A partir de git 2.13 se puede nombrar a los stages (Probablemente tengas ya una versión superior ya que esta salió en 2017, en año el el cual la gente flipaba con el meme del cocinero que echa sal).

```bash
git stash push -m "Nombre descriptivo para el stash"
```

Por lo que podras utilizar este nombre como el identificador en los `apply` y `drop`.

