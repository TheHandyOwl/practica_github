# practica_github

- ¿Qué comando utilizaste en el paso 11? ¿Por qué?

```$ git reset --hard HEAD~1
HEAD is now at 1489c3e Primer git nuestro
```

El enunciado ‘perdiendo los cambios realizados en el working copy’ nos indica que queremos deshacernos de los avances del último commit. Es decir, dejamos todo como estaba en el commit anterior, perdiendo o descartando cualquier cambio que hiciesemos en el último commit y en el working area.
Así que ‘git reset’ con la opción ‘--hard’ y para que sea el último añadimos ‘HEAD~1’

- ¿Qué comando o comandos utilizaste en el paso 12? ¿Por qué?

```$ git reflog
1489c3e HEAD@{0}: reset: moving to HEAD~1
2c3a6a1 HEAD@{1}: commit: Modifico git-nuestro por primera vez
1489c3e HEAD@{2}: checkout: moving from master to styled
1489c3e HEAD@{3}: commit (initial): Primer git nuestro

$ git merge 2c3a6a1
Updating 1489c3e..2c3a6a1
Fast-forward
 git-nuestro.md | 19 +++++++++----------
 1 file changed, 9 insertions(+), 10 deletions(-)
- El merge del paso 13, ¿Causó algún conflicto? ¿Por qué?
No causó conflicto, porque master no ha sido modificada, y styled sí.
Pero styled está modificada con master como base y, por tanto, prevalecen los cambios de styled. Si hubiéramos modificado alguna línea de master sí que se hubiera generado conflicto.
$ git merge master
Already up-to-date.
```

Un merge fast-forward con el commit que hemos descartado, para avanzar la rama styled y posicionarla en el commit que ya teníamos creado pero que habíamos perdido.
Para estar seguro de cuál era el commit que buscaba, he utilizado ‘reflog’

- El merge del paso 19, ¿Causó algún conflicto? ¿Por qué?

Sí hay conflicto.
Cada rama realiza cambios desde el mismo padre, se están cambiando las mismas líneas ramas distintas generando el conflicto.

```$ git merge htmlify
Auto-merging git-nuestro.md
CONFLICT (content): Merge conflict in git-nuestro.md
Automatic merge failed; fix conflicts and then commit the result.
```

- El merge del paso 21, ¿Causó algún conflicto? ¿Por qué?

No hay conflicto.
La rama base styled partió de master, y master no se ha modificado en ningún momento. La rama style ha avanzado, y ahora master avanza con el fast-forward hasta ‘replicar’ lo que ha hecho la rama styled.

```$ git merge styled
Updating 1489c3e..ad329e3
Fast-forward
 git-nuestro.md | 19 +++++++++----------
 1 file changed, 9 insertions(+), 10 deletions(-)
```

- ¿Qué comando o comandos utilizaste en el paso 25?

He utilizado 2:
Personalizar alias para ahorrar modificadores
Ejecutar el comando personalizado

```$ git config alias.graph "log --graph --decorate --pretty=oneline"

$ git graph
*   ad329e3e662d5016440dc2f2f0867976dcc698ca (HEAD -> master, styled) Merge branch 'htmlify' into styled
|\
| * b756336cb4977ed8c7c379f37358bdb19866ccb9 (htmlify) Modificamos git-nuestro.md con código HTML, pero mantiene extensión .md
* | 2c3a6a1c1510684f54a2632e6e39a71451394877 Modifico git-nuestro por primera vez
|/
* 1489c3e964f61dd5122ab87184d1919d7eb2fda6 Primer git nuestro
```

- El merge del paso 26, ¿Podría ser fast forward? ¿Por qué?

Sí se podría hacer un fast-forward porque title parte de la rama master, hace sus modificaciones, y ahora la rama master sube hasta igualar la rama title.
Si hacemos un git diff title veremos que tan sólo cambia el título.

```$ git diff title
diff --git a/git-nuestro.md b/git-nuestro.md
index bc50ea9..b09c8eb 100644
--- a/git-nuestro.md
+++ b/git-nuestro.md
@@ -1,4 +1,3 @@
-# GITER NOSTER
 *Git* nuestro que estas en los repos
 Comprimidos sean tus *commits*
 Venga a nosotros tu *log*

$ git merge --no-ff title
unix2dos: converting file C:/Users/Carlos/git/practica_github/.git/MERGE_MSG to DOS format...
dos2unix: converting file C:/Users/Carlos/git/practica_github/.git/MERGE_MSG to Unix format...
Merge made by the 'recursive' strategy.
 git-nuestro.md | 1 +
 1 file changed, 1 insertion(+)
```

- ¿Qué comando o comandos utilizaste en el paso 27?

