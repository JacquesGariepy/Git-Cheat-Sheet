# Guide des meilleures pratiques Git

Ce guide détaille les bonnes pratiques d’utilisation de Git, aussi bien en individuel qu’en équipe, et répertorie les commandes essentielles, des conseils pour éviter les erreurs fréquentes, ainsi que des outils utiles sous Windows. Des checklists pratiques en annexe vous aideront à appliquer concrètement ces recommandations.

## Meilleures pratiques Git (usage individuel)

### Des commits atomiques et structurés

Un principe fondamental est de réaliser des **commits atomiques**, c’est-à-dire des validations focalisées sur **une seule modification ou fonctionnalité logique** ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=1,Changes%20Together)). Évitez de mélanger des changements sans rapport dans le même commit (par exemple, ne pas corriger un bug et ajouter une nouvelle fonction en une seule validation) – cela rend l’historique plus difficile à comprendre et complique d’éventuels retours en arrière ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=,multiple%20files%20have%20been%20modified)). Au contraire, un commit devrait regrouper des changements cohérents répondant à un même objectif ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Seulement%20ce%20conseil%20est%20incomplet,faire%20des%20commits%20n%E2%80%99importe%20comment)). 

Pensez que votre projet est comme un livre, et chaque commit en est un chapitre : l’enchaînement des commits doit raconter clairement l’évolution du code ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=livre%20et%20vos%20commits%20sont,un%20milieu%20et%20une%20fin)). En parcourant l’historique, vos coéquipiers (et votre « vous » du futur) doivent pouvoir suivre facilement ce qui a été fait et pourquoi. Il est souvent recommandé de commiter **fréquemment** pour sauvegarder votre travail, mais **“fréquemment” ne veut pas dire “n’importe comment”** ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Lorsque%20l%E2%80%99on%20d%C3%A9bute%20sur%20Git%2C,C%E2%80%99est%20un)). Mieux vaut quelques commits bien pensés que de nombreux commits désorganisés.

### Messages de commit clairs et conventionnés

Un bon commit s’accompagne toujours d’un **message explicite**. Le message doit **résumer brièvement ce qui a été modifié et pourquoi** ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Nommer%20les%20messages%20de%20commits)). En général, on utilise un court **résumé d’une ligne (50 caractères maximum)**, complété éventuellement d’une description plus détaillée dans le corps du message (après une ligne vide) pour donner du contexte ou les raisons du changement ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=)) ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=)). La première ligne doit être rédigée à l’**impératif présent** (ex: *« Corrige le bug X… »* ou *« Add feature Y… » en anglais) ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=,se%20terminer%20par%20un%20point)), sans point final, et éviter la voix passive. 

Il existe des **conventions de message** largement adoptées. Par exemple, la spécification *[Conventional Commits](https://www.conventionalcommits.org)* préconise un format structuré : 

```
<type>(<scope>): <sujet>
<ligne vide>
<corps du message éventuellement>
<ligne vide>
<footer éventuellement>
``` 

Ainsi, on préfixe le message par un type (feat, fix, docs, refactor, etc.) indiquant la nature du changement ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Le%20type%20du%20commit%20d%C3%A9crit,peut%20prendre%20diff%C3%A9rentes%20valeurs)) ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=,jour%20de%20version%20par%20exemple)), un éventuel périmètre (module impacté), puis un sujet concis. Exemple : `feat(auth): ajout de la connexion OAuth` sera compris comme une nouvelle fonctionnalité relative à l’authentification. Ce format améliore la lisibilité et permet même d’automatiser la gestion des versions et le **changelog** du projet ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Le%20message%20d%E2%80%99un%20commit%20doit,utilis%C3%A9e%20est%20la%20%C2%AB%C2%A0Conventional%20Commits%C2%AB)). Quel que soit le style adopté, **nommez correctement vos commits et décrivez le plus possible ce que vous avez fait** – cela prend un peu de temps, mais c’est du temps gagné lors de la relecture de l’historique ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=faire%20avec%20au%20minimum%203,commits%20bien%20distincts)).

*Bonnes pratiques supplémentaires :* si un commit résout une **issue** (ticket) dans un gestionnaire de bugs, mentionnez-le dans le footer du message (ex: `Fix #123`), ce qui pourra automatiquement fermer le ticket lors du merge sur la branche principale. 

### Utilisation efficace des branches

**Ne travaillez jamais directement sur la branche principale (`main` ou `master`) pour des développements.** Pour chaque nouvelle fonctionnalité, amélioration ou correctif, **créez une branche dédiée**. Cela isole vos changements et évite d’affecter le code stable avant d’être prêt ([ Conflits de merge Git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/using-branches/merge-conflicts#:~:text=d%C3%A9veloppeurs%29,de%20r%C3%A9soudre%20les%20conflits%20d%27%C3%A9dition)). Travailler à plusieurs sur la même branche provoque très rapidement des conflits et des risques d’écrasement mutuel de code – **“vous ne devez JAMAIS, mais alors JAMAIS travailler à plusieurs sur la même branche”** insiste un guide pour débutants ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=match%20at%20L93%20Vous%20vous,plusieurs%20sur%20la%20m%C3%AAme%20branche)). À la place, chaque développeur ou chaque tâche devrait avoir sa propre branche, qui sera fusionnée après validation. 

**Gardez les branches principales fonctionnelles :** la branche `master`/`main` doit toujours contenir du code en état de marche, testé et prêt pour une mise en production ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=L%C3%A0%20encore%20c%E2%80%99est%20une%20autre,termes%20de%20code%20et%20applicative)). De même, si vous avez une branche de développement intégration (souvent appelée `develop` dans certains workflows), elle ne doit contenir que des fonctionnalités terminées et intégrées, pas du travail en cours instable ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Master%20doit%20contenir%20du%20code,mis%20en%20production%20pour%20utilisation)). Le code en cours de développement *doit* résider sur des branches de fonctionnalité ou de correctif, jamais directement sur `main` ou `develop` ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=L%C3%A0%20encore%20c%E2%80%99est%20une%20autre,termes%20de%20code%20et%20applicative)). Cette discipline garantit qu’à tout moment, une nouvelle personne rejoignant le projet sait que ce qui est sur `main` correspond à la dernière version stable déployée, et que `develop` (si utilisé) correspond à l’état prêt à être livré de la prochaine version.

**Adoptez une convention de nommage des branches.** Utilisez des préfixes explicites pour indiquer le type de travail : par exemple `feature/` pour une nouvelle fonctionnalité, `bugfix/` pour une correction de bug, `hotfix/` pour un correctif urgent en production, `chore/` pour des tâches de maintenance, etc ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Le%20type%20d%E2%80%99une%20branche%20doit,des%20types%20de%20branches)). Suivez une syntaxe cohérente, comme `type/description-breve-[IDticket]`. Exemples : `feature/ajout-login-google`, `bugfix/correction-typo-page-accueil`, `hotfix/critical-crash-fix-512` (où 512 pourrait référencer un ID de bug). Gardez des noms **courts** mais explicites (idéalement < 50 caractères) et utilisez le kebab-case (mots en minuscules séparés par des tirets) ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Le%20nom%20de%20la%20branche,r%C3%A8gles%20doivent%20%C3%AAtre%20respect%C3%A9es)). Une convention claire aide toute l’équipe à comprendre le rôle d’une branche rien qu’à son nom.

**Évitez de coder plusieurs fonctionnalités à la fois sur la même branche.** Si vous commencez à travailler sur une fonctionnalité A sur la branche `feature/A`, puis que vous devez en parallèle débuter la fonctionnalité B, ne mélangez pas tout sur `feature/A`. Mettez éventuellement A de côté (commit, push sur sa branche) et créez une **nouvelle branche** `feature/B` à partir de `main` (ou `develop`) pour la fonctionnalité B. Ceci permet ensuite de faire des pull requests séparées, et de livrer l’une sans attendre l’autre si besoin. **En résumé : une branche = une feature ou un correctif.** Cette règle améliore la traçabilité et réduit les conflits. 

**Utilisez `.gitignore` pour un historique propre** : Assurez-vous d’ignorer les fichiers qui ne doivent pas être versionnés (fichiers de build, dépendances externes, fichiers temporaires, clés secrètes, etc.). Un repository peut vite être pollué par des fichiers inutiles si on n’y prend garde. Le fichier `.gitignore` sert justement à lister les motifs de fichiers à exclure du suivi Git ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=3.%20Ignoring%20)). Idéalement, configurez-le dès le début du projet (beaucoup de templates existent pour chaque langage/framework), et faites-le évoluer en cours de route si de nouveaux types de fichiers temporaires apparaissent ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=The%20,automatically%20generated%20files%20from%20Git)). **Ne suivez jamais les fichiers de configuration personnels, mots de passe, clés privées, etc.** (voir plus loin la section sur les erreurs à éviter).  

Enfin, **pensez à supprimer vos branches locales et distantes une fois fusionnées** pour éviter l’encombrement du dépôt. Après le merge d’une feature via une pull request, supprimez la branche correspondante. Sinon, vous accumulerez des dizaines de branches obsolètes, rendant le dépôt confus ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=4,Branches)) ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=To%20avoid%20this%2C%20you%20can,branch%20has%20been%20fully%20merged)). Des plateformes comme GitHub proposent de supprimer automatiquement la branche après merge. En nettoyant régulièrement, vous évitez de vous demander plus tard « cette branche a-t-elle encore un travail en cours ou peut-elle être supprimée ? » ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=are%20no%20longer%20any%20unique,commits%20associated%20with%20it)).

## Meilleures pratiques Git en équipe

Travailler efficacement en équipe avec Git nécessite d’adopter un **workflow** commun et quelques règles de collaboration. Il n’existe pas de solution unique, mais voici les pratiques répandues :

### Workflows de branches recommandés

- **Git Flow (Workflow Gitflow)** – Ce modèle, popularisé par Vincent Driessen, convient aux projets avec des **cycles de versions planifiés**. Il définit des branches principales stables (`main` pour les versions en production et `develop` pour l’intégration des prochaines fonctionnalités) et des branches de support : feature branches pour les nouvelles fonctionnalités (depuis `develop`), release branches pour la préparation d’une version (générées depuis `develop` en vue d’un déploiement prochain), et hotfix branches pour les correctifs urgents (depuis `main`) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Il%20convient%20de%20cr%C3%A9er%20une,main)) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=,les%20hotfix%20vers%20la%20production)). Dans Git Flow, **les features partent de `develop` et reviennent dans `develop` une fois terminées** ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=sauvegarde%2Fcollaboration,main)). Quand suffisamment de features sont prêtes, on crée une **branche de release** depuis `develop` pour fignoler la version (derniers tests, corrections mineures, documentation) sans interrompre le travail sur `develop` ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Une%20fois%20que%20,nouveau%20un%20merge%20de%20la)). Une fois la release validée, elle est fusionnée dans `main` (et taggée avec un numéro de version) **puis également fusionnée en retour dans `develop`** pour que `develop` intègre les correctifs apportés en phase de release ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=fonctionnalit%C3%A9%20suppl%C3%A9mentaire%20ne%20pourra%20%C3%AAtre,le%20d%C3%A9but%20de%20la%20livraison)) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=branche%20,le%20d%C3%A9but%20de%20la%20livraison)). Pour un **hotfix** en production : on crée une branche depuis `main` pour corriger le bug urgent, puis on fusionne cette branche de hotfix à la fois dans `main` **et** dans `develop` (afin que le correctif soit aussi appliqué dans la branche de développement) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=%24%C2%A0git%C2%A0flow%C2%A0hotfix%C2%A0start%C2%A0hotfix_branch)) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Outre%20les%20flows%20,ressemble%20%C3%A0%20ceci)). Git Flow apporte une structure rigoureuse qui **protège le code en production** tout en permettant le développement parallèle de fonctionnalités ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=)). En contrepartie, il peut être considéré comme complexe ou lourd pour les équipes pratiquant l’intégration continue : il crée des branches longues vécues (notamment `develop` qui diverge de `main` jusqu’à la prochaine release) et demande une gestion disciplinée des merges.

- **GitHub Flow** – Un workflow plus **léger** et adapté aux déploiements fréquents (notamment utilisé pour le développement web en continu). **Ici, pas de branche `develop`** : on ne conserve qu’une branche principale (`main`) qui représente toujours l’état prêt à déployer (ou déployé) en production ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=GitHub%20Flow%20is%20a%20simpler,need%20to%20manage%20multiple%20versions)). Chaque fonctionnalité ou bug à traiter est développé dans une **branche courte créée depuis `main`**, puis fait l’objet d’une **pull request** pour être fusionné dans `main` une fois validé. GitHub Flow repose donc fortement sur les **pull requests et les revues de code** pour valider les changements avant de les intégrer. Il **n’y a pas de branches de version ou de release distinctes** – les déploiements se font directement à partir de `main` (éventuellement en utilisant des tags pour marquer les versions déployées). Ce modèle convient bien aux équipes agiles déployant en continu ou très fréquemment, et aux **petites équipes** qui n’ont pas besoin de gérer plusieurs versions en parallèle ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=GitHub%20Flow%20is%20a%20simpler,need%20to%20manage%20multiple%20versions)). Il simplifie le workflow (une seule branche longue durée) et réduit le “merge hell” (les enfer des conflits) car les branches de travail sont de courte durée. **GitLab Flow** est une variante proche, intégrant en plus la notion d’environnements (préproduction, production) ou d’intégration avec les issues, mais on ne la détaillera pas ici. 

