# Ramas en Git

En Git, las ramas son versiones independientes de un proyecto que permiten el desarrollo paralelo de diferentes características o soluciones.
Se utilizan para experimentar, aislar cambios, implementar nuevas funcionalidades y mantener versiones estables y de desarrollo separadas.
Las ramas facilitan la colaboración en equipo al proporcionar un entorno flexible para trabajar en diferentes partes del proyecto simultáneamente.

Al trabajar con ramas se utilizan principalmente cuatro comandos: `git branch`, `git switch`, `git checkout` y `git merge`.

## Creación de Ramas

El comando `git branch` sirve principalmente para la creación y eliminación de ramas:

```bash
git branch        # Muestra todas las ramas
git branch nueva-rama          # Crea una nueva rama desde HEAD
git branch nueva-rama <commit> # Crea una nueva rama desde un commit específico
git branch -d rama-a-eliminar  # Borra una rama (solo si ya se hizo merge de ella)
```

Otros comandos útiles:

```bash
git branch -D <rama>                # Borra una rama sin importar su estado
git branch -m nuevo-nombre           # Renombra la rama actual
git branch -m antiguo nuevo-nombre   # Renombra una rama específica
git branch -a                        # Lista las ramas locales y remotas
git branch -r                        # Lista solo las ramas remotas
```

## Moverse entre Ramas

Para moverse entre ramas hay dos formas: con `checkout` y con `switch`.

### `git switch` (Recomendado)

Desde Git 2.23, se recomienda el uso de `git switch` para cambiar entre ramas, ya que es más seguro y está diseñado específicamente para esta tarea.

```bash
git switch rama1       # Cambia a una rama existente
git switch -c rama1    # Crea y cambia a una nueva rama
```

### `git checkout` (No recomendado para cambiar de rama)

Antes de `git switch`, `git checkout` se usaba tanto para cambiar de rama como para restaurar archivos, lo que lo hacía confuso. Aunque aún funciona, `git switch` es la mejor opción para cambiar de rama.

Esto no es para que lo uses, sino para que sepas que checkout está obsoleto.

```bash
git checkout rama1      # Cambia a una rama existente
git checkout -b rama1   # Crea y cambia a una nueva rama (equivalente a `switch -c`)
```

## Combinar Ramas

Cuando quieres combinar ramas se utiliza `git merge`. Dado el siguiente caso:

Si nos encontramos en la rama `main` y ejecutamos:

```bash
git merge feature
```

A la rama `main` se le incorporarán los cambios de la rama `feature`.

### Tipos de merge:

- **Merge con fast-forward**: Si `main` no tiene commits nuevos desde que se creó `feature`, Git simplemente moverá el puntero de `main` hacia `feature`.
- **Merge con commit de fusión**: Si `main` y `feature` tienen cambios distintos, Git creará un commit adicional para fusionar ambos historiales.

Si hay conflictos, Git solicitará que los resuelvas antes de completar el merge.

Para confirmar el merge después de resolver los conflictos:

```bash
git add .
git commit -m "Resolviendo conflictos en merge"
```

