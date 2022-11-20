---
layout: post
title: "La code review: un exercice plus difficile qu’il n’y parait"
category: ["team collaboration"]
tags: [github, "pull request"]
---


Il y a quelques jours, en faisant le ménage de printemps dans ma bibliothèque virtuelle qu’est Dropbox, je suis retombé un peu par hasard sur la première présentation que j’avais fait en arrivant chez LiveMentor il y a maintenant trois ans.
<!--more-->
À l’époque, l’équipe, fraîchement composée, accueillait beaucoup de membres juniors ou stagiaires. J’avais été impressionné par l’intégration continue qui était en place et la vélocité avec laquelle les nouvelles fonctionnalités étaient mises en ligne.

Il n’y avait, par contre, pas de code reviews systématiques avant les déploiements. Beaucoup de petits projets partaient directement en production, et lorsqu’il y avait des reviews, elles étaient souvent mono-directionnelles (une développeur plus expérimenté qui review un développeur plus junior). Cela entraînait une vague de bugfix après chaque déploiement.

Tout le monde était déjà convaincu des bienfaits de la code review: outre l’amélioration de la qualité globale d’une codebase, le bénéfice d’avoir au minimum deux personnes conscientes de chaque changement qui part en production ou encore le filet de sécurité qu’est la découverte de bugs avant qu’ils ne partent en production, la code review est surtout un très bon outils pour faire du partage de connaissance.

C’est donc dans ce contexte que nous avons instauré la règle d’une review minimum par pull request (PR) et le fait que n’importe qui peut faire une review de n’importe qui d’autre.

J’avais alors compilé quelques bonnes pratiques sur le process de code review dans une [présentation](old_presentation). Je me suis surpris à redécouvrir certaines d’entre elles.

Je trouve intéressant de partager cette liste non exhaustive de bonnes pratiques sur l’écriture de pull requests, la façon de donner du feedback et d’y répondre.

## Ouvrir une pull request, ça ne se fait pas en un click !

C’est une situation commune à de nombreux développeurs: après une longue session de forte concentration qui a permis de donner naissance à quelques centaines de lignes de code, le serveur d’intégration vous indique que tout est vert, la fonctionnalité sur laquelle vous travaillez depuis quelques jours est enfin prête. Il est alors tentant d’ouvrir une nouvelle PR, de lui donner un titre, d’immédiatement cliquer sur publier, de demander une review et de passer à autre chose.

Il manque cependant une étape cruciale… remplir le champ de description de la PR. C’est un espace idéal pour expliquer les motivations derrière les changements introduits, exprimer les choix d’implémentations et faciliter la vie de la personne qui va faire la review. Très largement utilisé dans le monde de l’open source où la personne qui ouvre la PR et la personne qui la relit ne se connaissent pas forcément, il est trop souvent oublié en entreprise.

Une bonne description de pull request devrait au minimum contenir les points suivants:

### Du contexte, du contexte et encore du contexte !

Rien de mieux que d’introduire une pull request en expliquant ce qui a initié les changements.

Il peut s’agir d’un item planifié de la roadmap, d’un ticket de bug, la découverte d’un refactor potentiel lors d’une session de pair programming, l’envie de tester un nouvel outil, des problèmes de performances, …

👉 _C’est le “pourquoi ?” de la PR_

### Un but unique et explicite

Qu’allons nous atteindre en intégrant cette branche ?
Si le but de la pull request ne peut être résumé en une courte phrase, c’est souvent un symptôme indiquant que la PR est trop grosse et introduit trop de changements.

👉 _C’est le “quoi ?” de la PR_

### Des explications sur les choix d’implémentation

Documenter les choix d’implémentation dans une description de PR ne peut être que bénéfique. En expliquant pourquoi tel design pattern est utilisé, comment sont découpés les modules, pourquoi avoir choisi telle convention de nommage ou encore quelles étaient les contraintes qu’il fallait prendre en compte, l’auteur du code donne le raisonnement derrière son implémentation.

Cela va conditionner la personne qui va relire le code et lui donner le liant pour analyser les changements proposés.

👉 _C’est le “comment ?” de la PR_

### On veut des résultats !

Lorsque cela fait sens, donner des indications sur la façon de tester les changements va grandement faciliter la review.

En développement web, beaucoup de changements sont visuels. Notre application principale étant hébergée sur Heroku nous utilisons beaucoup les “review apps” dans notre équipe.

Chaque pull request va automatiquement lancer une nouvelle application de staging avec le code de la nouvelle branche.

