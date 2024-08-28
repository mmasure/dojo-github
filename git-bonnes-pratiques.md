# Git bonnes pratiques

## 1. Branching

### 1.1 - Introduction

Le `branching` est un concept primordial pour l'utilisation de `Git`. Nous allons voir ensemble dans cette documentation l'utilité de ces dernières.

Une branche est un pointeur sur une version spécifique de votre `repository`. Chaque `branche` contient un état différent du travail effectué sur votre projet. Plusieurs personnes peuvent travailler sur la même `branche`.

Généralement, il existe deux branches principales : `master` et `develop`. Elles sont présentes tout au long de votre projet. Il est cependant vivement conseillé d'avoir d'autres branches. Nous allons voir plus tard qu' ajouter des branches contextuelles peut aider pour faciliter la gestion et la maintenabilité de votre projet.

Le `branching` vous permet aussi de mettre en place des processus de `CI/CD`. Vous pouvez brancher des actions à effectuer lors de modifications sur certaines branches. Par exemple, vous pouvez lancer de la `CI` lorsqu'un `commit` est poussé sur une branche de `release` ou alors un processus de `CD` lors d'actions sur la branche `master`.

Vous pouvez créer autant de branches que vous le souhaitez. Vous pouvez aussi protéger des branches pour éviter certains changements, suppressions, push forcé...

Il est recommandé d'utiliser une nomenclature de nommage pour les branches afin de garantir une meilleure compréhension sur votre projet.

### 1.2 - Avantages

Le principal avantage d'utiliser des branches est d'isoler votre travail des autres. Par exemple, vous pouvez travailler sur une fonctionnalité pendant que votre collègue résout un bug sans impacter votre travail et la production. Les branches permettent d'utiliser les `merge` et donc de fusionner le travail d'une branche dans une autre. Le concept de `branching` permet aussi des processus tels que les `merge requests` par exemple (voir ici).

### 1.3 - Les branches

#### **1.3.1 - Master**

`Master` est la branche principale du projet et représente l'état actuel de l'environnement de production.

Vous ne devez jamais pousser directement des `commits` sur cette branche en effectuant un `push` indépendant. Tous les `commits` présents sur cette branche doivent provenir d'une action de `merge` d'une branche `release` ou `hotfix`.

Lorsque vous modifiez le contenu de cette branche, vous devez taguer une nouvelle version de votre branche. En effet, étant donné que la branche `master` est la représentation de l'état actuel de votre projet en production, vous devez spécifier que votre version de production a évolué. Grace à ces `tags`, vous pouvez facilement revenir à une version antérieure si un bug venait à être découvert sur l'environnement de production. Cela assure aussi, une meilleure traçabilité sur votre projet.

**1.3.2 Develop**

`Develop` est la branche où toutes les nouvelles fonctionnalités ou les corrections de bugs sont poussés. Elle contient donc les dernières évolutions ou corrections de bugs apportés.

Comme pour `master`, vous ne devez jamais pousser directement des `commits` sur cette branche en effectuant un `push` indépendant. Tous les `commits` présents sur cette branche doivent provenir des branches `feature` ou `bugfix`, `release` ou encore `hotfix`.

**1.3.3 Hotfix**

Les branches `hotfix` sont utilisées lorsque qu'un bug majeur est découvert sur l'environnement de production. Vous devez donc créer une branche provenant de la dernière version de `master`

Lorsque le bug est corrigé et prêt à être intégré dans l'environnement de production, vous devez effectuer un `merge` de votre correction dans les branches `master` et `develop`.

Après avoir effectué un `merge` avec `master`, vous devez taguer la nouvelle version de l'état de production.

N'utilisez pas ces branches pour effectuer des corrections de bugs mineurs.

**1.3.4 Release**

Les branches de `release` sont utilisées pour regrouper les nouvelles fonctionnalités et corrections de bugs à déployer sur l'environnement de production. Ces branches sont créées depuis `develop` et doivent être incluses dans la branche `master`.

