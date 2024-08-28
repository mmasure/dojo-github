---
description: >-
  En suivant cette convention, vous contribuez à une gestion plus efficace des
  Pull Requests et à une traçabilité transparente des tâches à travers Jira.
---

# Pull requests

## Création d'une pull request

#### Étapes pour Créer une Pull Request sur GitHub

### **1. Faites des Modifications Locales :**

Assurez-vous d'avoir déjà effectué les modifications souhaitées localement sur votre branche.

### **2. Poussez vos Modifications sur GitHub :**

Utilisez la commande `git push` pour pousser vos modifications sur votre branche sur GitHub.

```bash
git push origin VOTRE_BRANCHE
```

### **3. Accédez à la Page du Répertoire sur GitHub :**

Rendez-vous sur la page du repository sur GitHub où vous avez effectué vos modifications.

### **4. Sélectionnez votre Branche :**

Dans la barre de navigation, assurez-vous de sélectionner votre branche dans le menu déroulant des branches.

### **5. Cliquez sur "Pull Request" :**

Sur la page principale de votre branche, cliquez sur le bouton "Pull Request" situé à côté du menu des branches.

### **6. Remplissez le Formulaire de Pull Request :**

* **Base:** Sélectionnez la branche de destination qui est généralement la branche principale (develop) du projet.
* **Compare:** Assurez-vous que votre branche est sélectionnée.
* Remplissez le titre et la description pour expliquer vos modifications de manière claire et concise.

### **7. Soumettez la Pull Request :**

Cliquez sur le bouton vert "Create Pull Request" pour soumettre votre demande de tirage.

### **8. Attendez la Révision :**

Les membres de l'équipe examineront vos modifications. Attendez les commentaires et effectuez des ajustements si nécessaire.

### **9. Merge de la Pull Request :**

Si vos modifications sont approuvées, un membre de l'équipe peut fusionner votre Pull Request. Sinon, effectuez les ajustements demandés et répétez le processus.

{% hint style="info" %}


Conseils Supplémentaires :

* **Commits Significatifs :** Assurez-vous que vos commits sont significatifs et bien documentés.
* **Communication Transparente :** Communiquez clairement dans la description de la Pull Request pour expliquer l'objectif de vos modifications.
{% endhint %}

## Convention de Nommage pour les Pull Requests

#### 1. **Nom de la Branche :**

* Utilisez un nom de branche descriptif et en minuscules.
* Incluez le numéro de l'issue Jira pertinente.
* Ecrit en anglais

Exemple : `feature/projet-1234-nouvelle-fonctionnalite`

#### 2. **Titre de la Pull Request :**

* Commencez par le numéro de l'issue Jira entre crochets.
* Ajoutez un titre décrivant l'objectif de la PR&#x20;
  * si plusieurs tâches, titre concis avec les tâches Jira en description
  * Reprendre l'intitulé d'une user story.

Exemple : `[PROJ-1234]: Added new feature`

#### 3. **Description de la Pull Request :**

* Incluez une description détaillée de vos modifications.
* Incluez les liens vers les tickets Jira avec markdown

```
[PROJ-1234](https://ticketjiraurl.com): Titre du ticket
```

#### 4. **Commentaires Additionnels :**

* Ajoutez des commentaires supplémentaires si nécessaire.

#### 5. **Références à Jira dans les Commits :**

* Utilisez des messages de commit clairs et référencez la tâche/user story Jira.

{% hint style="info" %}
Voir le chapitre concernant les [commits.md](commits.md "mention")
{% endhint %}

#### 6. **Assignation de la Pull Request :**

* Assurez-vous que la PR est assignée à la personne appropriée pour la revue.

#### 7. **Merge de la Pull Request :**

* Assurez-vous que la PR ne peut être fusionnée (rebase obligatoire) que lorsque les critères de validation sont satisfaits.

#### 8. **Nettoyage de la Branche :**

* Après la fusion, supprimez la branche si elle n'est plus nécessaire.

#### Avantages de cette Convention :

* **Clarté :** La convention offre une clarté immédiate sur le contenu de la PR.
* **Intégration avec Jira :** La référence constante à l'issue Jira facilite le suivi des tâches associées.
* **Automatisation :** Certains flux de travail peuvent être automatisés en fonction de la convention de nommage.

## Labels PR et issues

| Name           | Description                                | example                                                                                |
| -------------- | ------------------------------------------ | -------------------------------------------------------------------------------------- |
| bug            | Something isn't working                    | Bugfix, hotfix tag                                                                     |
| documentation  | Improvements or additions to documentation | projet ou information technique                                                        |
| duplicate      | This issue or pull request already exists  | duplication de PR ou issue                                                             |
| feature        | New feature or request                     | New feature                                                                            |
| help wanted    | Extra attention is needed                  | Need informations/help                                                                 |
| invalid        | This doesn't seem right                    | <ul><li>have to wait something (PR or futur task)</li><li>problem user story</li></ul> |
| question       | Further information is requested           |                                                                                        |
| wontfix        | This will not be worked on                 | Will not be fix now                                                                    |