Lorsque nous avons de la QA (Quality Assurance) fonctionnelle à faire sur des interfaces, nous avons pris l’habitude de préparer un compte élève par scénario de test identifié lors des specifications.

Le membre de l’équipe qui se charge de faire la QA n’a alors plus qu’à récupérer chaque lien pour facilement se connecter sur un compte élève et tester chaque scénario.

---

Pour harmoniser les descriptions de pull request, Github permet l’édition d’un template.

Voici celui que nous utilisons actuellement:

{% gist 87f7677c187e8e6112b9954667648106 %}

Quelques exemples d’utilisation du template :

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/pr_template_example_1.png" alt="Pull Request Template example 1" %}

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/pr_template_example_2.png" alt="Pull Request Template example 2" %}

Chaque projet utilise un template différent. Il est toujours intéressant d’aller fouiner sur certains projets open source pour trouver de l’inspiration.
Rien qu’en regardant des projets comme [VueJS](vuejs_pr_template), [Atom](atom_pr_template), [ESLint](eslint_pr_template) ou encore [Webpack](webpack_pr_template), on se rend compte de ce qui importe pour chaque équipe.

> Lorsque j’écris une description de pull request je me mets en tête que la personne qui va me relire ne connaît rien des changements que j’ai implémenté. Même lorsque ce n’est pas le cas, ça m’aide à prendre du recul et faciliter la review.

Au passage c’est très agréable lors d’un git blame de retomber sur une PR vieille de plusieurs mois bien documentée. Il est alors très simple de se replonger dedans.

## Les bonnes pratiques de la création de pull requests

En plus d’une description solide des changements introduits, voici quelques bonne pratiques sur la création de pull requests :

### Petite PR = facile à relire

Plus une pull request est grosse, plus elle va être compliquée et longue à relire.

Lors d’une review de PR, le niveau d’attention va naturellement diminuer après une dizaine de minutes. C’est dans ces occasions que l’on laisse souvent passer des bugs qui auraient pu être évités.

Même sur de gros projets, il est souvent facile de faire un découpage en multiples pull requests indépendantes

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/pr_big_vs_small.png" caption="Pour un reviewer, la deuxième PR est plus attrayante que la première" %}
### Le premier reviewer de chaque PR devrait être son créateur.

Avant d’envoyer un email important nous prenons tous le temps de le relire, vérifier qu’il n’y a pas de faute d’orthographe et que toutes les informations que l’on voulait communiquer sont présentes.

Il est judicieux de faire la même chose avant de publier une nouvelle PR. Une relecture du diff git permet de facilement repérer des fichiers versionnés par erreur, des pans de code commentés ou encore de grosses typos.

Autant de travail en moins pour le reviewer et de crédibilité en plus pour l’auteur de la PR.

### Les messages de commits… tout une histoire

Il est normal lors d’un développement de faire beaucoup d’itérations, de tâtonner, de faire marche arrière. On se retrouve alors souvent avec un historique de commits qui ressemble à cela.

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/commits_history_mess.png" alt="Commit History Mess" %}

Avant d’ouvrir une pull request il est judicieux de prendre le temps de repasser sur l’historique de commits en en déplaçant, en fusionnant et en réécrivant les messages de commits grace au [rebase interactif](interactive_rebase) de GIT.

Idéalement, l’historique de commits devrait raconter une histoire. Dans certains cas, il nous arrive même de recommander à la personne qui va relire une PR de faire une review commit par commit.

### Éviter le formatage de code intempestif

Il est tentant, lorsque l’on ouvre un vieux fichier, de reformatter tout ce qui ne respecte pas l’analyseur syntaxique de code ou d’ordonner différemment l’ordre des méthodes.

C’est néanmoins à éviter car cela introduit beaucoup de bruit pour la personne qui va relire le code.

Idéalement il faudrait ouvrir une première PR dédiée juste au formatage du code. Ou alors centraliser tout le formatage dans un commit et faciliter la review commit par commit.

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/linting_in_pr_diff.png" alt="Linting in PR diff" caption="Difficile de repérer les changements au milieu de tout se formatage…" %}
## Pourquoi se donner tant de mal ?

L’enjeux crucial derrière tout cela c’est de réduire le temps moyen entre l’ouverture et la clôture d’une pull request. Cela permet d’augmenter la qualité moyenne d’une codebase sans perdre en vélocité.

Les variables qui rentrent en compte dans cette métrique sont:

