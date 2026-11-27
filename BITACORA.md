# Bitácora del laboratorio de rescate

**Programa:** Análisis y Desarrollo de Software (SENA)  
**Actividad:** Sesión 2 – Laboratorio de rescate (Guía RAP 01: Control de versiones con Git y GitHub)

---

## Preparación del repositorio

Comandos utilizados para iniciar el laboratorio:

- `git clone https://github.com/Chulixx162/laboratorio-rescate-julio-casanova.git`: descarga una copia local del repositorio creado en GitHub.
- `cd laboratorio-rescate-julio-casanova`: ingresa a la carpeta del proyecto.
- `git add .`: agrega los archivos creados (`notas.txt` y `BITACORA.md`) al área de preparación (*staging*).
- `git commit -m "feat: agrega notas y bitácora"`: guarda la primera versión de los archivos en el historial local.
- `git push`: sube los commits locales al repositorio remoto en GitHub.

---

## Reto 1: Ver el historial

### Comandos utilizados

```bash
git log
git log --oneline
git log --oneline --graph --all
```

### ¿Qué muestra cada comando?

- **`git log`**: muestra el historial completo de commits de forma detallada, incluyendo el código identificador completo, el autor, la fecha exacta y el mensaje de cada commit. (Se presiona `q` para salir).
- **`git log --oneline`**: muestra el mismo historial de forma resumida, ocupando una sola línea por commit con su código abreviado y el mensaje.
- **`git log --oneline --graph --all`**: muestra el historial resumido acompañado de un gráfico de líneas en texto que permite visualizar todas las ramas y cómo se dividen o unen.

### ¿Qué es el código que aparece al inicio de cada commit?

Es el **hash (SHA-1)** del commit: un código alfanumérico único que Git genera automáticamente para identificar cada versión guardada de forma exacta e irrepetible.

---

## Reto 2: Ver qué cambió antes de guardar

### Comandos utilizados

```bash
git diff
git add notas.txt
git diff
git diff --staged
git commit -m "docs: modifica línea 2 de notas"
```

### ¿Por qué el segundo `git diff` no mostró nada?

Porque `git diff` sin opciones solo muestra los cambios que están en el directorio de trabajo y que **todavía no** se han agregado al área de preparación. Al ejecutar `git add notas.txt`, el cambio pasó a *staging*, por lo que ya no quedaban diferencias pendientes fuera de esa área.

### ¿Qué muestra `--staged`?

`git diff --staged` muestra exactamente las diferencias de los archivos que ya fueron agregados con `git add` y que están listos para incluirse en el próximo commit.

---

## Reto 3: "Borré un archivo por error"

### Comandos utilizados

```bash
rm notas.txt        # o del notas.txt en Windows CMD/PowerShell
git status
git restore notas.txt
git status
```

### ¿Qué hace `git restore`?

Descarta los cambios o eliminaciones realizadas en un archivo dentro del directorio de trabajo y lo devuelve al estado que tenía en el último commit guardado.

### ¿Funcionaría si nunca hubieras hecho commit de ese archivo?

No. Git solo puede restaurar archivos que ya estén guardados en el historial o rastreados en el índice. Si el archivo nunca tuvo un commit, Git no tiene ninguna copia para recuperarlo.

---

## Reto 4: "Agregué un archivo que no debía subir"

### Comandos utilizados

```bash
git add .
git status
git restore --staged secreto.env
git status
git add .gitignore
git commit -m "chore: agrega .gitignore para archivos .env"
git push
```

### ¿Qué diferencia hay entre `git restore` y `git restore --staged`?

- **`git restore <archivo>`** deshace los cambios directamente en el archivo dentro de la carpeta de trabajo, dejándolo como estaba en el último commit.
- **`git restore --staged <archivo>`** solo saca el archivo del área de preparación (*staging*) para que no entre en el próximo commit, pero mantiene intacto el archivo y sus cambios en el disco.

---

## Reto 5: "Escribí mal el mensaje del commit"

### Comandos utilizados

```bash
git add notas.txt
git commit -m "cambios"
git log --oneline
git commit --amend -m "docs: agrega línea 4 a notas"
git log --oneline
git push
```

### ¿Qué hace `git commit --amend`?

Permite corregir el mensaje (o agregar cambios olvidados) del último commit local, reemplazándolo por uno nuevo antes de subirlo al repositorio remoto.

> **Importante:** Solo debe usarse cuando todavía no se ha hecho `git push`, ya que modifica el historial y podría causar conflictos si otros compañeros ya descargaron ese commit.

---

## Reto 6: "Subí un commit que dañó el proyecto"

### Comandos utilizados

```bash
git add notas.txt
git commit -m "fix: limpia notas"
git push
git revert HEAD
git log --oneline
git push
```

### ¿`git revert` borró el commit equivocado o creó uno nuevo?

Creó un **commit nuevo** (`Revert "fix: limpia notas"`) que aplica los cambios opuestos al commit erróneo para restaurar el archivo, dejando ambos commits registrados en el historial.

### ¿Por qué es la opción segura cuando ya se hizo push?

Porque no elimina ni reescribe el historial que ya fue publicado en GitHub. De esta forma, si otros integrantes del equipo ya descargaron los cambios, su historial no se rompe ni genera conflictos graves al sincronizar.

---

## Reto 7: "Hice un commit antes de tiempo"

### Comandos utilizados

```bash
git add borrador.txt
git commit -m "feat: agrega borrador"
git log --oneline
git reset --soft HEAD~1
git log --oneline
git status
git add borrador.txt
git commit -m "feat: agrega borrador completo"
git push
```

### ¿Qué diferencia hay entre `git reset --soft` y `git revert`?

- **`git reset --soft HEAD~1`** elimina el último commit del historial local, pero conserva el archivo y sus cambios en el área de preparación para poder seguir editándolo y volver a hacer el commit.
- **`git revert`** no borra el commit anterior, sino que crea un commit nuevo encima que deshace sus cambios.

### ¿Cuándo se usa cada uno?

- **`git reset --soft`** se usa cuando el commit es **solo local** (aún no se ha hecho `push`) y se quiere deshacer para corregirlo o completarlo.
- **`git revert`** se usa cuando el commit **ya se subió (`push`)** a un repositorio compartido y es necesario deshacerlo de manera segura.

---

## Reto 8: "Tengo trabajo a medias y debo cambiar de tarea"

### Comandos utilizados

```bash
git stash
git status
git stash list
git switch -c fix/urgente
git switch main
git stash pop
git status
git add notas.txt
git commit -m "docs: agrega línea 5 a notas"
git push
```

### ¿Para qué sirve `git stash`?

Sirve para guardar temporalmente los cambios que están a medias sin necesidad de hacer un commit incompleto, dejando el área de trabajo limpia para poder cambiar de rama o atender otra tarea urgente.

### ¿Qué hace `git stash pop`?

Recupera los últimos cambios guardados en el *stash*, los vuelve a aplicar sobre la rama actual y elimina ese registro de la lista temporal.

---

## Cierre: Historial final del repositorio

Resultado de ejecutar `git log --oneline --graph`:

```text
* 4eec730 (HEAD -> main, origin/main, origin/HEAD) docs: agrega línea 5 a notas
* 41de125 (fix/urgente) feat: agrega borrador completo
* 05acfc3 Revert "fix: limpia notas"
* b1a4bca fix: limpia notas
* d9bb2c7 docs: agrega línea 4 a notas
* fe92202 chore: agrega .gitignore para archivos .env
* 3d2fc97 docs: modifica línea 2 de notas
* b4729ca feat: agrega notas y bitácora
```