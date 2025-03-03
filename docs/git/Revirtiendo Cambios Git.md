# Revirtiendo Cambios

La magia de git es que da la capacidad de desacer cambios que has hecho y volver a una versión del código anterior en casode cometer algún fallo.

## Restore

Imagina que trabajas en un archivo de git y quieres deshacer los cambios que estabas haciendo al mismo, para ello puedes volver a la version guardada en el HEAD.

Hay varias formas de hacer esto, pero para este caso de uso usaremos el command `restore`.

```bash
git restore <archivo>
```

Recuerdo que si especificamos `.` es el equivalente a decir todos los archivos de donde estoy trabajando.

Pero podemos utilizar `restore` para ir a un commit en particular.

```bash
git restore --source <idcommit> -- <archivo>
```

La función de `restore` es traer a tu directorio de trabajo una versión anterior de un archivo. También sirve para recuperar archivos que hayas borrado con rm.

>Desde Git 2.23, se introdujo el comando git restore como una alternativa más clara y específica a git checkout para deshacer cambios en archivos individuales. Antes de esta versión, git checkout se utilizaba tanto para cambiar de ramas como para restaurar archivos, lo que generaba confusión.

## Reset

Cuando tu borras un archivo con `git rm <archivo>` te darás cuenta que no puedes restaurarlo con `checkout`. Esto se debe a que se ha borrado el trackeo del archivo en git.

Para recuperarlo puedes usar `git reset`. Esto, restaura tu directorio de trabajo al commit que especifiques.

También se utiliza para volver a un determinado commit en el caso de que te des cuenta que ha habido un error y tengas que descartar el trabajo hecho, es decir, deshacer cambios ya commiteados.

Existen tres modos principales de reset:

- Soft (--soft): Mantiene los cambios en el área de preparación (staging area), lo que significa que puedes volver a hacer commit fácilmente.
- Mixed (--mixed) (por defecto): Elimina los cambios del área de preparación, pero los mantiene en el directorio de trabajo.
- Hard (--hard): Elimina completamente los cambios tanto del área de preparación como del directorio de trabajo. Úsalo con precaución, ya que no podrás recuperar los cambios eliminados.

```bash
git reset --soft <commit>
git reset --mixed <commit>
git reset --hard <commit>
```

## Revert

git revert es un comando que te permite deshacer cambios ya confirmados, pero de una forma menos destructiva que reset.

En lugar de modificar la historia de commits, revert crea un nuevo commit que deshace los cambios de un commit específico.

```bash
git revert <commit>
```

También puedes revertir una serie de commits. Esto tambien es aplicable a `revert`:

```bash
git revert -n master~5..master~2
```

Este comando revierte los cambios desde master~5 hasta master~2, pero los deja en el área de preparación para que los revises antes de hacer un nuevo commit.

## ¿Cuándo usar reset y cuándo revert?

- Usa reset cuando quieras reescribir la historia de commits, especialmente si los cambios aún no han sido compartidos con otros.
- Usa revert cuando necesites deshacer un commit sin perder la historia ni afectar a otros colaboradores en el repositorio.

