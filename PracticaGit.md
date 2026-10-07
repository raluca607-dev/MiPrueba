#### RALUCA LORENA ZAINESCU
# PRÁCTICA GUIADA DE GIT

### **Crear el directorio *prueba_git* y accedemos a él**

```bash
mkdir prueba_git
cd prueba_git
```

 Después de esto nos encontramos aquí: 
 ```bash
  Raluca@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFON/prueba_git
```

Dentro del directorio creo dos archivos: `texto.txt` y `script.sh`.

El archivo `texto.txt` contiene:

```text
uno
dos
tres
CUATRO
```

El archivo `script.sh` contiene:

```bash
echo Listado completo
ls -l
```

Le doy permisos de ejecución al script y lo ejecuto:

```bash
chmod a+x script.sh
./script.sh
```

---

# 1. Inicialización del repositorio

Inicializo el directorio como un repositorio Git:


```bash
git init
```
Lo cual nos devuelve este mensaje:
```bash
Reinitialized existing Git repository in C:/Users/Raluca/Desktop/AFON/prueba_git/.git/
```

Esto crea el directorio oculto `.git`, donde Git almacena la información necesaria para gestionar las versiones del proyecto.

Compruebo el estado:

```bash
git status
```
La salida es esta:

```text
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        script.sh
        texto.txt

nothing added to commit but untracked files present (use "git add" to track)

```
Los archivos aparecen como `Untracked` porque todavía no están siendo gestionados por Git.

Cambio el nombre de la rama principal a `main`:

```bash
git branch -m main
```

Compruebo el estado:

```bash
git status
```

Ahora Git indica que estoy trabajando en `main`.

```bash
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        script.sh
        texto.txt

nothing added to commit but untracked files present (use "git add" to track)

```

Restauro el nombre de la rama a `master`:

```bash
git branch -m master
```

---

# 2. Añadiendo archivos: add y commit

Añado `script.sh` al área de preparación:

```bash
git add script.sh
git status
```

Ahora `script.sh` aparece preparado para realizar un commit, mientras que `texto.txt` continúa como `Untracked`.

La salida es esta:

```text
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   script.sh

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        texto.txt
```

Realizo el primer commit:

```bash
git commit -m "Confirmación inicial"
[master (root-commit) 3164c9a] Confirmación inicial
 1 file changed, 2 insertions(+)
 create mode 100644 script.sh

```

Con este commit se guarda la primera versión de `script.sh`.

Compruebo el estado:

```bash
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        texto.txt

nothing added to commit but untracked files present (use "git add" to track)

```

A continuación añado `texto.txt`:

```bash
git add texto.txt
git commit -m "Añadido archivo texto.txt"
git status
```

Después del commit, el repositorio queda limpio:

```text
On branch master
nothing to commit, working tree clean
```

---

# 3. Modificando archivos

Modifico `texto.txt` añadiendo una nueva línea:

```text
uno
dos
tres
CUATRO
cinco
```

Compruebo el estado:

```bash
git status
```

Git detecta que `texto.txt` ha sido modificado:

```text
Changes not staged for commit:
    modified: texto.txt
```

Preparo el archivo para el siguiente commit:

```bash
git add texto.txt
git status
```

Ahora añado otra línea a `texto.txt`:

```text
uno
dos
tres
CUATRO
cinco
seis
```

Al ejecutar:

```bash
git status
```
Vemos que el mismo archivo aparece tanto en los cambios preparados como en los cambios no preparados.
```bash
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   texto.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   texto.txt

```


Esto ocurre porque `git add` preparó la versión que contenía hasta `cinco`, pero después el archivo volvió a modificarse añadiendo `seis`.

Para preparar también esta última modificación ejecuto:

```bash
git add texto.txt
```

Realizo el commit:

```bash
git commit -m "Ampliada la explicación del texto"
git status
```

El repositorio vuelve a quedar limpio:

```text
On branch master
nothing to commit, working tree clean
```

---

# 4. Información: git show y git log

Para consultar información sobre el último commit ejecuto:

```bash
git show
```

Git muestra información sobre el commit y los cambios realizados.
```bash
Author: Ralu <raluca607@gmail.com>
Date:   Wed Oct 7 09:45:10 2026 +0200

    Ampliada la explicación del texxto

diff --git a/texto.txt b/texto.txt
index e41dfd2..445880e 100644
--- a/texto.txt
+++ b/texto.txt
@@ -1,4 +1,6 @@
 uno
 dos
 tres
-CUATRO
\ No newline at end of file
+CUATRO
+cinco
+seis
\ No newline at end of file

```
Para consultar el historial completo:

```bash
git log
```

Aparecen los commits realizados hasta ahora:

```text
commit 4951839ce6c95bc900eddaa34c0f858d7400a4d9 (HEAD -> master)
Author: Ralu <raluca607@gmail.com>
Date:   Wed Oct 7 09:45:10 2026 +0200

    Ampliada la explicación del texxto

commit d2c5e7bd4e5580002b253574e848e8b222f53b66
Author: Ralu <raluca607@gmail.com>
Date:   Wed Oct 7 09:38:47 2026 +0200

    Añadido archivo texto.txt

commit 3164c9a0362939e11c5a5ae7731af839d58803b0
Author: Ralu <raluca607@gmail.com>
Date:   Wed Oct 7 09:34:12 2026 +0200

    Confirmación inicial


```

Cada commit tiene un identificador único denominado **hash**, además del autor, la fecha y el mensaje del commit.

También es posible realizar en un único comando el `add` de archivos ya rastreados y el `commit`:

```bash
git commit -a -m "Nueva versión"
```

Este comando no sirve para añadir archivos nuevos que todavía sean `Untracked`.

---

# 5. Diferencias: git diff

Modifico nuevamente `texto.txt`.

Elimino:

```text
tres
```

y añado:

```text
siete
```

El archivo queda:

```text
uno
dos
CUATRO
cinco
seis
siete
```

Ejecuto:

```bash
git diff
```

La salida es esta:

```diff
diff --git a/texto.txt b/texto.txt
index 445880e..7360c22 100644
--- a/texto.txt
+++ b/texto.txt
@@ -1,6 +1,6 @@
 uno
 dos
-tres
 CUATRO
 cinco
-seis
\ No newline at end of file
+seis
+siete
\ No newline at end of file
```

El símbolo `-` indica una línea eliminada y `+` una línea añadida.

Realizo el commit:

```bash
git commit -a -m "Probando diff"
```

Git informa de que se ha realizado una inserción y una eliminación.
```bash

[master d68896c] Probando diff
 1 file changed, 2 insertions(+), 2 deletions(-)
```


Ahora añado:

```text
ocho
```

al final de `texto.txt`.

Vuelvo a comprobar las diferencias:

```bash
git diff
```

Ahora aparecerá `ocho` como una nueva línea:

```diff
diff --git a/texto.txt b/texto.txt
index 7360c22..e17be5d 100644
--- a/texto.txt
+++ b/texto.txt
@@ -3,4 +3,5 @@ dos
 CUATRO
 cinco
 seis
-siete
\ No newline at end of file
+siete
+ocho
\ No newline at end of file

```

---

# 6. Ignorar archivos: .gitignore

Creo un archivo llamado `.gitignore`.

Su contenido es:

```gitignore
# ignora los archivos terminados en .class
*.class

# ignora los archivos terminados en ~
*~

# pero no importante~
!importante~
```

De esta forma Git ignorará los archivos `.class` y los archivos cuyo nombre termine en `~`, excepto `importante~`.

Creo un archivo `Hola.java`:

```java
class Hola {
    public static void main(String[] args) {
        System.out.println("Welcome to the Java World");
    }
}
```

Lo compilo:

```bash
javac Hola.java
```

Esto genera `Hola.class`, que será ignorado por Git gracias al `.gitignore`.

También creo:

```text
temporal~
importante~
```

Compruebo los archivos:

```bash
ls -a
```

Entre los archivos aparecen:

```text
./  ../  .git/  .gitignore  Hola.java  importante~  script.sh  temporal~  texto.txt
```

Compruebo el estado:

```bash
git status
```

`Hola.class` y `temporal~` no aparecen entre los archivos pendientes porque están siendo ignorados.

Sin embargo, `importante~` sí aparece debido a la excepción:

```gitignore
!importante~
```

```bash
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   texto.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        Hola.java
        importante~

no changes added to commit (use "git add" and/or "git commit -a")

```


Añado todos los archivos pendientes, que se infica con el "." :

```bash
git add .
```

Realizo un commit:

```bash
git commit -m "Añadidos fuentes en java y archivos importantes"
```

Para consultar todos los archivos almacenados en el último commit ejecuto:

```bash
git ls-tree --name-only -r HEAD
```

El resultado es este:

```text
.gitignore
Hola.java
importante~
script.sh
texto.txt
```

---

# 7. Tags y gestión de versiones

Los commits tienen identificadores hash que pueden resultar difíciles de recordar.

Para identificar una versión de forma más sencilla creo un tag para la versión actual:

```bash
git tag v0.7
```

Consulto el historial resumido:

```bash
git log --oneline
```

La salida es:

```text
04424af (HEAD -> master, tag: v0.7) Añadidos fuentes en java y archivos importantes
d68896c Probando diff
4951839 Ampliada la explicación del texxto
d2c5e7b Añadido archivo texto.txt
3164c9a Confirmación inicial
```

También creo el tag `v0.3` sobre el commit correspondiente a:

```text
Ampliada la explicación del texto
```

Primero obtengo su hash mediante:

```bash
git log --oneline
```

Después ejecuto:

```bash
git tag v0.3 4951839
```

