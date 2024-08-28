# DOJO Bonne pratique Git et exercices

## Sommaire
1. [Le Gitflow](#chapitre-1--gitflow)
2. [Pull request: merge, squash ou rebase ?](#chapitre-2--différence-entre-merge-squash-et-rebase-pour-une-pull-request)
3. [Commit convention](#chapitre-3--conventions-de-nommage-dun-commit)
4. [Advanced: Les commandes qui sauveront votre code](#4-advanced-les-commandes-qui-sauveront-votre-code-)
5. [Conclusion](#conclusion)

# Chapitre 1 : Gitflow

**Gitflow** est un modèle de gestion de branches pour Git, développé par Vincent Driessen. Ce workflow est particulièrement adapté pour les projets de grande envergure ou pour les équipes qui souhaitent maintenir un processus de développement structuré. Voici les principales branches et étapes du modèle Gitflow :

## Branches Principales

1. **`main`** (anciennement `master`) :
    - C'est la branche principale qui contient toujours le code en production stable. Chaque commit sur cette branche est considéré comme prêt à être déployé.

2. **`develop`** :
    - Cette branche contient le code qui est prêt pour la prochaine version. C'est ici que toutes les nouvelles fonctionnalités sont intégrées avant d'être fusionnées dans `main`.

## Branches de Support

1. **Feature branches** :
    - Utilisées pour le développement de nouvelles fonctionnalités. Ces branches partent de `develop` et, une fois complétées, sont fusionnées de nouveau dans `develop`.
    - Convention de nommage : `feature/nom-de-la-fonctionnalité`

2. **Release branches** :
    - Utilisées pour préparer une nouvelle version de production. Elles partent de `develop` et sont fusionnées dans `main` et `develop` une fois prêtes. Ces branches permettent de faire des corrections de bugs et des ajustements de dernière minute avant une version.
    - Convention de nommage : `release/x.y.z`

3. **Hotfix branches** :
    - Utilisées pour corriger des bugs critiques sur `main`. Elles partent de `main` et, une fois corrigées, sont fusionnées dans `main` et `develop` pour assurer que la correction soit aussi présente dans les futures versions.
    - Convention de nommage : `hotfix/x.y.z` realité: `hotfix/nom-du-fix`

## Workflow de Gitflow

1. **Développement de Fonctionnalités** :
    - Créer une branche `feature` à partir de `develop`.
    - Travailler sur la fonctionnalité.
    - Fusionner la branche `feature` dans `develop` une fois terminée.

2. **Préparation d'une Version** :
    - Créer une branche `release` à partir de `develop` lorsque toutes les fonctionnalités prévues sont complétées.
    - Effectuer les tests finaux, corriger les bugs mineurs, et ajuster la documentation.
    - Fusionner la branche `release` dans `main` (version prête à être déployée) et `develop` (préparation pour la prochaine version).

3. **Correction de Bugs Critiques** :
    - Créer une branche `hotfix` à partir de `main` pour corriger le bug.
    - Appliquer les correctifs.
    - Fusionner la branche `hotfix` dans `main` (pour déployer la correction) et `develop` (pour que la correction soit intégrée dans les futures versions).

# Chapitre 2 : Différence entre Merge, Squash et Rebase pour une Pull Request

Lors de la gestion d'une pull request (PR), il est crucial de décider comment intégrer les changements dans la branche cible. Voici les trois méthodes principales :

## 1. Merge

- **Description** : Le merge combine deux branches en créant un commit de merge, qui a deux parents (les deux branches à fusionner).
- **Avantages** : Préserve l'historique complet de chaque commit. Utile pour les équipes qui veulent voir tous les changements détaillés.
- **Inconvénients** : L'historique peut devenir complexe et difficile à suivre avec de nombreux commits de merge.
- **Utilisation** : `git merge branch-feature`

## 2. Squash

- **Description** : Le squash combine tous les commits d'une branche en un seul commit avant de le fusionner dans la branche cible.
- **Avantages** : Simplifie l'historique en ne créant qu'un seul commit pour une série de changements, ce qui rend l'historique plus lisible.
- **Inconvénients** : On perd les détails de l'historique individuel des commits.
- **Utilisation** : `git merge --squash branch-feature`, puis `git commit` pour créer un seul commit.

## 3. Rebase

- **Description** : Rebase prend les commits d'une branche et les applique sur une autre branche, en modifiant l'historique des commits.
- **Avantages** : Crée un historique de commits plus linéaire et plus propre. Évite les commits de merge, ce qui rend l'historique plus facile à suivre.
- **Inconvénients** : Peut être complexe et dangereux si mal utilisé, car il réécrit l'historique.
- **Utilisation** : `git rebase branch-main` (puis `git push --force` si vous avez déjà poussé les commits rebased).

# Chapitre 3 : Conventions de Nommage d'un Commit

Les conventions de nommage des commits sont cruciales pour maintenir un historique de projet clair et utile. Voici quelques-unes des meilleures pratiques :

1. **Style Impératif** : Utiliser un verbe à l'impératif pour décrire ce que fait le commit. Ex. : `Add new user login feature`, `Fix bug in payment processing`.

2. **Limite de Caractères** : La première ligne du message de commit (le résumé) doit être concise et ne pas dépasser 50 caractères. Cela permet de maintenir l'historique des commits lisible dans les interfaces Git et sur les plateformes d'hébergement comme GitHub.

3. **Séparation du Résumé et du Corps** : Si le commit nécessite plus d'explications, laissez une ligne vide après le résumé, puis ajoutez un corps de message de commit qui détaille les changements. Le corps du message peut expliquer pourquoi les changements ont été faits et fournir tout contexte supplémentaire nécessaire.
    - Exemple :
      ```
      Add user authentication feature
 
      This commit introduces a new authentication system that allows users to log in using their email and password. The feature is implemented using JWT for token-based authentication.
      ```

4. **Utilisation de Mots-clés et de Préfixes** : Certaines équipes utilisent des mots-clés ou des préfixes pour catégoriser les commits et indiquer leur nature. Voici quelques exemples de préfixes couramment utilisés :
    - **`feat:`** pour indiquer une nouvelle fonctionnalité (ex. : `feat: add user profile page`).
    - **`fix:`** pour une correction de bug (ex. : `fix: resolve login issue`).
    - **`chore:`** pour des tâches de maintenance ou des changements qui ne touchent pas directement le code de production (ex. : `chore: update dependencies`).
    - **`refactor:`** pour des modifications de code qui n'affectent pas la fonctionnalité (ex. : `refactor: improve performance of sorting algorithm`).
    - **`docs:`** pour des changements dans la documentation (ex. : `docs: update API documentation`).
    - **`style:`** pour des changements de style de code qui n'affectent pas le comportement du programme (ex. : `style: format code with Prettier`).
    - **`test:`** pour ajouter ou corriger des tests (ex. : `test: add unit tests for user service`).

5. **Références aux Issues** : Il est souvent utile de faire référence aux tickets d'incidents ou aux user stories associés pour maintenir une traçabilité entre les commits et les problèmes suivis. Utilisez des conventions comme `Fixes #123` ou `Closes #123` dans le corps du message de commit pour lier le commit à un problème spécifique.
    - Exemple :
      ```
      fix: correct validation error on user input
 
      This commit resolves issue #456 by fixing the input validation logic in the signup form.
      ```

6. **Utiliser des Messages Descriptifs** : Les messages de commit doivent être suffisamment descriptifs pour que quelqu'un d'autre (ou vous-même dans le futur) puisse comprendre ce que fait le commit sans avoir à regarder le code. Évitez les messages vagues comme `fix bugs` ou `update`.

7. **Exemples de Bons Messages de Commit** :
    - `feat: add email confirmation for new users`
    - `fix: resolve crash on app startup`
    - `chore: remove deprecated API endpoints`
    - `refactor: streamline order processing logic`
    - `docs: update README with setup instructions`


# Chapitre 4 : Advanced: Les commandes qui sauveront votre code

## La commande `git reset`

La commande `git reset` est une des commandes les plus puissantes et parfois dangereuses de Git. Elle permet de revenir en arrière dans l'historique des commits, de modifier l'état de l'index (staging area) et de la copie de travail. Voici les principaux usages de `git reset` et comment l'utiliser en toute sécurité.

## Utilisation de `git reset`

### 1. `git reset --soft`

- **Description** : Cette option déplace simplement le HEAD vers le commit spécifié sans modifier l'index ni la copie de travail.
- **Utilisation** : C'est utile lorsque vous souhaitez annuler des commits récents tout en gardant les modifications dans l'index pour les réorganiser ou les modifier avant de les committer à nouveau.
- **Exemple** :
  ```bash
  git reset --soft HEAD~1
  ```
Cela déplace le HEAD vers le commit précédent, mais laisse les modifications dans l'index et la copie de travail.

### 2. `git reset --mixed (par défaut)`
  
- **Description** : Déplace le HEAD vers le commit spécifié et réinitialise l'index, mais laisse la copie de travail intacte. C'est l'option par défaut si aucun autre argument n'est spécifié.
- **Utilisation** : Utile lorsque vous souhaitez annuler des commits récents et désindexer les modifications tout en conservant les changements dans votre copie de travail.
- **Exemple** :
   ```bash
   git reset --mixed HEAD~2
   ```
Cela réinitialise les deux derniers commits, laissant les fichiers modifiés dans la copie de travail mais les désindexant.

### 3. `git reset --hard`
   
- **Description** : Déplace le HEAD vers le commit spécifié, réinitialise l'index et la copie de travail pour correspondre exactement à cet état.
- **Utilisation** : À utiliser avec prudence. Cette commande supprimera toutes les modifications non committées dans l'index et la copie de travail. Elle est utile pour annuler complètement des modifications ou pour synchroniser l'état de travail avec un point de commit spécifique.
- **Exemple** :
   ```bash
   git reset --hard HEAD~3
   ```
Cela réinitialise le HEAD, l'index et la copie de travail aux trois commits précédents, supprimant toutes les modifications locales.

### Références et Conseils

- **Utiliser avec précaution** : La commande git reset --hard est particulièrement risquée car elle peut entraîner la perte de modifications non sauvegardées. Il est conseillé de l'utiliser seulement si vous êtes sûr de vouloir supprimer des changements locaux.
- **Créer un backu** : Avant d'utiliser git reset, surtout avec --hard, il peut être judicieux de créer une nouvelle branche pour sauvegarder l'état actuel, par exemple :
   ```bash
   git branch backup-avant-reset
   ```

# Le `git rebase` interactif

Le `git rebase` interactif est un outil puissant qui permet de réorganiser, modifier et nettoyer l'historique des commits dans un dépôt Git. Il est souvent utilisé pour améliorer la lisibilité de l'historique des commits avant de fusionner une branche ou pour effectuer des modifications de commits déjà effectués.

## Qu'est-ce que le `git rebase` interactif ?

Le `git rebase` interactif permet aux développeurs de choisir et de modifier la façon dont les commits sont appliqués. Cela inclut la possibilité de réécrire des messages de commits, de fusionner plusieurs commits en un seul, de réordonner des commits, et de supprimer des commits inutiles. Cette flexibilité permet de maintenir un historique de commits propre et significatif.

## Comment utiliser le `git rebase` interactif ?

### Lancer un rebase interactif

Pour démarrer un rebase interactif, utilisez la commande suivante :

```bash
git rebase -i <commit-hash> ou <HEAD~n>
```

- `<commit-hash>` : Le hash du commit sur lequel vous souhaitez baser le rebase interactif. Vous incluez tous les commits jusqu'à ce commit, mais pas ce commit lui-même.
- `<HEAD~n>` : Une autre façon de spécifier combien de commits en arrière à partir du HEAD vous voulez inclure dans le rebase.
Par exemple, pour lancer un rebase interactif sur les 3 derniers commits, utilisez :

```bash
git rebase -i HEAD~3
```

## L'interface de git rebase interactif
Une fois que vous lancez un rebase interactif, une interface de texte s'ouvre (généralement dans votre éditeur de texte par défaut). Cette interface affiche une liste des commits sélectionnés, avec chaque commit précédé d'un mot-clé indiquant l'action par défaut à effectuer (pick par défaut).

Exemple :

```plaintext
pick f7f3f6d Change the title of the document
pick 310154e Update README file
pick a5f4a0d Add user authentication
```

Actions courantes dans un rebase interactif
- pick : Applique le commit. C'est l'action par défaut.
- reword : Applique le commit mais vous permet de modifier le message de commit.
- edit : Met en pause après avoir appliqué ce commit, vous permettant de modifier la copie de travail.
- squash (ou s) : Combine le commit avec le précédent, vous permettant de fusionner les modifications et de modifier le message de commit résultant.
- fixup (ou f) : Comme squash, mais il conserve uniquement le message de commit précédent.
- drop (ou d) : Supprime le commit de l'historique.

Exemple d'utilisation
Supposons que vous avez les commits suivants et que vous voulez les réorganiser et nettoyer l'historique :

```plaintext
pick f7f3f6d Change the title of the document
pick 310154e Update README file
pick a5f4a0d Add user authentication
```

Vous pourriez les modifier comme suit :

```plaintext
pick f7f3f6d Change the title of the document
squash 310154e Update README file
reword a5f4a0d Add user authentication with enhanced security
```

Dans cet exemple :

Le deuxième commit sera combiné avec le premier (squash).
Le message du troisième commit sera modifié (reword).
Terminer le rebase
Une fois que vous avez effectué les modifications nécessaires dans l'interface de rebase, enregistrez et fermez l'éditeur. Git appliquera alors les changements un par un, en vous permettant d'éditer les messages de commit ou de faire d'autres ajustements si nécessaire.

Si des conflits surviennent pendant le rebase, Git vous arrêtera et vous permettra de résoudre les conflits manuellement. Après avoir résolu les conflits, utilisez :

```bash
git rebase --continue
```
Pour annuler le rebase interactif en cours et revenir à l'état précédent, utilisez :

```bash
git rebase --abort
```
## Résumé

Le git rebase interactif est un outil puissant pour nettoyer et améliorer l'historique des commits. Bien utilisé, il permet de maintenir un historique linéaire et clair, facilitant la collaboration et la gestion du code. Cependant, il est important de l'utiliser avec précaution, en particulier sur les branches partagées, car il réécrit l'historique et peut causer des problèmes si des commits ont déjà été poussés et partagés avec d'autres développeurs.


# Conclusion

En suivant ces conventions et bonnes pratiques, vous pouvez maintenir un historique de commits propre, lisible et utile. Cela facilite non seulement la collaboration au sein d'une équipe, mais aide aussi à comprendre l'évolution du projet et à diagnostiquer les problèmes plus efficacement. Un bon message de commit raconte une histoire claire et complète du projet, rendant le processus de développement plus transparent et plus facile à gérer pour tous les participants.