* le temps entre l’ouverture d’une PR et le début de la review
* le temps de review lui même
* le temps d’implémentation des potentiels changements issus de la review

L’idée est donc de faire tout son possible pour faciliter la vie de la personne qui va relire la PR. Cela impactera le temps de review lui même mais aussi le délai entre l’ouverture d’une PR et le début de la review. En tant que reviewer, si je sais qu’une review ne va pas être trop chronophage je serai enclin à la commencer plus tôt.

## D’ailleurs comment donner du feedback lors d’une relecture de code ?

Maintenant que l’on a vu quelques astuces pour écrire une bonne PR, entre en jeu le reviewer. Relire une pull request peut être chronophage et délicat. Voici donc quelques bonne pratiques à garder en tête lors d’une relecture de PR.

### “Ask, don’t tell”

Poser des questions ouvertes permet de lancer la discussion plutôt que de créer de la frustration.

Si par exemple, lors d’une review, on remarque qu’une logique a été dupliquée, on ne va pas écrire

> Duplication de code !

mais plutôt

> J’ai remarqué que les logiques entre X et Y sont très proches. Est ce que tu penses qu’il y a moyen de factoriser le code ?

### Demander des clarifications

En tant que reviewer je ne suis pas omniscient, il m’arrive régulièrement de demander des clarifications. Cela peut être un signe que le code n’est pas assez explicite mais c’est peut être aussi que je suis passé à côté de quelque chose.

> Je ne suis pas sûr de comprendre cette validation. Est ce que ça permet d’éviter qu’un élève pose deux questions à la fois ?

> Que fait cette méthode ? Son nom me fait croire que ça fait X mais quand je regarde le code j’ai l’impression que ça fait Y

### Il existe un biais de négativité à l’écrit

Sans l’intonation de la voix ou les expression faciales il est plus difficile de traduire un sentiment à l’écrit qu’à l’oral.

Une question neutre telle que:

> Est ce que tu as pris le temps de tester cette méthode avant de soumettre la PR ?

…aura tendance à être interprétée comme négative lorsqu’elle est lue.

De la même manière, le sarcasme est difficile à identifier à l’écrit.

Heureusement nous avons pleins d’emojis à notre disposition 😀 😁😃 😄 😅 😆 😉 😊😋 😎.

### Explicite > implicite

Cela paraît évident, mais, plus nous sommes explicite avec notre feedback, plus il sera de qualité. Si l’auteur d’une PR répond à un commentaire en demandant plus d’explication, c’est que le feedback donnée n’était pas assez bon.

Plutôt que d’écrire

> On peut utiliser un map ici

on écrira

> J’ai remarqué ce pattern où tu itères sur un tableau, fais quelques opérations et stock chaque résultat dans un tableau préalablement défini. Dans ce genre de situation on peut utiliser la méthode map dans Enumerable. Tu peux trouver de bons exemples ici: http://…

### Un problème, des centaines de solutions différentes

Il est important de comprendre la différence entre une solution différente de celle que, moi reviewer, j’aurais écrit et une solution qui ne résout pas le problème initial.

Il existe, bien souvent, plusieurs chemins qui mènent à la même destination.

La relecture de PR permet dans cette métaphore de se laisser guider vers de nouveaux chemins que l’on aurait pas forcément pris tout seul.

### Il est trop tard pour questionner les choix architecturaux ou de modélisation

> Pourquoi est ce qu’on introduirait pas la notion de “crédits” pour gérer nos réductions ?

> Que penses tu d’utiliser Redis plutôt que PostgreSQL pour cette feature ?

Bien que légitimes, ce genre de questions qui remettent en cause l’intégralité de choix d’implémentation d’une fonctionnalité sont souvent le signe de manque de spécifications ou de communication.

Échanger avec son équipe en amont de la phase de développement afin de s’aligner sur une stratégie d’implémentation permet d’éviter de se retrouver dans cette situation.

### Souligner les bonnes pratiques

Une review n’est pas que négative. C’est également une bonne opportunité de complimenter le code d’un membre de l’équipe ou de partager sa gratitude lorsque la lecture a permis de découvrir quelque chose de nouveau.

> ❤️ la façon dont tu as géré le cas particulier des users avec un mot de passe temporaire, très malin :)

> Je ne connaissais pas l’existence de la méthode zero? sur les nombres . C’est super pratique ! Je vais l’utiliser à partir d’aujourd’hui.

### Proposer son aide