Compruebo los tags:

```bash
git log --oneline
```
La salida es:

```bash
04424af (HEAD -> master, tag: v0.7) Añadidos fuentes en java y archivos importantes
d68896c Probando diff
4951839 (tag: v0.3) Ampliada la explicación del texxto
d2c5e7b Añadido archivo texto.txt
3164c9a Confirmación inicial
```

Puedo consultar directamente la versión `v0.3`:

```bash
git show v0.3
```

También puedo desplazarme temporalmente hasta esa versión:

```bash
git checkout v0.3
```

Para regresar a la última versión de la rama principal:

```bash
git checkout master
```

---

# 8. Ramas

Creo una nueva rama llamada `Prueba`:

```bash
git branch Prueba
```

Consulto el historial:

```bash
git log --oneline
```

La última versión está asociada tanto a `master` como a `Prueba`.

Consulto las ramas existentes:

```bash
git branch
```

Resultado:

```text
  Prueba
* master
```

El `*` indica la rama en la que estoy trabajando actualmente.

Cambio a la rama `Prueba`:

```bash
git switch Prueba
```

Compruebo nuevamente:

```bash
git branch
```

Resultado:

```text
* Prueba
  master
```

Realizo un commit en esta rama:

```bash
git commit -a -m "Rama para pruebas de código"
```

Consulto el historial:

```bash
git log --oneline
```

Ahora `HEAD` apunta a `Prueba`.

Modifico `Hola.java` añadiendo:

```java
System.out.println("I have no more branches to commit, said the Ent");
```

Realizo el commit:

```bash
git commit -a -m "Llegan los Ents a Java"
```

Creo un tag para esta versión:

```bash
git tag v0.7-Ent-Release
```

Consulto el historial:

```bash
git log --oneline
```

La rama `Prueba` contiene ahora cambios que no están presentes en `master`.

```bash
cf95fcb (HEAD -> Prueba, tag: v0.7-Ent-Release) Llegan los Ents a Java
04424af (tag: v0.7, master) Añadidos fuentes en java y archivos importantes
d68896c Probando diff
4951839 (tag: v0.3) Ampliada la explicación del texxto
d2c5e7b Añadido archivo texto.txt
3164c9a Confirmación inicial
```

Vuelvo a la rama principal:

```bash
git switch master
```

Compruebo el contenido de `Hola.java` y veo que la nueva línea no aparece porque fue añadida únicamente en la rama `Prueba`.

Finalmente fusiono los cambios de `Prueba` con `master`:

```bash
git merge Prueba
```

De esta forma los cambios realizados en la rama de pruebas pasan a formar parte de la rama principal.

```bash
Updating 04424af..cf95fcb
Fast-forward
 Hola.java | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```
---
# 9. Eliminar y quitar de seguimiento

Elimino el archivo `importante~`.

```bash
git rm importante~
```

Para quitar los archivos en seguimiento, como los `.class`:

```bash
git reset HEAD *.class
```

# 10. Repositorio remoto con GitHub

Creo un nuevo repositorio en GitHub para almacenar esta práctica.

Una vez creado, enlazo mi repositorio local con el repositorio remoto.

```bash
git remote add origin URL_DE_MI_REPOSITORIO
```

Por ejemplo:

```text
https://github.com/USUARIO/prueba_git.git
```

Antes de subir los archivos puedo descargar y sincronizar los cambios que existan en el repositorio remoto.

```bash
git pull origin master
```

Finalmente envío mi rama local al repositorio remoto:

```bash
git push -u origin master
```

La opción `-u` permite asociar la rama local `master` con la rama remota correspondiente.

Después de realizar esta asociación, las siguientes actualizaciones se pueden enviar simplemente mediante:

```bash
git push
```

Para descargar cambios del repositorio remoto puedo utilizar:

```bash
git pull
```

---

# 10. Clonar un repositorio

Git también permite obtener una copia local completa de un repositorio remoto mediante `clone`.

La sintaxis es:

```bash
git clone URL_REPOSITORIO
```

Por ejemplo:

```bash
git clone https://github.com/USUARIO/prueba_git.git
```

Esto crea en el equipo una copia del repositorio remoto, incluyendo sus archivos y su historial de versiones.

---

# Conclusión

Durante esta práctica he utilizado Git para gestionar las distintas versiones de un proyecto.

He utilizado los comandos:

- `git init` para crear un repositorio.
- `git status` para consultar su estado.
- `git add` para preparar archivos.
- `git commit` para crear versiones.
- `git log` y `git show` para consultar el historial.
- `git diff` para comparar modificaciones.
- `.gitignore` para excluir archivos.
- `git tag` para identificar versiones.
- `git branch` y `git switch` para trabajar con ramas.
- `git merge` para fusionar ramas.
- `git push` y `git pull` para sincronizar el repositorio local con GitHub.
- `git clone` para obtener una copia de un repositorio remoto.