Sur ces branches, vous pouvez pousser des `commits` indépendants. Cependant, les modifications doivent uniquement être des corrections de bugs pour cette `release`. Vous devez rapatrier ces corrections dans `develop` (attention à l'historique dans ce cas).

**1.3.5 Feature/bugfix**

Ces branches sont l'endroit où les développeurs passent la majorité de leur temps. C'est aussi l'endroit où les fonctionnalités et les corrections de bugs mineurs naissent. Pour créer ce type de branche, vous devez les créer depuis `develop`. Lorsque vous avez terminé, vous devez effectuer un `merge` de la branche dans `develop`.

**1.3.6 Résumé**

|                        | feature/bugfix      | develop | release            | hotfix             | main     |
| ---------------------- | ------------------- | ------- | ------------------ | ------------------ | ---------- |
| main/contextual branch | contextual          | main    | contextual         | contextual         | main       |
| created from           | develop             | :x:     | develop            | main             | :x:        |
| merge back into        | develop             | :x:     | master and develop | master and develop | :x:        |
| prefix                 | feature/ ou bugfix/ | :x:     | release/           | hotfix/            | :x:        |
| environment            | dev                 | dev     | staging            | staging            | production |

## 2 - Gitflow

### 2.1 - Schéma

Le `gitflow` représente la manière de travailler sur un projet `Git`. Il décrit les processus à suivre lors de création de branches, fonctionnalités, corrections de bugs... Vous devez suivre ce schéma pour contribuer à un projet. Cette méthode tient compte de toutes les phases du cycle de vie d'un projet. Nous allons l'expliquer dans le paragraphe suivant.

### 2.2 - Workflow

**2.2.1 Créer une fonctionnalité ou une correction de bug**

Quand vous souhaitez ajouter une nouvelle fonctionnalité ou corriger un bug, vous devez créer une nouvelle branche. Ne poussez jamais une amélioration sur une autre branche que ces dernières.

Ces branches sont créées depuis `develop`. Vous devez préfixer le nom des branches avec `feature/` ou `bugfix/`. Le nom de votre branche doit être compréhensible par quelqu'un d'autre (ne nommez pas votre branche avec le numéro de ticket par exemple).

Quand la fonctionnalité/correction du bug est terminée, vous devez effectuer un `fast-forward merge` dans `develop` et regrouper les `commits` en un seul. De cette manière l'historique du projet reste clair et vous pouvez voir en un coup d’œil les fonctionnalités et les corrections de bugs ajoutées.

**Exemple (nouvelle fonctionnalité)**

Créez la branche de fonctionnalité avec le préfixe `feature/`.

```shell
$ git checkout -b feature/product-naming-validation develop
```

Poussez le travail effectué.

```shell
$ git commit -m "Add the first, second and third file"
$ git commit -m "Add the last file"
# Only if needed
$ git push origin feature/product-naming-validation
```

Une fois votre fonctionnalié terminée, vous devez effectuer un `merge` dans `develop`. Mais si vous effectuez un `merge` "classique", deux `commits` vont apparaître si les deux branches n'ont pas divergé ou trois `commits` si les branches ont divergé (les deux `commits` de la fonctionnalité et le `merge commit`). Si vous faites ça, vous polluez l'historique du projet et ce n'est pas une bonne pratique.

Donc, avant de faire le `merge`, vous devez effectuer un rebase interactif de votre branche de fonctionnalité avec la branche `develop` pour ré-écrire votre historique et réunir vos commits en un seul. La commande `git merge-base <branch> <branch>` peut être utile pour connaître le moment où les branches se sont séparées et donc l'endroit où commencer le `rebase`.

Récupérer le premier commit à partir duquel exécuter le `rebase`.

```shell
$ git merge-base feature/product-naming-validation develop
2d7629ed15276420d908273af43d4410df46723c # This is the sha1 of the commit from which you have to start the rebase
```

Effectuez un `rebase` interactif depuis ce commit avec `develop`.

```shell
$ git rebase -i 2d7629ed15206420d908273af23d1410df46723c
```

Un shell interactif va s'ouvrir.

```shell
pick deb0a9e Add the first, second and third file
pick 7b1eab6 Add the last file

# Rebase 2d7629e..7b1dab6 onto 2d7629e (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
#       However, if you remove everything, the rebase will be aborted.
#
#
# Note that empty commits are commented out
```

Retravaillez l'historique de vos `commits` en suivant la documentation de la commande. Dans ce cas, nous ré-écrivons le message du premier `commit` et regroupons les deux `commits` en un seul.

```shell
r deb0a9e Add the first, second and third file
f 7b1eab6 Add the last file

# Rebase 2d7629e..7b1eab6 onto 2d7629e (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
#       However, if you remove everything, the rebase will be aborted.
#
#
# Note that empty commits are commented out
```

Quand vous quittez le shell, un autre shell va s'ouvrir pour ré-écrire le message du premier `commit` comme demandé au dessus (préfixé avec la lettre r)

```shell
Add the first, second and third file

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Fri Apr 26 16:21:54 2019 +0200
#
# interactive rebase in progress; onto 2d7629e
# Last command done (1 command done):
#    reword deb0a9e Add the first, second and third file
# Next command to do (1 remaining command):
#    fixup 7b1eab6 Add the last file
# You are currently editing a commit while rebasing branch 'feature/product-naming-validation' on '2d7629e'.
#
# Changes to be committed:
#       new file:   1.md
#       new file:   2.md
#       new file:   3.md
#
```

Renommez le commit en utilisant le préfixe `[FEATURE]` ou `[BUGFIX]`.

```shell
[FEATURE] Add product name validation

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Fri Apr 26 16:21:54 2019 +0200
#
# interactive rebase in progress; onto 2d7629e
# Last command done (1 command done):
#    reword deb0a9e Add the first, second and third file
# Next command to do (1 remaining command):
#    fixup 7b1eab6 Add the last file
# You are currently editing a commit while rebasing branch 'feature/product-naming-validation' on '2d7629e'.
#
# Changes to be committed:
#       new file:   1.md
#       new file:   2.md
#       new file:   3.md
#
```

Poussez les modifications (en mode forcé si vous les avez déjà poussées)

```shell
$ git push origin feature/product-naming-validation
# Only if needed
$ git push --force origin feature/product-naming-validation
```

Maintenant, vous pouvez effectuer un `fast-forward merge` de votre branche `feature` dans `develop`.

```shell
$ git checkout develop
Switched to branch 'develop'
$ git merge --ff feature/product-naming-validation
Updating 2d7629e..46bcd5c
Fast-forward
 1.md | 0
 2.md | 0
 3.md | 0
 4.md | 0
 4 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 1.md
 create mode 100644 2.md
 create mode 100644 3.md
 create mode 100644 4.md
```

Poussez le résultat sur `develop`.

```shell
$ git push origin develop
Username for 'https://gitlab.com': baziuk.vincent@gmail.com
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: To create a merge request for develop, visit:
remote:   https://gitlab.com/VincentBAZIUK/git-test/merge_requests/new?merge_request%5Bsource_branch%5D=develop
remote:
To https://gitlab.com/VincentBAZIUK/git-test.git
   2d7629e..46bcd5c  develop -> develop
```

Enfin, vous pouvez supprimer votre branche `feature`.

```shell
git branch -d origin feature/product-naming-validation
Deleted branch feature/product-naming-validation (was 46bcd5c).
```

**2.2.2 Créer une nouvelle release**

Quand vous décidez de créer une nouvelle `release` qui contient les `feature` ou `bugfix` de `develop`, vous devez créer une branche de `release` préfixée par `release/`. Cette branche peut exister plusieurs jours avant d'être fusionnée dans `master`. En effet, vous pouvez ajouter des corrections de bugs relatifs à cette `release`. Vous ne devez pas ajouter de fonctionnalités sur cette branche. Après avoir effectuer un `merge` de cette branche dans `master`, vous devez taguer votre nouvelle version de `master`. Ensuite, vous devez rapatrier les changements dans `develop`.

Créez la branche de `release` depuis `develop`.

```shell
$ git checkout -b release/1.0 develop
```

Si vous souhaitez ajouter des corrections de bugs, vous pouvez effectuer ces changements de la même manière que vous faites dans les branches de `bugfix`. Vos messages de `commits` doivent être préfixés par `[BUGFIX]`

```shell
$ git commit -m "[BUGFIX] Fix login bug in first release"
$ git commit -m "[BUGFIX] Fix bug when clicking on form register submit button"
$ git push origin release/1.0
```

Une fois que votre `release` est dans une version stable et que vous voulez la publier sur votre environnement de production, vous devez effectuer un `merge` de cette dernière dans `master`.

```shell
$ git checkout master
$ git merge --no-ff release/1.0

Merge branch 'release/1.0'

# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.

Merge made by the 'recursive' strategy.
 1.md | 0
 2.md | 0
 3.md | 0
 4.md | 0
 5.md | 0
 6.md | 0
 6 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 1.md
 create mode 100644 2.md
 create mode 100644 3.md
 create mode 100644 4.md
 create mode 100644 5.md
 create mode 100644 6.md
```

Enfin, poussez les changements dans `master` et taguer votre nouvelle version.

```shell
$ git push origin master
$ git tag -a 1.0 -m "First release"
$ git push origin 1.0
```

Si vous avez ajouté des corrections de bugs, vous devez effectuer un `fast-forward merge` dans `develop`. Mais avant, vous devez effectuer un `rebase` de la branche `release` avec `develop`.

```shell
$ git checkout release/1.0
Switched to branch 'release/1.0'

$ git rebase develop
First, rewinding head to replay your work on top of it...
Applying: [BUGFIX] Fix login bug in first release
Applying: [BUGFIX] Fix bug when clicking on form register submit button

$ git checkout develop
Switched to branch 'develop'

$ git merge --ff release/1.0
Updating c43cebc..19ed91d
Fast-forward
 5.md | 0
 6.md | 0
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 5.md
 create mode 100644 6.md

$ git push origin develop
```

Enfin, supprimez la branche de `release`.

```shell
$ git branch -d release/1.0
```

**2.2.3 Créer un hotfix**

Quand un bug majeur est détécté sur votre application de production, vous devez créer un `hotfix` pour le corriger. Pour ce faire, créez une branche `hotfix` préfixée par `hotfix/`. Quand vous avez terminé votre correction, vous devez effectuer un merge dans `master` et `develop`.

Créez la branche depuis `master`.

```shell
$ git checkout -b hotfix/1.0.1 master
```

Corrigez le bug. Les messages de `commits` doivent être préfixés par `[HOTFIX]`.

```shell
$ git commit -m "[HOTFIX] Fix maintenance mode bug"
$ git push origin hotfix/1.0.1
```

Une fois que vous avez terminé, effectuez un `merge` dans `master` puis publiez le résultat. Terminez en taguant une nouvelle version.

```shell
$ git checkout master
$ git merge --no-ff hotfix/1.0
Merge branch 'hotfix/1.0.1'

# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.

$ git push origin master
$ git tag -a 1.0.1
$ git push 1.0.1
```

Maintenant, vous devez rapatrier les changements dans `develop` en effectuant un `fast-forward merge`. Donc, avant, vous devez effectuer un `rebase` de votre branche avec `develop`.

```shell
$ git checkout hotfix/1.0.1
$ git rebase develop
$ git push --force origin hotfix/1.0.1
```

Maintenant, vous pouvez effectuer un `fast-forward merge` de la branche `hotfix` dans `develop` et supprimer la branche `hotfix`.

```shell
$ git merge --ff hotfix/1.0.1
$ git push origin develop
$ git branch -d hotfix/1.0.1
```

## 3 - Message de commit

### 3.1 - Introduction

Le message de commit est une notion très importante lorsque vous utilisez `Git`. C'est la première chose que vous voyez lorsque vous arrivez sur un projet. Il est important d'être très rigoureux quand vous écrivez vos messages. Cela permet aux autres développeurs de voir en un coup d'oeil ce qu'il s'est passé sur le projet. N'oubliez pas que "les autres développeurs", ce sera vous dans deux mois. Avant d'effectuer un `commit` vous devez ré-écrire votre historique afin déviter des `commits` inutiles (ex: Missing file, really fix bug, fix bug 2...). Utilisez la commande `rebase -i` pour faire cela.

### 3.2 - Le message

Le `message de commit` peut être composé de trois parties : le `header` ou `titre/résumé`, le `body` ou `description` et le `footer`. Il est donc préférable de ne pas utiliser la commande `git commit -m "<message>"`. Utilisez plutôt la commande `git commit` seule pour qu'un éditeur de texte s'ouvre.

Exemple :

```shell
$ git commit

<header>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and 
# an empty message aborts the commit.
#
# On branch develop
# Changes to be committed:
#       new file:   10.md
#
# Untracked files:
#       .idea/
#
```

**3.2.1 Le titre**

Le titre du commit doit être formaté de la manière suivante :

```
[TYPE] <Short summary (=message)>
  │       │
  │       └─⫸ Message au présent. Première lettre en majuscule. Pas de point.
  │
  └─⫸ Type de commit : FEATURE|PACKAGE|CI|DOC|BUGFIX|HOTFIX
```

La première lettre du `message` doit être en majuscule et il ne doit pas se terminer par un point. Vous devez l'écrire dans une forme impérative. N'utilisez pas de négation.

Les différents types valides sont :

| Type    | Description                                  |
| ------- | -------------------------------------------- |
| FEATURE | Ajout d'une nouvelle fonctionnalité          |
| PACKAGE | Installation/Mise à jour d'une dépendance    |
| REFACTO | Amélioration du code, mise à niveau          |
| CI      | Modification liée à la CI/CD du projet       |
| DOC     | Ajout d'une documentation                    |
| BUGFIX  | Réparation d'un bug durant le développpement |
| HOTFIX  | Réparation d'un bug en production            |

Pour écrire un bon message, gardez en tête que votre `message de commit` doit suivre cette phrase : `If applied, this commit will <commit message>` (ex: If applied, this commit will fix bug on authentication page).

**3.2.2 La description**

La `description` complète le `résumé`. Vous pouvez ajouter du texte pour expliquer en détail pourquoi ce `commit`. Vous pouvez utiliser une liste en utilisant des `-` indentés par deux espaces.

```
  - My first change description
  - Another change description
```

**3.2.3 Le footer**

Il est optionnel et permet d'ajouter des métadonnées telles que l'issue ou les issues associées au commit.

```
related to #1
```

**3.2.4 Exemple**

```
[FEATURE] Create my first feature

  - My first change description
  - Another change description
  
related to #1
```

### 3.3 - Lier une issue

Si vous voulez faire un lien entre votre `commit` et une `issue` vous pouvez utiliser la notation `#<issue_id>`. Evitez de l'utiliser dans le `titre` du `commit`. Lorsque vous faites ça, vous pouvez cliquer sur le lien de l'`issue` pour être redirigé vers cette dernière ou la survoler pour voir le titre.

## 4 - Merge request

### 4.1 - Introduction

Comme indiqué dans le nom, une `merge request` est une demande de `merge` d'une branche dans une autre. On retrouve aussi parfois cette fonctionnalité sous le nom de `pull request`. Afin d'en créer une, vous devez d'abord exposer publiquement vos branches. La demande doît être assignée à un mainteneur du projet qui effectuera une revue de code. Il peut l'accepter, exiger certains changements, commenter des modifications effectuées...

Une fois que la `merge request` est créée, vous pouvez continuer d'ajouter du travail sur cette branche. Vous devez utiliser ce processus lorsque vous souhaitez effectuer un `merge` d'une `release` ou d'un `hotfix` (si possible) dans la branche `master`.

### 4.2 Comment l'utiliser ?

Pour créer une `merge request` dans `Gitlab`, vous pouvez le faire à beaucoup d'endroits sur l'interface web. Par exemple, vous pouvez cliquer sur le bouton suivant dans la page principale du projet.

Ensuite, vous devez configurer la `merge request` en ajoutant quelques informations (branches, reviewer...) comme indiqué ci dessous:

Une `merge request` est aussi un endroit où vous pouvez discuter de la demande de `merge`. Vous pouvez poser des questions ou le reviewer peut aussi vous demander d'effectuer certains changements.

Si le reviewer demande des changements, lorsque vous les poussez, ils viennent s'ajouter à ceux existants dans la `merge request`.

Enfin, le reviewer peut accepter la `merge request` manuellement ou grace à l'interface web (dans ce cas, faites attention à l'historique).

