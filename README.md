
Los pasos a ejecutar son los siguientes (los pasos en negrita indican que hay una pregunta
asociada):
1) Crear un repositorio
git init 
git config --global user.name "José Ramón Alonso"
git config --global user.email jose.alonso.programmer@gmail.com
vi .gitignore
git add .gitignore
git commit -m "Creado el fichero .gitignore" .gitignore

2) Crear un archivo git-nuestro.md con el contenido:
vi git-nuestro.md 

3) Añadir git-nuestro.md al staging area
git add git-nuestro.md

4) Mover lo que hay en el staging area al repositorio
git commit -m "Creado el archivo git-nuestro.md" git-nuestro.md
git commit -a es equivalente a utilizar el comando git add en todos los archivos que hubo en el último commit, y correr luego el comando git commit.

5) Crear una rama llamada “styled”
git branch styled

6) Listar las ramas que hay en el repositorio
git branch 

7) Moverse a la rama “styled”
git checkout styled

8) Comprobar que se está en la rama correcta
git branch

9) Modificar en el archivo git-nuestro.md:
vi git-nuestro.md 

10) Añadir los cambios al staging área y luego pasarlos al repositorio
git add git-nuestro.md
git commit -m "Fichero modificado" git-nuestro.md

11) Deshacer el último commit (perdiendo los cambios realizados en el working copy)

git reset --hard HEAD~1

El comando "reset" mueve el puntero HEAD a donde indica el argumento; en este caso, HEAD~1 significa la penúltima acción, nodo o "commit". El modificador "--hard" indica que se altera también el espacio de trabajo.

12) Rehacer el último commit (el que acabamos de deshacer)

git reflog y se busca el id correspondiente al commit del paso 10 ---> a83298a HEAD@{2}: commit: Fichero modificado
git reset --hard a83298a

El argumento de "reset" no es un desplazamiento explícito del puntero, como en el punto anterior, sino el identificador SHA de un nodo determinado.

13) Hacer un merge con ‘master’ (styled absorbe a master)

git merge master 
Mensaje obtenido: "Already up-to-date."

Este comando no borra ninguna rama ni crea ningún nodo. No hay conflicto, ya que los nodos de la rama "master" forman parte de la rama "styled". Las dos ramas forman una lista, no hay ninguna ramificación, por tanto las ramas no se pueden fusionar.

14) Volver a la rama master
git checkout master

15) Crear una nueva rama llamada “htmlify”
git branch htmlify

16) Cambiar a la rama htmlify
git checkout htmlify
Un solo comando para los dos pasos: git checkout -b htmlify

IMPORTANTE --->
El resultado de "git log" depende de la rama.
El resultado de "git reflog" es el mismo en cualquier rama.

17) Modificar en el archivo git-nuestro.md:
vi git-nuestro.md

18) Hacer un commit
git add git-nuestro.md
git commit -m "Fichero modificado con etiquetas HTML" git-nuestro.md

19) Hacer un merge de “htmlify” en “styled” (styled absorbe a htmlify)

git checkout styled
git merge htmlify
---> CONFLICT (content): Merge conflict in git-nuestro.md
Automatic merge failed; fix conflicts and then commit the result.

Sí hay conflictos, porque el mismo fichero tiene contenidos diferentes. En la rama "styled" está en formato Markdown, pero en  la rama "htmlify" está en formato HTML. El nodo creado como resultado de la fusión de ramas tiene dos padres, uno de la rama "styled" y el otro, de la rama "htmlify".

20) Si hay conflictos, deberemos resolverlos quedándonos con el contenido de la rama “styled”.
Editamos el fichero y resolvemos el conflicto.
git add git-nuestro.md y git commit -m "Fichero modificado para resolver los conflictos del paso 19" 
Si se da un nombre de fichero, da error:
fatal: cannot do a partial commit during a merge.

21) Desde “master”, hacer un merge con “styled”
git checkout master
git merge styled

22) Crear una rama “title” y cambiarse a esa rama
git checkout -b title

23) Añadir un título (a tu gusto) al archivo git-nuestro.md y hacer un commit.
vi git-nuestro.md
git add git-nuestro.md
git commit -m "Titulo agregado al fichero" git-nuestro.md
Con un solo comando: git commit -a -m "Titulo agregado al fichero"

24) Volver a la rama master
git checkout master

25) Dibujar el diagrama
git log --graph --decorate --pretty=oneline

26) Hacer un merge “no fast-forward” de “title” en “master” (master absorbe a title)
git merge --no-ff title
Se abre el editor para que introduzcamos el mensaje asociado a la fusión.