- **Trunk-Based Development (développement sur tronc)** – C’est le workflow privilégié des pratiques de **DevOps/CI-CD poussées**. Ici, l’idée extrême est d’**éviter les branches longues** : tout le monde intègre ses changements très fréquemment (plusieurs fois par jour) sur la branche principale (le **“tronc”**), idéalement via l’automatisation CI. On peut avoir de très courtes branches de quelques heures ou un jour, mais elles sont intégrées dès que possible. En fait, dans sa forme pure, les développeurs **committent directement sur le tronc** partagé sans passer par des branches de feature longues ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Trunk,into%20a%20shared%20trunk%20at)) ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=changes%20other%20developers%20are%20making,merging%20into%20the%20main%20branch)). Le tronc (souvent `main`) est donc constamment à jour et **toujours prêt à être déployé en production** à tout instant ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=match%20at%20L688%20least%20once,be%20ready%20for%20release%20anytime)). Cette stratégie vise à éviter absolument la dérive de branches et les conflits complexes : en intégrant en continu, on minimise l’écart entre le travail de chacun. **Des *feature flags*** (commutateurs de fonctionnalités) sont souvent utilisés pour désactiver dans le code les fonctionnalités en cours de développement qui sont mergées incomplètes, afin de ne pas impacter les utilisateurs avant qu’elles soient terminées. Le trunk-based development nécessite une grande discipline (tests automatisés robustes, déploiements automatisés, etc.) et convient bien aux **équipes expérimentées** pratiquant l’intégration continue à grande échelle. Il élimine pratiquement les merges retardés mais peut intimider les développeurs juniors car on travaille directement sur la branche commune du produit ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=frequently%20to%20the%20trunk%2C%20often,CD)) ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Because%20trunk,any%20conflicts%20that%20may%20arise)). De plus, sans branches de release, il faut un processus de déploiement très fiable. En contrepartie, c’est un atout pour la **livraison continue** : des changements petits et fréquents, moins de conflits, un historique linéaire.

**Quelle stratégie choisir ?** Cela dépend de la taille de l’équipe, du rythme de déploiement et de la complexité du projet. Aucune approche ne convient à tous les cas de figure ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=%2A%20There%20is%20no%20one,help%20shape%20your%20Git%20workflow)). Pour une petite équipe qui déploie souvent, un GitHub Flow ou un développement sur tronc (éventuellement avec PRs) est souvent plus efficace et moins lourd administrativement qu’un Git Flow complet. En revanche, pour un produit mature nécessitant des versions stables bien séparées (ex: logiciels versionnés, projets open-source avec contributions externes), Git Flow peut apporter la stabilité requise (par exemple, on peut refuser les contributions directes sur `master` et exiger des PR vers `develop` pour contrôle). **L’important est d’avoir une convention commune bien comprise de tous** ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Heureusement%2C%20il%20existe%20des%20m%C3%A9thodes,un%20article%20c%E2%80%99est%20Git%20Flow)), et de documenter votre workflow dans le README ou un document interne. Assurez-vous aussi que votre workflow s’aligne sur vos contraintes métiers (fréquence des releases, besoin de hotfix d’urgence, etc.) ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=Match%20a%20release%20schedule)) ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=A%20workflow%20should%20complement%20your,a%20branch%20to%20a%20version)). Et quelle que soit la stratégie, privilégiez les **branches de courte durée** : plus une branche reste longtemps sans être fusionnée, plus les risques de conflits augmentent ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=Short)).

### Pull requests et revues de code

Les **pull requests (PR)** sont un élément central de la collaboration avec Git, particulièrement dans les workflows GitHub Flow et similaires. Une PR est à la fois une demande de fusion de votre branche dans la branche cible (souvent `main` ou `develop`) et un espace de discussion pour examiner le code. Voici quelques bonnes pratiques :

- **Faites des PR dès que votre travail est prêt à être revu**, même si la fonctionnalité n’est pas encore parfaite. Il est possible de marquer une PR comme “brouillon” (*draft*) si ce n’est pas finalisé, tout en sollicitant un retour précoce de vos pairs. Ne gardez pas indéfiniment une grosse branche privée : intégrez tôt et souvent, cela rejoint l’idée des commits et branches atomiques.

- **Limitez la taille des PR** : idéalement une PR ne doit traiter qu’une fonctionnalité ou un bug. Évitez les “usine à gaz” de 50 fichiers modifiés et 10 000 lignes – c’est décourageant à relire et propice aux erreurs. Si vous vous rendez compte que votre branche a gonflé excessivement, envisagez de la découper en plusieurs PR plus petites (par exemple, extraire quelques changements de refactoring annexes dans une PR séparée). Des PR courtes sont plus faciles à comprendre et à tester, et seront revues plus rapidement par vos collègues.