### 4.3 Gestion des tags

Dans `Gitlab`, il est possible d'utiliser des tags pour nous aider à l'organisation, la visualisation et la communication sur nos `merge request`.

**4.3.1 Doing**

Le tag `doing` est utilisé pour les `merge request` qui ne sont pas encore prêtes à être revues. Une fois que la `merge request` est prête pour la revue, le développeur doit retirer ce tag.

**4.3.2 Aucun tag**

Le fait de ne pas mettre de tag sert à dire que la `merge request` est prête à être revue. Sa simple présence dans la liste des `merge request` nous indique qu'elle est prête à être revue ou qu'elle attend des approbations.

**4.3.3 In review**

Le tag `in review` permet de savoir que des questions ont été posées au développeur ou que le relecteur a demandé des modifications. Lorsqu'un relecteur effectue une demande de modifications ou pose une question, c'est à lui d'ajouter le tag `in review`. Lorsque le développeur a répondu aux questions ou effectué les modifcations, il doit retirer ce tag pour repasser dans l'état `Aucun tag`.

**4.4.4 Ready to deploy**

Ce tag permet d'indiquer que la `merge request` est prête à être mergée par le développeur de la fonctionnalité. Le deuxième développeur qui approuve la `merge request` doit ajouter le tag `ready to deploy` à la `merge request`.