Lorsque nous faisons un retour qui implique des changements conséquents ou bien spécifique, il peut être judicieux de proposer une session de pair programming à l’auteur de la PR pour implémenter la solution.

> J’ai l’impression qu’on pourrait utiliser le Composite pattern ici pour répondre à ce problème. Qu’est ce que tu en penses ? Si tu es aligné ça te dit de faire une session de pair programming pour implémenter ça ensemble ?

### Ne pas s’attarder sur le style du code

Les codes reviews ne sont pas le bon endroit pour ouvrir des débats sur le style du code.

> Je préfère qu’on utilise des espaces plutôt que des tabulations.

> Pourquoi est ce que tu n’as pas utilisé de CamelCase dans le nom de tes variables ?

Ce genre de commentaires n’apportent aucune valeur ajoutée, il existe des outils d’analyse de code automatique qui font très bien ce travail. En se basant sur un style guide défini et accepté par l’équipe on élimine facilement ces retours stylistiques.

### Considérer la peer code review

Il arrive de ne pas savoir par où commencer une review, ou de se sentir submergé par la tâche qui nous attend. Rien n’empêche d’aller voir l’auteur de la PR et de lui demander de faire la code review directement avec lui.

C’est généralement très efficace.

### Le temps de réaction à une demande de review est important

Si un relecteur attend quatre jours et trois relances pour faire la review d’un collègue, il y a de fortes chances pour qu’il soit court-circuité lors de la prochaine pull request.

Savoir se dégager du temps dans la journée pour relire le code de ses paires est une bonne hygiène de travail.

De manière générale, on concentre beaucoup d’attention sur la qualité le volume et la rapidité à fournir du code d’un développeur. Chez LiveMentor nous essayons d’attacher autant d’importance aux code reviews.

## Savoir répondre au feedback

### Mettre son égo de côté

C’est peut-être l’exercice le plus difficile du processus de code review. Il m’a personnellement fallu un peu de temps lors de mes premières expériences pour comprendre que les reviews se font sur le code, pas les développeurs.

Après avoir eu ce déclic, il était facile de ne plus se mettre en position de défense pour répondre aux commentaires laissés sur mes PRs mais d’en profiter pour progresser.

Cela ne veut pas dire, cependant, qu’il faut accepter 100% des retours. Nous avons tous nos convictions après tout.

### Ne pas se laisser submerger

Il y a souvent de bonnes idées qui émergent lors d’une code review.

Lorsqu’elles dépassent le scope initial de la PR, une bonne pratique est d’en prendre note et de l’extraire dans un ticket ou issue pour pouvoir adresser le sujet dans un second temps.

### Répondre à tous les commentaires

Sinon, comment le reviewer va t-il savoir si l’auteur de la PR a bien lu son retour ou si il l’ignore juste ?

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/github_emojis_reaction.png" alt="Github emoji reaction" caption="Github permet également de répondre rapidement avec des emojis" %}

### Passer offline au besoin

Lorsqu’une discussion dure, que plusieurs messages ont été échangés sur un sujet particulier et qu’aucun point de convergence n’est en mire, il est souvent plus efficient d’aller voir la personne avec laquelle l’échange a commencé et en parler de vive voix.

En prenant ensuite soin de résumer le résultat de la discussion directement dans la PR.

Donner du feedback n’est pas quelque chose d’inné. Mais avec un peu d’entraînement et de bonnes pratiques cela devient un réflexe.

---
## Sources
[https://github.com/blog/1943-how-to-write-the-perfect-pull-request](https://github.com/blog/1943-how-to-write-the-perfect-pull-request)

[https://github.com/thoughtbot/guides/tree/master/code-review](https://github.com/thoughtbot/guides/tree/master/code-review)

[http://kevinlondon.com/2015/05/05/code-review-best-practices.html](http://kevinlondon.com/2015/05/05/code-review-best-practices.html)

[https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message)

[old_presentation]: https://advocate-monkey-12625.netlify.com/
[vuejs_pr_template]: https://github.com/vuejs/vue/blob/dev/.github/PULL_REQUEST_TEMPLATE.md
[atom_pr_template]: https://github.com/atom/atom/blob/master/.github/PULL_REQUEST_TEMPLATE/feature_change.md
[eslint_pr_template]: https://github.com/eslint/eslint/blob/master/.github/PULL_REQUEST_TEMPLATE.md
[webpack_pr_template]: https://github.com/webpack/webpack/blob/master/.github/PULL_REQUEST_TEMPLATE.md
[interactive_rebase]: https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History
