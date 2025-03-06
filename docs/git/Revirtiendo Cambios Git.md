# Revirtiendo Cambios

La magia de Git radica en su capacidad para deshacer cambios y volver a una versión anterior del código en caso de cometer algún error.

## Restore

Imagina que estás trabajando en un archivo en Git y quieres deshacer los cambios realizados en él. Para hacerlo, puedes restaurar el archivo a la versión guardada en el último commit (HEAD).

Existen varias formas de hacer esto, pero para este caso de uso, utilizaremos el comando `restore`.

```bash
git restore <archivo>
```

Si especificamos `.` en lugar de un archivo específico, se restaurarán todos los archivos en el directorio de trabajo.

Además, podemos usar `restore` para regresar a un commit específico.

```bash
git restore --source <id_commit> -- <archivo>
```

El comando `restore` trae una versión anterior de un archivo a tu directorio de trabajo. También es útil para recuperar archivos que hayas borrado con `rm`.

> Desde Git 2.23, se introdujo el comando `git restore` como una alternativa más clara y específica a `git checkout` para deshacer cambios en archivos individuales. Antes de esta versión, `git checkout` se utilizaba tanto para cambiar de ramas como para restaurar archivos, lo que generaba confusión.

## Reset

Cuando borras un archivo con `git rm <archivo>`, notarás que no puedes restaurarlo con `git checkout`. Esto ocurre porque el archivo ya no está siendo rastreado por Git.

Para recuperarlo, puedes usar `git reset`. Este comando restaura tu directorio de trabajo al estado de un commit específico.

También puedes usar `git reset` para deshacer cambios ya confirmados (commiteados) y regresar a un commit anterior si te das cuenta de que hubo un error.

Existen tres modos principales de `reset`:

- **Soft (--soft):** Mantiene los cambios en el área de preparación (staging area), lo que significa que puedes volver a hacer commit fácilmente.
- **Mixed (--mixed)** (por defecto): Elimina los cambios del área de preparación, pero los mantiene en el directorio de trabajo.
- **Hard (--hard):** Elimina completamente los cambios tanto del área de preparación como del directorio de trabajo. Úsalo con precaución, ya que los cambios eliminados no se pueden recuperar, a menos que los hayas guardado en el reflog de Git.

```bash
git reset --soft <commit>
git reset --mixed <commit>
git reset --hard <commit>
```

## Revert

`git revert` es un comando que te permite deshacer cambios ya confirmados (commiteados), pero de una manera menos destructiva que `reset`.

En lugar de modificar la historia de commits, `git revert` crea un nuevo commit que deshace los cambios de un commit específico.

```bash
git revert <commit>
```

También puedes revertir una serie de commits. Esto también se puede aplicar a `git revert`:

```bash
git revert -n master~5..master~2
```

Este comando revierte los cambios desde `master~5` hasta `master~2`, pero los deja en el área de preparación para que los revises antes de hacer un nuevo commit.

## ¿Cuándo usar reset y cuándo revert?

- Usa `reset` cuando quieras modificar la historia de commits, especialmente si los cambios no se han compartido aún con otros colaboradores en el repositorio remoto.
- Usa `revert` cuando necesites deshacer un commit sin perder la historia ni afectar a otros colaboradores en el repositorio.

