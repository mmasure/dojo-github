# Commits

Le commit content les éléments structurels suivants, permettant de communiquer à l'intention des consommateurs de votre bibliothèque. Pour cela, il sera structure de la manière suivante:

```
<type>(optional scope]): <description>
```

Les commits seront écrit exclusivement en **ANGLAIS.** Le message aura dans son scope les tickets Jira associé au commiteur. Plusieurs cas peut-être rencontré selon la taille de la feature et si il englobe ou non plusieurs tickets. Dans le cas d'un **ticket US** traité avec l'ensemble de ses tâches, on associera le scope avec le numéro de la user story.&#x20;

```
ex: feat(PROJET-N#): <description>
    body du commit: list tasks number
```

Si la feature ne correspond qu'à une **seul tâche** d'une user story alors, on associera uniquement son référencement.

```
ex: feat(PROJET-N#): <description>
    body du commit: Nothing
```

La description d'un commit devrait être écrit en commençant par un verbe au prétérit avec première lettre en majuscule. Il devra être le plus explicite et le plus court possible. Si dans le cas où vous envoyait un commit pour une user story, utilisez le body du commit pour compléter la description de celui-ci.

&#x20;Pour le type de commit, nous renseigneront principalement les suivant:

<table><thead><tr><th width="157">Type</th><th width="197">For ?</th><th>When ?</th></tr></thead><tbody><tr><td>feat</td><td>feature</td><td>new feature</td></tr><tr><td>fix</td><td>fix or hotfix</td><td>bugfix</td></tr><tr><td>refactor</td><td>refactoring</td><td>refactoring code</td></tr><tr><td>docs</td><td>documentation</td><td>Source code doc<br>Command docs (package.json)</td></tr><tr><td>cicd</td><td>devops</td><td> cicd configuration<br>github action config</td></tr><tr><td>test</td><td>test</td><td>adding test only</td></tr></tbody></table>

{% hint style="info" %}
Attention, les règles précédentes ne sont à appliquer uniquement sur les commits qui seront rebase sur la develop. En aucun cas celles-ci ne seront appliqué sur des commits 'éphémères' d'une feature. Vous les appelez comme vous le souhaitez si elle ne possède pas d'importance.
{% endhint %}
