
-------------------------------------------------------
# KC-Modulo-de-Git

**EJERCICIO 1**

Aclaración: se usan indistintamente las palabras "commit", acción y nodo.

**¿Qué comando utilizaste en el paso 11? ¿Por qué?**

11) Deshacer el último commit (perdiendo los cambios realizados en el working copy)

```git reset --hard HEAD~1```

El comando "reset" mueve el puntero HEAD a donde indica el argumento; en este caso, HEAD~1 significa la penúltima acción, nodo o "commit". El modificador "--hard" indica que se altera también el espacio de trabajo.

**¿Qué comando o comandos utilizaste en el paso 12? ¿Por qué?**

12) Rehacer el último commit (el que acabamos de deshacer)

```git reflog``` y se busca el id correspondiente al commit del paso 10 ---> 

```a83298a HEAD@{2}: commit: Fichero modificado```

```git reset --hard a83298a```

El argumento de "reset" no es un desplazamiento explícito del puntero, como en el punto anterior, sino el identificador SHA de un nodo determinado.

**El merge del paso 13, ¿Causó algún conflicto? ¿Por qué?**

13) Hacer un merge con ‘master’ (styled absorbe a master)

```git merge master```

Mensaje obtenido: 
```Already up-to-date```

Este comando no borra ninguna rama ni crea ningún nodo. No hay conflicto, ya que los nodos de la rama "master" forman parte de la rama "styled". Las dos ramas forman una lista, no hay ninguna ramificación, por tanto las ramas no se pueden fusionar.

**El merge del paso 19, ¿Causó algún conflicto? ¿Por qué?**

19) Hacer un merge de “htmlify” en “styled” (styled absorbe a htmlify)

```git checkout styled```

```git merge htmlify```
---> ```CONFLICT (content): Merge conflict in git-nuestro.md
Automatic merge failed; fix conflicts and then commit the result.```

Sí hay conflictos, porque el mismo fichero tiene contenidos diferentes. En la rama "styled" está en formato Markdown, pero en  la rama "htmlify" está en formato HTML. El nodo creado como resultado de la fusión de ramas tiene dos padres, uno de la rama "styled" y el otro, de la rama "htmlify".

20) Si hay conflictos, deberemos resolverlos quedándonos con el contenido de la rama “styled”.

Editamos el fichero y resolvemos el conflicto.

```git add git-nuestro.md```

```git commit -m "Fichero modificado para resolver los conflictos del paso 19"```

Si se da un nombre de fichero, da error:
```fatal: cannot do a partial commit during a merge```

**El merge del paso 21, ¿Causó algún conflicto? ¿Por qué?**

21) Desde “master”, hacer un merge con “styled”

```git checkout master```

```git merge styled```

No hay conflicto, porque los nodos de las ramas forman una lista, al igual que en el punto 13.

**¿Qué comando o comandos utilizaste en el paso 25?**

25) Dibujar el diagrama

```git config alias.graph 'log --graph --decorate --pretty=oneline' ```

```git graph ```

**El merge del paso 26, ¿Podría ser fast forward? ¿Por qué?**

26) Hacer un merge “no fast-forward” de “title” en “master” (master absorbe a title)

```git merge --no-ff title```

La fusión "fast-forward" implica que el puntero HEAD de "master" pase a apuntar al puntero HEAD de "title". Como los nodos de "master" y de "title" forman una lista, la fusión podría haber sido de este tipo.
En general, la fusión "fast-forward" no es posible si el último nodo de la rama actual no es padre de la rama que se quiere fusionar. Git realizaría una fusión a tres bandas, utilizando los dos nodos apuntados por el extremo de cada una de las ramas y por el ancestro común a ambas. Y crearía un nodo con más de un padre. 

**¿Qué comando o comandos utilizaste en el paso 27?**

27) Deshacer el merge (sin perder los cambios del working copy)

```git reset HEAD~1```

**¿Qué comando o comandos utilizaste en el paso 28?**

28) Descartar los cambios

```git checkout -- git-nuestro.md```

**¿Qué comando o comandos utilizaste en el paso 29?**

29) Eliminar la rama “title”

```git branch -d title```

```error: The branch 'title' is not fully merged. If you are sure you want to delete it, run 'git branch -D title'```

Nos advierte de que al borrar la rama, dejaremos un nodo huérfano.

**¿Qué comando o comandos utilizaste en el paso 30?**

30) Rehacer el merge que hemos deshecho

Gracias al histórico de comandos de git ("reflog"), podemos recuperar el nodo que quitamos en el punto anterior.

```git reflog ---> c1d169e HEAD@{1}: merge title: Merge made by the 'recursive' strategy.```

```git reset --hard c1d169e```

**¿Qué comando o comandos usaste en el paso 32?**

32) Volver al commit inicial cuando se creó el poema

```git reflog ---> 23e419a HEAD@{27}: commit: Creado el archivo git-nuestro.md```

```git reset --hard 23e419a ```

**¿Qué comando o comandos usaste en el punto 33?**

33) Volver al estado final, cuando pusimos título al poema

```git reflog ---> 23e419a HEAD@{27}: commit: Creado el archivo git-nuestro.md```

```git reset --hard 23e419a ```