- **Soignez la description de la PR** : le titre doit être explicite (par ex. *“Ajout de l’authentification OAuth2”* plutôt que *“Corrections diverses”*). Dans le descriptif, résumez le **contexte** (pourquoi ce changement ? quel problème résout-on ou quelle fonctionnalité ajoute-t-on ?), éventuellement **comment tester** ou reproduire le bug corrigé, et mentionnez les points d’attention. Liez la PR à un ticket d’issue si applicable (via des mots-clés *fix #num* ou en utilisant l’UI de votre forge logicielle). Une bonne description permet au relecteur de comprendre les tenants et aboutissants sans avoir à deviner. C’est aussi utile plus tard pour la traçabilité.

- **Assignez des reviewers et demandez des retours** : ne fusionnez pas vos propres PR sans regard extérieur (sauf cas très trivial). Même un petit patch bénéficie d’une deuxième paire d’yeux – cela permet de repérer d’éventuelles régressions, d’améliorer la qualité du code ou simplement de partager la connaissance du changement avec l’équipe ([How to correctly review a pull request | JimBobBennett](https://jimbobbennett.dev/blogs/how-to-review-a-pr/#:~:text=How%20to%20correctly%20review%20a,check%20the%20code%20for%20correctness)). Cultivez une culture de relecture bienveillante et constructive : les commentaires doivent viser le **code**, pas la personne, et chercher à améliorer la solution. En tant qu’auteur, soyez ouvert aux critiques et prêt à expliquer vos choix. En tant que relecteur, saluez ce qui est bien, posez des questions si quelque chose n’est pas clair, et suggérez des modifications si nécessaire.

- **Testez et validez avant de merger** : la PR est l’endroit où s’exécute généralement l’intégration continue (CI). Assurez-vous que la pipeline passe (tests unitaires OK, linter OK, build sans erreur). N’hésitez pas à tirer la branche sur votre machine pour la tester manuellement si besoin (la commande `git pull origin feature/ma-branche` vous permet de récupérer la branche du contributeur et de l’exécuter localement). En équipe, vous pouvez convenir qu’au moins un reviewer approuve (**“LGTM”** pour *Looks Good To Me*) avant de pouvoir fusionner.

- **Choisissez le mode de fusion adapté** : selon la politique du projet, vous pourriez faire un **merge commit** classique, un **squash merge** (écraser tous les commits de la PR en un seul commit sur la cible, utile pour garder un historique linéaire), ou un **rebase merge** (rebaser les commits de la PR sur la cible puis les intégrer sans commit de merge). Chacune a ses avantages ; l’important est de le définir par convention. Par exemple, beaucoup de projets open-source utilisent le **squash** pour que chaque PR apparaisse comme un seul commit dans `main`, ce qui simplifie le log. D’autres préfèrent garder les commits individuels pour faciliter le suivi des modifications fines.

- **Supprimez la branche** une fois la PR fusionnée (sauf si elle doit être conservée pour d’autres raisons). Comme évoqué, cela garde le dépôt propre.

En appliquant ces pratiques, les pull requests deviennent un outil efficace de partage de connaissance, de relecture de code et de maintien de la qualité logicielle. Une bonne revue de code attrape non seulement les bugs, mais peut aussi améliorer la lisibilité, la performance, la sécurité, etc., du code. **Respectez le temps de vos relecteurs** : évitez de sortir une PR énorme le vendredi 18h, ou de demander une review urgente sur du code non testé. Si vous êtes relecteur, essayez de faire le retour dans un délai raisonnable (idéalement sous un jour ou deux), pour que l’auteur puisse intégrer vos retours rapidement ([Best Practices for Reviewing Pull Requests in GitHub](https://rewind.com/blog/best-practices-for-reviewing-pull-requests-in-github/#:~:text=A%20good%20code%20review%20process,hours%20after%20its%20first%20submission)).

### Gestion des conflits et intégration

Les conflits Git surviennent lorsqu’on tente de fusionner des branches qui ont modifié les mêmes parties du code de manière incompatible. Bien que parfois inévitables, vous pouvez **réduire leur occurrence** en suivant les conseils précédents : branches courtes, intégration fréquente, travail isolé par fonctionnalité ([ Conflits de merge Git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/using-branches/merge-conflicts#:~:text=d%C3%A9veloppeurs%29,de%20r%C3%A9soudre%20les%20conflits%20d%27%C3%A9dition)). 

**Prévenir les conflits :** Synchronisez régulièrement votre branche avec la branche principale ou de base. Par exemple, avant de pousser votre travail ou d’ouvrir une PR, faites un `git pull --rebase origin main` (ou un merge depuis `main`) pour intégrer les dernières modifications de vos collègues. Plus vous attendez, plus votre branche diverge et plus le risque de conflits augmente ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=Short)). Si vous voyez que vous et un collègue éditez des fichiers connexes, coordonnez-vous éventuellement pour séquencer les merges. En résumé, garder les branches **à jour fréquemment** est la meilleure prévention.

**Résolution de conflits :** malgré tout, il arrivera que Git vous signale un conflit lors d’un `git merge` ou d’un `git rebase`. Pas de panique : Git laisse les marqueurs de conflit `<<<`, `===`, `>>>` dans les fichiers concernés. Ouvrez ces fichiers et décidez quelle partie du code garder (parfois c’est l’un ou l’autre, parfois une combinaison des deux). De nombreux IDE ou outils GUI (y compris VS Code, Sourcetree, GitKraken…) offrent des interfaces de merge qui présentent côte-à-côte les différentes versions et vous permettent de choisir plus facilement. **Après avoir résolu un conflit**, supprimez les marqueurs et assurez-vous que le fichier compile/fonctionne, puis ajoutez-le (`git add fichier_conflit`) et terminez la fusion ou le rebase (`git commit` pour un merge manuel, ou `git rebase --continue` en cas de rebase). 

Un conseil important : **committez ou stashez vos travaux en cours avant d’intégrer des changements distants**. Git refusera de lancer un merge si vous avez des modifications locales non committées, pour éviter de tout mélanger ([ Conflits de merge Git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/using-branches/merge-conflicts#:~:text=Git%20ne%20parvient%20pas%20%C3%A0,lancer%20le%20merge)). Si vous n’êtes pas prêt à committer, faites un `git stash` pour mettre temporairement de côté vos changements, effectuez le pull/merge, résolvez les conflits puis appliquez (`git stash pop`) vos changements stashés si besoin. 

Si un conflit est trop compliqué ou si vous vous êtes trompé en cours de résolution, souvenez-vous que vous pouvez **annuler un merge en cours** : la commande `git merge --abort` (ou `git rebase --abort` pendant un rebase) permet d’annuler l’opération et de revenir à l’état initial comme si rien ne s’était passé ([Obtention de modifications à partir d’un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/getting-changes-from-a-remote-repository#:~:text=%C3%89tant%20donn%C3%A9%20que%20,se%20trouvait%20avant%20le%20tirage)). C’est utile si vous voulez re-tenter la fusion plus tard ou prendre une autre approche (par ex., *rebase* au lieu de *merge*). 

**Choix merge vs rebase :** ces deux méthodes intègrent les changements d’une branche dans une autre, mais différemment. Un merge crée un commit de fusion, conservant l’historique divergent tel quel. Un rebase, lui, réapplique vos commits sur la nouvelle base comme s’ils avaient été committés plus tard (réécriture d’historique). Pour une intégration en équipe, **préférez le merge pour les branches partagées** (afin de ne pas réécrire l’historique public), et gardez le rebase pour vos travaux locaux ou pour cleaner l’historique de votre branche *avant* de la partager. **Ne rebase *jamais* des commits déjà poussés sur un dépôt public** car cela crée des incohérences pour vos collègues ([ Git rebase | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase#:~:text=Ne%20pas%20rebaser%20l%27historique%20public)). En clair, si votre branche feature est publique et que d’autres l’ont récupérée, évitez de la rebaser (privilégiez un merge de `main` dans votre feature pour la mettre à jour). En revanche, rebaser vos commits locaux avant de pousser la PR (ou squash vos commits) peut donner un historique final plus linéaire. Adaptez-vous à la politique du projet : certaines équipes interdisent tout rebase sur des branches partagées, d’autres l’autorisent avec précaution (ex: en s’assurant d’utiliser `--force-with-lease` lors du push pour ne pas écraser des commits qui ne seraient pas à vous).

En synthèse, la collaboration Git réussie repose sur : un historique propre et compréhensible, une structuration des branches logique, une synchronisation régulière, des revues de code attentives, et une communication fluide au sein de l’équipe. Avec ces bases, vous éviterez bien des problèmes et le travail avec Git deviendra naturel.

## Commandes Git pratiques essentielles

Cette section rassemble les commandes Git les plus courantes et utiles, avec une brève explication et des exemples d’utilisation. Ces commandes permettent de gérer le cycle de vie d’un fichier (du travail local à la publication distante), la navigation entre branches, et la manipulation de l’historique.

### `git init`

Initialise un nouveau dépôt Git local dans le répertoire courant. Cette commande crée un sous-répertoire `.git` contenant tous les fichiers de métadonnées du repository. À utiliser une seule fois au début d’un projet (pas nécessaire si vous clonez un dépôt existant).  

```bash
$ git init
```

*(Exemple : exécuter `git init` dans un dossier de projet pour le convertir en dépôt Git – les fichiers ne sont pas encore suivis tant qu’on ne les a pas ajoutés.)*

### `git clone`

Clone (copie) un dépôt Git existant depuis un serveur distant (GitHub, GitLab, Bitbucket, dépôt SSH, etc.) vers votre machine. Cette commande récupère tout l’historique du projet. Syntaxe : `git clone <URL-du-dépôt> [nom-du-dossier]`. 

```bash
$ git clone https://github.com/utilisateur/mon-projet.git
```

*(Exemple : clone le dépôt distant “mon-projet” depuis GitHub dans un dossier local du même nom. Après clonage, vous avez déjà une copie de tout le projet et une télécommande `origin` configurée vers le dépôt distant.)*

### `git status`

Affiche l’état du répertoire de travail et de l’index (staging area). Cette commande indique quels fichiers sont **non suivis** (untracked), modifiés mais **non indexés** (changes not staged), ou prêts à être committés (staged). Elle rappelle aussi sur quelle branche on se situe et si l’historique local est en avance ou en retard par rapport à la distante. En somme, c’est la commande de référence pour savoir **“où j’en suis”** dans Git ([ Découvrir Git avec Bitbucket Cloud | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/learn-git-with-bitbucket-cloud#:~:text=3,rapport%20%C3%A0%20votre%20d%C3%A9p%C3%B4t%20Bitbucket)).

```bash
$ git status
```

*(Exemple : après avoir modifié quelques fichiers, `git status` listera ces fichiers sous des catégories comme “modified” ou “untracked” pour vous indiquer les actions possibles – les ajouter avec `git add` ou les committer.)*

### `git add`

Ajoute des modifications au **staging** (index) en vue du prochain commit. On peut ajouter un fichier spécifique (`git add fichier.txt`), un répertoire (`git add mon_dossier/`), ou utiliser `git add .` pour ajouter tous les changements dans le répertoire courant. **Important** : un fichier modifié ne sera inclu dans un commit *que s’il a été ajouté à l’index* au préalable. Cela permet de sélectionner précisément les changements à committer.

```bash
$ git add README.md
```

*(Exemple : prépare le fichier README.md modifié pour le commit. Après cette commande, `git status` montrera ce fichier dans la section “Changes to be committed”.)*

### `git commit`

Enregistre un **instantané** des changements suivis dans l’historique du dépôt local ([ git commit | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/saving-changes/git-commit#:~:text=La%20commande%20,partie%20des%20plus%20fr%C3%A9quemment%20utilis%C3%A9es)). En d’autres termes, crée un nouveau commit avec les fichiers qui ont été préalablement ajoutés à l’index (avec `git add`). Par défaut, Git ouvrira votre éditeur pour saisir le message de commit. On peut utiliser l’option `-m "message"` pour fournir le message en ligne de commande.

```bash
$ git commit -m "feat: ajout du fichier de configuration initial"
```

*(Exemple : crée un commit avec le message indiqué. Seuls les fichiers ajoutés (staged) sont enregistrés dans ce commit. Les autres modifications resteront en attente. Chaque commit a un identifiant unique (SHA-1) et pointe vers un ou plusieurs parents dans l’historique.)*

👉 **Astuce :** utilisez `git commit --amend` pour modifier le dernier commit (par ex., pour corriger son message ou ajouter un fichier oublié). Cela réécrit le commit, **à n’utiliser que si le commit n’a pas encore été poussé** au dépôt distant, afin de ne pas altérer l’historique déjà partagé.

### `git branch`

Manipule les branches. Sans argument, `git branch` liste les branches locales existantes, en marquant d’un astérisque `*` celle qui est actuellement cochée (HEAD). On peut créer une nouvelle branche avec `git branch nom-branche` (cela crée la branche mais ne s’y déplace pas automatiquement), ou supprimer une branche avec `git branch -d nom-branche` (option `-D` en majuscule pour forcer la suppression si la branche n’est pas fusionnée). On peut aussi renommer une branche locale avec `-m`.

```bash
$ git branch
$ git branch feature/nouvelle-interface
$ git branch -d feature/ancienne-interface
```

*(Exemple : la première commande affichera par exemple `main` et `feature/nouvelle-interface` avec `* main` indiquant qu’on est sur main. La deuxième crée une nouvelle branche “feature/nouvelle-interface” en dupliquant l’état courant, et la troisième supprime une branche fusionnée nommée “feature/ancienne-interface”.)*

### `git checkout` / `git switch`

Permet de **changer de branche** ou d’en créer une nouvelle en se déplaçant dessus. Historiquement, on utilisait `git checkout -b nouvelle-branche` pour créer et passer sur une branche. Depuis Git 2.23, la commande `git switch` est introduite pour clarifier cette action : `git switch -c nouvelle-branche` fait la même chose. On peut aussi utiliser `git checkout branche-existante` pour simplement naviguer vers une branche existante, ou même pour restaurer un fichier (`git checkout -- fichier.txt` pour annuler les modifications non committées d’un fichier).  

```bash
$ git switch -c feature/login-google    # crée et se déplace sur la branche
$ git switch main                      # revient sur la branche main
```

*(Exemple : on était sur `main`, on crée une branche de feature et on s’y place en une commande. Après travail, `git switch main` nous ramène sur main. Si main a avancé entre temps, un `git pull` peut être nécessaire avant de merger la feature.)*

### `git fetch`

Récupère les derniers commits d’une branche distante **sans intégrer** les changements dans votre travail en cours. Cette commande interroge le serveur (tous les *remotes* ou un remote spécifique) et met à jour vos références locales des branches distantes (appelées *origin/nom-de-branche*). Elle **n’affecte pas votre branche actuelle**, elle se contente de télécharger les nouveautés du serveur. On l’utilise souvent pour voir ce qui a changé en ligne avant de merger ou rebaser. On peut faire `git fetch origin` (pour tout récupérer) ou préciser une branche : `git fetch origin main`. 

```bash
$ git fetch origin
```

*(Exemple : contacte le dépôt `origin` (défini par `git clone` automatiquement) et télécharge tous les nouveaux commits des branches distantes. Après cela, vous pouvez consulter le journal (`git log origin/main`) pour voir les nouveaux commits, ou comparer avec votre branche locale.)*

### `git merge`

Fusionne une branche source dans la branche courante. Par exemple, si vous êtes sur la branche `main` et que vous voulez y intégrer la branche `feature/login`, exécutez `git merge feature/login`. Git appliquera les commits de `feature/login` qui n’étaient pas encore sur `main`, créant au besoin un commit de merge si l’historique divergeait. La commande combine donc les historiques et tente de **résoudre automatiquement les différences* ([ Conflits de merge Git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/using-branches/merge-conflicts#:~:text=d%C3%A9veloppeur%20B%2C%20un%20conflit%20peut,de%20r%C3%A9soudre%20les%20conflits%20d%27%C3%A9dition))】. Si des conflits surviennent (fichiers modifiés différemment dans les deux branches), Git vous le signalera pour intervention manuelle (voir plus haut Gestion des conflits). 

```bash
$ git checkout main
$ git merge feature/login
```

*(Exemple : on se place sur `main`, puis on fusionne la branche de feature. Si aucun conflit, ce sera fast-forward (si possible) ou un commit de merge. Après cela, main inclut le travail de `feature/login`.)*

👉 **Note :** On peut aussi fusionner une branche distante dans la locale en une étape, par ex. `git merge origin/main` pour intégrer les derniers commits de la branche main distante (équivalent à fetch + merg ([Obtention de modifications à partir d’un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/getting-changes-from-a-remote-repository#:~:text=Fusion%20des%20modifications%20dans%20votre,branche%20locale))7】.

### `git pull`

Met à jour la branche courante en récupérant puis fusionnant (ou rebasant) les modifications distantes en une seule commande. En interne, `git pull` effectue d’abord un `git fetch` puis un `git merge` (par défau ([Obtention de modifications à partir d’un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/getting-changes-from-a-remote-repository#:~:text=,dans%20la%20m%C3%AAme%20commande))9】. C’est pratique pour synchroniser rapidement sa branche avec le serveur. **Attention :** assurez-vous que votre travail local est committé avant un pull, sinon le merge pourrait poser problème ou Git refusera s’il y a des modifications locales non indexé ([Obtention de modifications à partir d’un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/getting-changes-from-a-remote-repository#:~:text=%C3%89tant%20donn%C3%A9%20que%20,se%20trouvait%20avant%20le%20tirage))4】. La syntaxe courante est `git pull <remote> <branche>` (si vous omettez, ça pull le remote par défaut `origin` et la branche suivie). 

```bash
$ git pull origin main
```

*(Exemple : depuis la branche main locale, récupère les commits récents du `origin/main` et les fusionne. S’il y a un conflit, il faudra le résoudre comme décrit plus haut. Si vous souhaitez éviter les commits de merge automatiques, vous pouvez faire `git pull --rebase` pour rebaser vos commits locaux au lieu de créer un commit de merge.)*

Après un `git pull`, si tout se passe bien, votre branche locale est à jour. En cas d’erreur du type “non-fast-forward updates were rejected” cela signifie que votre dépôt local était en retard et qu’un pull était nécessai ([Poussée de commits vers un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/pushing-commits-to-a-remote-repository#:~:text=Gestion%20des%20erreurs%20autres%20que,de%20type%20avance%20rapide))3】. Le pull *doit* précéder un push lorsque votre branche distante a de nouveaux commits, sans quoi Git vous empêchera de pousser pour ne pas écraser ces commits.

### `git rebase`

La commande `git rebase` permet de **déplacer ou rejouer une suite de commits sur une autre base**. C’est un outil puissant de réécriture de l’historique. Concrètement, si votre branche de feature a divergé de `main`, un `git rebase main` sur votre branche va prendre tous vos commits et les appliquer **après** les commits de `main`, comme si vous aviez commencé votre travail à partir de la nouvelle tête de ma ([ Git rebase | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase#:~:text=Au%20niveau%20du%20contenu%2C%20le,compos%C3%A9e%20de%20commits%20tout%20nouveaux))9】. Cela **linéarise** l’historique : au lieu d’un graphe avec un embranchement (merge), vous obtenez une histoire en ligne droite où les commits de la feature suivent ceux de ma ([ Git rebase | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase#:~:text=))7】. En interne, Git crée de nouveaux commits identiques au contenu des anciens, mais avec un nouvel identifiant et un nouvel ancêtre, d’où le fait qu’on réécrit l’historiq ([ Git rebase | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase#:~:text=Au%20niveau%20du%20contenu%2C%20le,compos%C3%A9e%20de%20commits%20tout%20nouveaux))9】. 

Usage courant : se placer sur la branche qu’on veut rebaser (ex: `feature`), puis `git rebase main`. Après un rebase réussi, votre branche intègre les changements de `main` sans commit de merge. Vous pouvez alors `git push --force-with-lease` les nouvelles références de commits (puisque les anciens commits ont été remplacés). **N’utilisez rebase que sur des branches dont l’historique n’est pas partagé** (sinon, comme mentionné, ça pose des problèmes aux autres développeur ([ Git rebase | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase#:~:text=Ne%20pas%20rebaser%20l%27historique%20public))0】. 

Une autre utilisation est le **rebase interactif** (`git rebase -i`) qui permet de nettoyer l’historique avant de le publier : vous pouvez fusionner des commits (squash), réordonner ou éditer des messages. Par exemple, pour combiner plusieurs petits commits “WIP” en un seul commit logique avant de les pousser en PR.

```bash
$ git checkout feature/login
$ git rebase main
```

*(Exemple : sur la branche feature/login, on la “rebase” sur main. Si des conflits surviennent commit par commit, Git interrompra le process pour vous laisser les résoudre, puis vous continuerez le rebase. Une fois terminé, la branche feature/login aura un nouvel historique comme si elle était partie de la tête actuelle de main.)*

### `git cherry-pick`

Cette commande copie un commit existant (identifié par son hash ou son référence) dans la branche courante. C’est utile pour **porter un correctif** présent dans une branche vers une autre branche sans merger entièrement les deux branches. Par exemple, si un bug a été corrigé sur `main` par un commit spécifique et que vous voulez appliquer le même correctif sur la branche `release/1.0`, un cherry-pick reproduira le commit de correction sur la branche cible. 

```bash
$ git cherry-pick abc1234
```

*(Exemple : applique le commit dont le SHA-1 commence par abc1234 sur la branche actuelle, créant un nouveau commit avec les mêmes changemen ([Git par l'exemple - Cherie, ça va cherry-picker ! - DEV Community](https://dev.to/aurelievache/git-par-lexemple-cherie-ca-va-cherry-picker--37c8#:~:text=Le%20Git%20Cherry%20Pick%20permet,dans%20cette%20situation%20%21))4】. Si le contexte diffère, des conflits peuvent apparaître comme avec un merge, à résoudre manuellement.)*

En français simple, c’est l’équivalent d’un **copier-coller de commit** d’une branche à l’aut ([Git par l'exemple - Cherie, ça va cherry-picker ! - DEV Community](https://dev.to/aurelievache/git-par-lexemple-cherie-ca-va-cherry-picker--37c8#:~:text=Le%20Git%20Cherry%20Pick%20permet,dans%20cette%20situation%20%21))4】. Après cherry-pick, la branche courante a un nouveau commit (avec un nouvel identifiant) dont le contenu est celui du commit cherry-pické.

### `git stash`

Met de côté (stash) les modifications en cours sans les committer, pour retrouver un espace de travail propre. C’est très pratique si vous devez changer de branche ou tirer des modifications alors que vous n’avez pas fini une tâche. `git stash` sauvegarde l’état courant (changes tracked et non tracked optionnellement) dans une pile locale et restaure le HEAD à l’état de la dernière commit. Vous pouvez ensuite appliquer ces modifications plus tard avec `git stash pop` (qui récupère le stash le plus récent et l’enlève de la pile) ou `git stash apply` (qui l’applique sans le supprimer de la pile, si vous voulez le réutiliser plusieurs fois). 

```bash
$ git stash         # stash les modifs en cours
...                 # faites d'autres opérations, changement de branche, pull, etc.
$ git stash pop     # réapplique les modifications st ashées sur la branche courante
```

*(Exemple : vous éditez 5 fichiers sur la branche feature X, puis on vous demande un hotfix urgent sur main. Vous faites `git stash` pour sauvegarder vos travaux de X, puis `git checkout main` pour corriger le bug, committer et pousser. Ensuite, retour sur `git checkout feature X`, et `git stash pop` pour reprendre exactement où vous en étiez.)*

### `git log`

Affiche l’historique des commits. Par défaut, `git log` liste les commits récents avec leur hash, auteur, date et message. Il existe de nombreuses options pour formater ou filtrer : `--oneline` condense chaque commit sur une ligne (pratique pour un aperçu rapide), `--graph` affiche un graphe ASCII des branches et merges, `--author="Nom"` filtre par auteur, `--patch` montre le diff de chaque commit, etc. 

```bash
$ git log --oneline --graph --decorate --all
```

*(Exemple : affiche un historique abrégé de tous les commits (--all, y compris branches), avec un graph visuel des merges et les noms de branches/tags décorés. C’est une commande classique pour visualiser l’arbre des commits.)*

### `git remote`

Gère les dépôts distants (remote repositories). `git remote -v` liste les remotes configurés (nom et URL). Par défaut, après un clone, le remote s’appelle `origin`. On peut ajouter un nouveau remote : `git remote add nom URL`. Par exemple, ajouter un remote `upstream` pointant vers le repo officiel d’un projet open-source, en plus de votre fork `origin`. On peut renommer ou supprimer un remote (`git remote rename`, `git remote remove`). Cette commande ne sert pas quotidiennement si vous ne changez pas de remotes, mais est utile pour configurer le travail avec plusieurs dépôts. 

### `git push`

Envoie les commits de votre dépôt local vers un dépôt dista ([ Git Push | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/syncing/git-push#:~:text=The%20,These%20issues%20are%20discussed%20below))3】. C’est l’inverse du fetch : là où fetch importe des commits *vers* votre dépôt local, le push exporte vos commits *depuis* le local vers la branche distante correspondan ([ Git Push | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/syncing/git-push#:~:text=The%20,These%20issues%20are%20discussed%20below))3】. La syntaxe de base est `git push <remote> <branche ([ Git Push | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/syncing/git-push#:~:text=The%20,These%20issues%20are%20discussed%20below))1】. Par exemple, `git push origin main` va tenter de mettre à jour la branche `origin/main` avec vos commits locaux de `main`. Si la branche distante a avancé indépendamment (c.-à-d. si votre `main` local est en retard), Git refusera le push par protection, vous demandant d’abord de faire un pull pour intégrer les changements distan ([ Git Push | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/syncing/git-push#:~:text=remote%20repo,These%20issues%20are%20discussed%20below))1】. Cette règle prévient l’écrasement accidentel de commits sur le serveur.

```bash
$ git push origin feature/login-google
```

*(Exemple : pousse la branche locale `feature/login-google` vers le remote `origin`. Si cette branche n’existe pas encore sur le remote, elle sera créée. Après un push réussi, vos collègues peuvent à leur tour fetch/pull cette branche.)*

**Force push :** Si vraiment nécessaire, on peut forcer un push (`git push --force`) pour écraser la version distante. **Attention**, c’est dangereux car vous risquez de supprimer les commits que d’autres auraient récupérés (voir section erreurs à éviter). Préférez dans ce cas `git push --force-with-lease` qui n’écrasera la branche distante que si elle n’a pas de nouveaux commits qui ne sont pas dans votre historiq ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=which%20will%20no%20longer%20be,current))9】. En gros, *--force-with-lease* vérifie que vous n’effacez pas le travail de quelqu’un d’autre.

### `git revert`

Crée un nouveau commit qui annule les modifications d’un commit précédent. C’est la méthode recommandée pour “annuler” un commit déjà poussé, sans réécrire l’historique. Par exemple, `git revert <sha>` va appliquer les changements inverses du commit identifié par `<sha>` et committer le résultat (on vous demandera un message, par défaut “Revert <message du commit>”). Ainsi, le commit problématique reste dans l’historique, mais suivi d’un commit qui le neutralise. Cela préserve l’historique public intact (contrairement à un reset qui le modifierait). 

```bash
$ git revert f5e128c
```

*(Exemple : annule le commit f5e128c (par exemple un bug introduit) en créant un nouveau commit qui supprime ses effets. Vous pourrez ensuite pousser cette “réparation” sur la branche distante.)*

### `git reset`

Réinitialise l’état du HEAD et éventuellement de l’index ou du travail selon les options. C’est une commande plus complexe et potentiellement destructive (dans le sens où elle peut faire *perdre* des changements si mal utilisée). En simplifiant : 

- `git reset --soft <commit>` recule HEAD sur `<commit>` en conservant tous les changements dans l’index (les commits plus récents que `<commit>` deviennent des changements “à committer”). Utile pour “décommitter” sans perdre le travail : par ex, on a committé 3 commits, on veut les regrouper en un, on fait reset soft sur le commit d’avant ces 3, puis on recommit tout en un.

- `git reset --mixed <commit>` (par défaut si on met juste `git reset <commit>`) recule HEAD et vide l’index (staging), tout en gardant les modifications dans le répertoire de travail. Les fichiers modifiés par les commits annulés redeviennent modifications non indexées. 

- `git reset --hard <commit>` recule HEAD *et* remet l’index *et* le working directory à l’état de `<commit>` choisi. **Attention :** tout changement après ce commit (commits ou modifications locales) sera perdu définitivement dans cette branche. C’est pratique pour jeter un travail en cours et revenir à une version antérieure connue, mais à utiliser prudemment (ou après avoir push sur une autre branche pour ne rien perdre).

```bash
$ git reset HEAD~1   # annule le dernier commit, en conservant les modifications dans l'index
$ git reset --hard origin/main   # synchronise complètement la branche courante sur origin/main en écrasant tout changement local
```

*(Exemple : la première commande recule d’un commit (HEAD~1 signifie “le parent du HEAD actuel”), très utile juste après un mauvais commit non poussé. La deuxième force la branche courante à se caler sur la version distante, abandonnant tout travail local non committé.)*

En règle générale, **n’utilisez `reset` que sur votre dépôt local, jamais sur des commits déjà partagés**, sinon votre historique divergera du distant. Préférez `git revert` pour corriger un commit déjà public.

---

Ce ne sont là que les commandes les plus courantes. Git en comporte bien d’autres (ex: `git tag` pour créer des étiquettes de version, `git blame` pour voir qui a modifié chaque ligne d’un fichier, `git log --grep` pour chercher un mot-clé dans les messages de commit, etc.). N’hésitez pas à vous référer à la documentation Git officielle pour plus de détails. 

## Éviter les erreurs courantes (conseils et astuces)

Git est un outil puissant, et certaines erreurs classiques peuvent causer maux de tête et pertes de temps. Voici un florilège d’erreurs fréquentes et comment les prévenir ou les corriger :

- **Ne pas écrire de “mauvais” messages de commit :** Un message flou du type *“fix stuff”* ou *“wip”* ne renseigne personne sur la nature du changement. Prenez l’habitude de toujours écrire un message de commit descriptif (même pour un petit changement). Comme vu précédemment, donnez du contexte, mentionnez l’objectif du comm ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=While%20commit%20messages%20like%20these,author%20followed%20the%20previous%20advice))4】. Un bon message facilite le suivi du *pourquoi* du changement plus tard. *Astuce :* Si vous constatez une faute de frappe ou un oubli dans votre dernier message de commit, utilisez `git commit --amend` pour le corriger tant que le commit n’est pas encore pous ([Git happens! 6 Common Git mistakes and how to fix them](https://about.gitlab.com/blog/2018/08/08/git-happens/#:~:text=1,last%20commit%20message%20wrong))8】.

- **Éviter les commits géants aux changements hétérogènes :** Ne cumulez pas une tonne de modifications sans rapport dans un seul commit (ou une seule PR). Par exemple, ne **mixez pas refactoring, mise à jour de version et correction de bug** ensemble. Sinon, vos collègues auront du mal à relire, et si vous devez revenir en arrière, vous annulerez possiblement des changements voulus en même temps que les indésirabl ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=,multiple%20files%20have%20been%20modified))7】. Commitez **petit et souvent** : corrigez un bug = un commit, ajoutez une fonction = un commit (ou quelques commits logiques), et ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=1,Changes%20Together))3】. Si vous réalisez qu’un commit devient très volumineux, il est peut-être temps de le scinder avant de le pousser.

- **Ne pas ignorer le `.gitignore` :** Ce fichier est votre bouclier contre les *pollutions* de dépôt. Par exemple, committer accidentellement un fichier de log ou les dépendances Node_modules de 300 Mo alourdit inutilement le repo. Maintenez votre `.gitignore` à jo ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=3.%20Ignoring%20))7】 – il existe des modèles par langage sur internet (par ex. un `.gitignore` Python ignorera les `__pycache__`, un `.gitignore` Java ignorera les `.class`, etc.). Pensez à ajouter les fichiers de config personnelle (par ex. `.vscode/`, ou les fichiers `.env` contenant des secrets) dans le gitignore pour éviter de les suivre.  

- **Ne jamais versionner de données sensibles (mots de passe, clés privées, secrets API) :** C’est une erreur *extrêmement fréquente* et dangereuse. Un fichier de configuration contenant un mot de passe de base de données qui fuite sur GitHub, et c’est la catastrophe (des bots scrutent en permanence les commits publics à la recherche de secret ([L'erreur à ne surtout pas faire dans un dépôt Git public - Code-Garage](https://code-garage.fr/blog/erreur-a-ne-surtout-pas-faire-dans-un-depot-git-public#:~:text=L%27erreur%20%C3%A0%20ne%20surtout%20pas,recherche%20d%27une%20cl%C3%A9%20d%27API))7】. Utilisez des variables d’environnement ou des coffres-forts pour stocker ces infos, et **ignorez les fichiers contenant des secrets** (ajoutez-les au `.gitignore`). Si malgré tout vous commitez une donnée sensible, **changez immédiatement** la valeur compromise (rotations de clés, etc.) et purgez l’historique Git si le dépôt est public (via des outils comme BFG Repo-Cleane ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=Storing%20sensitive%20information%20in%20a,with%20access%20to%20the%20repository)) ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=If%20the%20data%20is%20already,every%20password%20and%20API%20key))5】. Même en interne, considérez qu’un secret commité est un secret potentiellement accessible à tous les collaborateurs du repo – pas bien. Sensibilisez votre équipe à ce suj ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=To%20avoid%20this%2C%20you%20should,them%20from%20being%20committed%20accidentally))4】.

- **Ne pas forcer le push sur les branches partagées :** Le `git push --force` est tentant quand “ça ne passe pas”, mais il peut faire disparaître le travail de vos collègues si vous l’utilisez à tort. Par exemple, si Alice et Bob travaillent sur la même branche et qu’Alice force un push de son côté, les commits de Bob risquent d’être écrasés. Par principe, **interdisez les push --force sur `main`** ou toute branche principale protégée. Si vraiment un réécriture d’historique est nécessaire sur une branche de travail partagée, utilisez `--force-with-lease` qui refusera d’écraser les commits qui ne sont pas dans votre versi ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=which%20will%20no%20longer%20be,current))9】. Cela ajoute une petite sécurité. Idéalement, communiquez avec votre équipe : “Je dois réécrire l’historique de la branche X pour nettoyer un problème, ne poussez rien dessus le temps que je finisse.”. Mieux vaut prévenir que guérir, car les dégâts d’un force push non coordonné peuvent être pénibles à réparer (il faut aller rechercher des commits orphelins dans les reflogs, etc.). En résumé : *“avec un grand pouvoir vient une grande responsabilité”* – n’utilisez `--force` qu’en dernier recours.

- **Ne laissez pas traîner des branches orphelines :** Après un merge, supprimez la branche (sauf si on doit la garder pour historisation). Des branches locales ou distantes obsolètes qui s’accumulent peuvent induire en erreur (quelqu’un peut croire que la branche “feature/apiv2” est encore active alors qu’elle a été mergée il y a 6 mois). Un petit ménage régulier s’impo ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=4,Branches)) ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=To%20avoid%20this%2C%20you%20can,branch%20has%20been%20fully%20merged))8】. De même, nettoyez vos **branches locales** de temps en temps en supprimant celles qui ont été intégrées – `git branch -d branche-mergee`. Pour les branches distantes, si l’option d’autodelete n’est pas active, vous pouvez faire `git push origin --delete nom-branche`.

- **Tester avant de committer/pusher :** Cela semble évident, mais assurez-vous que votre code compile et que les tests passent *avant* d’envoyer un commit, surtout sur la branche principale. Un commit qui casse la build ou les tests dans `main` peut pénaliser toute l’équipe (et sur un projet open-source, attirer la foudre de la communauté). Si vous faites du TDD, vous committez après avoir fait passer les tests, ce qui est parfait. Sinon, prenez au moins le temps de lancer les tests ou de vérifier l’application manuellement pour les changements critiques. Mettre en place une intégration continue qui rejette les commits problématiques est une excellente pratique pour épauler l’équipe.

- **Comprendre la différence entre revert et reset :** On l’a mentionné, mais c’est crucial. Si vous devez annuler un commit déjà poussé, utilisez `git revert` (ce qui ajoute un commit “d’annulation”). N’utilisez pas `git reset` car cela changerait l’historique de la branche distante et nécessiterait un force push – sauf si vous êtes dans le cas particulier d’une branche de feature sur laquelle vous êtes le seul et que vous n’avez pas encore partagée (dans ce cas un reset + amend de l’historique est envisageable). En règle générale, une fois que quelque chose est public, ne réécrivez pas le passé, ajoutez un correctif par-dess ([ Git rebase | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase#:~:text=Ne%20pas%20rebaser%20l%27historique%20public))0】. 

- **Utilisez les outils visuels et les alias :** Git en ligne de commande est puissant mais parfois ardu. Ne vous privez pas d’installer un outil visuel (voir section suivante) pour inspecter votre historique ou faire des opérations complexes. De même, configurez des alias utiles dans Git pour vos commandes fréquentes. Par exemple, `git config --global alias.lg "log --oneline --graph --decorate --all"` vous permet ensuite de taper `git lg` pour voir un historique joliné. Il existe de nombreux alias partagés sur internet (comme l’alias `git undo` pour reset le dernier commit non poussé, etc.). Ces aides peuvent vous faire gagner du temps et éviter des erreurs (par exemple, un alias pour `push --force-with-lease` pour ne pas taper par mégarde le simple --force).

- **Apprendre de ses erreurs grâce au reflog :** Si vous faites une bêtise (ex: un `git reset --hard` mal ciblé, ou un merge foireux annulé, etc.), sachez que **Git garde trace** de presque tout pendant au moins 30 jours via le `git reflog`. La commande `git reflog` liste toutes les positions qu’a prises HEAD récemment, même celles plus référencées. Ça signifie que *même si vous pensez avoir “perdu” des commits*, ils sont souvent récupérables tant qu’ils figuraient dans votre historique il y a peu. Vous pouvez ainsi retrouver le SHA d’un commit “supprimé” et le re-checkout pour le sauver. C’est un niveau avancé, mais c’est rassurant de savoir que Git est assez robuste : la plupart des erreurs ne sont pas fatales, il y a moyen de réparer (d’où l’importance aussi de bien comprendre Git pour se dépanner).

Pour résumer cette section, voici une liste de **principes clés** : Commits atomiques, messages explicites, ne pas exposer d’informations sensibles, intégrer fréquemment pour éviter les gros conflits, ne pas réécrire l’histoire partagée, tirer avant de pousser ( *“pull before push”* pour éviter les refus de non-fast-forwa ([Poussée de commits vers un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/pushing-commits-to-a-remote-repository#:~:text=Gestion%20des%20erreurs%20autres%20que,de%20type%20avance%20rapide))3】), et communiquer avec votre équipe. Avec ça, vous éviterez 90% des problèmes courants rencontrés par les développeurs sous Git.

## Outils Git courants sous Windows

Si la ligne de commande Git est disponible partout (notamment via **Git Bash** sur Windows, inclus dans l’installateur Git officiel), de nombreux outils facilitent l’utilisation de Git sous Windows en offrant des interfaces graphiques ou une intégration au système.

- **Intégration à l’explorateur Windows – TortoiseGit :** TortoiseGit est un client Git Open Source qui s’intègre directement dans l’explorateur de fichiers de Windows. Il ajoute des icônes de statut sur vos fichiers (pour indiquer s’ils sont modifiés, suivis, en conflit, etc.) et un menu contextuel riche sur un clic droit dans un dossier version ([Documentation – TortoiseGit – Windows Shell Interface to Git](https://tortoisegit.org/docs/tortoisegit/tgit-intro-features.html#:~:text=Git%20tortoisegit,tools%20you%27re%20already%20familiar%20with))7】. Vous pouvez ainsi effectuer les actions Git courantes (commit, push, pull, merge, view log, etc.) via des fenêtres explicatives, sans ouvrir de terminal. C’est très pratique pour les débutants ou ceux qui préfèrent les GUI. **TortoiseGit** est inspiré de TortoiseSVN, il est très complet et gratuit. Il nécessite Git pour Windows installé en arrière-plan, mais vous n’avez pas besoin de taper les commandes.

- **Visual Studio (2019, 2022) – Team Explorer / Git integration :** Visual Studio intègre nativement le support de Git. Vous pouvez cloner un repo depuis l’IDE, voir la liste des changements, effectuer des commits, pushes, pulls, gérer les branches et les conflits graphiquement. Visual Studio 2019 a introduit une nouvelle interface Git (fenêtre “Git”) plus moderne que l’ancien Team Explorer. Si vous utilisez **GitHub**, VS permet même la gestion intégrée des pull requests et des issues via l’extension GitHub. Microsoft a fortement amélioré l’expérience, à tel point que **GitHub support est maintenant intégré dans Visual Studio ([Visual Studio and GitHub - Microsoft](https://visualstudio.microsoft.com/vs/github/#:~:text=Visual%20Studio%20and%20GitHub%20,now%20built%20into%20Visual%20Studio))8】. Pour l’activer, il suffit d’ouvrir le menu **Git** dans VS (ou Team Explorer dans VS2017) quand vous avez un projet versionné. C’est utile pour rester dans son environnement de développement sans basculer vers un terminal ou un outil externe pour les opérations simples.

- **Visual Studio Code – Source Control** : VS Code (éditeur léger, à ne pas confondre avec Visual Studio IDE) possède un onglet “Contrôle de source” avec un support Git intégré. Vous voyez les modifications actuelles, vous pouvez stage/destage d’un clic, committer avec un champ de message intégré, et synchroniser (pull/push) facilement. Des extensions peuvent enrichir cette expérience (par exemple GitLens qui affiche l’historique inline dans l’éditeur). VS Code est très populaire, gratuit, et multi-plateforme, c’est un bon choix pour travailler avec Git de façon visuelle sans quitter l’éditeur.

- **GitKraken (client Git GUI)** : GitKraken est une application GUI multiplateforme (Electron) très appréciée pour sa **visualisation élégante** du graphe de commits et sa facilité d’utilisation. Elle propose une interface moderne pour créer des branches, résoudre des conflits via un merge tool visuel, rebaser en glisser-déposer, etc. La version de base de **GitKraken** est gratuite pour une utilisation avec des dépôts publics et loca ([GitKraken Desktop | Free Git GUI + Terminal | Mac, Windows, Linux](https://www.gitkraken.com/git-client#:~:text=Linux%20www,and%20access%20to%20premium))4】 (depuis 2022, l’utilisation avec des dépôts privés en contexte professionnel requiert une licence, sauf pour usage non commerci ([GitKraken is no longer free, Freak out, or Don't (founder's take) : r/git](https://www.reddit.com/r/git/comments/59j0yq/gitkraken_is_no_longer_free_freak_out_or_dont/#:~:text=r%2Fgit%20www,projects%3F%20use%20it%20for%20free))4】). Pour un usage open-source ou personnel, GitKraken est **free** et offre une expérience utilisateur fluide. Son graphe permet de comprendre rapidement les merges et rebases effectués. C’est un bon outil pour ceux qui sont allergiques à la ligne de commande ou qui veulent un visuel clair de leur historique. (À noter : GitKraken propose aussi des fonctionnalités supplémentaires comme un éditeur de Git hooks, ou une intégration avec les issues tracker, dans sa version payante).

- **Atlassian Sourcetree** : Sourcetree est un client Git gratuit pour Windows et Mac, édité par Atlassian (la société derrière Bitbucket, Jira…). **Sourcetree est gratuit même en usage commercial** et ne requiert plus de licence payan ([Sourcetree license (commercial use) - Atlassian Community](https://community.atlassian.com/t5/Sourcetree-questions/Sourcetree-license-commercial-use/qaq-p/1790335#:~:text=Community%20community,you%27re%20able%20to%20use))3】. Il simplifie l’utilisation de Git via une interface graphique convivia ([Sourcetree | Free Git GUI for Mac and Windows](https://www.sourcetreeapp.com/#:~:text=A%20free%20Git%20client%20for,you%20can%20focus%20on%20coding))4】. Vous pouvez y visualiser le graphe de commits, stage/destage partiellement des fichiers (par hunk), gérer les remotes, les submodules, etc. C’est un outil complet et robuste. Sourcetree convient bien pour gérer plusieurs repos (il a un gestionnaire de projets) et offre des diff et merge tools intégrés. C’est souvent le choix conseillé pour accompagner Bitbucket, mais il fonctionne avec n’importe quel serveur Git (GitHub, GitLab…).

- **GitHub Desktop** : Outil officiel de GitHub, open-source lui aussi. **GitHub Desktop** offre une interface très épurée pour les opérations Git de base et l’interaction avec GitHub. Il est particulièrement efficace pour les débutants ou pour ceux qui veulent juste cloner un repo GitHub, faire des modifications, et les pousser sans apprendre toutes les commandes. GitHub Desktop prend en charge les PR (il permet de les créer ou de les voir) et la gestion de base des conflits. C’est gratuit et ça fonctionne aussi avec n’importe quel dépôt Git, pas uniquement GitHub (on peut ajouter d’autres remotes). GitHub Desktop est **libre et gratuit ([About GitHub Desktop](https://docs.github.com/en/desktop/overview/about-github-desktop#:~:text=About%20GitHub%20Desktop%20GitHub%20Desktop,or%20other%20Git%20hosting%20services))6】. Il est moins riche en fonctionnalités que Sourcetree ou GitKraken, mais volontairement : il se concentre sur l’essentiel avec une UX simplifiée.

- **Autres outils notables** : *Git Extensions* (GUI Windows open-source, plus orienté “power user” avec énormément d’options), *Fork* (client Git premium avec une version d’essai gratuite très apprécié sur Mac et dispo sur Windows), *Tower* (client Git payant, longtemps Mac-only, maintenant sur Windows, connu pour son polish et ses guides Git), *Git Cola* (interface graphique légère multiplateforme), *SmartGit* (client payant riche en fonctionnalités)… Le choix d’un client Git GUI dépend des préférences. Beaucoup de développeurs finissent par utiliser une combinaison de ligne de commande et de GUI selon les situations.

En plus des clients Git, sous Windows, vous pouvez bénéficier de l’**intégration Shell** basique fournie par Git for Windows : l’installation offre “Git Bash” (un terminal type Unix avec les commandes Git) et “Git GUI” (une petite interface graphique de commit). Windows 10+ offre aussi la possibilité d’utiliser **WSL (Windows Subsystem for Linux)** et d’y installer Git nativement Linux, ce qui peut être utile pour certains workflows avancés.

Enfin, mentionnons **les plateformes en ligne** qui offrent des vues graphiques : sur GitHub, GitLab ou Bitbucket, l’interface web vous permet de voir l’historique des commits, de naviguer dans le code, de faire des comparaisons de branches (diffs), de commenter du code, etc. Ce ne sont pas des “outils Windows” en soi, mais ils font partie de l’expérience Git en équipe.

## Annexes : Checklists pratiques

Pour finir, voici des **checklists rapides** pour les actions Git courantes, afin de récapituler les étapes essentielles et éviter les oublis. Chaque checklist est suivie d’explications pour bien comprendre chaque point.

### Checklist : Faire un commit

1. **Sélectionner la bonne branche :** Assurez-vous d’être sur la branche appropriée (`git branch` pour lister, la branche actuelle est marquée par `*`). Si ce n’est pas le cas, changez de branche avec `git switch` ou `git checkout`. *(*Pourquoi ?*)* – Il serait fâcheux de committer sur `main` par mégarde ce qui devait aller sur une branche feature. Vérifiez toujours votre contexte avant de committer.

2. **Mettre à jour votre branche si besoin :** Si d’autres commits ont été ajoutés sur cette branche partagée ou sur la branche de base (ex: `main`), il peut être prudent de faire un `git pull` (ou `git pull --rebase`) avant de committer, surtout si vous travaillez à plusieurs sur la même branche. *(*Pourquoi ?*)* – Intégrer d’abord les changements distants permet de tester vos modifications avec les dernières updates et d’éviter des conflits surprises lors du push. Si votre branche n’est que locale, cette étape n’est pas nécessaire.

3. **Vérifier l’état du dépôt :** Lancez `git status` pour voir les fichiers modifiés. Décidez lesquels doivent être inclus dans le commit. *(*Pourquoi ?*)* – Parfois on a des modifications en cours qu’on ne souhaite pas encore committer. Le statut vous permet de voir clairement ce qui est en suivi ou non, et ce qui reste en suspens.

4. **Préparer (stager) les changements :** Utilisez `git add <fichier>` pour chaque fichier pertinent, ou des patterns (`git add .` pour tout, `git add src/` pour tout un dossier, etc.). Vous pouvez ajouter en plusieurs fois pour composer votre commit. *(*Pourquoi ?*)* – N’ajoutez que les changements liés au but de ce commit. Par exemple, n’ajoutez pas ce fichier de config modifié temporairement si ce n’est pas intentionnel. Vous pouvez aussi utiliser `git add -p` pour ajouter interactivement par portion de diff. L’idée est de **stager** uniquement ce qui doit aller ensemble.

5. **Revérifier le diff (optionnel mais recommandé) :** Avant de committer, un `git diff --staged` vous montrera exactement les changements qui seront enregistrés. *(*Pourquoi ?*)* – Cela permet de relire une dernière fois ce qu’on va committer et d’éviter “ah mince j’ai committé du code de débogage par erreur”. Prenez l’habitude de relire le patch, vous pouvez attraper des coquilles ou des oublis (ex: un morceau de code commenté que vous vouliez enlever).

6. **Commiter avec message clair :** `git commit -m "Message explicite"` ou sans `-m` pour éditer un message multi-ligne plus long. Respectez vos conventions de message (voir section dédiée). *(*Pourquoi ?*)* – Le message restera pour toujours dans l’historique, il doit donc avoir du sens. Incluez le *quoi* et idéalement le *pourquoi* du changement. Par exemple “Correctif du calcul de la taxe – résolution du bug #512” est beaucoup plus informatif que “fix bug” ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=While%20commit%20messages%20like%20these,author%20followed%20the%20previous%20advice)) ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=faire%20avec%20au%20minimum%203,commits%20bien%20distincts))6】

7. **Vérifier le résultat :** Faites un `git log -1` pour voir le dernier commit et confirmer qu’il correspond à vos attentes (bons fichiers, bon message). *(*Pourquoi ?*)* – Une double vérification évite de pousser un commit mal formé. Si le message a une typo ou si un fichier manque, vous pouvez corriger tout de suite (`git commit --amend` pour ajouter le fichier ou éditer le message, ou faire un commit additionnel si vous préférez).

8. **(Si nécessaire) Pousser la branche :** Ce n’est pas automatiquement dans “faire un commit”, mais souvent on enchaîne par un `git push`. Voyez la checklist “Push” ci-dessous. Si vous restez en local (travail en cours, WIP), inutile de push tout de suite. 

*Explications :* Cette checklist vise à assurer que chaque commit effectué est propre et utile. En particulier, les étapes de status/diff aident à ne pas committer accidentellement du code non désiré. Commiter souvent de petits changements vous permettra de localiser plus facilement l’introduction d’un bug en cas de problème (technique du *git bisect* par exemple), et vos coéquipiers vous remercieront pour des commits bien découpés et documentés.

### Checklist : Pusher des changements

1. **Tout votre travail local est committé :** Avant de pousser, assurez-vous que vous n’avez pas de modifications non committées (`git status` doit être “clean” ou seulement des changements non suivis sans importance). *(*Pourquoi ?*)* – Git ne poussera que les commits. Si vous avez des modifications en cours, elles ne partiront pas au serveur et pourraient créer des conflits plus tard. Si vous n’êtes pas prêt à committer, vous pouvez les stash, mais ne laissez pas traîner des modifs locales non synchronisées trop longtemps.

2. **Mettre à jour la branche locale (pull/rebase) :** C’est une bonne pratique de faire `git pull` (ou fetch + merge/rebase) juste avant un push, surtout sur la branche principale ou une branche partagée. *(*Pourquoi ?*)* – Si quelqu’un a poussé entre-temps, votre push sera rejeté tant que vous n’aurez pas intégré ses commi ([ Git Push | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/syncing/git-push#:~:text=remote%20repo,These%20issues%20are%20discussed%20below)) ([Poussée de commits vers un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/pushing-commits-to-a-remote-repository#:~:text=Gestion%20des%20erreurs%20autres%20que,de%20type%20avance%20rapide))3】. Autant le faire proactivement : récupérez les changements distants, gérez d’éventuels conflits en local, puis poussez. Cela garantit que vous poussez une version à jour. Si vous êtes seul sur la branche et sûr qu’elle n’a pas bougé, le pull peut être redondant, mais c’est une sécurité.

3. **Choisir la cible du push :** Identifiez le remote et la branche distants où vous voulez pousser. Par défaut, si votre branche locale est suivie (tracking) d’une branche distante, `git push` sans argument suffira. Sinon, utilisez `git push origin ma-branche`. *(*Pourquoi ?*)* – Parfois, on se trompe de remote (ex: pousser sur le mauvais dépôt) ou on oublie que le nom de la branche distante doit être spécifique. Vérifiez bien, surtout lors du premier push d’une nouvelle branche (il créera la branche distante du même nom, sauf si vous spécifiez un autre nom avant le `:`).

4. **Pousser (`git push`) :** Exécutez la commande de push. Observez la sortie : Git vous dira si ça a réussi ou non. En cas d’erreur *non-fast-forward*, cela signifie que votre branche locale est en retard sur la distante – retournez à l’étape 2 pour faire un pu ([Poussée de commits vers un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/pushing-commits-to-a-remote-repository#:~:text=Gestion%20des%20erreurs%20autres%20que,de%20type%20avance%20rapide))3】. *(*Pourquoi ?*)* – Un push réussi va mettre à jour le dépôt distant. Si c’est la branche principale, vos changements sont maintenant en prod (ou prêts à l’être). Si c’est une branche de feature, vos commits sont partagés avec l’équipe et une PR peut être ouverte.

5. **Vérifier sur la plateforme distante :** (Optionnel mais conseillé) Allez sur GitHub/GitLab/Bitbucket ou autre pour voir si vos commits apparaissent bien et si les pipelines CI passent. *(*Pourquoi ?*)* – Un dernier contrôle permet de repérer tout de suite si quelque chose ne va pas (par ex, vous avez poussé la mauvaise branche, ou la CI signale un test cassé). Il vaut mieux corriger rapidement après un push qu’apprendre 3 jours plus tard qu’on a cassé la build.

6. **Ne forcez pas le push sauf raison majeure :** Si Git refuse le push car “divergence”, ne faites pas `--force` spontanément. Suivez le conseil du message d’erreur : faites un pull pour fusionner les changements distants, puis retentez. N’envisagez `--force` que si vous savez exactement ce que vous faites (réécriture d’historique intentionnelle sur une branche non partagée ou après coordination avec l’équip ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=,Force%20Push)) ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=Force%20Push%20is%20a%20very,will%20no%20longer%20be%20current))6】.

*Explications :* Pusher semble trivial, mais c’est souvent là que les problèmes de synchronisation apparaissent. En intégrant toujours les nouveautés avant de pousser, vous évitez d’écraser le travail des autres par inadvertance. Cette discipline maintient l’arbre distant cohérent. Rappel : un push envoie *tous* vos commits locaux en attente sur la branche cible. Si vous avez fait plusieurs commits localement, ils partent tous. D’où l’importance qu’ils soient bien propres (checklist commit) car ils seront visibles de tous.

### Checklist : Ouvrir une Pull Request (et la mener à bien)

1. **Préparer la branche :** Assurez-vous que votre branche de travail contient **tous les commits liés à la fonctionnalité/bug** que vous voulez proposer. Nettoyez l’historique si nécessaire (par ex., via rebase interactif pour squash les commits “bruit” du genre fix typo) – *optionnel*. *(*Pourquoi ?*)* – Une PR est plus facile à accepter si l’historique est clair. Cependant, ce nettoyage n’est pas obligatoire ; c’est souvent accepté d’avoir plusieurs petits commits tant qu’ils sont bien nommés. Si vous débutez, ne passez pas trop de temps à réécrire l’historique, concentrez-vous sur le code en lui-même.

2. **Mettre à jour avec la branche cible :** Synchronisez votre branche avec la dernière version de la branche vers laquelle vous ferez la PR (souvent `main` ou `develop`). Faites `git pull origin main` dans votre branche (ou rebase sur main). *(*Pourquoi ?*)* – Cela garantit que votre code intègre les changements récents et réduit les risques de conflits. De plus, vous pourrez tester l’intégration en local. Si vous mergez une PR et qu’entre-temps main a bougé, la plateforme essaiera de merger mais peut signaler un conflit – autant le résoudre avant d’ouvrir la PR.

3. **Vérifier que tout passe (tests, lint) :** Exécutez la suite de tests en local si elle existe, ou au minimum compilez/vérifiez le fonctionnement. *(*Pourquoi ?*)* – Une PR qui échoue direct les tests CI ou qui casse l’application aura du mal à être acceptée. Montrez que vous avez validé votre travail. 

4. **Pousser la branche sur le remote :** Voir checklist “Push” précédente. Si c’est la première fois pour cette branche, `git push -u origin ma-branche` pour créer la branche distante et la lier. *(*Pourquoi ?*)* – Évidemment, la PR se base sur la branche distante, pas votre dépôt local. 

5. **Aller sur l’interface Git de votre forge pour créer la PR :** Sur GitHub, un bouton “Compare & pull request” apparaît généralement après un push récent. Sinon, cliquez sur “New pull request” et choisissez votre branche comme source et la branche cible (ex: base: main <- compare: feature/branchement). *(*Pourquoi ?*)* – C’est là que vous démarrez officiellement la PR. 

6. **Rédiger le titre et la description de la PR :** Comme conseillé, écrivez un **titre concis** (par ex. “[Feature] Ajout de l’authentification Google”) et une description détaillée. Mentionnez le problème résolu (“Closes #123 si vous voulez fermer une issue liée), le contexte, éventuellement une capture d’écran si c’est une UI, ou des étapes de reproduction pour un bug. *(*Pourquoi ?*)* – Plus vous facilitez le travail du relecteur, plus la review sera effica ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=A%20lot%20can%20be%20said,each%20update%20to%20the%20project))0】. C’est l’occasion d’expliquer les choix techniques si besoin (“J’ai utilisé telle lib car…, J’ai dû changer telle API, etc.”).

7. **Assigner des reviewers/labels/projet :** Suivant votre organisation, assignez la PR à un ou plusieurs développeurs pour relecture. Ajoutez des labels (ex: “backend”, “urgent”, …) ou liez à une milestone si votre outil le permet et que c’est pertinent. *(*Pourquoi ?*)* – Assigner formalise la demande de relecture. Sans destinataire, il y a un risque que personne ne prenne en charge la review. 

8. **Participer activement à la revue :** Une fois la PR ouverte, surveillez les commentaires. Répondez aux questions, engagez la discussion si on vous suggère des changements. *(*Pourquoi ?*)* – Une PR est collaborative. Si un reviewer prend du temps pour un retour, traitez-le avec attention. S’il pointe un problème, corrigez-le dans de nouveaux commits (ou amendez les commits existants si la politique le permet) puis poussez ces changements. GitHub les ajoutera à la PR automatiquement.

9. **Gérer les retours et les tests CI :** Si la CI signale des erreurs, corrigez-les (ajoutez des commits). Si un reviewer demande des modifications, effectuez-les puis **faites un push**. Eventuellement, marquez la conversation comme résolue ou laissez un commentaire. *(*Pourquoi ?*)* – La PR n’est pas statique, elle évolue jusqu’à ce qu’elle soit acceptable. Montrez que vous prenez en compte les retours. 

10. **Obtenir l’approbation :** Après corrections, un ou plusieurs relecteurs devraient approuver la PR (review “Approved” sur GitHub). Assurez-vous que personne d’autre ne voulait la relire (selon vos règles, peut-être 2 approbations requises). *(*Pourquoi ?*)* – C’est le feu vert pour intégrer. Sans approbation, ne fusionnez pas (sauf cas d’auto-merge autorisé et trivial, mais en général on attend au moins un avis).

11. **Fusionner la PR (merge)** : Utilisez le bouton “Merge” via l’interface (ou “Rebase and merge” / “Squash and merge” selon le mode choisi). Si vous avez l’option de supprimer la branche après coup, cochez-la le cas échéant. *(*Pourquoi ?*)* – La fusion via l’interface enregistre automatiquement les références de la PR (sur GitHub, ça ajoute un message “Merge pull request #Num”). Votre code est maintenant dans la branche cible. 

12. **Supprimer la branche** : Si ce n’est pas fait auto, supprimez la branche distante depuis l’interface ou via `git push origin --delete nom-branche`. Et en local, vous pouvez aussi la supprimer : `git branch -d nom-branche`. *(*Pourquoi ?*)* – Pour nettoyer comme évoqué. La branche ne sert plus, on la retire pour ne pas polluer.

13. **Communiquer si nécessaire** : Annoncez sur le canal adéquat (Slack, etc.) que la feature est mergée, surtout si cela nécessite une action (déploiement manuel, information aux QA, etc.). *(*Pourquoi ?*)* – Tout le monde ne suit pas en temps réel les merges Git. Une petite note “Feature X est maintenant merge dans main, sera déployée ce soir” aligne l’équipe.

*Explications :* Ce processus peut sembler lourd mais c’est la routine d’une bonne collaboration. Les étapes 1-4 sont techniques (préparer votre code). Les étapes 5-7 sont la création formelle de la PR. 8-9 couvrent l’itération pendant la review. Et 10-13 la conclusion/mise en prod. En suivant cela, vous aurez des PR bien tenues, ce qui accélère les revues et améliore la qualité globale du code intégré.

### Checklist : Fusionner (merge) une branche manuellement

*(Ce cas s’applique si vous mergez sans PR, par exemple en ligne de commande, ou pour intégrer `main` dans votre branche feature en local.)*

1. **Se placer sur la branche cible du merge :** `git checkout branche-cible` (par ex. `main` si vous voulez merge une feature dedans, ou votre feature si vous voulez merge `main` dans celle-ci). *(*Pourquoi ?*)* – La commande de merge va intégrer une autre branche *dans* la branche courante. Soyez donc sûr de vous trouver sur celle qui doit recevoir les changements.

2. **Mettre à jour la branche cible :** Si c’est une branche distante, faites un `git pull` pour qu’elle soit à jour. Si vous allez merger `origin/main` dans votre feature, assurez-vous d’abord que vous avez récupéré la dernière version de main : `git fetch origin` puis `git merge origin/main` ou `git pull origin main` sur votre feature. *(*Pourquoi ?*)* – On veut merger du code à jour. Si vous mergez une version périmée, vous risquez de devoir refaire un merge plus tard pour compléter.

3. **Exécuter le merge :** `git merge branche-source`. Git va tenter un **Fast-forward** si possible (c.-à-d. juste avancer le pointeur, pas de commit de merge) ou créer un commit de merge si les deux branches ont divergé. *(*Pourquoi ?*)* – C’est l’action de fusion proprement dite, combinant les historiques. Si tout va bien, vous aurez “Already up to date” (si rien à merger) ou “XX files changed” etc. 

4. **Gérer les conflits s’il y en a :** Si Git indique “Merge conflict in ...”, ouvrez les fichiers listés, résoudre les conflits comme expliqué précédemment (garder la bonne version, etc.). Ensuite, `git add` les fichiers résolus et terminez avec `git commit` pour valider le merge. *(*Pourquoi ?*)* – Un merge conflict stoppe le processus, c’est à vous de le régler. Tant que ce n’est pas fait, la fusion n’est pas finalisée.

5. **Vérifier le résultat :** Regardez le log (`git log --graph`) pour voir si le commit de merge apparaît (s’il y en a un). Compilez et testez votre code dans la branche resultante, surtout si les changements intégrés étaient nombreux. *(*Pourquoi ?*)* – On veut s’assurer que la fusion n’a pas introduit de régression ou cassé quelque chose. Par ex., si vous avez mergé main dans votre feature, testez votre feature avec le nouveau code de main intégré.

6. **Pousser la branche mise à jour (si branche distante) :** Si vous avez mergé localement, n’oubliez pas de pousser la branche cible vers le serveur pour partager le résultat. *(*Pourquoi ?*)* – Le merge local n’affecte que votre dépôt jusqu’à ce que vous le poussiez. Par exemple, si vous mergez une PR localement sans utiliser l’interface GitHub, il faut ensuite push main avec le commit de merge.

*Explications :* Ce processus est assez direct. La plupart du temps, on utilise la PR sur les plateformes pour faire le merge, mais savoir le faire en local est utile aussi (par ex. pour tester un merge avant de l’envoyer, ou pour intégrer rapidement les changements de main dans une branche en cours de dev). L’important est de toujours vérifier le contexte (qui reçoit quoi) et de tester après coup.

### Checklist : Rebaser une branche

1. **Sauvegarder le travail en cours :** Si vous avez des modifications non committées sur la branche que vous comptez rebaser, soit commitez-les, soit stash-les (`git stash`). *(*Pourquoi ?*)* – Le rebase ne fonctionne que sur des commits, il n’aime pas les modifications en cours. De plus, en cas de pépin, c’est mieux de ne rien risquer. Donc un `git status` clean avant de commencer.

2. **Mettre à jour la branche de base :** Par exemple, si vous allez faire `git rebase main` sur votre feature, assurez-vous que `main` local est synchronisé avec `origin/main` (faites un `git fetch origin` puis `git checkout main; git pull` si besoin). *(*Pourquoi ?*)* – On veut rebaser par rapport à la dernière version. Rebaser sur une base obsolète n’a aucun intérêt, autant prendre la plus récente.

3. **Se positionner sur la branche à rebaser :** `git checkout feature`. C’est cette branche dont les commits vont être rejoués ailleurs. *(*Pourquoi ?*)* – On applique la commande rebase depuis la branche qu’on déplace.

4. **Lancer le rebase :** `git rebase main` (ou la branche de base correspondante). Git va alors : partir du point de divergence de feature par rapport à main, prendre tous les commits de feature depuis ce point, les mettre de côté, puis avancer feature pour qu’elle pointe sur la tête de main, et enfin rejouer chaque commit de feature à la suite. *(*Pourquoi ?*)* – Le but final est que la branche feature ait le même contenu que si on l’avait créée à partir du HEAD actuel de main, sans merge commit. Cela rend l’historique linéai ([ Git rebase | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase#:~:text=))7】.

5. **Résoudre les conflits au fur et à mesure :** Si des commits de feature entrent en conflit avec main, Git stoppera en indiquant le fichier en conflit. Résolvez-le (éditez, `git add`), puis faites `git rebase --continue`. Cette étape peut se répéter pour plusieurs commits s’il y a plusieurs conflits distincts. *(*Pourquoi ?*)* – Le rebase applique commit par commit, donc vous pourriez avoir des conflits sur plusieurs commits différents. Prenez-les un par un. Si vraiment le rebase est trop conflictuel, vous pouvez l’abandonner avec `git rebase --abort` pour revenir à l’état initi ([Obtention de modifications à partir d’un dépôt distant - Documentation GitHub](https://docs.github.com/fr/get-started/using-git/getting-changes-from-a-remote-repository#:~:text=%C3%89tant%20donn%C3%A9%20que%20,se%20trouvait%20avant%20le%20tirage))4】.

6. **Terminer le rebase :** Quand Git a fini de réappliquer tous les commits, il vous rend la main, généralement avec un message du genre “Successfully rebased and updated <branch>”. *(*Pourquoi ?*)* – À ce point, votre historique local est réécrit. La branche feature a maintenant de nouveaux commits (avec de nouveaux SHAs). 

7. **Vérifier l’histoire et tester :** Faites un `git log` pour voir les commits de votre feature, ils devraient maintenant succéder aux commits de main. Testez votre code pour s’assurer que rien n’a été involontairement altéré pendant le rebase. *(*Pourquoi ?*)* – Normalement, comme on n’a fait que rejouer les mêmes changements, tout devrait être fonctionnellement identique. Néanmoins, si vous aviez résolu des conflits en choisissant une implémentation, assurez-vous que tout compile/passe les tests.

8. **Forcer le push de la branche rebasée :** Comme l’historique a changé, un `git push` normal sera refusé (le serveur ne reconnaît pas vos nouveaux commits vis-à-vis des anciens). Vous devez faire `git push --force-with-lease origin feature`. *(*Pourquoi ?*)* – Le `--force-with-lease` permet d’écraser la branche distante *si* elle n’a pas bougé depuis votre dernier pu ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=which%20will%20no%20longer%20be,current))9】. C’est plus sûr que `--force` nu. Après cela, la branche distante aura l’historique réécrit. **Attention** : si entre temps quelqu’un d’autre avait poussé sur cette branche, ses commits risquent d’être perdus. C’est pourquoi il faut communiquer et s’assurer qu’on est seul dessus dans ce cas (ou bien intégrer aussi ses commits dans le rebase si possible).

9. **Nettoyer (stash)** : Si vous aviez mis des changements en stash à l’étape 1, faites un `git stash pop` pour les récupérer une fois le rebase fini (en s’assurant qu’ils s’appliquent proprement). *(*Pourquoi ?*)* – Pour reprendre votre travail là où vous l’aviez laissé, s’il était en pause.

*Explications :* Le rebase est puissant pour garder un historique propre et à jour, mais il faut le manier avec précaution. Cette checklist aide à ne pas oublier de mettre à jour la base, ni de push correctement après coup. Rappelez-vous la règle d’or : **ne rebase pas du code déjà partagé publiquement** sans coordination. Pour un travail local, c’est super, pour un travail collaboratif, assurez-vous que tout le monde est OK avec la réécriture. Si vous rebasez une branche à plusieurs, informez vos collègues qu’ils devront peut-être recloner ou récupérer la nouvelle branche de force.

---

En suivant ces checklists et bonnes pratiques, vous devriez éviter les erreurs les plus communes et rendre votre utilisation de Git beaucoup plus fluide et efficace. Git est un outil incontournable du développement logiciel moderne, et investir du temps pour bien le maîtriser porte ses fruits sur la durée : historique clair, collaboration sereine, déploiements sans stress. Bon Git et bon code à vous ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Avoir%20une%20branche%20develop%20et,master%20non%20fonctionnelle)) ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=1,Changes%20Together))3】


# Git - Cheat Sheet
![Diagramme](/diagram.png)

## COMMANDE WINDOWS
	Liens:
  	https://docs.microsoft.com/fr-fr/windows-server/administration/windows-commands/windows-commands
	
  	Commandes:
	    - pwd : pour connaitre le directory
	    - mkdir : pour créer un repertoire
	    - ls : pour connaitre contenu du repertoire ou ls nomrep
	    - ls -la pour afficher que les dossier caché
	    - cd : deplacer ex.: cd nomrep 
	    - cd.. : pour reculer d'un rep

## COMMANDE GIT DIVERS
	- Q : touche clavier "Q" pour sortir des log si trop long par exemple

	- Verifier la version de git
		Commandes:
		git version

## CONFIGURATION

	- Configurer les informations de l'utilisateur pour le dépôt courant ou pour tous avec l’option --global
		
		Liens:
		https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration
		https://git-scm.com/docs/git-config
		
		- Définit le nom de l’utilisateur
			Commandes:
			git config --global user.name "jacques gariepy"
	
		- Définit l’email de l’utilisateur
			Commandes:
			git config --global user.email jacques.gariepy@...
	
		- Répertoriez toutes les variables définies dans le fichier de configuration, ainsi que leurs valeurs.
			Commandes:
			git config --list

## GESTION DES DÉPÔTS

	- Initialisation
		Cette commande crée un référentiel Git vide - essentiellement un répertoire avec des sous-répertoires
		
		Liens:
		https://git-scm.com/docs/git-init
		
		repository git contient : 	
		- derniere version
		- dossier .git
		- Historique
		- Zone d'index
		- Autre informations gestion
			
		Commandes:
		git init

		Étapes d'initialisation
		1. mkdir monrep = Crée un répertoire ou un sous-répertoire
		2. cd monrep = Affiche le nom du répertoire actif
		3. git init : crée
		
	- Clone d'une branche
		Duplique en local le dépôt git pointé par l’url. cibler un dépôt existant et créer un clone ou une copie du dépôt cible. 
		Prendre une copie local d'un repository décentralisé. Il faut se placer dans le répertoire root des depot local
		
		Liens:
		https://www.git-scm.com/docs/git-clone
		https://www.atlassian.com/fr/git/tutorials/setting-up-a-repository/git-clone

		Commandes :
		git clone url nomrep
		git clone https://github.com/JacquesGariepy/HelloWord.git clone_hello_word
			
	- Référentiels distants.
		Ajoute comme remote le dépôt pointé par l’url.
		Les référentiels distants sont des versions de votre projet qui sont hébergées sur Internet ou sur un réseau quelque part. 
		La commande git remote vous permet de créer, d'afficher et de supprimer des connexions avec d'autres dépôts.
		
		Liens:
		https://git-scm.com/docs/git-remote
		https://www.atlassian.com/fr/git/tutorials/syncing
			
		Commande :
		git remote 
		git remote -v : permet d'afficher la liste des remote
		git remote show <remote> :  permet d'avoir les informations complémentaires sur un remote
		git remote show origin
		git remote add origin https://github.com/JacquesGariepy/test.git


## GÉRER LES MODIFICATIONS

	- Liste tous changements apportés à l’espace de travail. Donne l'etat actuel de notre environnement
		
		Liens:
		https://git-scm.com/docs/git-status
		https://www.atlassian.com/fr/git/tutorials/inspecting-a-repository
		
		Commandes :
		git status

	- Supprime les modifications courantes du fichier.
		
		Commandes:
		git checkout ​<file>
		git checkout index.html
		
	- Indexation des modifications
		Cette commande met à jour l’index à l’aide du contenu actuel trouvé dans l’arborescence de travail
		
		Liens:
		https://git-scm.com/docs/git-add
		
		Commandes:
		git add : envoyer les fichiers dans l'indexation

		Étapes :
		git add ​--patch​ ​<file> : Ajoute les modifications d’un fichier a la zone d’index. --patch permet d’ajouter une partie seulement du fichier.
		git add index.html style.css
		git add . (ajouter tout dans l'index)

	-  Enlève les modifications d’un fichier de l'index, mais conserve son contenu. --patch permet d’enlever une partie seulement du fichier.
		
		Liens:
		https://git-scm.com/docs/git-reset/
		
		Commandes:
		git reset ​--patch​ [fichiers] :

	- Montre les modifications du fichier non indexé, pour un fichier indexé, ajouter l’option --staged.
		
		Commandes:
		git diff​ --staged ​​<file>
		git diff​ --staged ​index.html
		
	- Commit
		Enregistre les modifications présentes dans la zone d’index dans l’historique du dépôt
		Contient le contenu actuel de l’index et le message de journal donné décrivant les modifications.
		
		Liens:
		https://git-scm.com/docs/git-commit
		https://www.atlassian.com/fr/git/tutorials/saving-changes/git-commit
			
		Commandes:
		git commit -m "​<message>"
		git commit -m"Mon commit"

	-Étapes merge d'un fichier
	
		Commandes:
		git stash
		git pull
		git stash pop
		modifier le fichier local et s'il y a une modification serveur il y aura un merge a faire et tag seront mis dans le fichier. modifier le fichier en consequence en enlevant les tags
		git add . (ou le nom du fichier)
		git commit -m"ajout commentaire"
		git push

## SYNCHRONISER LES CHANGEMENTS
	
	- Mettre à jour les informations locales concernant leserveur distant.
		La commande git fetch Télécharger des objets et des refs à partir d’un autre référentiel
		La commande télécharge les validations, les fichiers et les références d’un référentiel distant dans votre référentiel local.
	
		Liens:
		https://git-scm.com/docs/git-fetch	
		https://www.atlassian.com/git/tutorials/syncing/git-fetch
		
		Commandes:
		git fetch <remote> 
		git fetch origin

	- Mettre à jour son dépôt local avec les commits présents sur le serveur distant.
		La commande git pull Recupere les mise a jour du serveur et les appliquer a notre repository local
		Intègre les modifications d’un référentiel distant dans la branche actuelle. 
		
		Liens:
		https://www.git-scm.com/docs/git-pull
		https://www.atlassian.com/fr/git/tutorials/syncing/git-pull
		https://www.atlassian.com/fr/git/tutorials/merging-vs-rebasing

		Commandes:
		git pull <remote> <branch> 
		git pull origin
		git pull --rebase (https://coderwall.com/p/7aymfa/please-oh-please-use-git-pull-rebase)

	- Envoie les commits en local sur le serveur distan
		Mettre à jour les références distantes avec les objets associés
		Utilisée pour charger le contenu d'un dépôt local vers un dépôt distant.
		
		Liens:
		https://git-scm.com/docs/git-push
		https://www.atlassian.com/fr/git/tutorials/syncing/git-push
		
		Commandes:
		git push ​<remote>  <branch> 
		git push -u origin master
		git push -u origin montags
		git push -u origin --tags

		Étapes ajout d'un fichier:
		git add Readme.md
		git commit -m"Ajout du fichier Readme.md"
		git push

## GESTION DE L’HISTORIQUE

	- Consulter l'historique des commits pour la branche courante. -n pour limiter aux n derniers commits.
		La commande git log affiche les instantanés validés. Il est utilisé pour répertorier et filtrer l’historique du projet et rechercher des modifications particulières.
		
		Liens:
		https://www.git-scm.com/docs/git-log
		https://www.atlassian.com/git/tutorials/git-log
		
		Commandes:
		git log​ [-n]
		git log : afficher l'historique
		git log -n 2 : affiche que les 2 derniers commit

	- Consulter les différences de contenu entre deux commits
		La commande git diff prend deux ensembles de données et génère une sortie révélant les changements entre eux. git diff est une commande Git multi-usage qui exécute une fonction de différenciation sur des sources de données Git
		
		Liens:
			https://git-scm.com/docs/git-diff
			https://www.atlassian.com/fr/git/tutorials/saving-changes/git-diff
			
		Commandes:
			git diff [tag|sha1] [tag|sha1]
			git diff index.html
			git diff --cached : permet de revoir les diff apres indexage

	- Consulter les modifications enregistrés lors du commit spécifié.
		Montre les modifications enregistrés lors du commit spécifié.
		Afficher des détails étendus sur les objets Git tels que les objets blob, les arborescences, les balises et les validations
		
		Liens:
		https://git-scm.com/docs/git-show
		https://www.atlassian.com/git/tutorials/git-show
	
		Commandes:
		git show 752c14ea195c369bac3c3b7896975ee9fd15eeb7 : pour recuper sha-1 click sur le sha-1 et appuyer dbl click sur la molette de la souriswaqw
		git show master : positionne sur le dernier commit. utilisation des tags pour afficher l'historique au lieu du sha-1

	- Consulter l'état d'un dépôt à un tag donné en utilisant la commande git checkout.
	
		Commandes:
		git checkout [tag|sha1|branche]
		git checkout v1.4
			
	- Créer un tag sur le commit courant
		Marquer des points spécifiques dans l’historique d’un référentiel comme étant importants
		Les tags sont des réfs qui pointent vers des points spécifiques de l'historique Git. Les tags sont généralement utilisés pour capturer un point de l'historique utilisé pour une version marquée (c.-à-d., v1.0.1). Un tag est similaire à une branche qui ne change pas.
		
		Liens:
		https://git-scm.com/book/en/v2/Git-Basics-Tagging
		https://www.atlassian.com/fr/git/tutorials/inspecting-a-repository/git-tag
			
		Commande :
		git tag monnomdetag : intitulé et un pointeur. head est l'endroit ou on est rendu dans les commits et ce tag suit les commits. il est possible d'ajouter des tags d'informations
		git checkout 752c14ea195c369bac3c3b7896975ee9fd15eeb7 : basculer sur un tag
		git tag monnomdetag : creer un tag 
		git tag monnomdetag -m"commentaire" : creer un tag avec commentaire
			
		Supprimer un tag
		git tag --d monnomdetag
		git tag [nom_tag]

	-Détail modifications
		Afficher quelle révision et quel auteur ont modifié en dernier chaque ligne d’un fichier
		
		Liens:
		https://www.git-scm.com/docs/git-blame
		https://www.atlassian.com/fr/git/tutorials/inspecting-a-repository/git-blame

		Commandes:
		git blame : voir les detail des modifications d'un fichier	
		git blame index.html
		git blame -L 10,20 index.html
		git blame -L 10,+4 index.html

## RÉÉCRIRE L’HISTORIQUE
	
	- Ajoute au dernier commit le contenue de la zone d’index et change le message de commit.
		
		Commandes :
		git commit --amend
		
	- Annuler certains commits existants
		Créer un nouveau commit qui est l'opposé du commit spécifié afin d’annuler ses effets.
		La commande peut être considérée comme une commande de type 'undo', cependant, il ne s’agit pas d’une opération d’annulation traditionnelle.
		
		Liens:
		https://git-scm.com/docs/git-revert
		https://www.atlassian.com/git/tutorials/undoing-changes/git-revert
			
		Commandes:
		git revert [commit]
		git revert [commit]

	- Reset/Annule tous les commits après `[commit]`, en conservant les modifications localement.
		La commande git reset est un outil complexe et polyvalent pour annuler les changements.
		Elle peut être appelée de trois façons, qui correspondent aux arguments de ligne de commande --soft, --mixed et --hard.
		
		Liens:
		https://git-scm.com/docs/git-reset
		https://www.atlassian.com/fr/git/tutorials/undoing-changes/git-reset
		
		Commandes:
		git reset index.html
		git reset <sha1-commit-hash> <file-path>
		git reset [commit] : Annule tous les commits après `[commit]`, en conservant les modifications localement.
		git reset --hard [commit] : Supprime tout l'historique et les modifications effectuées après le commit spécifié.

	- Réordonne, fusionne, supprime, modifie les n dernier commits local.
		Le rebasage est le processus de déplacement ou de combinaison d’une séquence de commits vers un nouveau commit de base.
		A ne pas faire sur des commit poussé sur le remote.
		
		Liens:
		https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase
		
		Commandes:
		git rebase –i HEAD~n

## TRAVAILLER EN BRANCHE

	- Répertorier branches, créer branche, supprimer branche et basculez entre les branches
		
		Liens:
		https://git-scm.com/docs/git-branch
		https://www.atlassian.com/git/tutorials/using-branches

		- Liste toutes les branches locales dans le dépôt courant. 
	
			Commandes:
			git branch​ -r -a : -r pour les branche distantes. -a pour tout
			
		- Création d'une branche :
			
			Commandes:
			git branch <branch>
			git branch dev
			git checkout -b dev : créer et ce positionne sur la branche
			
		Supression d'une branche locale :
			
			Commandes:
			git branch -d​  <branch>
			git branch -d​ dev
			
		- Supprimer une branche distante (remote)		
			
			Commandes:
			git push <remote> --delete <branch>
			git push origin --delete dev

		- Pour faire le lien entre le depot local et le remote
		
			Liens:
			https://devconnected.com/how-to-set-upstream-branch-on-git/
			
			Commandes:
			git push -u <remote> <branch>
			ou
			git push --set-upstream <remote> <branch>

		- Basculez vers une branche spécifiée. 
			L’arborescence de travail et l’index sont mis à jour pour correspondre à la branche. 
			Tous les nouveaux commits seront ajoutés à la pointe de cette branche.
			
			Liens:
			https://git-scm.com/docs/git-switch/2.23.0
			
			Commandes:			
			git switch <branch>
			git switch -dc <branch>
			git switch -c <branch>
			
	- Change de branche et ce placer sur le dernier commit de celle-ci. 
		-b pour en plus créer la branche.
		
		Commandes :
		git checkout ​-b​ <branch>

		- Affecter les changements d'une branche a une autre par les tags. Appliquer les modifications introduites par certaines validations existantes
		Étapes :
		Se placer sur la branche main via git checkout master, récupérer le sha-1 du tag via git log dev, ensuite faire le cherry-pick sha-1 et ensuite faire un push
		git checkout master
		git log dev
		git cherry-pick 752c14ea195c369bac3c3b7896975ee9fd15eeb7 (sha-1 de la branche de dev a implanter dans la branche master)
		git push
	
	- Merge de branche
		Combine dans la branche courante l'historique de la branche spécifiée via un commit de merge.
		Joindre deux ou plusieurs historiques de développement ensemble
		
		Liens:
		https://git-scm.com/docs/git-merge
		https://www.atlassian.com/git/tutorials/using-branches/git-merge
		https://www.atlassian.com/fr/git/tutorials/merging-vs-rebasing

		Commandes :
		- merge entre 2 branches :
		git merge BRANCH_A_MERGER

		- pour avorter un merge :
		git merge --abord
		
	- Déplace les commits de la branche courante sur la branche spécifiée.
		Le rebasage consiste à déplacer vers un nouveau commit de base ou à combiner une séquence de commits.
		
		Liens:
		https://git-scm.com/book/en/v2/Git-Branching-Rebasing
		https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase

		Commandes :
		git rebase [autre_branche] (la branche qui recoit le merge, ex dev vers master)
		git rebase master 

		Étapes:
		git checkout dev (ce placer sur la branche a merger)
		git rebase master (faire le rebase sur la branche cible)
			editer le fichier s'il y a un conflit
			git add fichier.txt
		git rebase --continue
		git checkout master (ce placer sur la branche qui a été merger)
		git merge dev

	- Applique un commit à l’espace de travail.
		Correspond au fait de sélectionner un commit d'une branche et de l'appliquer à une autre.
		-x permet d’ajouter le message « cherry-picked from commit [sha1_commit] ».
	
		Liens:
		https://git-scm.com/docs/git-cherry-pick
		https://www.atlassian.com/fr/git/tutorials/cherry-pick
			
		Commandes :
		git cherry-pick ​[-x]​ [commit]
		git cherry-pick master

## METTRE DE CÔTÉ DES MODIFICATIONS

	-Remiser/Rangez les modifications dans un répertoire de travail 
	git stash met temporairement en rayon (ou cache) les modifications que vous avez apportées à votre copie de travail afin de pouvoir travailler sur autre chose
		
		Liens:
		https://www.git-scm.com/docs/git-stash
		https://www.atlassian.com/git/tutorials/saving-changes/git-stash

		Commandes:
		git stash :  modifications remisées. lorsque vous voulez enregistrer l’état actuel du répertoire de travail et de l’index, mais que vous voulez revenir à un répertoire de travail propre. La commande enregistre vos modifications locales et rétablit le répertoire de travail pour qu’il corresponde au commit HEAD. https://git-scm.com/docs/git-stash/fr
			
		git stash save "modification de xyz" : Enregistre toutes les modifications courante dans une pile temporairement.
		git stash list : Liste toutes modifications mises de côté.
		git stash show id  : 0 represente l'index cd 
		git stash pop id : Appliquer les modifications à l’index id de la pile dans l’espace de travail. sinon id = 0
		git stash drop ​id : Supprime les modifications à l’index id de la pile. sinon id = 0
	
## BASCULER ENTRE VERSION
	Un « checkout » désigne un basculement entre différentes versions d'une entité cible. La commande git checkout agit sur trois entités distinctes : les fichiers, les commits et les branches
	
	Liens:
	https://git-scm.com/docs/git-checkout
	https://www.atlassian.com/fr/git/tutorials/using-branches/git-checkout
	
	Commandes:
	git checkout dev
	git checkout 752c14ea195c369bac3c3b7896975ee9fd15eeb7 : un « checkout » désigne un basculement entre différentes versions d'une entité cible
	git checkout -b 752c14ea195c369bac3c3b7896975ee9fd15eeb7 : permet de créer une branche avec le tag sha-1 et se deplacer dessus

## Nettoyage
	La commande git prune est un utilitaire d’entretien interne qui nettoie les objets Git inaccessibles ou « orphelins ». Les objets inaccessibles sont ceux qui sont inaccessibles par n’importe quelle réf. Dans la plupart des cas, les utilisateurs n’auront pas besoin d’appeler git prune directement, mais devraient plutôt appeler git gc, qui gère l’élagage ainsi que de nombreuses autres tâches d’entretien ménager.
	
	Liens :
	https://git-scm.com/docs/git-prune
	https://www.atlassian.com/git/tutorials/git-prune
	
	Commandes:
	git gc
	git prune -n --dry-run (Il n’exécutez pas la commande, il montre une sortie de ce qu’il fera)
	git prune -v --verbose (Afficher la sortie de tous les objets et actions effectués) 
	git prune $(cd ../another && git rev-parse --all)
	git prune --progress (Affiche la sortie qui indique la progression de l’élagage)
	git prune --expire <time> (Forcer l’expiration des objets passés)
	git prune <head> (La spécification d’un conservera toutes les options de cette référence de tête)
	
## IGNORER DES ÉLÉMENTS (.gitignore)
	Fichier qui spécifie les fichiers intentionnellement non suivis à ignorer
	Les fichiers ignorés sont généralement des artefacts de build et des fichiers générés par la machine qui sont dérivés de votre dépôt source ou qui ne devraient pas être commités. 
	
	Liens:
	https://www.git-scm.com/docs/gitignore
	https://www.atlassian.com/fr/git/tutorials/saving-changes/gitignore
	
	Commandes:
	touch .gitignore
	touch ~/.gitignore
	git config --global core.excludesFile ~/.gitignore

## Workflow quotidien pour un développeur

### Initialisation d'un dépôt

1. Créez un nouveau répertoire pour votre projet :
   ```
   mkdir monprojet
   cd monprojet
   ```

2. Initialisez un nouveau dépôt Git :
   ```
   git init
   ```

### Faire des modifications

1. Créez ou modifiez des fichiers dans votre répertoire de projet.

2. Vérifiez l'état de votre répertoire de travail :
   ```
   git status
   ```

3. Ajoutez les modifications à la zone de staging :
   ```
   git add <fichier>
   git add .
   ```

### Commit des modifications

1. Committez les modifications avec un message descriptif :
   ```
   git commit -m "Votre message de commit"
   ```

### Pousser les modifications

1. Ajoutez un dépôt distant (si ce n'est pas déjà fait) :
   ```
   git remote add origin <url_distant>
   ```

2. Poussez les modifications vers le dépôt distant :
   ```
   git push origin master
   ```

### Tirer les modifications

1. Récupérez et fusionnez les modifications du dépôt distant :
   ```
   git pull origin master
   ```

### Fusionner des branches

1. Créez une nouvelle branche pour votre fonctionnalité ou correction de bug :
   ```
   git checkout -b branche-fonctionnalité
   ```

2. Faites des modifications et committez-les dans la branche de fonctionnalité.

3. Revenez à la branche master :
   ```
   git checkout master
   ```

4. Fusionnez la branche de fonctionnalité dans la branche master :
   ```
   git merge branche-fonctionnalité
   ```

### Gérer les conflits

1. S'il y a des conflits lors d'une fusion, Git marquera les fichiers conflictuels.

2. Ouvrez les fichiers conflictuels et résolvez les conflits manuellement.

3. Ajoutez les fichiers résolus à la zone de staging :
   ```
   git add <fichier>
   ```

4. Continuez le processus de fusion :
   ```
   git commit -m "Conflits de fusion résolus"
   ```

### Mettre de côté des modifications

1. Mettez de côté vos modifications pour travailler sur autre chose :
   ```
   git stash
   ```

2. Appliquez les modifications mises de côté plus tard :
   ```
   git stash pop
   ```

### Synchroniser avec les dépôts distants

1. Récupérez les modifications du dépôt distant :
   ```
   git fetch origin
   ```

2. Tirez les modifications et rebasez vos commits locaux :
   ```
   git pull --rebase origin master
   ```

### Utiliser cherry-pick

1. Pour appliquer un commit spécifique d'une branche à une autre :
   ```
   git checkout master
   git log dev
   git cherry-pick <sha-1>
   git push
   ```

### Ressources supplémentaires

- [Documentation officielle de Git](https://git-scm.com/doc)
- [Tutoriels Git par Atlassian](https://www.atlassian.com/git/tutorials)

## Travailler en équipe avec les pull requests

### Créer une pull request

1. Créez une nouvelle branche pour votre fonctionnalité ou correction de bug :
   ```
   git checkout -b branche-fonctionnalité
   ```

2. Faites des modifications et committez-les dans la branche de fonctionnalité.

3. Poussez la branche de fonctionnalité vers le dépôt distant :
   ```
   git push origin branche-fonctionnalité
   ```

4. Créez une pull request sur la plateforme de gestion de votre dépôt (par exemple, GitHub, GitLab, Bitbucket).

### Revoir une pull request

1. Accédez à la pull request sur la plateforme de gestion de votre dépôt.

2. Examinez les modifications proposées, ajoutez des commentaires et des suggestions si nécessaire.

3. Approuvez ou demandez des modifications supplémentaires.

### Fusionner une pull request

1. Une fois la pull request approuvée, fusionnez-la dans la branche principale (par exemple, master) via la plateforme de gestion de votre dépôt.

2. Mettez à jour votre dépôt local avec les modifications fusionnées :
   ```
   git checkout master
   git pull origin master
   ```

### Commandes pour pousser et tirer les modifications

1. Pousser les modifications vers le dépôt distant :
   ```
   git push origin <branche>
   ```

2. Tirer les modifications du dépôt distant :
   ```
   git pull origin <branche>
   ```

### Ressources supplémentaires

- [Documentation officielle de Git](https://git-scm.com/doc)
- [Tutoriels Git par Atlassian](https://www.atlassian.com/git/tutorials)
