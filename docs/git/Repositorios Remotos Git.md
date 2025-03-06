# Repositorios Remotos Git

Los repositorios que creamos con `git init` son repositorios locales en nuestro ordenador, pero para trabajar de manera colaborativa se utilizan repositorios remotos.

Para crear un repositorio remoto puedes utilizar Github, Gitlab o uno propio que hagas con Gitea.

La forma de trabajar con ellos no es muy diferente. No nos vamos a centrar en aspectos concretos de cada plataforma (crear repositorios github, autenticarse, tokens...), solo nos vamos a centrar en como trabajar sobre ellos.

Cuando creas un repo en tu ordenador y creas un repositorio remoto, para subir los cambios tienes que decir a git donde va a mandar los cambios.

```bash
git remote add origin direccion_del_repositorio 
```

Esto te agrega la direccion al repositorio para trabajar. Origin es el nombre que por convencion, si declaras varios en el mismo repo tienen que tener nombre diferente.

Si tienes un repo remoto ya creado, lo tienes que traer a tu dispositivo, para eso clonas el repo:

```bash
git clone direccion_del_repositorio
```

Con el clone ya tiene por defecto el remoto de donde lo clonase con el nombre origin, si quieres cambiarle el nombre:

```bash
git remote rename origin <nuevo-nombre>
```

Tambien puedes eliminar un remoto:

```bash
git remote remove origin
```

## Trabajando Con repositorios

Ya teniendo el repositorio en tu máquina local si ejecutas `git fetch` mira si hay cambios del repositorio remoto comparado con tu repositorio local (solo descarga los cambios del remoto, pero no los fusiona con tu rama local. Para fusionarlos, debes ejecutar git merge o git pull.). En el caso de que haya cambios, puedes ejecutar:

```bash
git pull 
```

Esto hace pull al repo que tengas por defecto en upstream, sin tocar nada lo tendras en origin, si quieres otro a continuacion del pull pon el nombre del remoto que quieres.
Git pull es una combinación de git fetch y git merge, lo cual implica que al hacer un pull se traen los cambios y se intentan fusionar con la rama local automáticamente.

En este momento, puedes empezar a trabajar como en local y hacer los commits necesarios. Una vez ya terminado tienes que llevar los cambios al repositorio remoto:

```bash
git push origin main
```

Con esto mandas los cambios al remoto origin, a la rama main. Si el nombre de la rama local y la remota no coinciden, tendras que indicar:

```bash
git push origin <nombrelocal> <nombreremoto>
```

Si tienes definido el upstream, es decir, a donde manda por defecto los cambios de esa rama puedes simplemente hacer `git push`.

Para definir el upstream, normalmente cuando lo clonas lo tienes ya definido, pero si creas tu la nueva rama no por lo que tendras que declararlo con `-u`.

```powershell
git push -u origin main
```

---

>Siempre tienes que hacer pull antes de push, si no, el remoto lo rechaza. Si estas trabajando de forma colaborativa y tienes que hacer pull a un cambio, puede provocar un merge conflict.

## Pull Rebase

Si se da este caso de que alguien ha colaborado a la misma rama y ha enviado cambios antes que tú. No hagas pull directamente, esto dejará los dos commits y los juntará mediante un nuevo commit de merge. Esto hace muy dificil revisar el repositorio en caso de tener que buscar un error.

Para solucionar esto, puedes hacer un pull rebase:

```bash
git pull --rebase
```

Esto lo que hace es quitar tu commit del historial, hacer pull del contenido y posteriormente volver a agregar tu commit a HEAD.

El rebase solo funcionará sin conflictos si los cambios en el remoto no afectan las mismas líneas de código que tus cambios. Si hay conflictos, debes resolverlos manualmente como si fuera un merge conflict.

```bash
git rebase --abort
git pull
```

O tambien puedes corregir el rebase y hacer:

```bash
git rebase --continue
```

Otra alternativa es hacer fetch y merge.

## Merge Conflict

Un merge conflict ocurre cuando dos ramas diferentes contienen cambios incompatibles en el mismo archivo, y Git no puede determinar automáticamente cómo fusionar esos cambios.

En este caso, requiere intervención manual del usuario para resolver el conflicto, decidiendo qué cambios mantener y cómo combinarlos adecuadamente.

Para solucionar el conflicto, antes de nada hay que localizarlo, `git status`. Para corregirlo tendrás que editar el archivo que da el conflicto ya que git lo ha escrito esperando a mas informacion. Por ejemplo:

```html
Hola
<<<<<<< HEAD
<h1>Contenido en la rama principal</h1>
=======
<h1>Contenido en la rama1</h1>
>>>>>>> rama1
```

Esto es un conflicto simulado entre dos ramas, una main y otra rama1. En tal caso tendras que borrar las lineas para quedarte con el contenido que quieres, en este caso me quedare con el contenido de la rama main.

```
Hola
Contenido en la rama principal
```

Ahora le tenemos que informar a git que se ha solucionado:

```bash
git add .
git merge --continue
```

Los editores modernos y clientes de git como Code, IntelliJ o Gitkracken te ayuda a hacer esto de modo visual con una GUI pero esta es la forma que se haría desde consola.