27) Deshacer el merge (sin perder los cambios del working copy)
git reset HEAD~1

28) Descartar los cambios
git checkout -- git-nuestro.md

29) Eliminar la rama “title”
git branch -d title 
    error: The branch 'title' is not fully merged.
    If you are sure you want to delete it, run 'git branch -D title'.

Nos advierte de que al borrar la rama, dejaremos un nodo huérfano.
git branch -D title

30) Rehacer el merge que hemos deshecho
Gracias al histórico de comandos de git ("reflog"), podemos recuperar el nodo que quitamos en el punto anterior.

git reflog ---> c1d169e HEAD@{1}: merge title: Merge made by the 'recursive' strategy.
git reset --hard c1d169e

31) Volver a master y eliminar el resto de ramas
git checkout master
git branch -d styled
git branch -d htmlify
git branch -d title

32) Volver al commit inicial cuando se creó el poema
git reflog ---> 23e419a HEAD@{27}: commit: Creado el archivo git-nuestro.md
git reset --hard 23e419a 

33) Volver al estado final, cuando pusimos título al poema
git reflog ---> 1e2d7a8 HEAD@{5}: commit: Titulo agregado al fichero
git reset --hard 1e2d7a8 

34) Crear los siguientes tags:
inicial: en el primer commit
styled: modificación del paso 10
htmlify: modificación del paso 17-18
title: modificación del paso 30

git reflog ---> Busco la acción que me interese
git reset --hard id ---> Me sitúo en el nodo a etiquetar
git tag tag_name ---> Creo la etiqueta pedida.

31) Ir al tag htmlify
git reset --hard htmlify

No puede haber ramas y tags con el mismo nombre porque tanto unos como otros son punteros del grafo
jose-200-5410es% git blame git-nuestro.md 
1e2d7a86 (José Ramón Alonso Tapia 2017-06-24 19:20:37 +0200  1) * Titulo del poema
1e2d7a86 (José Ramón Alonso Tapia 2017-06-24 19:20:37 +0200  2) 
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200  3) Git nuestro
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200  4) *Git* nuestro que estas en los repos
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200  5) Comprimidos sean tus *commits*
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200  6) Venga a nosotros tu *log*
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200  7) En el local como en el *remote*
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200  8) Danos hoy nuestro *pull* de cada dia
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200  9) Perdona nuestros *conflictos*
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200 10) Como tambien perdonamos los de otros ge
a83298a8 (José Ramón Alonso Tapia 2017-06-24 13:06:53 +0200 11) No nos dejes caer en detached HEAD
a83298a8 (José Ramón Alonso Tapia 2017-06-24 13:06:53 +0200 12) y libranos de SVN
88552841 (José Ramón Alonso Tapia 2017-06-24 18:06:58 +0200 13) Venga a nosotros tu *log*
88552841 (José Ramón Alonso Tapia 2017-06-24 18:06:58 +0200 14) En el local como en el *remote*
88552841 (José Ramón Alonso Tapia 2017-06-24 18:06:58 +0200 15) Danos hoy nuestro *pull* de cada dia
88552841 (José Ramón Alonso Tapia 2017-06-24 18:06:58 +0200 16) Perdona nuestros *conflictos*
88552841 (José Ramón Alonso Tapia 2017-06-24 18:06:58 +0200 17) Como tambien perdonamos los de otros ge
88552841 (José Ramón Alonso Tapia 2017-06-24 18:06:58 +0200 18) No nos dejes caer en *detached HEAD*
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200 19) y libranos de *SVN*
23e419a0 (José Ramón Alonso Tapia 2017-06-24 13:02:25 +0200 20) `git commit --ammend`

- git reflog master@{two.days.ago} ----> Muy útil
- Cherry picking in git means to choose a commit from one branch and apply it onto another.
This is in contrast with other ways such as merge and rebase which normally applies many commits onto a another branch.

Make sure you are on the branch you want apply the commit to.

git checkout master
Execute the following:
git cherry-pick <commit-hash>

-------------------------------------------------------

# KC-Modulo-de-Git

Ejercicio 1

Aclaración: se usan indistintamente las palabras "commit", acción y nodo.

**¿Qué comando utilizaste en el paso 11? ¿Por qué?**
11) Deshacer el último commit (perdiendo los cambios realizados en el working copy)

git reset --hard HEAD~1

El comando "reset" mueve el puntero HEAD a donde indica el argumento; en este caso, HEAD~1 significa la penúltima acción, nodo o "commit". El modificador "--hard" indica que se altera también el espacio de trabajo.