En este caso nos olvidamos de --hard para mantener los cambios en el working copy

```$ git reset HEAD~1
Unstaged changes after reset:
M       git-nuestro.md
```

- ¿Qué comando o comandos utilizaste en el paso 28?

He recuperado la ultima version del repositorio

```$ git checkout -- git-nuestro.md

$ git status
On branch master
nothing to commit, working tree clean
```

- ¿Qué comando o comandos utilizaste en el paso 29?

Primero le he preguntado si me dejaba borrar la rama y decía que quedaba huérfana. Así que he forzado el borrado

```$ git branch -d title
error: The branch 'title' is not fully merged.
If you are sure you want to delete it, run 'git branch -D title'.

$ git branch -D title
Deleted branch title (was 7de0495).
- ¿Qué comando o comandos utilizaste en el paso 30?
Buscar con reflog, vemos que la diferencia está en el título, y un merge para volver al commit del merge, que es un fast-forward.

$ git reflog
ad329e3 HEAD@{0}: reset: moving to HEAD~1
2a595bb HEAD@{1}: merge title: Merge made by the 'recursive' strategy.
ad329e3 HEAD@{2}: checkout: moving from title to master
7de0495 HEAD@{3}: commit: Añadimos título GITER NOSTER
ad329e3 HEAD@{4}: checkout: moving from master to title

$ git diff 2a595bb
diff --git a/git-nuestro.md b/git-nuestro.md
index bc50ea9..b09c8eb 100644
--- a/git-nuestro.md
+++ b/git-nuestro.md
@@ -1,4 +1,3 @@
-# GITER NOSTER
 *Git* nuestro que estas en los repos
 Comprimidos sean tus *commits*
 Venga a nosotros tu *log*

$ git merge 2a595bb
Updating ad329e3..2a595bb
Fast-forward
 git-nuestro.md | 1 +
 1 file changed, 1 insertion(+)
```

- ¿Qué comando o comandos usaste en el paso 32?

Buscar el commit con reflog y con checkout hasta al inicial.

```$ git reflog
2a595bb HEAD@{0}: merge 2a595bb: Fast-forward
ad329e3 HEAD@{1}: reset: moving to HEAD~1
2a595bb HEAD@{2}: merge title: Merge made by the 'recursive' strategy.
ad329e3 HEAD@{3}: checkout: moving from title to master
7de0495 HEAD@{4}: commit: Añadimos título GITER NOSTER
ad329e3 HEAD@{5}: checkout: moving from master to title
ad329e3 HEAD@{6}: merge styled: Fast-forward
1489c3e HEAD@{7}: checkout: moving from styled to master
ad329e3 HEAD@{8}: commit (merge): Merge branch 'htmlify' into styled
2c3a6a1 HEAD@{9}: checkout: moving from htmlify to styled
b756336 HEAD@{10}: commit: Modificamos git-nuestro.md con código HTML, pero mantiene extensión .md
1489c3e HEAD@{11}: checkout: moving from master to htmlify
1489c3e HEAD@{12}: checkout: moving from styled to master
2c3a6a1 HEAD@{13}: merge 2c3a6a1: Fast-forward
1489c3e HEAD@{14}: reset: moving to HEAD~1
2c3a6a1 HEAD@{15}: commit: Modifico git-nuestro por primera vez
1489c3e HEAD@{16}: checkout: moving from master to styled
1489c3e HEAD@{17}: commit (initial): Primer git nuestro

$ git checkout 1489c3e
Note: checking out '1489c3e'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 1489c3e... Primer git nuestro
```

- ¿Qué comando o comandos usaste en el punto 33?

Podríamos tirar de reflog, pero tenemos la rama master en el último commit, desde el que hemos saltado al commit inicial.
Usamos git graph para ver que el id es el commit del último merge

```$ git checkout master
Previous HEAD position was 1489c3e... Primer git nuestro
Switched to branch 'master'

$ git graph
*   2a595bbbd8fee9066c1c8d9a04fdab5f862f477c (HEAD -> master) Merge branch 'title'
|\
| * 7de04952f2b8043de659ac0f3330f3b20439616a Añadimos título GITER NOSTER
|/
*   ad329e3e662d5016440dc2f2f0867976dcc698ca Merge branch 'htmlify' into styled
|\
| * b756336cb4977ed8c7c379f37358bdb19866ccb9 Modificamos git-nuestro.md con código HTML, pero mantiene extensión .md
* | 2c3a6a1c1510684f54a2632e6e39a71451394877 Modifico git-nuestro por primera vez
|/
* 1489c3e964f61dd5122ab87184d1919d7eb2fda6 Primer git nuestro
```