**¿Qué comando o comandos utilizaste en el paso 12? ¿Por qué?**
12) Rehacer el último commit (el que acabamos de deshacer)

git reflog y se busca el id correspondiente al commit del paso 10 ---> a83298a HEAD@{2}: commit: Fichero modificado

git reset --hard a83298a

El argumento de "reset" no es un desplazamiento explícito del puntero, como en el punto anterior, sino el identificador SHA de un nodo determinado.

**El merge del paso 13, ¿Causó algún conflicto? ¿Por qué?**
13) Hacer un merge con ‘master’ (styled absorbe a master)

git merge master
Mensaje obtenido: "Already up-to-date."

Este comando no borra ninguna rama ni crea ningún nodo. No hay conflicto, ya que los nodos de la rama "master" forman parte de la rama "styled". Las dos ramas forman una lista, no hay ninguna ramificación, por tanto las ramas no se pueden fusionar.

**El merge del paso 19, ¿Causó algún conflicto? ¿Por qué?**
19) Hacer un merge de “htmlify” en “styled” (styled absorbe a htmlify)

git checkout styled
git merge htmlify
---> CONFLICT (content): Merge conflict in git-nuestro.md
Automatic merge failed; fix conflicts and then commit the result.

Sí hay conflictos, porque el mismo fichero tiene contenidos diferentes. En la rama "styled" está en formato Markdown, pero en  la rama "htmlify" está en formato HTML. El nodo creado como resultado de la fusión de ramas tiene dos padres, uno de la rama "styled" y el otro, de la rama "htmlify".

20) Si hay conflictos, deberemos resolverlos quedándonos con el contenido de la rama “styled”.

Editamos el fichero y resolvemos el conflicto.

git add git-nuestro.md y git commit -m "Fichero modificado para resolver los conflictos del paso 19" 

Si se da un nombre de fichero, da error:
fatal: cannot do a partial commit during a merge.

**El merge del paso 21, ¿Causó algún conflicto? ¿Por qué?**
21) Desde “master”, hacer un merge con “styled”

git checkout master
git merge styled

No hay conflicto, porque los nodos de las ramas forman una lista, al igual que en el punto 13.

**¿Qué comando o comandos utilizaste en el paso 25?**
25) Dibujar el diagrama

git config alias.graph 'log --graph --decorate --pretty=oneline' 
git graph 

**El merge del paso 26, ¿Podría ser fast forward? ¿Por qué?**
26) Hacer un merge “no fast-forward” de “title” en “master” (master absorbe a title)

git merge --no-ff title

La fusión "fast-forward" implica que el puntero HEAD de "master" pase a apuntar al puntero HEAD de "title". Como los nodos de "master" y de "title" forman una lista, la fusión podría haber sido de este tipo.
En general, la fusión "fast-forward" no es posible si el último nodo de la rama actual no es padre de la rama que se quiere fusionar. Git realizaría una fusión a tres bandas, utilizando los dos nodos apuntados por el extremo de cada una de las ramas y por el ancestro común a ambas. Y crearía un nodo con más de un padre. 
**¿Qué comando o comandos utilizaste en el paso 27?**
27) Deshacer el merge (sin perder los cambios del working copy)

git reset HEAD~1

**¿Qué comando o comandos utilizaste en el paso 28?**
28) Descartar los cambios

git checkout -- git-nuestro.md

**¿Qué comando o comandos utilizaste en el paso 29?**
29) Eliminar la rama “title”

git branch -d title 
    error: The branch 'title' is not fully merged.
    If you are sure you want to delete it, run 'git branch -D title'.

Nos advierte de que al borrar la rama, dejaremos un nodo huérfano.

30) Rehacer el merge que hemos deshecho

**¿Qué comando o comandos utilizaste en el paso 30?**
30) Rehacer el merge que hemos deshecho

Gracias al histórico de comandos de git ("reflog"), podemos recuperar el nodo que quitamos en el punto anterior.

git reflog ---> c1d169e HEAD@{1}: merge title: Merge made by the 'recursive' strategy.
git reset --hard c1d169e

**¿Qué comando o comandos usaste en el paso 32?**
32) Volver al commit inicial cuando se creó el poema

git reflog ---> 23e419a HEAD@{27}: commit: Creado el archivo git-nuestro.md
git reset --hard 23e419a 

**¿Qué comando o comandos usaste en el punto 33?**
33) Volver al estado final, cuando pusimos título al poema

git reflog ---> 23e419a HEAD@{27}: commit: Creado el archivo git-nuestro.md
git reset --hard 23e419a 


echo "# KC-Modulo-de-Git" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/josealonso/KC-Modulo-de-Git.git
git push -u origin master

