# Le petit guide des meilleurs pratiques de Git

## 1. Introduction à Git

Git est un **système de gestion de versions distribué** conçu pour suivre les modifications du code source et faciliter le travail collaboratif des développeurs. Créé en 2005 par Linus Torvalds (le créateur de Linux), Git s’est rapidement imposé comme l’un des outils de versionnement les plus populaires grâce à sa rapidité et à sa flexibilité. **Pourquoi utiliser Git ?** Tout projet logiciel implique des évolutions et corrections au fil du temps : Git permet d’enregistrer chaque modification sous forme de *commit* (validation) horodaté avec un message descriptif, offrant un historique complet du projet. On peut ainsi revenir à une version antérieure du code, comparer des changements, identifier l’introduction d’un bug, ou encore annuler des erreurs. Git facilite également la **collaboration** : plusieurs développeurs peuvent travailler simultanément sur différentes fonctionnalités sans se gêner, puis fusionner leurs travaux lorsque prêt.

Contrairement aux anciens systèmes centralisés (comme SVN) où les développeurs devaient se connecter à un serveur commun pour chaque opération, Git est **distribué**. Cela signifie que chaque développeur clone le dépôt complet sur sa machine : il possède **l’intégralité de l’historique en local**, peut effectuer des commits et naviguer dans l’historique hors-ligne, puis synchroniser avec les autres via un serveur distant lorsque nécessaire. Cette architecture distribuée apporte robustesse (chaque copie est une sauvegarde) et flexibilité (travail hors connexion possible). Git est également très performant et **extensible** : il peut gérer des projets de toutes tailles avec des milliers de fichiers et de contributions, tout en offrant de nombreuses commandes et options avancées pour répondre à des besoins variés ([Git - git Documentation](https://git-scm.com/docs/git/fr#:~:text=Git%20est%20un%20syst%C3%A8me%20de,aux%20structures%20de%20bas%20niveau)).

**Comment fonctionne Git ?** Plutôt que d’enregistrer les différences (diffs) entre fichiers comme le font certains outils, Git prend des **instantanés** de l’état complet du projet à chaque commit. Chaque commit pointe vers son ou ses commits parents (formant une chaîne ou un graphe d’historique) et est identifié de manière unique par un hash (SHA-1 par défaut). Git utilise un principe de staging (*index*) : les modifications doivent être indexées (staged) via `git add` avant d’être validées par un `git commit`. Cela permet de préparer précisément le contenu du prochain commit. Les *branches* ne sont rien de plus que des pointeurs mobiles vers des commits : une branche comme `main` (ou `master`) pointe toujours vers le dernier commit de la séquence la concernant. Créer une branche est très léger (juste créer un nouveau pointeur) et fusionner des branches consiste à créer un commit spécial qui a plusieurs parents (ou à déplacer un pointeur dans le cas d'une avance rapide). Cette conception rend **les branches et la fusion très bon marché en Git**, favorisant les workflows où l’on crée de nombreuses branches de fonctionnalité pour isoler le travail.

En résumé, Git apporte :
- **Traçabilité** : chaque modification est historisée avec son auteur, date et message.
- **Réversibilité** : possibilité de revenir en arrière ou d’annuler des changements (on verra les commandes comme `git revert` ou `git reset`).
- **Collaboration** : fusionner les contributions, détecter les conflits et les résoudre.
- **Expérimentation** : branches pour développer des fonctionnalités isolément, sans risque pour le code principal.
- **Distribution** : chaque copie de travail est un dépôt complet, réduisant la dépendance à un serveur central.

Git est utilisé massivement dans l’industrie. Des plateformes comme **GitHub**, **GitLab** ou **Bitbucket** reposent sur Git pour héberger le code, gérer les pull requests (propositions de modifications) et la collaboration open-source ou en entreprise. Maîtriser Git est devenu indispensable pour tout développeur. Dans ce guide complet, nous allons couvrir son utilisation de A à Z : depuis l’installation et les commandes de base pour débutant, jusqu’aux techniques avancées et aux meilleures pratiques employées par les experts, de manière à faire de ce document une véritable **bible de Git**.


# Meilleures pratiques Git (usage individuel)

### Des commits atomiques et structurés

Un principe fondamental est de réaliser des **commits atomiques**, c’est-à-dire des validations focalisées sur **une seule modification ou fonctionnalité logique** ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=1,Changes%20Together)). Évitez de mélanger des changements sans rapport dans le même commit (par exemple, ne pas corriger un bug et ajouter une nouvelle fonction en une seule validation) – cela rend l’historique plus difficile à comprendre et complique d’éventuels retours en arrière ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=,multiple%20files%20have%20been%20modified)). Au contraire, un commit devrait regrouper des changements cohérents répondant à un même objectif ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Seulement%20ce%20conseil%20est%20incomplet,faire%20des%20commits%20n%E2%80%99importe%20comment)). 

Pensez que votre projet est comme un livre, et chaque commit en est un chapitre : l’enchaînement des commits doit raconter clairement l’évolution du code ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=livre%20et%20vos%20commits%20sont,un%20milieu%20et%20une%20fin)). En parcourant l’historique, vos coéquipiers (et votre « vous » du futur) doivent pouvoir suivre facilement ce qui a été fait et pourquoi. Il est souvent recommandé de commiter **fréquemment** pour sauvegarder votre travail, mais **“fréquemment” ne veut pas dire “n’importe comment”** ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Lorsque%20l%E2%80%99on%20d%C3%A9bute%20sur%20Git%2C,C%E2%80%99est%20un)). Mieux vaut quelques commits bien pensés que de nombreux commits désorganisés.

### Messages de commit clairs et conventionnés

Un bon commit s’accompagne toujours d’un **message explicite**. Le message doit **résumer brièvement ce qui a été modifié et pourquoi** ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Nommer%20les%20messages%20de%20commits)). En général, on utilise un court **résumé d’une ligne (50 caractères maximum)**, complété éventuellement d’une description plus détaillée dans le corps du message (après une ligne vide) pour donner du contexte ou les raisons du changement ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=)) ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=)). La première ligne doit être rédigée à l’**impératif présent** (ex: *« Corrige le bug X… »* ou *« Add feature Y… » en anglais) ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=,se%20terminer%20par%20un%20point)), sans point final, et éviter la voix passive. 

Il existe des **conventions de message** largement adoptées. Par exemple, la spécification *[Conventional Commits](https://www.conventionalcommits.org)* préconise un format structuré : 

```
<type>(<scope>): <sujet>
<ligne vide>
<corps du message éventuellement>
<ligne vide>
<footer éventuellement>
``` 

Ainsi, on préfixe le message par un type (feat, fix, docs, refactor, etc.) indiquant la nature du changement ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Le%20type%20du%20commit%20d%C3%A9crit,peut%20prendre%20diff%C3%A9rentes%20valeurs)) ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=,jour%20de%20version%20par%20exemple)), un éventuel périmètre (module impacté), puis un sujet concis. Exemple : `feat(auth): ajout de la connexion OAuth` sera compris comme une nouvelle fonctionnalité relative à l’authentification. Ce format améliore la lisibilité et permet même d’automatiser la gestion des versions et le **changelog** du projet ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Le%20message%20d%E2%80%99un%20commit%20doit,utilis%C3%A9e%20est%20la%20%C2%AB%C2%A0Conventional%20Commits%C2%AB)). Quel que soit le style adopté, **nommez correctement vos commits et décrivez le plus possible ce que vous avez fait** – cela prend un peu de temps, mais c’est du temps gagné lors de la relecture de l’historique ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=faire%20avec%20au%20minimum%203,commits%20bien%20distincts)).

*Bonnes pratiques supplémentaires :* si un commit résout une **issue** (ticket) dans un gestionnaire de bugs, mentionnez-le dans le footer du message (ex: `Fix #123`), ce qui pourra automatiquement fermer le ticket lors du merge sur la branche principale. 

### Utilisation efficace des branches

**Ne travaillez jamais directement sur la branche principale (`main` ou `master`) pour des développements.** Pour chaque nouvelle fonctionnalité, amélioration ou correctif, **créez une branche dédiée**. Cela isole vos changements et évite d’affecter le code stable avant d’être prêt ([ Conflits de merge Git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/using-branches/merge-conflicts#:~:text=d%C3%A9veloppeurs%29,de%20r%C3%A9soudre%20les%20conflits%20d%27%C3%A9dition)). Travailler à plusieurs sur la même branche provoque très rapidement des conflits et des risques d’écrasement mutuel de code – **“vous ne devez JAMAIS, mais alors JAMAIS travailler à plusieurs sur la même branche”** insiste un guide pour débutants ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=match%20at%20L93%20Vous%20vous,plusieurs%20sur%20la%20m%C3%AAme%20branche)). À la place, chaque développeur ou chaque tâche devrait avoir sa propre branche, qui sera fusionnée après validation. 

**Gardez les branches principales fonctionnelles :** la branche `master`/`main` doit toujours contenir du code en état de marche, testé et prêt pour une mise en production ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=L%C3%A0%20encore%20c%E2%80%99est%20une%20autre,termes%20de%20code%20et%20applicative)). De même, si vous avez une branche de développement intégration (souvent appelée `develop` dans certains workflows), elle ne doit contenir que des fonctionnalités terminées et intégrées, pas du travail en cours instable ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Master%20doit%20contenir%20du%20code,mis%20en%20production%20pour%20utilisation)). Le code en cours de développement *doit* résider sur des branches de fonctionnalité ou de correctif, jamais directement sur `main` ou `develop` ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=L%C3%A0%20encore%20c%E2%80%99est%20une%20autre,termes%20de%20code%20et%20applicative)). Cette discipline garantit qu’à tout moment, une nouvelle personne rejoignant le projet sait que ce qui est sur `main` correspond à la dernière version stable déployée, et que `develop` (si utilisé) correspond à l’état prêt à être livré de la prochaine version.

**Adoptez une convention de nommage des branches.** Utilisez des préfixes explicites pour indiquer le type de travail : par exemple `feature/` pour une nouvelle fonctionnalité, `bugfix/` pour une correction de bug, `hotfix/` pour un correctif urgent en production, `chore/` pour des tâches de maintenance, etc ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Le%20type%20d%E2%80%99une%20branche%20doit,des%20types%20de%20branches)). Suivez une syntaxe cohérente, comme `type/description-breve-[IDticket]`. Exemples : `feature/ajout-login-google`, `bugfix/correction-typo-page-accueil`, `hotfix/critical-crash-fix-512` (où 512 pourrait référencer un ID de bug). Gardez des noms **courts** mais explicites (idéalement < 50 caractères) et utilisez le kebab-case (mots en minuscules séparés par des tirets) ([Git : comment nommer ses branches et ses commits ? - Code Heroes](https://www.codeheroes.fr/2020/06/29/git-comment-nommer-ses-branches-et-ses-commits/#:~:text=Le%20nom%20de%20la%20branche,r%C3%A8gles%20doivent%20%C3%AAtre%20respect%C3%A9es)). Une convention claire aide toute l’équipe à comprendre le rôle d’une branche rien qu’à son nom.

**Évitez de coder plusieurs fonctionnalités à la fois sur la même branche.** Si vous commencez à travailler sur une fonctionnalité A sur la branche `feature/A`, puis que vous devez en parallèle débuter la fonctionnalité B, ne mélangez pas tout sur `feature/A`. Mettez éventuellement A de côté (commit, push sur sa branche) et créez une **nouvelle branche** `feature/B` à partir de `main` (ou `develop`) pour la fonctionnalité B. Ceci permet ensuite de faire des pull requests séparées, et de livrer l’une sans attendre l’autre si besoin. **En résumé : une branche = une feature ou un correctif.** Cette règle améliore la traçabilité et réduit les conflits. 

**Utilisez `.gitignore` pour un historique propre** : Assurez-vous d’ignorer les fichiers qui ne doivent pas être versionnés (fichiers de build, dépendances externes, fichiers temporaires, clés secrètes, etc.). Un repository peut vite être pollué par des fichiers inutiles si on n’y prend garde. Le fichier `.gitignore` sert justement à lister les motifs de fichiers à exclure du suivi Git ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=3.%20Ignoring%20)). Idéalement, configurez-le dès le début du projet (beaucoup de templates existent pour chaque langage/framework), et faites-le évoluer en cours de route si de nouveaux types de fichiers temporaires apparaissent ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=The%20,automatically%20generated%20files%20from%20Git)). **Ne suivez jamais les fichiers de configuration personnels, mots de passe, clés privées, etc.** (voir plus loin la section sur les erreurs à éviter).  

Enfin, **pensez à supprimer vos branches locales et distantes une fois fusionnées** pour éviter l’encombrement du dépôt. Après le merge d’une feature via une pull request, supprimez la branche correspondante. Sinon, vous accumulerez des dizaines de branches obsolètes, rendant le dépôt confus ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=4,Branches)) ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=To%20avoid%20this%2C%20you%20can,branch%20has%20been%20fully%20merged)). Des plateformes comme GitHub proposent de supprimer automatiquement la branche après merge. En nettoyant régulièrement, vous évitez de vous demander plus tard « cette branche a-t-elle encore un travail en cours ou peut-elle être supprimée ? » ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=are%20no%20longer%20any%20unique,commits%20associated%20with%20it)).

## Meilleures pratiques Git en équipe

Travailler efficacement en équipe avec Git nécessite d’adopter un **workflow** commun et quelques règles de collaboration. Il n’existe pas de solution unique, mais voici les pratiques répandues :

### Workflows de branches recommandés

- **Git Flow (Workflow Gitflow)** – Ce modèle, popularisé par Vincent Driessen, convient aux projets avec des **cycles de versions planifiés**. Il définit des branches principales stables (`main` pour les versions en production et `develop` pour l’intégration des prochaines fonctionnalités) et des branches de support : feature branches pour les nouvelles fonctionnalités (depuis `develop`), release branches pour la préparation d’une version (générées depuis `develop` en vue d’un déploiement prochain), et hotfix branches pour les correctifs urgents (depuis `main`) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Il%20convient%20de%20cr%C3%A9er%20une,main)) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=,les%20hotfix%20vers%20la%20production)). Dans Git Flow, **les features partent de `develop` et reviennent dans `develop` une fois terminées** ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=sauvegarde%2Fcollaboration,main)). Quand suffisamment de features sont prêtes, on crée une **branche de release** depuis `develop` pour fignoler la version (derniers tests, corrections mineures, documentation) sans interrompre le travail sur `develop` ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Une%20fois%20que%20,nouveau%20un%20merge%20de%20la)). Une fois la release validée, elle est fusionnée dans `main` (et taggée avec un numéro de version) **puis également fusionnée en retour dans `develop`** pour que `develop` intègre les correctifs apportés en phase de release ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=fonctionnalit%C3%A9%20suppl%C3%A9mentaire%20ne%20pourra%20%C3%AAtre,le%20d%C3%A9but%20de%20la%20livraison)) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=branche%20,le%20d%C3%A9but%20de%20la%20livraison)). Pour un **hotfix** en production : on crée une branche depuis `main` pour corriger le bug urgent, puis on fusionne cette branche de hotfix à la fois dans `main` **et** dans `develop` (afin que le correctif soit aussi appliqué dans la branche de développement) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=%24%C2%A0git%C2%A0flow%C2%A0hotfix%C2%A0start%C2%A0hotfix_branch)) ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Outre%20les%20flows%20,ressemble%20%C3%A0%20ceci)). Git Flow apporte une structure rigoureuse qui **protège le code en production** tout en permettant le développement parallèle de fonctionnalités ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=)). En contrepartie, il peut être considéré comme complexe ou lourd pour les équipes pratiquant l’intégration continue : il crée des branches longues vécues (notamment `develop` qui diverge de `main` jusqu’à la prochaine release) et demande une gestion disciplinée des merges.

- **GitHub Flow** – Un workflow plus **léger** et adapté aux déploiements fréquents (notamment utilisé pour le développement web en continu). **Ici, pas de branche `develop`** : on ne conserve qu’une branche principale (`main`) qui représente toujours l’état prêt à déployer (ou déployé) en production ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=GitHub%20Flow%20is%20a%20simpler,need%20to%20manage%20multiple%20versions)). Chaque fonctionnalité ou bug à traiter est développé dans une **branche courte créée depuis `main`**, puis fait l’objet d’une **pull request** pour être fusionné dans `main` une fois validé. GitHub Flow repose donc fortement sur les **pull requests et les revues de code** pour valider les changements avant de les intégrer. Il **n’y a pas de branches de version ou de release distinctes** – les déploiements se font directement à partir de `main` (éventuellement en utilisant des tags pour marquer les versions déployées). Ce modèle convient bien aux équipes agiles déployant en continu ou très fréquemment, et aux **petites équipes** qui n’ont pas besoin de gérer plusieurs versions en parallèle ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=GitHub%20Flow%20is%20a%20simpler,need%20to%20manage%20multiple%20versions)). Il simplifie le workflow (une seule branche longue durée) et réduit le “merge hell” (les enfer des conflits) car les branches de travail sont de courte durée. **GitLab Flow** est une variante proche, intégrant en plus la notion d’environnements (préproduction, production) ou d’intégration avec les issues, mais on ne la détaillera pas ici. 

- **Trunk-Based Development (développement sur tronc)** – C’est le workflow privilégié des pratiques de **DevOps/CI-CD poussées**. Ici, l’idée extrême est d’**éviter les branches longues** : tout le monde intègre ses changements très fréquemment (plusieurs fois par jour) sur la branche principale (le **“tronc”**), idéalement via l’automatisation CI. On peut avoir de très courtes branches de quelques heures ou un jour, mais elles sont intégrées dès que possible. En fait, dans sa forme pure, les développeurs **committent directement sur le tronc** partagé sans passer par des branches de feature longues ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Trunk,into%20a%20shared%20trunk%20at)) ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=changes%20other%20developers%20are%20making,merging%20into%20the%20main%20branch)). Le tronc (souvent `main`) est donc constamment à jour et **toujours prêt à être déployé en production** à tout instant ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=match%20at%20L688%20least%20once,be%20ready%20for%20release%20anytime)). Cette stratégie vise à éviter absolument la dérive de branches et les conflits complexes : en intégrant en continu, on minimise l’écart entre le travail de chacun. **Des *feature flags*** (commutateurs de fonctionnalités) sont souvent utilisés pour désactiver dans le code les fonctionnalités en cours de développement qui sont mergées incomplètes, afin de ne pas impacter les utilisateurs avant qu’elles soient terminées. Le trunk-based development nécessite une grande discipline (tests automatisés robustes, déploiements automatisés, etc.) et convient bien aux **équipes expérimentées** pratiquant l’intégration continue à grande échelle. Il élimine pratiquement les merges retardés mais peut intimider les développeurs juniors car on travaille directement sur la branche commune du produit ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=frequently%20to%20the%20trunk%2C%20often,CD)) ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Because%20trunk,any%20conflicts%20that%20may%20arise)). De plus, sans branches de release, il faut un processus de déploiement très fiable. En contrepartie, c’est un atout pour la **livraison continue** : des changements petits et fréquents, moins de conflits, un historique linéaire.

**Quelle stratégie choisir ?** Cela dépend de la taille de l’équipe, du rythme de déploiement et de la complexité du projet. Aucune approche ne convient à tous les cas de figure ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=%2A%20There%20is%20no%20one,help%20shape%20your%20Git%20workflow)). Pour une petite équipe qui déploie souvent, un GitHub Flow ou un développement sur tronc (éventuellement avec PRs) est souvent plus efficace et moins lourd administrativement qu’un Git Flow complet. En revanche, pour un produit mature nécessitant des versions stables bien séparées (ex: logiciels versionnés, projets open-source avec contributions externes), Git Flow peut apporter la stabilité requise (par exemple, on peut refuser les contributions directes sur `master` et exiger des PR vers `develop` pour contrôle). **L’important est d’avoir une convention commune bien comprise de tous** ([5 erreurs à ne jamais faire quand on utilise Git | Apprendre la programmation](https://apprendre-la-programmation.net/5-erreurs-utilisation-git/#:~:text=Heureusement%2C%20il%20existe%20des%20m%C3%A9thodes,un%20article%20c%E2%80%99est%20Git%20Flow)), et de documenter votre workflow dans le README ou un document interne. Assurez-vous aussi que votre workflow s’aligne sur vos contraintes métiers (fréquence des releases, besoin de hotfix d’urgence, etc.) ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=Match%20a%20release%20schedule)) ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=A%20workflow%20should%20complement%20your,a%20branch%20to%20a%20version)). Et quelle que soit la stratégie, privilégiez les **branches de courte durée** : plus une branche reste longtemps sans être fusionnée, plus les risques de conflits augmentent ([ Git Workflow | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/comparing-workflows#:~:text=Short)).

## 2. Installation et configuration de Git sous Windows

Avant de pouvoir utiliser Git, il faut l’installer et le configurer sur votre système. Nous allons détailler l’installation sous Windows, y compris l’intégration avec Visual Studio et l’explorateur de fichiers Windows.

### 2.1 Téléchargement et installation de Git pour Windows

La méthode la plus simple pour installer Git sur Windows est de télécharger l’installateur officiel **Git for Windows**. Cet installateur inclut la version en ligne de commande de Git ainsi que quelques outils optionnels. Pour l’obtenir : rendez-vous sur la page officielle de téléchargement  ([Git - Installation de Git](https://git-scm.com/book/fr/v2/D%C3%A9marrage-rapide-Installation-de-Git#:~:text=Il%20existe%20aussi%20plusieurs%20mani%C3%A8res,vous%20%C3%A0%20https%3A%2F%2Fmsysgit.github.io)) (par ex. <https://git-scm.com/download/win>) et exécutez l’installateur obtenu. Celui-ci s’occupe de tout installer et de configurer les variables d’environnement nécessaires.

**Options de l’installation** : L’installateur Git pour Windows pose plusieurs questions de configuration. Les choix par défaut conviennent généralement, mais voici quelques points importants :
- **Éditeur par défaut** : Git utilise par défaut Vim comme éditeur pour les messages de commit ou les merges. Si vous n’êtes pas à l’aise avec Vim, l’installeur propose de choisir un autre éditeur (par exemple Visual Studio Code, Notepad++ ou autre) pour rédiger les messages de commit.
- **Ajustements de PATH** : Vous pouvez permettre l’utilisation des commandes Git depuis n’importe quel invité de commande ou PowerShell. L’option recommandée est d’ajouter Git au PATH pour l’utilisateur courant.
- **Fin de ligne (EOL)** : Git propose de gérer automatiquement la conversion des fins de ligne entre Windows (CRLF) et UNIX (LF). L’option par défaut *"Checkout Windows-style, commit Unix-style line endings"* est conseillée, sauf besoin particulier. Cela stocke les fichiers dans le dépôt avec des fins de ligne UNIX et convertit en CRLF lors du checkout sur Windows, évitant des problèmes de diffs à cause des caractères de fin de ligne.
- **Options supplémentaires** : L’installeur propose d’autres réglages (comme l’activation de **Git Credential Manager** pour stocker vos identifiants de manière sécurisée, ou l’utilisation de **symlinks**). Activez Git Credential Manager, car il facilite la gestion des mots de passe ou tokens d’accès aux dépôts distants en les stockant dans le gestionnaire d’informations d’identification Windows.

Une fois l’installation terminée, vous devriez avoir accès à Git via **Git Bash** (un terminal type Unix fourni) ou via l’invite de commandes classique. Vous pouvez vérifier que tout fonctionne en ouvrant un terminal et en tapant : 

```bash
git --version
``` 

Cette commande doit afficher le numéro de version de Git installé. Par exemple : `git version 2.x.y.windows.1`. **Git est maintenant installé** sur votre système.

### 2.2 Configuration initiale de Git (identité et préférences)

Avant de commencer à utiliser Git, configurez vos informations d’utilisateur, car chaque commit enregistre un nom et une adresse email d’auteur :
```bash
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"
``` 
Ces paramètres globaux seront utilisés pour tous vos dépôts (vous pouvez les surcharger par dépôt plus tard si nécessaire avec `--local`). Assurez-vous d’utiliser l’email associé à votre compte GitHub/GitLab si vous comptez pousser sur ces plateformes, afin que vos commits soient bien rattachés à votre compte.

D’autres configurations utiles :
- **Couleurs** : Par défaut Git colorise ses sorties (pour `git status` par ex). C’est généralement activé par défaut. Si ce n’est pas le cas : `git config --global color.ui auto`.
- **Branch par défaut** : Historiquement Git nommait la branche principale “master”, mais depuis Git 2.28 vous pouvez choisir le nom par défaut des nouvelles branches initiales. Pour utiliser “main” comme nom de branche par défaut lors des `git init` : `git config --global init.defaultBranch main`.
- **Outil de fusion** : Envisagez de configurer un outil de merge par défaut (par exemple en utilisant VS Code ou autre) pour faciliter la résolution de conflits via une interface graphique. Par exemple, pour VS Code : `git config --global merge.tool vscode` et `git config --global mergetool.vscode.cmd "code --wait $MERGED"`.
- **Alias** (optionnel) : Vous pouvez définir des alias pour les commandes fréquentes. Par ex. `git config --global alias.st status` créera une commande `git st` équivalente à `git status`, ce qui est plus rapide à taper.

### 2.3 Intégration de Git dans Visual Studio

Si vous utilisez **Visual Studio** (l’environnement de développement complet, pas VS Code), l’intégration de Git est très bien prise en charge. Visual Studio 2019 et versions ultérieures incluent nativement un support Git dans l’IDE. Vous pouvez effectuer toutes les tâches courantes de gestion de code source sans quitter Visual Studio ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=Visual%20Studio%20fournit%20une%20interface,vous%20connecter%20%C3%A0%20un%20fournisseur)). 

**Installation via Visual Studio** : Les dernières versions de Visual Studio proposent d’installer **Git for Windows** automatiquement lors de l’installation de l’IDE (via les charges de travail de développement). Assurez-vous d’avoir sélectionné l’option de contrôle de code source Git lors de l’installation. Si Visual Studio est déjà installé sans Git, vous pouvez soit installer Git séparément (comme fait en 2.1) soit utiliser le Visual Studio Installer pour ajouter la composante “Git”.

**Ouverture de projets Git** : Dans Visual Studio, vous pouvez :
- **Cloner un dépôt** distant depuis GitHub, Azure DevOps, etc. via **Git > Clone Repository...** et fournir l’URL. Visual Studio créera le clone local et ouvrira le dossier en tant que projet.
- **Créer un nouveau dépôt Git** depuis un projet existant : si vous avez un code non versionné ouvert dans Visual Studio, un bouton *“Ajouter à la source de contrôle”* (Add to Source Control) apparaît généralement en bas à droite. En cliquant dessus, choisissez Git, Visual Studio va exécuter `git init` en arrière-plan et initialiser un dépôt local, puis vous pourrez choisir de le connecter à un service distant (GitHub, Azure Repos…) éventuellement.

**Fenêtres et outils Git dans Visual Studio** : Une fois dans un projet versionné avec Git, Visual Studio bascule en “contexte Git”. Vous aurez accès à :
- Le **menu Git** dans la barre de menu principal, qui donne accès aux actions (Commit, Push, Pull, Branch, Repository, etc.).
- La **fenêtre “Git Changes” (Modifications Git)**, généralement ancrée à gauche, qui liste les fichiers modifiés non encore commités. Vous pouvez y sélectionner les fichiers à indexer (stager) en cochant les cases ou via clic droit *“Stage”*. Rédigez ensuite le message de commit dans le champ prévu et cliquez sur **“Commit”** (Valider) ou **“Commit All”** (tout valider). Visual Studio permet même de *commit & push* en une étape avec *“Commit All and Push”*, pratique pour envoyer directement vos modifications après validation ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=match%20at%20L363%20simplement%20votre,a)).
- La **liste des branches** est accessible en bas de Visual Studio (barre de statut) ou via le menu Git. Vous pouvez créer, renommer, supprimer ou changer de branche depuis l’IDE, sans commande manuelle. Par exemple, *“Git > Branch > New Branch…”* équivaut à `git branch` + `git checkout` et Visual Studio peut aussi directement basculer sur la nouvelle branche (il y a une option pour checker la branche après création ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=match%20at%20L410%20%C3%A0%20cocher,branch))).
- La **fenêtre d’historique (Git Repository)** : Visual Studio 2019+ propose une vue de l’historique des commits et de la structure des branches (avec un graphe) via *Git > View History* ou *Git Repository*. Vous pouvez y visualiser les commits entrants/sortants, comparer différentes branches ou commits, rétablir (reset) ou revenir à un commit ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=%C3%A0%20cocher%20Branche%20de%20validation,branch)) ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=match%20at%20L453%20une%20fen%C3%AAtre,son%20parent%20c%C3%B4te%20%C3%A0%20c%C3%B4te)), etc. C’est utile pour inspecter le journal sans taper `git log`.
- L’**outil de fusion intégré** : En cas de conflit lors d’un pull ou d’une fusion, Visual Studio détecte les fichiers en conflit et propose une interface de résolution de conflit. Vous pouvez ouvrir chaque fichier en conflit dans un éditeur visuel à trois volets (base, version locale, version distante) pour choisir les changements à conserver. Une fois satisfait, vous enregistrez et marquez comme résolu.

En somme, Visual Studio offre une expérience Git assez complète en GUI. Vous pouvez toujours utiliser les commandes Git en parallèle (Visual Studio détectera les changements faits en externe), mais beaucoup d’opérations courantes peuvent être faites par simple clic, ce qui est pratique.

### 2.4 Intégration de Git dans l’explorateur de fichiers Windows

En plus de l’IDE, il est possible d’intégrer Git directement à l’explorateur Windows pour travailler de manière graphique sur vos dépôts. Par défaut, l’installation de Git for Windows ajoute deux entrées au menu contextuel (clic droit sur un dossier) : **Git Bash Here** (ouvrir un terminal Git Bash dans ce dossier) et **Git GUI Here** (ouvrir l’interface graphique de base fournie avec Git). La Git GUI officielle permet de réaliser des commits, des pushes/pulls et quelques opérations basiques via des boutons, mais son interface est minimaliste.

Pour une intégration plus poussée dans l’explorateur, beaucoup de développeurs utilisent **TortoiseGit**. TortoiseGit est un client Git s’intégrant dans l’explorateur Windows de manière similaire à TortoiseSVN. Une fois TortoiseGit installé, un clic droit sur un dossier ou des fichiers donne accès à toutes les commandes Git courantes via des menus contextuels (Git Commit, Git Push, Git Pull, Git Branch, etc.), et des icônes superposées indiquent l’état des fichiers (modifié, suivi, à jour, en conflit…). Par exemple, un fichier modifié non commité peut apparaître avec une icône rouge, un fichier à jour a une coche verte, etc. **TortoiseGit permet de gérer ses dépôts depuis l’explorateur Windows** ([Git/Windows — Wikilivres](https://fr.wikibooks.org/wiki/Git/Windows#:~:text=TortoiseGit%20est%20un%20client%20pour,ses%20d%C3%A9p%C3%B4ts%20depuis%20l%27explorateur%20Windows)), sans ouvrir de terminal. On peut initier un commit en cliquant droit sur le dossier du dépôt > "Git Commit -> *master*..." (ou autre branche), ce qui ouvre une fenêtre où lister les changements, sélectionner ceux à inclure, entrer le message et valider. De même, "Git Push..." ou "Git Pull..." sont disponibles dans le menu. TortoiseGit intègre également un outil de diff et merge visuel (appelé TortoiseGitMerge) pour comparer les modifications ou résoudre des conflits via l’interface graphique.

**Astuces Explorer** : Si vous n’utilisez pas TortoiseGit, vous pouvez toujours :
- Lancer **Git Bash** dans un dossier (clic droit *Git Bash Here*) pour exécuter des commandes Git dans ce dossier. Pratique pour démarrer rapidement un terminal au bon endroit.
- Utiliser l’interface **Git GUI** (clic droit *Git GUI Here*) pour des commits rapides si vous préférez une GUI minimale.
- De nombreuses autres GUI tierces existent : **GitHub Desktop** (client officiel GitHub, simplifié), **GitKraken**, **SourceTree** (par Atlassian), etc. Elles offrent des interfaces visuelles pour Git avec des fonctionnalités avancées (historique graphique, gestion de stash, conflits, etc.). Ces outils peuvent être utiles si vous n’êtes pas à l’aise en ligne de commande, mais sachez que toutes ces interfaces reposent sur les mêmes commandes Git sous-jacentes.

Enfin, que vous utilisiez Visual Studio, l’explorateur ou la ligne de commande, **Git crée un dossier caché `.git`** à la racine de chaque dépôt local. Ce dossier contient l’intégralité de l’historique et des données du dépôt. Vous ne devez **pas modifier manuellement** son contenu, au risque de corrompre le dépôt. En revanche, il est utile de savoir qu’en supprimant ce dossier `.git`, vous “dé-versionnerez” le dossier (il ne sera plus un dépôt Git). Cela peut servir si vous avez initialisé un dépôt au mauvais endroit par erreur : supprimez `.git` (ou utilisez la commande `git rm --cached` de manière appropriée) pour annuler l’initialisation.

Avec Git installé et configuré, voyons maintenant les **commandes essentielles** et leur utilisation pratique.

## 3. Les commandes Git essentielles (cas d’usage, erreurs fréquentes et corrections)

Dans cette section, nous passerons en revue les commandes Git les plus courantes, de leur syntaxe de base à des exemples concrets. Pour chaque commande, nous expliquerons son rôle, comment l’utiliser, et nous signalerons les erreurs typiques que rencontrent les utilisateurs ainsi que la manière de les résoudre.

### 3.1 `git init`

**Description :** La commande `git init` crée un nouveau dépôt Git dans le répertoire courant (ou dans un répertoire spécifié). Elle initialise un dossier `.git` avec toutes les structures internes nécessaires. C’est la première commande à utiliser pour commencer à versionner un projet existant non versionné. Par exemple, si vous avez un dossier de projet sur votre disque et que vous souhaitez le mettre sous Git, ouvrez un terminal dans ce dossier et tapez `git init`. 

Cela crée un sous-répertoire caché `.git` contenant le squelette du dépôt (les répertoires objects/, refs/, etc.) mais **aucun fichier n’est encore suivi** à ce stade ([Git - Démarrer un dépôt Git](https://git-scm.com/book/fr/v2/Les-bases-de-Git-D%C3%A9marrer-un-d%C3%A9p%C3%B4t-Git#:~:text=%24%20git%20init)). En effet, `git init` ne commence pas à suivre vos fichiers automatiquement : il prépare juste le dépôt. Vous devrez ensuite ajouter des fichiers et effectuer un commit initial.

**Par défaut**, Git va créer une première branche nommée `master` (jusqu’à Git v2.28 où ce nom par défaut a été modifiable, comme mentionné plus haut). De nos jours, beaucoup de projets configurent le nom de branche initial en `main` pour des raisons de convention.

**Cas d’utilisation :** Vous avez un projet local non versionné (par ex. du code source dans un dossier) :
1. Ouvrez une console dans ce dossier (via Git Bash Here ou `cd mon_projet`).
2. Exécutez `git init`. 
3. Le projet est maintenant un dépôt Git vide. Faites un premier commit : 
   - `git add .` (pour ajouter tous les fichiers actuels, ou sélectionner fichier par fichier).
   - `git commit -m "Premier commit"` pour valider. Git crée alors un commit contenant l’instantané de vos fichiers actuels.
4. (Optionnel) Connectez le dépôt local à un dépôt distant : par exemple, si vous avez créé un projet sur GitHub, ajoutez le remote : `git remote add origin https://github.com/utilisateur/mon_projet.git`, puis poussez le commit initial : `git push -u origin master` (ou main).

**Erreurs fréquentes :**
- *Initialisation au mauvais endroit* : Il peut arriver d’exécuter `git init` dans le mauvais dossier par inadvertance. Cela crée un dépôt où on ne le souhaite pas (par ex, au lieu du dossier projet, on l’a fait dans son parent). Solution : supprimer le dossier `.git` créé au mauvais emplacement, puis relancer `git init` au bon endroit. Vérifiez toujours le chemin courant (`pwd` sur Git Bash) avant d’initialiser.
- *Vouloir versionner un dépôt dans un dépôt* : Évitez de faire un `git init` à l’intérieur d’un autre dépôt Git (un sous-dossier d’un projet déjà versionné). Git permet des dépôts imbriqués (sous-modules), mais c’est une configuration avancée. Si vous vous retrouvez avec un dossier `.git` imbriqué, c’est probablement une erreur. Supprimez-le ou restructurez vos projets.
- *Oublier de faire le commit initial* : Après `git init`, si vous oubliez de faire un commit, le dépôt restera vide. Si vous essayez de pousser vers un distant sans commit, cela échouera (puisqu’il n’y a pas de HEAD). Assurez-vous de faire au moins un commit après init.  
- *Conflit de nom de branche initial* : Si le distant (ex: GitHub) a déjà un dépôt avec la branche `main` et que localement vous avez `master` (ou vice-versa), le premier push peut être rejeté. Vous pourriez voir une erreur du type *"src refspec master does not match any"*. Solution : soit renommer votre branche locale pour correspondre (`git branch -m master main`), soit préciser la cible lors du push (`git push origin HEAD:main`). 

En général, `git init` est simple et sans danger ; en cas d’erreur, la suppression du dossier `.git` permet de “réinitialiser” la situation.

### 3.2 `git clone`

**Description :** `git clone` sert à **copier un dépôt existant** (généralement depuis un serveur distant) sur votre machine. C’est la commande à utiliser pour obtenir le code source d’un projet déjà versionné par Git. Lorsque vous clonez, Git crée un nouveau dossier portant le nom du projet (sauf si vous en spécifiez un autre) et y récupère *l’intégralité de l’historique du dépôt*. En effet, la différence avec SVN ou d’autres outils est que `git clone` ne récupère pas juste la dernière version mais **toutes les versions** du projet ([Git - Démarrer un dépôt Git](https://git-scm.com/book/fr/v2/Les-bases-de-Git-D%C3%A9marrer-un-d%C3%A9p%C3%B4t-Git#:~:text=Si%20vous%20souhaitez%20obtenir%20une,o%C3%B9%20il%20%C3%A9tait%20au%20moment)). Vous avez donc un duplicata complet du dépôt. 

La syntaxe de base est `git clone <url_du_dépôt> [nom_du_dossier]`. L’URL peut être :
- une URL **HTTPS** (ex. `https://github.com/user/projet.git`),
- une URL **SSH** (ex. `git@github.com:user/projet.git`) si vous avez configuré des clés SSH,
- ou un chemin local (on peut cloner un dossier local ou un partage réseau contenant un dépôt).

Après le clonage, Git positionne automatiquement le contexte sur la branche par défaut (souvent `master` ou `main`) et configure le **remote** nommé `origin` pointant vers la source du clone. Ainsi, la branche distante `origin/main` est configurée pour suivre votre branche `main` locale, ce qui facilitera les futurs `git pull` et `git push`.

**Cas d’utilisation :** Vous souhaitez contribuer à un projet open-source sur GitHub :
1. Copiez l’URL du dépôt (par ex. via le bouton "Clone" sur GitHub, choisissez l’URL HTTPS ou SSH).
2. Dans votre terminal, naviguez à l’endroit où vous voulez placer le projet, puis tapez `git clone https://github.com/user/projet.git`. Cela créera un dossier `projet` et y téléchargera tous les fichiers et l’historique.
3. Une fois terminé, entrez dans le dossier (`cd projet`). Vous avez maintenant le code du projet, avec une branche locale `main` (ou autre) suivie par origin. Vous pouvez commencer à travailler.
4. Si le dépôt est privé, assurez-vous d’avoir les bonnes autorisations (via vos identifiants ou clé SSH). En HTTPS, Git demandera votre login/mot de passe (ou token) lors du premier accès.

**Erreurs fréquentes :**
- *URL incorrecte ou accès refusé* : Si l’URL fournie est fausse ou si vous n’avez pas la permission, vous obtiendrez une erreur (ex: *"Repository not found"* ou *"Permission denied"*). Vérifiez l’URL et vos identifiants. Pour les dépôts GitHub privés en HTTPS, depuis août 2021 il faut utiliser un **token d’accès personnel** au lieu du mot de passe. En SSH, assurez-vous que votre clé publique est bien ajoutée à votre compte distant.
- *Cloner dans un dossier non vide* : Git refuse de cloner si la cible n’est pas vide (pour éviter d’écraser des fichiers). Par exemple, si vous exécutez `git clone url .` (dans le dossier courant) alors que des fichiers y sont présents, vous obtiendrez une erreur. Solution : spécifiez un dossier nouveau ou assurez-vous que le dossier courant est vide.
- *Lent pour les très gros dépôts* : Puisque par défaut `git clone` récupère tout l’historique, cloner un projet énorme peut être long. Pour pallier cela, on peut faire un *clone superficiel* (shallow clone) avec l’option `--depth 1` pour n’obtenir que l’historique récent (ex: `git clone --depth 1 url` ne récupère que le dernier commit de chaque branche). Notez cependant que cela limite l’accès à l’historique complet tant qu’on n’a pas enlevé cette profondeur.
- *Post-clone, pas de branche `main` locale* : Si le dépôt distant n’a pas de branche nommée `main` ou `master` (cas rare, ou nom différent), votre clone peut se retrouver en *HEAD détachée* ou sans branch checkoutée. Il convient alors de créer/checkout la branche souhaitée manuellement. En général, ce n’est pas un souci car presque tous les dépôts ont une branche par défaut configurée.

En somme, `git clone` facilite la récupération initiale d’un projet. Rappelez-vous qu’après un clone, vous pouvez commencer à travailler en local mais il est judicieux de faire un `git pull` régulier pour rester à jour avec le dépôt d’origine si celui-ci évolue.

### 3.3 `git add`

**Description :** La commande `git add` sert à **indexer (stager) des modifications** en vue d’un commit. Git distingue la zone de travail (vos fichiers modifiés mais pas encore enregistrés) et la zone d’index (aussi appelée *staging area*). `git add` prend les modifications d’un ou plusieurs fichiers et les place dans l’index, ce qui indique à Git "prépare ces changements pour le prochain commit". Sans faire `git add`, un `git commit` n’enregistrera pas les modifications d’un fichier (sauf utilisation de l’option `-a` de commit, que nous verrons).

En pratique, on utilise souvent :
- `git add <fichier>` pour ajouter un fichier spécifique (nouveau ou modifié).
- `git add <répertoire>` pour ajouter tout un dossier (récursivement).
- `git add .` pour ajouter tout le contenu du répertoire courant (et sous-répertoires) qui n’est pas encore indexé.
- `git add -A` ou `git add --all` pour ajouter **toutes** les modifications (y compris les suppressions de fichiers suivis, ce que `git add .` fait aussi à partir de Git 2.x).
- `git add -p` pour une ajout *interactif par hunks* (permet de sélectionner partie par partie des diff à ajouter, très utile pour ne committer qu’une partie d’un fichier).

Important : ajouter un fichier suité (tracked) modifié ou nouveau aura pour effet qu’il sera inclus dans le prochain commit. Ajouter un fichier supprimé (i.e un fichier qui a été effacé du disque) signale à Git qu’on souhaite enregistrer sa suppression lors du commit.

**Cas d’utilisation :** Vous avez modifié plusieurs fichiers dans votre dépôt. Avant de commiter, vous examinez `git status` qui montre par exemple :
```
Modifications qui ne seront pas validées :
    modifié : file1.txt
    modifié : dir/file2.txt
Fichiers non suivis :
    newfile.txt
```
Vous décidez de ne commiter qu’une partie :
1. `git add file1.txt` – vous ajoutez les changements de file1.txt à l’index.
2. `git add newfile.txt` – vous indiquez de suivre et inclure le nouveau fichier.
3. `git status` montre maintenant ces deux fichiers en "*Changes to be committed*" (modifications en attente de validation). Le file2.txt reste en non indexé (vous le laisserez pour un commit ultérieur).
4. Maintenant, `git commit -m "Message"` va n’inclure que file1.txt et newfile.txt dans ce commit, pas les changements de file2.txt qui n’était pas indexé.
5. Plus tard, vous pourrez indexer file2.txt et le commiter séparément.

**Erreurs fréquentes :**
- *Oublier d’ajouter un fichier avant le commit* : C’est probablement l’erreur la plus courante chez les débutants. On modifie un fichier, on lance `git commit -m "fix issue"`, mais on constate que le fichier n’était pas dans le commit (Git répond souvent "rien à committer, la copie de travail reste inchangée" si aucun fichier n’était ajouté). Solution : toujours faire un `git status` avant de committer pour vérifier que tout ce que l’on veut committer est bien "*staged*". Si quelque chose manque, utiliser `git add`.
- *Confusion entre ajouter et committer* : `git add` ne met pas les modifications dans le dépôt définitivement, il les prépare seulement. Il faut le `git commit` ensuite. Parfois, des utilisateurs pensent qu’après un add c’est suffisant (non, les modifications n’apparaissent pas dans l’historique tant qu’elles ne sont pas commit).
- *Ajouter des fichiers indésirables* : Si vous faites `git add .` sans avoir configuré un `.gitignore`, vous risquez d’ajouter des fichiers que vous ne souhaitiez pas (fichiers de build, temporaires, binaires volumineux, etc.). Si c’est arrivé, pas de panique : avant de committer, éditez votre `.gitignore` pour y ajouter ces fichiers, puis faites `git reset <fichier>` pour retirer de l’index les fichiers indésirables (voir `git reset` plus loin pour l’usage de `git reset <file>` qui *unstage* un fichier).
- *Utilisation de l’option `-a` de git commit* : On peut sauter l’étape add en faisant `git commit -a -m "msg"` (qui ajoute et commite en une commande, mais **seulement pour les fichiers déjà suivis**). Si vous faites ça en pensant inclure un nouveau fichier non suivi, il ne sera pas commité (puisque -a n’ajoute pas les nouveaux fichiers). D’où l’importance de bien ajouter les nouveaux fichiers avec `git add` au moins la première fois.
- *Chemins contenant des espaces ou caractères spéciaux* : Faites attention à bien échapper ou entourer de guillemets les chemins avec espaces dans les commandes. Par ex. `git add "Mon Fichier.txt"`.
- *Performance sur gros ajouts* : `git add .` sur un dépôt avec des milliers de fichiers peut être long. Parfois, on préfère ajouter seulement ce qui est nécessaire, pour éviter de faire mouliner Git sur de gros dossiers inutilement.

En somme, souvenez-vous que **rien n’entre dans un commit sans passer par `git add`** (sauf option -a ou commit initial vide). Cette étape vous offre un contrôle granulaire sur ce que vous validez.

### 3.4 `git commit`

**Description :** La commande `git commit` crée un **nouveau commit** (une *validation*) avec les changements actuellement indexés. En faisant un commit, vous enregistrez une étape dans l’historique du projet. Chaque commit a un identifiant unique (un hash SHA-1 de 40 caractères, souvent abrégé dans les affichages) et contient :
- Un **message de commit** fourni par le développeur, qui décrit les modifications apportées.
- Le **snapshot** complet du projet après application des modifications (d’un point de vue conceptuel, Git stocke l’état complet, mais optimise le stockage en ne sauvegardant que les blobs nouveaux ou modifiés).
- Les métadonnées : l’auteur, la date de création, et éventuellement le commiteur (différent de l’auteur si on applique un patch d’autrui, etc., plus rare) et le parent du commit (le commit précédent sur cette branche, ou plusieurs parents en cas de merge).

La syntaxe courante : `git commit -m "Message court décrivant le commit"`. L’option `-m` permet de passer directement le message. Si vous tapez simplement `git commit` sans `-m`, Git ouvrira l’éditeur de texte par défaut pour que vous composiez un message multi-ligne. Par convenance, on utilise souvent `-m` pour les messages simples.

**Bonnes pratiques de message :** Un commit message devrait idéalement tenir en une courte phrase (50 caractères environ pour la ligne de titre) qui résume l’action, puis éventuellement un corps de message plus détaillé après une ligne vide, si le changement est complexe. Par exemple :
```
Ajoute la fonctionnalité d'export PDF

- Ajout de la librairie PDFgen.
- Implémentation de la classe ExportService avec format PDF.
- Mise à jour du README pour documenter l'utilisation.
```
Ce format facilite la lecture de l’historique. Pensez que vos collègues ou vous-même dans 6 mois lirez ces messages : ils doivent être clairs et explicatifs.

**Cas d’utilisation :** Suite de l’exemple précédent (après avoir fait `git add` sur les fichiers voulus) :
1. Exécutez `git commit -m "Fix #123 : corrige le bug d'affichage du profil utilisateur"` (exemple de message incluant éventuellement un numéro de ticket ou une courte description).
2. Git crée un commit. Si c’est votre premier commit, votre branche master/main pointera sur ce commit. Si c’était un commit suivant, la branche avancera.
3. Vous pouvez vérifier avec `git log -1` pour voir le dernier commit. Il listera le commit avec son SHA, l’auteur, la date et le message que vous avez écrit.
4. Si vous aviez lancé juste `git commit` sans -m, l’éditeur se serait ouvert. Vous auriez tapé votre message, enregistré et fermé l’éditeur pour finaliser le commit. S’il n’y a aucun changement indexé au moment du commit, Git vous informe qu’il n’y a *rien à valider*.

**Erreurs fréquentes :**
- *Aucun fichier indexé, commit vide* : Si vous lancez `git commit` alors que rien n’est staged, vous verrez *"No changes added to commit"* ou *"rien à committer"*. Solution : utiliser `git add` sur les modifications souhaitées puis recommit.
- *Message de commit manquant* : Si vous oubliez `-m` et que votre éditeur par défaut est Vim, vous pourriez être dérouté en tombant dans un éditeur inconnu. Pour quitter Vim : appuyez sur `Esc`, tapez `:wq` (write & quit) pour enregistrer le message (ou `:q!` pour quitter sans committer). Alternativement, fermez la fenêtre de l’éditeur si c’est Notepad ou VS Code une fois le message écrit. Si vous fermez l’éditeur sans écrire de message, le commit sera annulé (Git indique *"Aborting commit due to empty commit message"*).  
- *Message erroné ou oubli d’inclure un fichier* : On réalise parfois qu’on a commit trop vite. Soit le message est bancal, soit on a oublié d’indexer un fichier important. Dans ce cas, si le commit n’a pas encore été poussé, on peut le corriger avec `git commit --amend`. Cette commande reprend le dernier commit et permet d’y ajouter ce qu’on a oublié. Par exemple, indexez le fichier manquant puis `git commit --amend` (sans -m) ouvre l’éditeur avec l’ancien message en pré-rempli afin de le modifier si besoin, et inclut le nouveau changement. Résultat : au lieu d’avoir un nouveau commit, cela remplace le dernier commit par une nouvelle version (fusionnant l'ancien et la nouvelle modification) ([Git - Annuler des actions](https://git-scm.com/book/fr/v2/Les-bases-de-Git-Annuler-des-actions#:~:text=Une%20des%20annulations%20les%20plus,amend%60)) ([Git - Annuler des actions](https://git-scm.com/book/fr/v2/Les-bases-de-Git-Annuler-des-actions#:~:text=%24%20git%20commit%20,amend)). **Attention** : l’amend réécrit l’historique, donc ne l’utilisez pas si le commit initial a déjà été envoyé à d’autres (sinon cela crée un décalage d’historique).
- *Trop de changements en un seul commit* : Ce n’est pas une erreur technique mais une mauvaise pratique. Évitez les commits fourre-tout du style "WIP" (work in progress) contenant 50 fichiers sans explication. Il vaut mieux découper en commits logiques et cohérents. Si vous avez un gros lot de modifications, essayez de les structurer et de les committer séparément.
- *Commettre sur la mauvaise branche* : Il peut arriver de commencer à travailler sur `main` par erreur alors que vous vouliez créer une branche de fonctionnalité. Si vous avez commité sur `main` accidentellement, vous pouvez corriger en créant une nouvelle branche à partir de ce commit (`git branch feature-X HEAD~1` pour déplacer le dernier commit sur une nouvelle branche, puis `git reset HEAD~1` sur main pour l’enlever de main, méthode plus avancée) ou plus simplement reset le commit sur main et recommencer sur la bonne branche. C’est un scénario plus avancé, mais prudence à toujours vérifier `git branch` (ou regarder l’indicateur de branche dans VS) avant de committer.

En résumé, `git commit` est le cœur de l’enregistrement en Git. Prenez l’habitude de committer **souvent** avec des messages clairs. Des commits plus petits sont plus faciles à gérer, à relire et à annuler en cas de problème.

### 3.5 `git push`

**Description :** La commande `git push` permet d’**envoyer vos commits locaux vers un dépôt distant**. Typiquement, après avoir fait un ou plusieurs commits en local, on utilise `git push` pour partager ces commits avec l’équipe (via un serveur central, par ex. GitHub ou autre). Le push transfère les objets commits, arbres, blobs manquants vers le serveur et met à jour la référence de la branche distante pour pointer vers le nouveau dernier commit.

La forme usuelle est `git push <remote> <branche>`. Par exemple, `git push origin main` envoie la branche `main` locale vers la branche correspondante du remote nommé `origin`. Si cette branche n’existe pas encore sur le remote, elle sera créée (sauf si la politique du serveur l’interdit). On peut aussi pousser toutes les branches avec `git push origin --all` (peu courant sauf initialement), ou pousser des tags (`git push origin --tags`).

Lors du tout premier push d’une nouvelle branche, Git peut afficher un message disant qu’aucune branche distante n’est configurée pour suivre votre branche locale. Dans ce cas, la commande recommandée est `git push -u origin <branche>` qui pousse ET définit cette branche comme la branche amont (upstream) par défaut pour les prochains pulls/pushes ([ workflow git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows#:~:text=git%C2%A0push%C2%A0origin%C2%A0main)). Le `-u` (ou `--set-upstream`) n’est à faire qu’une fois par branche.

**Cas d’utilisation :** Vous avez fait quelques commits sur la branche `feature/login`. Maintenant vous souhaitez les partager :
1. Si c’est la première fois pour cette branche : `git push -u origin feature/login`. Cela crée la branche `feature/login` sur le dépôt distant `origin` et y pousse vos commits. Dorénavant, Git associera votre branche locale à origin/feature/login.
2. Si la branche existait déjà (par ex. vous aviez fait la commande ci-dessus auparavant ou elle était créée sur le serveur) : `git push` tout court suffira (Git sait que `feature/login` doit aller sur `origin/feature/login` grâce au suivi).
3. Après le push, vos collègues peuvent faire un `git pull` et récupérer vos changements. Sur la plateforme distante (GitHub/GitLab), vous pouvez également ouvrir une Pull Request / Merge Request pour que vos modifications soient fusionnées dans la branche principale après relecture.
4. À noter : `git push` ne transfère pas par défaut vos **tags** ou vos **branches locales** qui ne sont pas configurées. Il faut les pousser explicitement s’il y en a.

**Erreurs fréquentes :**
- *Branche upstream non configurée* : Si vous faites `git push` sans -u pour une nouvelle branche, vous aurez un message du genre *"fatal: The current branch X has no upstream branch"* ou *"git push --set-upstream origin X"* en suggestion. La solution est, comme indiqué, d’ajouter `-u origin X` la première fois. Cela ne signifie pas une erreur grave, juste qu’il faut préciser la cible.
- *Refus de push (non fast-forward)* : C’est une situation très courante. Vous tentez `git push` et Git répond par une erreur :  
  ```
  ! [rejected]        main -> main (non-fast-forward)
  error: failed to push some refs to 'https://...'
  hint: Updates were rejected because the tip of your current branch is behind its remote counterpart. Merge the remote changes (e.g. 'git pull') before pushing again.
  ```  
  En clair, votre branche locale est en retard par rapport à la distante : quelqu’un d’autre a poussé des commits que vous n’avez pas. Git refuse alors d’écraser ces commits distants en poussant les vôtres ([ workflow git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows#:~:text=Mais%20comme%20son%20historique%20local,un%20message%20d%27erreur%20d%C3%A9taill%C3%A9)). La solution recommandée est d’exécuter `git pull` pour fusionner (ou rebaser) les modifications distantes dans votre travail local, résoudre les éventuels conflits, puis retenter le push. Une fois que votre historique intègre les commits distants (plus les vôtres), le push sera accepté.
- *Refus à cause de la politique du dépôt* : Certaines branches, comme `main` ou `master`, peuvent être **protégées** sur le serveur (particulièrement sur GitHub/Bitbucket) pour empêcher les push directs (on oblige à passer par des PR). Dans ce cas, vous verrez une erreur disant que la ref ne peut pas être mise à jour ou *"Protected branch hook declined"*. La solution est alors de créer une PR au lieu de pousser directement, ou de s’assurer d’avoir les droits nécessaires et d’utiliser la méthode requise par l’organisation (parfois un push avec signature ou autre).
- *Push sur la mauvaise branche* : Assurez-vous toujours de pusher la bonne branche vers la bonne cible. Par exemple, évitez `git push origin main` si vous vouliez pousser *dev* et vice-versa. Pousser une branche sur une autre (ex: `git push origin featureX:main` en spécifiant deux noms) peut être dangereux si c’est par erreur. Git le permet seulement si l’historique de featureX contient l’historique de main ou via un `--force` (dangereux).
- *Push forcé (`--force`)* : Si vous avez réécrit l’historique local (par exemple via `git commit --amend` ou `git rebase`), votre push initial sera rejeté pour cause de non-fast-forward (voir plus haut). La seule façon de mettre à jour le distant est alors d’utiliser `git push --force` (ou mieux, `--force-with-lease` pour un peu plus de sécurité). **Attention** : un push --force va écraser sur le serveur les commits qui ne sont plus dans votre historique local, ce qui peut impacter vos collègues. N’utilisez le --force que sur des branches où vous êtes certain que personne d’autre n’a travaillé dessus simultanément, ou après en avoir informé l’équipe. C’est souvent acceptable sur une branche personnelle de feature, mais jamais sur `main` partagé sans coordination.
- *Problèmes d’authentification* : Lors du push, si vos identifiants ne sont pas configurés correctement, Git demandera un login/mot de passe (pour HTTPS) ou échouera (pour SSH). Par ex, sur GitHub en HTTPS, saisir son user/password donnera une erreur car il faut un token. Configurez le **Git Credential Manager** (normalement installé avec Git sur Windows) pour qu’il gère les tokens OAuth/personnels. Ou mieux, utilisez une clé SSH : ajoutez votre clé publique sur GitHub/GitLab, puis utilisez l’URL SSH du dépôt. Une fois configuré (et l’agent SSH lancé), plus besoin de saisir quoi que soit pour push.

En bref, `git push` est l’action de publication de vos travaux. Pensez à mettre à jour votre dépôt local (`git pull`) avant de pousser pour minimiser les conflits, et évitez les push forcés sur les branches partagées.

### 3.6 `git pull`

**Description :** La commande `git pull` sert à **récupérer les modifications depuis un dépôt distant et les intégrer** à votre branche courante. En interne, `git pull` est en fait un raccourci pour deux opérations : `git fetch` suivi d’un `git merge` (par défaut) ou `git rebase` selon la configuration. Concrètement, `git pull` contacte le serveur distant, télécharge les nouveaux commits de la branche suivie, puis fusionne ces commits dans votre branche actuelle.

La syntaxe de base est `git pull <remote> <branche>` mais dans la plupart des cas, on peut juste faire `git pull`. En effet, si votre branche locale suit une branche distante (par exemple votre `origin/main`), `git pull` saura quoi chercher et où fusionner. Vous pouvez configurer Git pour que le pull utilise `--rebase` au lieu de merge (via `git config pull.rebase true` globalement), ou alors invoquer explicitement `git pull --rebase` quand vous voulez éviter un commit de merge automatique.

**Cas d’utilisation :** Vous êtes sur la branche `main`, et vous voulez synchroniser votre code avec les dernières modifications poussées par l’équipe sur le serveur :
1. Exécutez `git pull origin main` (ou simplement `git pull` si déjà configuré). 
2. Git se connecte à origin, vérifie la branche main distante, télécharge tout commit nouveau non présent en local.
3. Si de nouveaux commits ont été récupérés, Git essaie de les **fusionner** dans votre `main` local. S’il y a eu des modifications simultanées aux mêmes endroits (conflits), Git vous avertira qu’un conflit doit être résolu (voir section conflits). S’il n’y a pas de conflits, il créera soit un *fast-forward* (si votre local était en retard et pas de divergence, il avance simplement le pointeur), soit un commit de merge (si vous aviez aussi des commits locaux *en plus* de ceux du serveur, alors vos commits locaux + les commits distants seront fusionnés dans un commit merge).
4. Une fois `git pull` terminé, votre branche locale intègre les contributions de vos collègues. Vous pouvez continuer à développer en étant à jour.

**Erreurs fréquentes :**
- *Conflits lors du pull* : C’est le cas le plus épineux. Si vous avez modifié les mêmes fichiers que quelqu’un d’autre de manière incompatible, le `git pull` échoue en milieu de chemin en indiquant des conflits. Git aura fait le fetch, puis lancé un merge et détecté le conflit. Vous verrez des messages comme "*Auto-merging <file>*" suivi de "*CONFLICT (content): Merge conflict in <file>*". À ce stade, votre dépôt est en état de **fusion en cours**. Vous devez résoudre manuellement les conflits dans les fichiers concernés (voir la section 7 sur gestion des conflits). Après résolution, vous terminez la fusion par un `git commit` (Git avait stoppé avant de créer le commit de merge, c’est à vous de le faire après résolution). Si pour une raison ou une autre vous voulez annuler l’opération de pull/fusion, vous pouvez utiliser `git merge --abort` pour revenir à l’état d’avant le pull.
- *Pull bloqué par modifications locales non commités* : Si vous avez des changements dans votre working directory (non commités) et que ces mêmes fichiers ont été modifiés sur le remote, Git refusera d’écraser vos modifications locales. Vous verrez une erreur du genre : *"Your local changes to the following files would be overwritten by merge"* et la liste des fichiers. Git annulera alors le pull. Solution : deux options – soit **committez** vos changements locaux (ou stash-les) puis refaites `git pull`, soit si vous ne voulez pas conserver vos changements locaux car vous préférez écraser par la version distante, utilisez `git restore <fichier>` pour annuler vos modifications locales sur ces fichiers puis refaites le pull. **Ne jamais** forcer un pull en ignorant ce message, Git ne le fait pas de toute façon.
- *Aucun suivi de branche* : Si vous venez de créer une nouvelle branche locale qui n’existe pas encore sur le remote et que vous faites `git pull`, Git ne saura pas quoi tirer. Il affichera *"There is no tracking information for the current branch."* Dans ce cas, vous devez soit préciser la source (`git pull origin nom-de-branche`), soit configurer le suivi en poussant d’abord la branche (avec `-u` lors du push). Généralement, on ne fait pas de pull sur une branche qui n’a pas de pendant distant. À l’inverse, si vous voulez mettre à jour une branche locale depuis son remote la première fois, vous pouvez faire `git branch --set-upstream-to origin/branchX` puis `git pull`.
- *Mises à jour stables vs rebase* : Par défaut, un pull peut créer un commit de merge qui encombre l’historique, surtout si vous travaillez seul sur une branche. Beaucoup de devs préfèrent `git pull --rebase`, qui évite ce commit de merge en rebasant vos commits locaux au-dessus des commits distants nouvellement récupérés. Cependant, en cas de conflits, la résolution se fait lors du rebase (plusieurs petits merges), ce qui peut être un peu plus complexe. Adaptez selon votre flux de travail (on en reparlera dans la partie Git avancé).
- *Impact des hooks ou réglages spéciaux* : Dans de rares cas, un `git pull` peut être influencé par des hooks Git ou des configurations (par ex un hook pre-merge commit sur le serveur, etc.). Si un hook de commit est activé en local, il s’exécutera lors du commit de merge. Gardez-le en tête si vous avez installé des outils comme *husky* etc.

En conclusion, faites un `git pull` **régulièrement** pour éviter de vous éloigner trop de l’état du dépôt distant. Il vaut mieux de petits pulls fréquents (souvent sans conflits) qu’un gros pull après des semaines (avec potentiellement des conflits massifs). Et avant de commencer un nouveau développement, faites un pull pour partir de la dernière version.

### 3.7 `git fetch`

**Description :** `git fetch` télécharge les objets et références depuis un dépôt distant, **sans fusionner** quoi que ce soit dans votre branche locale. Autrement dit, ça met à jour votre copie locale de l’état du serveur, mais ça n’affecte pas votre travail courant. On peut voir `git fetch` comme "dis-moi ce qu’il y a de nouveau sur le serveur mais n’applique rien encore". 

Par exemple, `git fetch origin` ira contacter `origin`, et mettre à jour toutes vos branches distantes suivies (origin/main, origin/dev, etc.) à leur dernier état. Vous pouvez alors examiner ces mises à jour (via `git log origin/main` par ex) et décider comment les intégrer (par un merge, rebase, cherry-pick, etc.). 

**Cas d’utilisation :** Vous travaillez sur la branche `main`, mais avant de faire quoi que ce soit, vous voulez juste voir s’il y a de nouveaux commits sur GitHub depuis votre dernière pull. Vous pourriez faire :
1. `git fetch origin`. Cela ne touche pas votre branche `main` locale; si de nouveaux commits sont arrivés, ils seront téléchargés et référencés dans `origin/main`.
2. Vous pouvez comparer votre `main` local et `origin/main` : par exemple `git log main..origin/main` pour voir ce qui vous manque, ou `git diff main origin/main` pour voir les changements.
3. Ensuite, quand vous le souhaitez, vous pouvez intégrer ces changements. Soit vous faites `git merge origin/main` (ce qui est conceptuellement la seconde moitié d’un pull), soit vous faites `git rebase origin/main`, etc. 
4. En somme, fetch vous donne la maîtrise sur le moment où vous fusionnez. Parfois on fetch toutes les branches (`git fetch --all`) puis on traite chaque fusion plus tard. C’est aussi utile quand on suit plusieurs branches distantes (feature de collègues) sans vouloir toutes les fusionner de suite.

**Erreurs fréquentes :**
- *Oublier de fusionner après fetch* : Il n’y a pas de vraie erreur technique, mais il arrive que des développeurs fassent `git fetch` pensant avoir mis à jour leur code… alors qu’ils n’ont pas fait le merge derrière. Après un fetch, votre code local est inchangé, seul le reflet distant l’est. Pour intégrer, utilisez `git merge origin/branche` correspondante ou `git pull` directement.
- *Confusion entre fetch et pull* : On peut se demander quand utiliser l’un ou l’autre. En gros, utilisez `git pull` pour la simplicité (récupérer & intégrer en une commande) quand vous êtes prêt à intégrer tout de suite. Utilisez `git fetch` si vous souhaitez juste voir les nouveautés ou bien mettre à jour plusieurs branches distantes sans toucher à votre branche actuelle. Par exemple, si vous avez 5 PR ouvertes, un `git fetch` vous permet de récupérer toutes les branches de ces PR sans les fusionner, puis vous pouvez les checkout l’une après l’autre pour les tester.
- *Nettoyer les références obsolètes* : `git fetch` ne supprime pas par défaut les branches distantes qui ont été supprimées sur le serveur. Avec le temps, vous pourriez avoir des `origin/feature-ancien` qui n’existent plus réellement. Pour nettoyer, utilisez `git fetch --prune` (ou configurez `fetch.prune true`) qui supprimera les références locales obsolètes.
- *Problèmes d’authentification* : Comme pour pull/push, un fetch nécessite l’accès au serveur. Donc s’il y a un souci de credentials, la fetch échouera de même. Les messages d’erreur d’authentification s’appliquent pareil. Vérifiez vos accès si `git fetch` renvoie *"Permission denied"* ou *"could not read from remote repository"*.

Globalement, `git fetch` est sûr et ne modifie pas votre travail local. N’hésitez pas à fetch fréquemment, surtout dans un contexte où plusieurs personnes pushent souvent sur plusieurs branches, pour avoir une vue à jour. C’est un outil précieux pour la **surveillance de l’activité du dépôt** sans perturber votre code en cours.

### 3.8 `git merge`

**Description :** `git merge` permet de **fusionner une branche dans une autre**. On l’utilise typiquement pour intégrer le contenu d’une branche de fonctionnalité dans la branche principale, ou pour synchroniser une branche avec une autre. La commande s’utilise depuis la branche cible (celle qu’on veut mettre à jour) en passant la branche source comme paramètre. Par exemple, si vous êtes sur `main` et que vous voulez fusionner la branche `featureX` dedans, vous faites `git merge featureX`. Après un merge réussi, la branche cible contiendra tous les commits de la branche source (directement ou via un commit de merge) et son historique.

Git fait automatiquement soit un **"fast-forward"**, soit un **"merge commit"** :
- Fast-forward (FF) : si la branche cible n’a pas divergé et est strictement un ancêtre de la branche source, alors Git avance simplement le pointeur de cible jusqu’au commit de source. Aucun nouveau commit n’est créé, on “rapproche” juste l’historique. Cela arrive souvent quand vous mergez `origin/main` dans votre feature qui était à jour, etc.
- Merge commit : si la branche cible a des commits qui ne sont pas sur la branche source (divergence), Git doit créer un commit spécial de fusion ayant deux parents (un parent sur chaque branche) pour joindre les deux historiques. Ce commit de merge aura par défaut un message du type "Merge branch 'featureX' into main" (modifiable).

En cas de conflits pendant la fusion, Git interrompt le processus pour vous laisser résoudre manuellement (voir gestion des conflits section 7).

**Cas d’utilisation :** Vous avez terminé la branche `featureX` et souhaitez la intégrer dans `develop` (en suivant une logique GitFlow par ex) :
1. Assurez-vous d’abord d’être sur la branche cible : `git checkout develop`.
2. Optionnellement, mettez-la à jour depuis origin si d’autres ont pu pousser sur develop : `git pull origin develop` (pour partir du dernier état).
3. Lancez la fusion : `git merge featureX`. 
   - S’il n’y a pas de conflit et qu’un merge commit est nécessaire, Git créera le commit de merge et vous verrez un message "*Merge made by the 'recursive' strategy.*" dans la sortie.
   - Si c’était un fast-forward possible (c.-à-d. `develop` n’avait rien de plus que featureX n’avait déjà), Git va juste déplacer HEAD de develop en avant. Dans ce cas, l’historique de featureX devient en ligne sur develop, comme s’il n’y avait qu’une branche.
4. Si des conflits surviennent, Git va lister les fichiers en conflit. Vous devrez les ouvrir, choisir les changements à garder, puis `git add` les fichiers une fois résolus et enfin `git commit` pour finaliser la fusion. Ce commit de merge incluera les deux branches.
5. Après le merge réussi, la fonctionnalité est dans develop. Vous pouvez supprimer la branche featureX (`git branch -d featureX`), car ses commits sont maintenant dans l’historique de develop.

**Erreurs fréquentes :**
- *Conflits de merge* : Très courant (pas vraiment une "erreur" mais un aléa). Sur la résolution, on a une section dédiée plus loin.
- *Merge d’une branche non à jour* : Si vous mergez une branche source qui n’a pas été synchronisée avec la cible depuis longtemps, vous allez potentiellement intégrer du code dépassé ou provoquer plus de conflits. C’est une bonne pratique de mettre à jour la branche de fonctionnalité avec la dernière version de la cible (par un merge inverse ou rebase) avant de l’intégrer, pour réduire les surprises.
- *Oublier d’engager le commit de merge* : Quand il y a des conflits, Git ne crée pas le commit tout seul. Il attend que vous résolviez et commitiez. Si après un conflit résolu vous oubliez de `git commit`, vous resterez en état de merge inachevé. `git status` vous le rappelera avec "*All conflicts fixed but you are still merging*". La solution est simplement de faire le commit (ou si on veut annuler, `git merge --abort`).
- *Utiliser `--no-ff` ou pas* : Par défaut, Git fait des fast-forward quand il peut, ce qui ne laisse pas de trace de merge commit. Beaucoup d’équipes préfèrent toujours avoir un commit de merge, même si un FF était possible, pour garder une trace qu’une fusion a eu lieu (surtout dans un workflow où on merge via PR, on aime voir un commit de merge). Pour cela, on peut utiliser `git merge --no-ff <branche>` qui forcera la création d’un commit de merge. À l’inverse, `--ff-only` permettra d’éviter un merge commit accidentel : la fusion n’aura lieu que si c’est un FF possible, sinon Git annulera (vous obligeant peut-être à rebase d’abord).
- *Merge dans la mauvaise direction* : Soyez attentif à bien vous placer sur la branche cible avant de merger. Une erreur classique est de se trouver sur la branche de feature et de taper `git merge main`, ce qui va **fusionner main dans la feature** (et non l’inverse). Ce n’est pas catastrophique en soi (ça mettra la feature à jour avec main, ce qui peut être utile aussi), mais ce n’était peut-être pas l’intention initiale. Donc toujours vérifier `git branch` actif avant de lancer la commande.
- *Tentative de merge de branches sans lien* : Si vous essayez de fusionner deux branches qui n’ont pas d’historique commun (par ex. deux dépôts complètement différents), Git va refuser en disant "*fatal: refusing to merge unrelated histories*". Il ne fait cela que si les deux branches n’ont aucun ancêtre commun. Ça peut arriver si, par exemple, on fait `git init` séparément sur deux machines et qu’on veut merge sans base commune. L’option `--allow-unrelated-histories` existe pour forcer, mais en général c’est signe qu’on fait quelque chose de conceptuellement étrange (mieux vaudrait ajouter un remote et pull, etc.).
- *Historique illisible en cas de merges nombreux* : Un inconvénient du merge (par opposition au rebase) est que si on multiplie les branches et merges, l’historique de commits devient non linéaire et peut être difficile à suivre. Ce n’est pas une erreur de la commande en soi, mais cela motive certains workflows comme GitHub Flow de ne faire que des merges de feature et pas de merges entre features multiples, ou de squash-merger pour garder l’historique plus propre.

En conclusion, `git merge` est la manière standard de combiner du travail parallèle. Bien géré (commits atomiques, résolutions de conflits attentives), il permet à de grandes équipes de collaborer. On détaillera plus loin différentes stratégies de branching qui définissent comment et quand réaliser ces merges (section 5).

### 3.9 `git rebase`

**Description :** `git rebase` est une commande puissante permettant de **réappliquer une série de commits sur une autre base**. C’est une alternative à la fusion pour intégrer des changements d’une branche dans une autre, en réécrivant l’historique des commits. En termes simples, si vous avez la branche A et B qui ont divergé, un `git rebase A` exécuté sur B va **déplacer les commits de B pour les "reposer" sur A**, comme si B avait été développé à partir de A directement. L’historique final sera linéaire (pas de commit de merge) : tous les commits de B suivront ceux de A.

Le rebase a plusieurs usages :
- **Mettre à jour une branche de travail** avec les dernières modifications d’une autre branche sans créer de merge commit. Par exemple, vous travaillez sur featureX, main a avancé, vous faites `git rebase main` sur featureX : vos commits de featureX seront rejoués après ceux arrivés sur main, rendant featureX à jour et l’historique sans embranchement.
- **Nettoyer l’historique** via rebase interactif (`git rebase -i`). On peut éditer, squasher, réordonner les commits avant de les publier, pour obtenir un ensemble de commits plus propre.
- **Extraire une série de commits** pour les appliquer ailleurs (similaire à cherry-pick multiple).

**Cas d’utilisation (mise à jour d’une branche)** : Vous développez la branche `featureX` depuis quelques jours. Entre-temps, la branche `develop` (base de votre feature) a reçu de nouveaux commits d’autres développeurs. Avant de faire votre pull request, vous décidez de rebaser pour éviter un commit de merge et intégrer les dernières modifs :
1. Assurez-vous d’avoir committé tout votre travail en cours sur featureX (le rebase ne fonctionne que sur des commits, pas sur des modifications non commitées : si vous avez du travail non fini, stash-le ou committez-le temporairement).
2. `git checkout featureX` (vous êtes déjà dessus probablement).
3. `git fetch origin` pour mettre à jour l’état de develop distant. Puis `git rebase origin/develop` (vous pouvez rebaser sur la copie locale develop à jour).
4. Git va alors prendre chacun de vos commits de featureX et les appliquer *en ordre* au sommet de origin/develop. S’il n’y a pas de conflits, il revalidera chaque commit avec un nouvel identifiant (puisque la base change).
5. Si des **conflits** apparaissent sur l’un des commits rejoués, le rebase s’arrête comme un merge pour résolution. Vous voyez quel commit (par son message partiel) pose problème. À ce stade, vous résolvez les conflits fichier par fichier, puis vous faites `git add` et au lieu de `git commit`, on utilise `git rebase --continue` pour poursuivre le rebase. Git va continuer avec le commit suivant jusqu’à finir. Vous pourriez aussi `git rebase --abort` pour annuler toute l’opération et revenir à l’état initial si c’est trop compliqué.
6. Une fois terminé, votre branche featureX contient les mêmes changements qu’avant, mais ses commits peuvent avoir de nouveaux SHAs et sont maintenant placés après les commits de develop. 
7. Vous pouvez pousser featureX. **Attention** : comme l’historique a été réécrit, un `git push` normal sera refusé si featureX existait déjà en distant. Il faudra forcer : `git push -f origin featureX`. Cela écrasera l’ancienne histoire de featureX sur le serveur (ce qui est ok si c’était votre branche perso pas encore mergée, mais si des collègues l’avaient basée, ils devront re-synchroniser).

**Erreurs/fréquences et précautions :**
- *Réécriture d’historique public* : La règle d’or est **de ne pas rebaser des commits déjà partagés** avec d’autres développeurs (par exemple, ne rebasez pas la branche main ou dev sur laquelle tout le monde collabore). Rebase modifie les commits (nouvelles références), donc si quelqu’un d’autre avait ces commits, ça va provoquer des incohérences (il devra soit tout merge en double, soit reset sur la nouvelle base). Rebase est parfait pour les branches de topic isolées *avant* merge. Après merge/push partagé, évitez-le.
- *Conflits multiples* : Lors d’un rebase, vous pouvez rencontrer des conflits sur plusieurs commits successifs, surtout si votre branche était assez vieille. Cela peut être un peu plus fastidieux qu’un seul conflit sur un gros merge, car on les règle commit par commit. L’avantage est qu’on sait exactement quel commit introduit le conflit, ce qui peut clarifier la résolution.
- *Perte de contexte ou de commit* : Si mal utilisé, rebase peut "perdre" un commit, par exemple si vous droppez un commit par erreur lors d’un rebase interactif. Toujours faire attention durant un rebase -i. Une bonne pratique est de *sauvegarder la branche avant rebase*, par ex. en créant un pointeur : `git branch backup-featureX` puis rebase sur featureX. Comme ça, en cas de pépin, vous avez encore l’ancienne suite de commits.
- *Rebase vs merge* : C’est un débat. Rebase donne un historique linéaire, facile à suivre, comme si le travail s’était fait en série. Merge préserve le vrai historique parallèle et qui a divergé/quand. Ni l’un ni l’autre n’est "mauvais", ils sont complémentaires. Le rebase est souvent utilisé pour nettoyer l’historique localement, puis on effectue un merge propre pour réunir (ex: on peut rebase une branche de feature pour la mettre à jour, puis faire un merge --no-ff dans main, combinant avantages : la PR a un commit de merge, mais interne la feature ne contient pas de merges du dev branch).
- *Rebase interactif* : avec `git rebase -i <base>` (par ex. HEAD~5 pour les 5 derniers commits), vous ouvrez un éditeur listant vos commits récents et vous pouvez choisir pour chaque commit : *pick* (le garder), *squash* (le combiner avec le commit précédent en un seul), *reword* (modifier juste son message), *edit* (faire une pause pour éditer les changements pendant le rebase), ou *drop* (supprimer ce commit). Ceci est très utile avant de partager une branche pour fusion : vous pouvez par exemple squash vos multiples petits commits "fix typo" ou "debug print removed" en un seul commit plus cohérent, et ne présenter que des commits logiques. Attention, cette action modifie les commits suivants.
- *Annuler un rebase* : Si tout se passe mal, souvenez-vous de `git rebase --abort` qui remet l’état comme avant le rebase. Et si vous avez fini un rebase mais voulez revenir en arrière, vous pouvez retrouver l'ancienne base via `reflog` (Git garde une trace de HEAD avant rebase). Par ex. `git reflog` puis `git reset --hard HEAD@{N}` où N est l’entrée HEAD avant rebase.

En résumé, `git rebase` est excellent pour maintenir un historique linéaire et clair, mais il doit être utilisé avec précaution. Il *ressemble* à une simple opération de merge du point de vue du résultat final (les fichiers finaux seront les mêmes qu’après un merge, en théorie), mais la façon dont il y arrive est différente et implique de recréer des commits. Une fois maîtrisé, rebase est un outil incontournable pour les Git users avancés.

### 3.10 `git checkout`

**Description :** Historiquement, `git checkout` était une commande multi-usage : elle permet à la fois de **changer de branche** et de **restaurer des fichiers**. Depuis Git 2.23, il est recommandé d’utiliser `git switch` (pour naviguer entre branches) et `git restore` (pour restaurer des fichiers) afin de clarifier les intentions. Cependant, `git checkout` reste très courant et fait ces deux choses selon le contexte. Nous décrirons ici son usage principal, le changement de branche.

`git checkout <branche>` permet de se déplacer sur une autre branche existante. Cela met à jour votre copie de travail avec le contenu du commit pointé par cette branche. On peut également utiliser `git checkout -b <nouvelle_branche>` pour **créer** une nouvelle branche et basculer dessus en une commande, ou `git checkout <commit/tag>` pour passer en mode *détaché* sur un commit spécifique (sans changer de branche).

**Cas d’utilisation (navigation entre branches)** : Vous êtes sur la branche `main` mais vous souhaitez repartir travailler sur `featureX` :
- `git checkout featureX` : Git va 
  - s’assurer qu’il n’y a pas de modifications non commités qui empêcheraient le changement (si oui, voir erreurs ci-dessous),
  - puis mettre à jour tous les fichiers de votre dossier aux versions de `featureX`,
  - et enfin pointer HEAD sur la branche featureX.
- Vous verrez peut-être un message "*Switched to branch 'featureX'*". Si `featureX` n’existe pas, vous auriez eu une erreur. On utilise alors `git checkout -b featureX` pour la créer à partir de la branche courante.
- Maintenant vous êtes sur featureX, vous pouvez y committer tranquillement. Plus tard, vous pourrez revenir sur main via `git checkout main`.

Autre usage : "*Annuler des changements dans un fichier*". La syntaxe serait `git checkout -- <chemin>` pour dire "restaure ce fichier comme il est dans HEAD". Par exemple, vous avez modifié `config.txt` mais souhaitez abandonner ces modifs : `git checkout -- config.txt` remet le fichier tel qu’il était au dernier commit (attention, vos modifications non commités dans ce fichier seront perdues). Notez que l’option `--` est là pour séparer le nom de fichier de la potentielle confusion avec un nom de branche.

**Erreurs fréquentes :**
- *Changements non commités qui empêchent le checkout* : Si votre working directory n’est pas propre (il y a des modifications non indexées ou indexées mais non commit) **et** que ces modifications entrent en conflit avec les fichiers de la branche sur laquelle vous allez, Git refusera le checkout. Par ex, vous avez modifié `app.java` dans featureX, mais `app.java` a aussi été modifié sur main d’une façon incompatible. Git dira "*Your local changes to the following files would be overwritten by checkout:* ...". Solution : soit committez vos changements en cours sur la branch actuelle avant de changer de branche, soit stash-les (`git stash push -m "temp"`), soit si vous voulez les jeter : `git checkout -- <fichier>` pour chaque fichier ou `git reset --hard` (dangereux) pour tout annuler localement. L’idée est qu’il ne doit pas y avoir de modifications en conflit au moment du switch.
- *HEAD détaché (detached HEAD)* : Si vous faites `git checkout <ID_de_commit>` ou un tag, vous n’êtes pas sur une branche, donc HEAD est "détaché". Cela signifie que si vous faites un commit à ce stade, il ne sera pas sur une branche nommée, juste sur ce HEAD flottant. C’est possible, mais il faut savoir qu’ensuite, si vous revenez sur une autre branche, ce commit peut devenir inatteignable (sauf via reflog) car aucune référence ne le pointe. Pour éviter de perdre un commit fait en détaché, la bonne pratique est de créer une branche à partir de ce HEAD détaché (`git switch -c newbranch`) avant ou après le commit. Un HEAD détaché n’est pas une erreur en soi (Git l’autorise pour inspecter un ancien commit, bisect, etc.), mais novices paniquent en voyant "*HEAD detached at abc1234*". Il suffit de comprendre que vous n’êtes pas sur une branche. Pour "rattacher", checkout une branche existante ou créer une nouvelle branche.
- *Suppression de fichiers non suivis lors d’un checkout* : Par défaut, `git checkout` ne touche pas aux fichiers non suivis (untracked). Donc pas de risque de perte de ce côté-là. Ce sont uniquement les modifications suivies (tracked) qui posent problème. Si un fichier existe dans la branche cible mais pas dans la branche actuelle, et que vous avez un fichier non suivi du même nom, je crois que Git refusera de checkout car le fichier non suivi va gêner la création du fichier versionné. Dans ce cas il vous alertera pour que vous déplaciez ou supprimiez le fichier non suivi.
- *Changement de branche lent* : Sur les très gros dépôts, passer d’une branche à une autre peut prendre un peu de temps car Git doit réécrire potentiellement des milliers de fichiers sur le disque pour correspondre au commit cible. C’est normal. Sur un SSD ça reste raisonnable, mais sur un HDD cela peut se sentir. Évitez de frequently switch sur de très gros projets si possible ou essayez d’isoler les composants (ce n’est pas toujours possible).
- *`git switch` vs `git checkout`* : `git switch` est plus récent. Par exemple `git switch featureX` équivaut à `git checkout featureX` pour le changement de branche, et `git switch -c newBranch` équivaut à `git checkout -b newBranch`. Il ne gère pas la partie restauration de fichiers (qui est couverte par `git restore`). Vous pouvez utiliser l’une ou l’autre commande selon votre version de Git, le concept reste le même. Dans ce guide, on mentionne `checkout` car il est très répandu, mais sachez que la tendance est d’utiliser `switch` pour plus de clarté.

En résumé, `git checkout` (ou switch) est la commande pour naviguer entre les branches de développement, et occasionnellement pour annuler des changements de fichiers (bien que `git restore` soit la méthode moderne pour cela). C’est l’une des commandes de base du travail quotidien avec Git.

### 3.11 `git branch`

**Description :** La commande `git branch` sert principalement à **gérer les branches** dans votre dépôt. Sans argument, `git branch` liste les branches locales existantes, en indiquant par une étoile `*` celle qui est actuellement cochée. Avec des arguments, elle permet de créer, renommer ou supprimer des branches.

Principales utilisations :
- `git branch <nom>` : crée une nouvelle branche portant `<nom`, pointant sur le commit courant. **Note** : cela ne vous bascule pas dessus (elle est créée mais vous restez sur la branche actuelle).
- `git branch -d <nom>` : supprime la branche nommée (s’il a déjà été fusionnée dans sa branche parente typiquement). `-D` force la suppression même si la branche n’est pas fusionnée (risque de perte de commits non intégrés).
- `git branch -m <ancien> <nouveau>` : renomme la branche locale.
- `git branch -r` : liste les branches distantes (celles vues via fetch/pull, en lecture seule locale).
- `git branch -a` : liste toutes les branches, locales et distantes.

**Cas d’utilisation :** En développant avec Git, vous créez de nombreuses branches de travail. Par exemple :
- Vous êtes sur `main` et voulez démarrer une nouvelle feature : `git branch featureX` va créer la branche featureX sur le même commit que main. Ensuite `git checkout featureX` pour y aller (ou directement `git checkout -b featureX` comme vu).
- Après avoir mergé featureX dans develop, vous voulez nettoyer : `git branch -d featureX` supprimera la branche locale. (Si Git répond "*error: The branch 'featureX' is not fully merged.*", il refuse car vous n’avez pas intégré featureX ailleurs. Si vous êtes sûr de vouloir la supprimer quand même, utilisez `git branch -D featureX`).
- Vous avez mal orthographié une branche ou souhaitez renommer `featureX` en `feature-login`: `git branch -m featureX feature-login`. Si vous étiez positionné sur featureX, cette commande va renommer la branche courante, sinon il faut spécifier l’ancien nom. NB: Renommer une branche ne renomme pas la branche côté distant, s’il y en a une, c’est une opération séparée (supprimer l’ancienne sur le remote et pousser la nouvelle).
- Pour voir sur quoi pointent vos branches, `git branch -v` donne la dernière commit de chaque. Et `git branch -vv` même leur branche de suivi (vous verrez par ex `main     abc1234 [origin/main] message...` indiquant que main suit origin/main).

**Erreurs fréquentes :**
- *Trop de branches locales encombrantes* : Ce n’est pas une erreur de commande, mais un souci d’organisation. Il arrive qu’on accumule des branches qu’on n’utilise plus. Faites le ménage de temps en temps avec `git branch -d` sur celles mergées. Pour celles poussées sur le serveur, vous pouvez les supprimer sur le serveur via l’interface (ou `git push origin --delete nom` en CLI).
- *Branches distantes orphelines* : Si vous listez les branches distantes (`git branch -r`) vous pourriez voir des branches qui n’existent plus côté serveur parce que quelqu’un les a supprimées. Comme mentionné, `git fetch --prune` aide à nettoyer ces références.
- *Suppression accidentelle d’une branche non fusionnée* : Si vous forcez un `git branch -D` sur une branche sur laquelle vous aviez des commits pas encore mergés ailleurs, ces commits deviennent orphelins (pas perdus immédiatement grâce au reflog, mais plus référencés). Vous pouvez les récupérer via `git reflog` ou en recréant la branche à la main s’il le faut (si vous avez le SHA du commit par ex). Soyez prudent avec `-D`, Git vous prévient par le message dans le -d normal, écoutez-le.
- *Comprendre HEAD vs nom de branche* : Lorsque vous créez une branche, c’est juste un pointeur. HEAD est ce sur quoi vous êtes positionné. Créer une branche sans checkout signifie HEAD reste sur l’ancienne. Parfois, les nouveaux utilisateurs font `git branch feature` puis commencent à commit en pensant être sur feature alors qu’ils sont restés sur main. Ils finissent par committer sur main. Moralité : après `git branch feature`, n’oubliez pas `git checkout feature`. Ou combinez en `git checkout -b feature` d’emblée pour éviter ça.
- *Nom de branche contenant des slash (/) ou caractères spéciaux* : Git permet des noms style `feature/login-ui`. Ce sont juste des conventions, mais attention si vous essayez de créer une branche qui a le même nom qu’un préfixe d’une autre existante, ça peut poser souci (par ex. avoir `feature` comme branche et `feature/login` une autre). Ce n’est pas interdit, mais ça peut semer la confusion. Aussi, évitez les espaces ou caractères non autorisés dans les noms de branches. Utilisez `-` ou `/` comme séparateurs, c’est standard.

**Branches locales vs distantes** : Rappel, lorsqu’on clone, on obtient une branche locale (say main) qui suit origin/main, plus d’autres références distantes (origin/featureX par ex). Si vous voulez **créer une branche locale à partir d’une branche distante** que vous n’avez pas encore en local, faites `git checkout -b featureX origin/featureX`. Ce sera plus facile que `git branch featureX origin/featureX` + `git checkout featureX`. Visual Studio ou autres outils font ça aussi via l’UI ("checkout remote branch"). 

`git branch` est une commande d’inspection et de maintenance. Vous pouvez tout à fait ne l’utiliser que pour lister, et utiliser `checkout -b` pour créer et `push`/`branch -d` pour supprimer. Lister souvent vos branches et leurs états aide à savoir où vous en êtes.

### 3.12 `git stash`

**Description :** La commande `git stash` vous permet de **mettre de côté temporairement des modifications** non commitées. C’est extrêmement utile lorsque vous êtes en train de travailler sur quelque chose et que vous devez soudainement changer de contexte (passer sur une autre branche pour corriger un bug urgent, faire un pull, etc.) sans avoir le temps de finaliser vos modifications en cours. Le *stash* agit comme une pile (stack) de changements : chaque `git stash` pousse les modifications courantes dans le stash, et vous pouvez les récupérer plus tard.

Par défaut, `git stash` va :
- Prendre toutes les modifications suivies (indexées ou non) dans votre répertoire de travail (par rapport à HEAD) et les sauvegarder dans une nouvelle "entrée" du stash.
- Réinitialiser votre working directory à HEAD, comme si vous n’aviez jamais touché ces fichiers (les modifications disparaissent de la vue, mais sont stockées dans la pile stash).
- Ne pas inclure les nouveaux fichiers non suivis, par défaut. Il existe l’option `git stash -u` ou `--include-untracked` pour également stasher les fichiers non suivis (mais pas les fichiers ignorés .gitignore à moins d’ajouter `-a` pour *all*).

Chaque stash est identifié par un nom du style `stash@{0}`, `stash@{1}`, etc., 0 étant le plus récent.

**Cas d’utilisation :** Vous êtes au milieu d’une refonte du module X sur la branche `develop`, et on vous signale un bug critique à corriger sur `main` immédiatement.
1. Vous n’êtes pas prêt à commiter votre travail en cours (il ne compile peut-être même pas), donc vous faites `git stash save "WIP refonte module X"` (vous pouvez ajouter un message après save, sinon juste `git stash` utilise un message par défaut). 
   - Git prend vos modifications (disons vous aviez 5 fichiers modifiés et 1 nouveau fichier) et les range dans le stash. Si vous n’avez pas mis -u, le nouveau fichier restera dans votre dossier mais non suivi. Pour gérer le nouveau fichier aussi, il fallait faire `git stash -u`.
   - Votre répertoire de travail revient propre, comme au dernier commit.
2. Maintenant vous pouvez `git checkout main` (sans conflit puisque plus de modifs locales) et créer une branche bugfix ou corriger directement. Vous corrigez le bug, committez, poussez.
3. Vous revenez sur `develop` : `git checkout develop`.
4. Pour reprendre votre travail là où vous l’aviez laissé, récupérez votre stash : `git stash pop`. Cette commande applique le stash le plus récent sur votre working directory *et* retire l’entrée du stash (pop = appliquer + drop). Si vous voulez appliquer sans supprimer du stash (par exemple pour l’appliquer sur deux branches, ou au cas où l’application a des conflits et vous voulez garder l’original), utilisez `git stash apply`.
5. Git fusionne alors les changements du stash avec la branche courante. En général, si vous êtes revenu sur la même branche et qu’aucun autre changement n’est intervenu, ça s’applique sans conflit et vous retrouvez exactement votre état initial. Si entre-temps, des modifications sur develop ou vos propres commits ont touché les mêmes lignes, vous pourriez avoir des conflits lors de l’application du stash, à résoudre manuellement comme un merge.
6. Une fois appliqué, si vous avez utilisé `pop`, votre stash@{0} est supprimé. Si vous aviez fait `apply`, la stash entry reste et il faudrait faire `git stash drop` manuellement pour la retirer quand plus besoin.

**Erreurs/fréquences :**
- *Oublier les fichiers non suivis* : Comme mentionné, `git stash` de base ignore les untracked. Nombreux sont les devs qui stashent puis changent de branche, pour découvrir que certains fichiers qu’ils avaient créés restent dans leur dossier (parce que non suivis donc non stashes) ou, pire, qu’ils sont allés sur une autre branche où un fichier du même nom existe et du coup le checkout est bloqué. Pour éviter cela, pensez à `git stash -u` si vous avez des nouveaux fichiers, ou mieux, ajoutez même les nouveaux fichiers (staging) avant stash, ainsi ils seront pris. Ou utilisez l’option `--include-untracked` pour plus de clarté.
- *Stack multiple* : Vous pouvez stash plusieurs fois. Par ex. stash@{0} est le plus récent. Vous pouvez voir la liste avec `git stash list`. Vous verrez peut-être plusieurs entrées si vous avez stashe plusieurs choses. Vous pouvez choisir laquelle appliquer : `git stash apply stash@{2}` par ex. Si vous ne spécifiez rien, stash apply/pop prend stash@{0}. Pensez à nommer vos stashes (`git stash save "message"`) pour vous repérer.
- *Conflit en appliquant un stash* : Ça peut arriver, surtout si vous stashez longtemps et que la base a bougé. La résolution se fait comme un merge conflict normal. Une fois résolu, exécutez `git stash drop` si vous aviez appliqué, ou `git stash pop` terminera automatiquement après résolution avec un commit intermédiaire ou en vous laissant committer (à vérifier, stash pop sur conflit demande peut-être drop manuel après commit? En fait, en cas de conflit, stash pop n’a pas droppé l’entrée, donc après résolution vous committez et puis vous pouvez faire `git stash drop` manuellement). Le stash n’est pas très verbeux sur comment finir en cas de conflit, mais l’idée est la même : résoudre, puis dropper l’entrée si plus nécessaire.
- *Nettoyer la pile de stash* : Parfois on stash et on oublie. Des semaines après, `git stash list` montre plein de vieux stashes qu’on n’a plus besoin. Vous pouvez tous les vider via `git stash clear` (ça supprime tout). Ou supprimer individuellement avec drop. Avoir trop d’entries stash ne ralentit pas Git de manière notable, mais c’est bien de garder de l’ordre.
- *Utiliser le stash pour basculer des changements d’une branche à une autre* : On peut stash sur une branche A, passer en B, et apply le stash là. Attention, le stash applique les diffs par rapport à la base sur laquelle il a été créé. Si la base diffère trop, vous aurez conflits ou application potentiellement hasardeuse. Mais ça peut servir pour "transférer" un travail commencé sur la mauvaise branche par exemple. 
  - Ex: oups j’ai codé sur main, je voulais sur feature. Je stash sur main (stocke modifications), checkout feature, stash pop. Voilà mes changements migrés. Ensuite commit sur feature. (Alternative plus élégante dans ce cas: `git switch -c feature` même si modifs en cours, depuis Git 2.23, il va faire ça pour vous, ou `git commit` temporaire puis cherry-pick sur autre branche).
- *Ne pas confondre stash et commit* : Stasher n’est pas committer. Un stash n’est pas visible dans l’historique normal et n’est pas partagé avec les autres. Si vous stash et passez à un autre ordinateur ou clone, ce stash n’y sera pas. C’est local seulement. Donc ne stash pas des travaux que tu veux pousser. Commits existent pour ça. Stash est vraiment temporaire local.

`git stash` est donc un sauveteur de contexte, très pratique pour les développeurs multitâches ou pour garder son index propre quand on fait des pulls/merges.

### 3.13 `git reset`

**Description :** La commande `git reset` est un outil puissant pour **annuler des changements locaux** à différents niveaux. Elle peut agir sur trois zones différentes de Git : la HEAD (le commit actuel), l’index (staging) et la copie de travail. Selon les options (`--soft`, `--mixed`, `--hard`), `git reset` va déplacer le HEAD sur un commit cible et éventuellement mettre à jour l’index et la working directory pour refléter ce commit.

En termes simples :
- `git reset --soft <commit>` : recule HEAD sur `<commit>` (ou avance, mais on l’utilise souvent pour reculer), **sans toucher** à l’index ni au working directory. Autrement dit, les commits que vous "annulez" deviennent des modifications en attente (elles restent dans l’index).
- `git reset --mixed <commit>` (c’est le comportement par défaut si on ne spécifie rien) : déplace HEAD, **déindexe** les changements de commits supprimés (l’index correspond alors au commit cible), mais **ne touche pas** au working directory. Les modifications des commits annulés deviennent des modifications non indexées dans votre copie de travail. C’est utile pour "démarquer" un commit tout en conservant les changements dans les fichiers pour éventuellement les corriger/recommitter.
- `git reset --hard <commit>` : c’est la plus drastique. HEAD bouge vers `<commit>`, l’index est ajusté sur ce commit, **et la copie de travail est rendue identique à ce commit**. Cela signifie que toute modification ou commit plus récent que `<commit>` est PERDU (sauf via reflog). On l’utilise pour abandonner définitivement des commits ou modifications.

`<commit>` peut être un SHA, ou HEAD~1 (le commit juste avant HEAD), etc. Souvent on fait `git reset HEAD~1` pour annuler le dernier commit.

Aussi, `git reset <fichier>` (sans `--`flag) a un usage différent : ça désindexe un fichier (équivalent de `git restore --staged <fichier>` avec Git moderne). Par exemple, si vous avez fait un `git add` mais voulez le retirer de l’index sans perdre la modif, on fait `git reset <file>`.

**Cas d’utilisation :** Vous venez de faire un commit trop hâtivement et souhaitez l’annuler mais garder les changements pour les recommitter correctement :
1. Vous avez commit "fix stuff" mais vous réalisez que vous avez oublié un fichier ou le message est mauvais. Plutôt que de faire un commit amend (une autre option), vous décidez de reset. Faites `git reset HEAD~1`. Par défaut c’est un reset --mixed.
2. HEAD recule d’un commit, votre commit "fix stuff" a disparu de l’historique. Cependant, ses modifications sont restées dans votre arbre de travail **et** elles sont désindexées (non staged). `git status` va montrer ces fichiers comme "modifiés mais non indexés". C’est exactement comme avant d’avoir commité.
3. Maintenant vous pouvez, par exemple, `git add` le fichier manquant et recommitter proprement avec le bon message.
4. Notez que si vous aviez poussé ce commit, il faudrait une autre approche (un revert ou un amend+push force). Reset ne doit être fait que pour des commits locaux non partagés de préférence.

Autre exemple : Vous avez fait une bêtise sur 3 commits et voulez tout annuler comme si ça n’était jamais arrivé :
- `git reset --hard HEAD~3`. Attention cela supprime les 3 derniers commits et TOUTES les modifications associées, de l’index et de la copie de travail. Votre état local revient à l’état d’il y a 3 commits. (Les commits supprimés existent encore dans reflog un temps, mais plus dans la vue normale).
- Si ces commits avaient été poussés, c’est plus délicat car vous modifiez l’historique partagé. Il vaudrait mieux dans ce cas faire 3 reverts ou une nouvelle branche force push, etc., selon le contexte.

**Erreurs/fréquences :**
- *Mauvais usage de --hard* : C’est le plus dangereux. Un `git reset --hard` peut faire perdre du travail si on n'a pas bien compris. Par exemple, des novices font `git reset --hard origin/main` pensant que ça va synchroniser leur branche locale sur le remote (ce que ça fait effectivement) mais sans réaliser qu’ils écrasent aussi potentiellement des travaux locaux non commités. D’ailleurs Git vous alerte "uncommitted changes will be lost" si vous le faites. À n’utiliser que si vous êtes sûr de vouloir abandonner les modifications locales.
- *Annuler un commit déjà poussé* : Comme mentionné, `git reset` ne "annule" le commit que localement. Le serveur distant ou les collègues en ont toujours la trace. Si vous forcez un push après un reset (--hard ou --mixed), vous réécrivez l’historique distant. Cela peut être acceptable si c’est une branche en développement avec peu de consommateurs, mais c’est déconseillé sur main. Dans ces cas, `git revert` (voir section suivante) est préférable.
- *Confondre reset HEAD~1 et checkout HEAD~1* : Un `git checkout HEAD~1` vous amène sur le commit précédent en détaché, alors qu’un `git reset HEAD~1` reste sur la même branche mais recule son HEAD. Pour "enlever" un commit de la branche, c’est reset. Pour juste naviguer temporellement, c’est checkout. 
- *Utiliser reset pour désindexer* : Comme indiqué, `git reset <file>` est la manière de retirer un fichier de l’index (staging) tout en gardant ses modifications. Par exemple, vous avez `git add .` par erreur et ajouté un fichier de config, vous voulez l’enlever du prochain commit : `git reset config.yml` va le retirer de l’index (il reste modifié dans WD, uncommitted).
- *--mixed vs sans option* : Par défaut, `git reset` est en mode --mixed. Donc `git reset HEAD~1` c’est mixed. La plupart du temps on précise l’option pour clarté. 
- *Reset HEAD~N sur plus de commits qu’on a* : Git donnera une erreur du type "bad revision" si vous essayez de reculer plus loin que le début de la branche.
- *Recupérer après reset* : Si vous avez fait un reset trop loin ou par erreur, tant que vous n’avez pas fait beaucoup d’autres choses, vous pouvez souvent retrouver le commit perdu via `git reflog` (chaque HEAD est loggé). Reflog listera l’ancien HEAD comme HEAD@{1} par ex. Alors `git checkout HEAD@{1}` retrouvera ce commit. Vous pourrez alors soit refaire un branch le dessus ou cherry-pick. 
  - Ex: `git reset --hard HEAD~1` oh non j’ai perdu un commit que je voulais garder ; `git reflog` repère le SHA de ce commit, ou HEAD@{1}; on peut faire `git cherry-pick <sha>` pour le réappliquer.

En un mot, `git reset` est l’outil pour réécrire localement votre progression de commits. *Soft* pour ne bouger que HEAD, *mixed* pour HEAD + index, *hard* pour tout. Soyez prudent, surtout avec *hard*, et sachez quand ne pas l’utiliser (histoire publiée). 

### 3.14 `git revert`

**Description :** La commande `git revert` crée un **nouveau commit** qui annule les modifications d’un commit précédent spécifique. Contrairement à reset qui modifie l’historique, revert préserve l’historique en l’étendant. Il est souvent utilisé pour annuler proprement des changements dans des branches déjà partagées ou pour revenir sur une fonctionnalité sans réécrire l’historique.

Concrètement, `git revert <commit>` va calculer l’inverse des changements introduits par `<commit>` et les appliquer sur la branche courante en tant que nouveaux changements, puis les committer. Le commit original reste dans l’historique, mais un nouveau commit (de "revert") vient annuler son effet. Git génère par défaut un message de commit du style "Revert '<message du commit original>'" avec le hash du commit annulé.

Vous pouvez revert plusieurs commits (généralement un par un). Attention, pour revert un merge commit, il faut spécifier des options supplémentaires (--mainline) pour indiquer quelle branche considérer comme principale, etc., c’est avancé.

**Cas d’utilisation :** Vous avez fusionné une PR qui finalement pose problème. Vous voulez revenir en arrière sans enlever le commit de l’historique :
1. Identifiez le commit incriminé (par exemple via `git log` ou l’interface GitHub).
2. Sur la branche où le commit a été appliqué (ex: `main`), exécutez `git revert <sha_du_commit>`.
3. Git ouvrira un éditeur pour le message de commit de revert. Modifiez-le si besoin (expliquez pourquoi vous annulez). Enregistrez et fermez.
4. Un nouveau commit est créé disant "Revert: <original message>" ([ Resetting, Checking Out & Reverting | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting#:~:text=A%20revert%20is%20an%20operation,has%20no%20file%20level%20functionality)). Ce commit introduit les modifications inverses. Si le commit original avait ajouté une ligne, le revert la supprime; s’il avait modifié de X à Y, revert modifie de Y à X.
5. Poussez ce commit. Maintenant l’historique montre que le commit problématique est toujours là, mais suivi d’un commit qui l’annule. Le code final n’inclut plus l'effet du commit initial.
6. Si plus tard vous voulez réappliquer ce commit (imaginons que le bug est résolu), on pourrait soit revert le revert, soit cherry-pick l’original commit sur la branche à nouveau.

**Erreurs/fréquences :**
- *Conflit lors d’un revert* : Si entre temps d’autres changements ont touché les mêmes zones, un revert peut provoquer des conflits, comme un merge. Vous devrez résoudre les conflits puis `git revert --continue` (similaire au rebase continue). Le commit de revert sera créé après résolution.
- *Revert vs reset confusion* : Revert n’enlève pas un commit de l’historique, il le "compense". Donc l’historique aura le commit original plus un commit d’annulation. C’est intentionnel, car on veut garder trace. Sur un projet collaboratif, **préférez revert pour annuler des commits partagés**, car ça évite les push forcés. Reset ne doit pas être utilisé sur les branches partagées, revert est la solution propre ([ Resetting, Checking Out & Reverting | Atlassian Git Tutorial ](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting#:~:text=remote%20shared%20repositories,members%20may%20be%20dependent%20on)).
- *Revert d’un commit ancien* : Vous pouvez revert un commit qui n’est pas le dernier. Par exemple, HEAD->C3, vous revert C2. Techniquement, Git applique l'inverse de C2 sur C3. Ça peut créer un état qui n’est pas exactement identique à comme si C2 n’avait jamais existé si il y a eu d’autres commits qui dépendaient de C2. Souvent il faut réfléchir : si C3 dépend de C2, revert C2 peut nécessiter ajuster C3 aussi. Parfois on revert une série de commits dans l’ordre inverse de leur application pour bien tout annuler.
- *Revert d’un merge commit* : C’est tricky. Imaginons on a merge feature branch dans main via commit M. Pour revert M, Git doit savoir quel parent du merge est la "cible". Habituellement on veut annuler le contenu de la feature, donc on garde le parent main et on inverse les diffs de l’autre parent. `git revert -m 1 <mergeCommit>` (si 1 est le parent main). Ce commit revert va introduire un commit inverse de tous les changements de la feature. On utilise ça souvent quand une feature merge cause un gros problème, on revert le merge entier, ce qui est plus simple que revert chaque commit de la feature. 
- *Double revert* : Revert annule un commit, mais si plus tard on revert le revert, on remet les changements initiaux. Ceci est viable, par ex. revert un hotfix que l’on avait appliqué temporairement.
- *Revert et commit partiel* : Si un commit initial faisait plein de choses et qu’on ne veut en annuler qu’une partie, revert va tout annuler. Il faudrait alors réappliquer manuellement ce qu’on voulait garder. Parfois, mieux vaut faire un commit correctif manuel plutôt qu’un revert auto dans ces cas partiels.

La commande revert est un peu le "CTRL+Z" de l’historique partagé : on ne supprime pas l’action, on fait une action contraire. C’est très utilisé dans les projets pro pour corriger rapidement sans casser la synchronisation des dépôts clones.

### 3.15 `git log`

**Description :** `git log` affiche l’**historique des commits** du dépôt. Par défaut, elle liste les commits de la branche courante, du plus récent au plus ancien, avec SHA, auteur, date et message. C’est indispensable pour examiner l’historique, chercher quand un changement a été introduit, etc. 

`git log` a de **nombreuses options** pour filtrer ou formater la sortie :
- `git log --oneline` : format condensé (une ligne par commit avec le début du SHA et le message). Très pratique pour une vue rapide.
- `git log --graph` : affiche un joli graphe ASCII représentant la structure des branches et merges dans l’historique. Souvent couplé avec --oneline et --all (pour voir toutes branches).
- `git log -p` : affiche en plus le diff (patch) de chaque commit. Utile pour voir le contenu exact de chaque commit (génère beaucoup de texte).
- `git log -n` : limite le nombre de commits (ex: `-5` pour les 5 derniers).
- Filtrage :
  - `git log --author="Nom"` : ne montre que les commits dont l’auteur correspond.
  - `git log --since="2023-01-01"` : commits après une date donnée, ou `--until=...` avant une date.
  - `git log --grep="keyword"` : filtre par mot-clé dans le message de commit.
  - `git log <chemin>` : n’affiche que les commits qui ont modifié le fichier/dossier donné.
  - `git log branchA..branchB` : affiche les commits qui sont sur branchB et pas sur branchA (différence d’historique, très utile pour voir ce qui diffère entre deux branches).

Vous pouvez combiner plusieurs filtres.

Par défaut, `git log` ouvre la sortie dans un pager (less) si c’est long. Vous pouvez naviguer avec flèches ou Espace, et taper `q` pour quitter l’affichage.

**Cas d’utilisation :** Vous voulez avoir un résumé des derniers commits sur votre branche :
- `git log --oneline -10` : montre les 10 derniers commits en résumé. Par ex:
  ```
  a1b2c3d Correction du bug de login
  f6e7d8a Ajout de la fonctionnalité X
  9a0b1c2 Merge branch 'feature/ui'
  ...
  ```
- Vous cherchez dans quel commit une certaine fonction a été ajoutée :
  `git log -S 'nomDeFonction' --oneline` va lister les commits contenant l’ajout ou suppression de la chaîne 'nomDeFonction' dans le diff (l’option -S fait une recherche dans les patches).
- Vous voulez voir l’historique d’un fichier spécifique : `git log -p -- MyFile.java` vous montrera tous les commits qui ont modifié MyFile.java, avec le patch pour ce fichier dans chaque commit.
- Comprendre un merge : `git log --graph --oneline --all` affichera un graphe de toutes les branches ce qui aide à visualiser les merges. Par exemple:
  ```
  *   c3f8e9d Merge branch 'feature' into main
  |\
  | * 98bd123 Implement feature
  | * 76ac9ef Initial commit of feature
  * | 1a2b3c4 Some commit on main
  |/
  * 4d5e6f7 Previous commit on main
  ```
  On voit le graphe du merge.

**Erreurs/fréquences :**
- *Sortie trop verbeuse* : Sur un dépôt ancien, `git log` simple peut dérouler des milliers de commits. Pensez aux filtres (date, nombre, grep) ou à `--oneline` pour compacter.
- *Recherche inefficace* : Si on cherche un commit par son contenu ou comportement, mieux vaut utiliser des outils spécialisés comme `git bisect` (non couvert ici) ou du grep sur log/diff comme montré. `git log -S` et `git log -G` (regex sur diff) sont très utiles pour traquer l’introduction ou la suppression d’un code particulier.
- *Comprendre HEAD, HEAD^, HEAD~ etc dans log* : Vous pouvez passer des références relatives. Par ex `git log HEAD~3..HEAD` montre les 3 derniers commits (exclusif du bas). HEAD~3..HEAD signifie commits après HEAD~3 jusqu'à HEAD. Ou `git log main..feature` (commits qu'a feature en plus que main).
- *Affichage de noms de branche et tags* : Par défaut, log mentionne la branche HEAD sur le dernier commit logué, mais pas les autres branches. Avec `--decorate`, les noms de branches et tags apparaissent à côté des SHAs sur la première occurrence. Par ex: `commit abc123 (HEAD -> main, origin/main, tag:v1.0)`.
- *Accès aux stats* : `git log --stat` donne un résumé des fichiers modifiés et lignes ajoutées/supprimées par commit, très lisible pour avoir une idée de la taille de chaque commit sans tout le diff.
- *Couleurs* : Ajoutez `--color` ou config `color.ui` pour avoir un log coloré (auteur en vert, SHA en yellow etc). Souvent déjà activé par défaut.

`git log` n’a pas de "piège" de modification du repo (c’est en lecture seule), donc c’est sans risque de l’utiliser, au pire on peut se perdre dans la navigation (taper `q` pour sortir). Apprendre ses options rend l’exploration de l’historique beaucoup plus efficace.

### 3.16 `git tag`

**Description :** `git tag` permet de **marquer des commits** avec des étiquettes conviviales, souvent utilisées pour marquer les versions (releases). Il y a deux types de tags :
- Les *tags légers* (lightweight) : juste un nom pointant vers un commit (comme une branche qui ne bouge pas).
- Les *tags annotés* (annotated) : un objet Git complet qui inclut un message, un auteur, une date, et peut être signé avec GPG. Recommandé pour les releases, car plus informatif.

Par défaut `git tag <name>` crée un tag léger sur HEAD. Tandis que `git tag -a <name> -m "Message"` crée un tag annoté avec le message donné (et ouvre l’éditeur si pas de -m).

**Cas d’utilisation :** Vous venez de finaliser la version 1.0 de votre logiciel sur la branche main. Vous souhaitez taguer ce commit :
1. Assurez-vous d’être sur le bon commit (souvent HEAD de main). 
2. Exécutez `git tag -a v1.0 -m "Release version 1.0"`. Cela crée un tag annoté nommé "v1.0".
3. Si vous faites `git tag` sans argument, la liste de tous les tags connus s’affiche, vous verrez "v1.0".
4. N’oubliez pas, par défaut les tags **ne sont pas poussés** sur le remote avec un simple `git push`. Il faut pousser le tag explicitement : `git push origin v1.0` (ou `git push --tags` pour tous les tags d’un coup). Sur GitHub/GitLab, une fois poussé, le tag sera visible et souvent utilisé pour publier des releases attachées.
5. Plus tard, pour voir à quel commit correspond un tag : `git show v1.0` vous montrera le commit complet (et le message du tag annoté).
6. Si par erreur vous avez taggé le mauvais commit, vous pouvez supprimer le tag local (`git tag -d v1.0`) puis s’il avait été poussé aussi le supprimer distant (`git push origin :refs/tags/v1.0`). Ensuite retagger le bon commit et repush.

**Erreurs/fréquences :**
- *Tag déjà existant* : Si vous essayez de créer un tag qui existe, Git dira erreur. Vous devrez peut-être le supprimer d’abord ou renommer. Évitez de réutiliser le même nom de tag pour un commit différent sur un repo partagé, ça peut créer de la confusion (les gens qui ont déjà récupéré l’ancien tag ne verront pas le changement sans forcer).
- *Oublier de pousser les tags* : Très fréquent. On tag en local, on push le commit, on pense que tout est sur origin. Mais `git pull` d’un collègue ne ramènera pas vos tags tant qu’il ne fait pas `git fetch --tags`. Il est donc important, quand c’est pertinent, de push les tags. Surtout pour les tags de version. Sinon par ex CI ou outils de déploiement ne verront pas le tag.
- *Légers vs annotés* : Un tag léger est juste un nom pour un commit. Il ne stocke pas l’auteur du tag ni message. Il est créé rapidement avec `git tag name` sans -a. On utilise tag léger parfois pour marquer un commit temporairement, ou pour des choses internes, mais pour des versions officielles, toujours faire -a (ou -s pour signé).
- *Tag signé* : `git tag -s v1.0` signera le tag avec votre clé GPG. Cela permet sur GitHub d’avoir "Verified". Il faut avoir configuré GPG. C’est bien pour les releases sécurité etc.
- *Lister les tags triés* : `git tag -l` supporte globbing, ex `git tag -l "v1.*"` pour lister tous tags commençant par v1. Pas d’erreurs ici mais bon à savoir.
- *Détail technique* : Un commit ne peut pas savoir quel tag le référence. Les tags sont comme des branches, des refs externes. Donc dans `git log` on voit les tags attachés aux commits dans la décoration. On peut chercher un commit par tag via `git show tagname`. 
- *Nom de tag vs nom de branche* : Ils partagent le même espace de noms global des refs, sauf qu’on différencie par prefix (refs/heads/ vs refs/tags/). Donc vous pouvez avoir un tag "develop" et une branche "develop", Git sait faire la différence en interne, mais pour les humains c’est confus. Évitez de nommer un tag pareil qu’une branche active.
- *Tags et versions multiples* : Les tags sont souvent utilisés pour gérer plusieurs versions maintenues. Par ex, tag v1.0, v1.1 etc sur main. Puis peut-être on crée des branches de maintenance à partir de ces tags. C’est plus du workflow, pas directement la commande.

En résumé, utiliser des tags vous permet de pointer aisément sur des jalons dans l’historique. C’est très pratique pour diffuser des versions stables aux utilisateurs, ou même pour vous, pour comparer des états du code entre versions (`git diff v1.0 v2.0`). N’oubliez pas de les partager (push) pour en faire profiter l’équipe.

### 3.17 `git cherry-pick`

**Description :** `git cherry-pick` applique un commit existant (identifié par son hash) sur la branche courante en en créant un **nouveau commit** avec les mêmes changements. C’est littéralement "cueillir une cerise" d’une branche pour la mettre sur une autre, sans prendre les autres commits autour. 

C’est utile si un correctif a été fait sur une branche (ex: main) et qu’on veut l’appliquer aussi à une autre branche (ex: develop ou une release antérieure), sans faire un merge complet. Ou pour extrair un commit spécifique d'une série.

**Cas d’utilisation :** Vous avez corrigé un bug sur la branche `develop` (commit abc123). Ce bug existe aussi sur la branche `production` (plus ancienne) que vous maintenez séparément. Vous voulez y appliquer la même correction sans tout merger.
1. Assurez-vous d’être sur la branche `production`: `git checkout production`.
2. Exécutez `git cherry-pick abc123` (abc123 étant le SHA du commit correctif sur develop).
3. Git va appliquer le diff de ce commit sur votre working directory de production, puis faire un commit avec le même message (préfixé "cherry-picked from abc123" en note dans le message si commit normal, ou peut-être c’est dans la metadata).
4. Si l’application se fait sans conflit, vous aurez un nouveau commit sur production qui introduit ce correctif. Si le code autour a divergé et cause un conflit, Git arrêtera en conflit, et il faudra résoudre comme d’habitude puis `git cherry-pick --continue` pour finaliser.
5. Poussez la branche production. Elle a maintenant le bug corrigé, sans avoir pris d’autres changements de develop.
6. Notez que le commit sur production aura un **nouvel identifiant SHA**, différent de develop, car l’historique est différent. Mais le contenu et le message seront les mêmes (à un près).
7. Si par erreur le commit cherry-pick n’était pas correct ou qu’il introduit des problèmes, on pourrait revert ce commit sur production sans toucher develop.

**Erreurs/fréquences :**
- *Dépendances entre commits* : Cherry-pick un commit qui faisait partie d’une série peut poser problème. Par exemple, commit B dépend de modifications introduites par commit A précédent. Si on cherry-pick B sans A, il se peut que le code ne compile pas ou manque quelque chose. Il faut donc examiner le commit qu’on veut cueillir, et s’il dépend d’autres commits, peut-être cherry-pick ceux-là aussi, dans le bon ordre. On peut cherry-pick plusieurs commits en une fois en listant les SHAs ou un range (ex: `git cherry-pick A^..B` pour prendre A et B).
- *Conflits* : As comme merges et rebase, cherry-pick peut avoir des conflits s’il ne peut pas appliquer proprement. La procédure de résolution est identique : corriger les fichiers, `git add`, `git cherry-pick --continue`. Ou `--abort` pour annuler le cherry-pick en cours si ça tourne mal.
- *Cherry-pick d’un commit de merge* : Par défaut, il n’est pas trivial de cherry-pick un commit de merge, car quel diff appliquer ? On peut soit éviter, soit utiliser l’option `-m` comme pour revert pour choisir la parent. Mieux vaut reproduire les modifications manuellement ou cherry-pick la série de commits d’une branche plutôt que le merge commit lui-même.
- *Historique moins clair* : L’usage intensif de cherry-pick peut rendre l’historique confus, car on voit le même changement apparaître plusieurs fois sur différentes branches sans relation de merge. Idéalement, on mentionne dans le message "Cherry-picked from commit abc123" (ce que Git fait automatiquement en commentaire dans un commit cherry-pick). Ainsi on sait d’où ça vient. Sur GitHub, si un commit mentionne "cherry picked from ..." et ce SHA existe sur une autre branche, l’interface peut faire le lien.
- *Alternative par merge ou rebase* : Parfois, utiliser cherry-pick pour porter un patch fix est le plus simple (ex: maintenir deux lignes de développement). D’autres fois, il vaudrait mieux faire un merge complet si la divergence n’est pas voulue ou trop compliqué. Cherry-pick est bien pour les backports isolés.
- *Annuler un cherry-pick* : Si vous cherry-pick par erreur un commit, vous pouvez soit `git reset HEAD~` s’il n’était pas poussé, soit `git revert` ce commit sur la branche.
- *Cherrypick multiple facilité* : Si vous avez une liste de SHAs, vous pouvez faire `git cherry-pick SHA1 SHA2 SHA3` en une commande, Git appliquera successivement chaque commit. En cas de conflit sur un commit au milieu, il s’arrête là. Vous devrez après résolution continuer (il reprendra la liste suivante je crois).
- *git cherry* (sans pick) : Il existe aussi `git cherry` pour voir quels commits d’une branche ne sont pas sur une autre, potentiellement pour savoir quoi cherry-pick. Ex: `git cherry main feature` listera les commits sur feature pas dans main, marqués +, et ceux potentiellement identiques (même patch id) marqués -. Pas très utilisé couramment, mais utile.

Dans l’ensemble, cherry-pick est une commande de "secours" pour la gestion de branches divergentes. Bien utilisée, elle évite du copier-coller manuel et garde l’authorship original. Il faut en comprendre les limitations (dépendances, conflits) et l’utiliser judicieusement pour ne pas perturber la cohérence du code entre branches.

### 3.18 `git remote`

**Description :**  
La commande `git remote` permet de gérer les dépôts distants (ou *remotes*) associés à votre dépôt local. Elle sert principalement à :  
- **Lister** les remotes configurés (`git remote` ou `git remote -v`),  
- **Ajouter** un nouveau remote (`git remote add <nom> <URL>`),  
- **Renommer** un remote (`git remote rename <ancien_nom> <nouveau_nom>`),  
- **Changer l’URL** d’un remote (`git remote set-url <nom> <nouvelle_URL>`),  
- **Supprimer** un remote (`git remote remove <nom>`).  

En général, lorsque vous clonez un dépôt, Git crée automatiquement un remote nommé `origin`, qui pointe vers le dépôt sur lequel vous avez cloné. Vous pouvez ensuite interagir avec ce remote pour pousser/puller du code, créer des branches distantes, etc.  

---

**Cas d’utilisation :**  
Vous avez un dépôt local et vous souhaitez l’associer à un dépôt hébergé en ligne (GitHub, GitLab, etc.) pour partager votre code.

1. **Initialisez** un dépôt Git local si ce n’est pas déjà fait :  
   ```bash
   git init
   ```
2. **Ajoutez** un remote pointant vers votre dépôt en ligne :  
   ```bash
   git remote add origin https://github.com/votrecompte/votreprojet.git
   ```
   - `origin` est le nom usuel donné au remote principal, mais vous pouvez choisir un autre nom.
3. **Vérifiez** la liste de vos remotes :  
   ```bash
   git remote -v
   ```
   Vous devriez voir `origin` avec deux URL (fetch/pull et push).
4. **Poussez** vos modifications locales vers ce remote :  
   ```bash
   git push -u origin main
   ```
   - L’option `-u` (ou `--set-upstream`) établit le suivi de la branche locale `main` avec la branche distante `origin/main`.
5. **Montrez** les informations détaillées d’un remote (branches suivies, etc.) :  
   ```bash
   git remote show origin
   ```
6. **Modifiez** l’URL d’un remote si nécessaire :  
   ```bash
   git remote set-url origin https://github.com/nouveaucompte/nouveauprojet.git
   ```
7. **Supprimez** un remote si vous n’en avez plus besoin :  
   ```bash
   git remote remove origin
   ```
   (ou `git remote rm origin` sur d’anciennes versions)

---

**Erreurs/fréquences :**  
- *Confusion entre remotes et branches distantes* : Le remote est un simple "pointeur" vers un dépôt distant. Les branches distantes (ex: `origin/main`) sont des références à ce qui existe sur ce dépôt. Ne confondez pas la branche locale `main` avec la branche distante `origin/main`.  
- *Nom de remote incorrect* : Si vous essayez de `git push origin main` alors que votre remote s’appelle `upstream`, vous aurez des erreurs. Vérifiez vos remotes avec `git remote -v`.  
- *URL mal configurée* : Si vous changez de plateforme (ex: GitHub → GitLab) et que vous oubliez de mettre à jour l’URL, vous aurez des erreurs d’authentification ou de permission.  
- *Écrasement de branches* : En ajoutant un nouveau remote, attention de ne pas pousser vos branches sur la mauvaise URL ou de forcer un push qui pourrait écraser un historique déjà existant.  
- *Problème de droits/accès* : Assurez-vous d’avoir les droits en écriture sur le dépôt distant. Sinon, Git refusera votre push.  
- *Remotes multiples* : Il est possible d’avoir plusieurs remotes (ex: `origin`, `upstream`, `fork`), ce qui peut compliquer la gestion de vos branches. Soyez clair sur qui est le dépôt "principal" et sur le rôle de chaque remote.  
- *Renommage* : Si vous renommez un remote, n’oubliez pas de corriger vos commandes (push, pull, fetch) et éventuellement vos branches locales qui traquaient l’ancien nom.

Dans l’ensemble, `git remote` est la base pour relier un dépôt local à un dépôt distant et gérer la coordination entre plusieurs dépôts. C’est un élément crucial pour le travail collaboratif.
---

Voilà pour la liste des commandes essentielles, avec pour chacune des exemples d’utilisation et les erreurs courantes à éviter/corriger. Dans la section suivante, nous aborderons l’organisation du dépôt et les bonnes pratiques pour garder un historique propre et un travail d’équipe efficace.

## 4. Organisation et bonnes pratiques pour structurer un dépôt proprement

Un dépôt Git bien organisé facilite la compréhension du projet, la collaboration et la maintenance à long terme. Voici des bonnes pratiques de structuration et de gestion d’un dépôt :

### 4.1 Structuration du contenu du dépôt

- **Répartition logique des répertoires** : Organisez vos fichiers en dossiers significatifs (par exemple `src/` pour le code source, `docs/` pour la documentation, `tests/` pour les tests, etc.). Évitez d’avoir tous les fichiers à la racine du dépôt.
- **Fichiers de documentation** : Incluez un fichier `README.md` à la racine du dépôt décrivant le projet, comment l’installer/utiliser, les contributeurs, etc. C’est souvent la première chose que les nouveaux arrivants lisent. De même, un fichier `CHANGELOG.md` peut lister les évolutions par version, et un `CONTRIBUTING.md` peut expliquer comment contribuer (utile pour les projets open-source).
- **Fichier LICENSE** : Si le projet a une licence (MIT, GPL, etc.), placez le texte de la licence dans un fichier `LICENSE` ou `LICENSE.txt` à la racine. Cela clarifie les conditions d’utilisation du code.
- **Fichiers de configuration et CI** : Gardez les configurations (par ex. `.editorconfig`, `CI/CD config` comme `.github/workflows/...` ou `.gitlab-ci.yml`) dans des emplacements standards. Ne les dispersez pas aléatoirement.
- **Exclusion des fichiers non pertinents** : Utilisez `.gitignore` pour éviter de versionner des fichiers qui ne doivent pas l’être (fichiers de build, dépendances instanciées, fichiers temporaires, données sensibles, etc.). Un dépôt propre ne contient pas de fichiers générés ou locaux spécifiques à un utilisateur. Cela évite des pollutions de diffs et garde l’historique concis. Par exemple, ignorez les répertoires `node_modules/` pour Node, les binaires `.exe` ou `.dll` compilés, les fichiers `.log`, les fichiers d’IDE comme `.vscode/` ou `.idea/`, etc.
- **Taille du dépôt** : Évitez de versionner de gros fichiers binaires ou des datasets volumineux directement dans Git (sauf si c’est le cœur du projet). Git n’est pas optimisé pour stocker d’énormes fichiers (chaque clone doit tout récupérer). Si vous devez versionner des assets volumineux, envisagez **Git LFS (Large File Storage)** ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Upstreams%20et%20forks%20git%20%3A,guide%20pratique%20et%20astuce%20sympa)) qui stocke les gros fichiers sur un serveur séparé tout en les référant dans Git. Sinon, stockez ces fichiers ailleurs (ex: un stockage cloud) et ne mettez que des références.
- **Arborescence cohérente sur toutes les branches** : Essayez de maintenir une structure similaire entre branches principales. Par ex, ne pas avoir un code complètement déplacé d’un dossier à un autre sur une branche parallèle sauf raison majeure. Sinon les merges et comparaisons deviennent complexes (rename detection).
- **Scripts utilitaires** : Si vous avez des scripts (de build, de déploiement), incluez-les dans le dépôt (ex: un `Makefile`, un script `build.sh`, etc.). Ainsi, tout le monde utilise les mêmes outils et cela documente le processus. Documentez leur usage dans le README.

### 4.2 Bonnes pratiques de commits et d’historique

- **Commits atomiques et logiques** : Un commit devrait idéalement représenter une **étape cohérente** du développement : par exemple "Implémente la fonctionnalité de recherche" ou "Corrige le bug d’affichage du profil". Évitez les commits fourre-tout qui mélangent des changements sans rapport. Inversement, évitez de disperser une même modification logique sur 10 commits (sauf si vous faites du commit très fréquent localement puis squashez). 
- **Fréquence de commit** : Commitez suffisamment souvent pour ne pas perdre beaucoup de travail et pour faciliter la review. Un adage est "*Commit early, commit often.*" Tant que vous travaillez sur une branche de fonctionnalité, vous pouvez committer dès que vous atteignez un point stable ou un jalon intermédiaire.
- **Messages de commit clairs** : Soignez vos messages de commit. La première ligne doit résumer l’essentiel. Exemple : `"Fix NPE when user profile is null"` au lieu de `"fix bug"` trop vague. Si le commit est complexe, ajoutez un corps de message plus détaillé après une ligne vide, expliquant le pourquoi du changement, éventuellement la méthode, ou les références de tickets (`Fixes #123` pour lier un ticket d’issue tracker). Un bon historique de commit sert de documentation vivante du projet.
- **Eviter les secrets dans l’historique** : Ne commitez jamais de mots de passe, clés API ou secrets sensibles. Utilisez des fichiers de configuration example (genre `.env.example`) et ajoutez le vrai `.env` dans le .gitignore. Si par mégarde un secret a été commit, changez-le (révoquez la clé, etc.) et envisagez de purger le commit de l’historique (via des outils comme BFG Repo-Cleaner), mais c’est compliqué si déjà poussé publiquement. La prévention est plus simple que la guérison ici.
- **Vérifier avant de pousser** : Avant un `git push`, surtout sur la branche principale, assurez-vous d’avoir testé votre code (compiler, lancer les tests unitaires). Configurez idéalement une intégration continue (CI) qui run les tests sur chaque push ou PR. Une bonne pratique est de ne pousser sur `main` que du code qui passe les tests et de préférer les merges via PR validées.
- **Historique lisible** : Sur les branches principales (main/master, develop), essayez d’avoir un historique propre. Cela peut impliquer de **squasher** certains commits de travail avant de fusionner. Par exemple, sur GitHub lors du merge d’une PR, vous pouvez "Squash and merge" pour condenser tous les commits de la PR en un seul commit de fonctionnalité. Cela garde main plus lisible (un commit = une fonctionnalité ou un bugfix), au prix de perdre la granularité interne (mais qui de toute façon n’a plus forcément d’importance une fois la PR fusionnée).
- **Ne pas réécrire l’historique public** (sauf cas spécial) : Une fois que des commits sont partagés (poussés sur une branche à laquelle d’autres contribuent), éviter de les modifier ou supprimer via rebase ou reset. Cela cause des désynchronisations pour les autres. Préférez ajouter de nouveaux commits (ex: un commit "fix review comments" plutôt que de re-squash le commit initial après l’avoir poussé). Bien sûr, sur vos branches de feature personnelle, vous pouvez réécrire tant que ce n’est pas en PR ou pas relu, pour nettoyer avant partage.
- **Tags pour versions stables** : Comme vu en section précédente, taguez les commits de version majeure ou release. Cela permet de facilement retrouver l’état du code correspondant à une version donnée, et de créer des branches de hotfix sur ces tags si besoin.
- **Gardez la branche principale *propre*** : Une stratégie commune est que la branche `main` ou `master` contienne toujours du code **déployable ou stable**. On ne pousse jamais directement sur main sans review, on passe par des branches intermédiaires (feature branches + PR). Et on ne merge sur main qu’après validation (tests passés, code review). Cela assure que l’historique de main correspond à des états stables (et si quelque chose échappe à cette règle, on revert rapidement pour revenir à l’état stable).

### 4.3 Gestion des dépendances et builds

- **Ne pas versionner les dépendances externes** : Par exemple, ne commitez pas les bibliothèques tierces jar, node_modules, etc. Utilisez un gestionnaire de paquets (Maven/Gradle, npm, pip, etc.) et laissez les utilisateurs les télécharger. Ça allège le dépôt et évite d’avoir du code tierce difficile à mettre à jour. Si une dépendance n’est pas disponible autrement (cas rare), envisagez un submodule ou un dépôt séparé.
- **Build reproductible** : Essayez de configurer le projet de sorte que n’importe quel développeur puisse cloner puis exécuter un petit nombre de commandes pour builder/lancer l’application. Documentez ces étapes dans le README. Par exemple "npm install && npm run dev" ou "mvn package", etc. Incluez la configuration (fichiers pom, package.json...) dans le dépôt, bien sûr.
- **Environnements spécifiques** : Si votre projet doit être configuré différemment selon les environnements (dev, test, prod), gérez cela via des fichiers de config non versionnés (injection de variables d’environnement, etc.) ou des profils. Ne modifiez pas le code ou les fichiers versionnés pour chaque env, sinon vous aurez des commits du style "disable debug logs for prod" puis "re-enable for dev" qui polluent. 
- **Nettoyage** : Au fur et à mesure, supprimez du dépôt les fichiers devenus inutiles (code mort, assets plus utilisés). Cela peut se faire dans des commits séparés avec mention "*Remove unused ...*". Un dépôt contient souvent des vestiges d’anciennes versions. Autant que possible, nettoyez pour que ce qui est versionné soit pertinent. (Attention toutefois à ne pas supprimer quelque chose d’encore utile).

### 4.4 Autres bonnes pratiques diverses

- **Intégration continue** : Même si ce n’est pas dans Git même, configurez un pipeline CI pour valider chaque commit pushé (tests, lint). L’état du dépôt n’en sera que plus sain. Commitez par exemple un fichier de config de GitHub Actions ou Jenkinsfile dans le dépôt.
- **Hooks Git locaux** : Vous pouvez utiliser des hooks (scripts dans `.git/hooks/`) pour appliquer des règles localement. Par exemple, un hook pre-commit pour exécuter un linter ou empêcher de committer du code avec des mots-clés interdits. Cependant, ces hooks ne se propagent pas via Git, il faut les partager séparément (ou utiliser un outil comme Husky pour les installer via npm scripts). 
- **Git Attributes** : Pensez à `.gitattributes` pour spécifier comment Git traite certains fichiers (ex: normaliser les fins de ligne, traiter certains fichiers binaires pour qu’ils ne montrent pas de diff, ou définir des fusionneurs personnalisés). Surtout sur les projets multi-OS, configurer `* text=auto` dans .gitattributes ou `core.autocrlf` globalement évite les problèmes de fin de ligne CRLF vs LF.
- **Revue de code** : Couplé à Git, utilisez des Pull Requests/Merge Requests pour toute fonctionnalité ou correction majeure. Cela vous force à bien isoler vos changements sur une branche, et donne une occasion de revoir et discuter avant d’intégrer. La qualité du code et de la structure du dépôt s’en trouve améliorée sur le long terme.
- **Conventions de branchement et de commit** : Adoptez des conventions communes (on en parle plus en détails dans la section branches suivante). Par exemple, pour les noms de branches (`feature/`, `bugfix/`...), pour le style de message de commit (certains projets aiment suivre *Conventional Commits* style, avec des préfixes fix:, feat:, etc.). Des conventions uniformes rendent l’historique plus homogène et filtrable.
- **Taille du dépôt** : Surveillez la taille du dépôt au fil du temps (`git count-objects -v` pour voir la taille des objets, etc.). Si vous détectez qu’il grossit anormalement, cherchez quels gros fichiers ont été ajoutés. Parfois, un développeur peut accidentellement committer un énorme fichier (dump de DB, vidéo, etc.). Si c’est récent, envisagez de le purger de l’historique et d'ajouter à .gitignore.

En suivant ces bonnes pratiques, votre dépôt restera **propre, lisible et efficace**. Un dépôt bien structuré couplé à un historique bien tenu facilite énormément la maintenance et l’arrivée de nouveaux contributeurs, et réduit les erreurs. Voyons maintenant comment gérer les branches et les stratégies collaboratives pour organiser le travail d’équipe sur le dépôt.

## 5. Gestion des branches et stratégies collaboratives (GitFlow, GitHub Flow, Trunk Based Development)

Git offre une grande flexibilité dans la gestion des branches, permettant à chaque équipe d’adopter le workflow qui lui convient. Plusieurs modèles se sont popularisés au fil du temps : **Git Flow**, **GitHub Flow**, et le **développement "Trunk-Based"** (basé sur le tronc). Chacun a ses avantages, inconvénients et contexte d’utilisation. Présentons-les et voyons comment organiser les branches de votre dépôt efficacement.

### 5.1 Principes de base de la gestion de branches

- **Pourquoi utiliser des branches ?** Les branches permettent de travailler sur des fonctionnalités, corrections ou expériences isolément du reste du code. Ainsi, la branche principale (souvent `main` ou `master`) peut rester stable pendant que du travail en cours se fait sur d’autres branches. On fusionne la branche de travail une fois qu’elle est prête et validée.
- **Branches locales vs distantes** : Une branche locale peut exister sans être poussée vers le serveur, utile pour vos expérimentations temporaires. Une branche distante (sur origin) est visible par l’équipe. Distinguez bien *création* et *publication* de branches.
- **Convention de nommage** : Adoptez un schéma cohérent, par exemple :
  - `feature/xxx` pour les nouvelles fonctionnalités,
  - `bugfix/xxx` ou `hotfix/xxx` pour les corrections de bugs,
  - `release/xxx` pour préparer des versions (si flux Git Flow),
  - ou tout simplement nom descriptif s’il n’y a pas de catégorisation formelle. Le préfixe aide quand même à vite identifier le type. Évitez les noms trop génériques ("test", "dev") qui ne disent rien ou pourraient entrer en collision.
- **Durée de vie des branches** : Certaines branches sont longues (ex: `main`, `develop` persistent tout le projet). D’autres sont temporaires (feature branches, qui peuvent être supprimées après merge). Faites le ménage des branches obsolètes sur le dépôt distant pour ne pas le saturer de branches inactives.
- **Mises à jour** : Gardez vos branches de travail à jour par rapport à la base (en les fusionnant ou rebasant régulièrement) pour éviter un gros choc de merge à la fin.
- **Pull Requests** : Intégrez la notion de PR/MR dans votre stratégie. C’est l’endroit où la branche est discutée, testée et finalement approuvée pour intégration.

### 5.2 Git Flow

**Git Flow** est un workflow de branche popularisé par Vincent Driessen ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Gitflow%20est%20un%20mod%C3%A8le%20de,Elles)) en 2010. Il est bien adapté aux projets qui ont un cycle de release planifié et des versions multiples à maintenir (par ex un logiciel avec versions 1.x, 2.x). Le principe central est de séparer les activités de développement et de publication en plusieurs types de branches :

- **Branche `main` (ou `master`)** : contient l’historique *officiel* des versions en production ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Au%20lieu%20d%27une%20seule%20branche,un%20num%C3%A9ro%20de%20version)). Chaque commit sur main devrait correspondre à une version publiée. Souvent, on tague ces commits.
- **Branche `develop`** : branche d’intégration pour le développement en cours ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Au%20lieu%20d%27une%20seule%20branche,un%20num%C3%A9ro%20de%20version)). C’est là que se fusionnent les fonctionnalités une fois prêtes, et où l’on prépare la prochaine version. Elle reflète l’état "en développement" (futur prochain release).
- **Branches de *feature*** : branches dérivées de `develop` pour développer de nouvelles fonctionnalités ou des améliorations. Nommées typiquement `feature/mon-feature`. Chaque feature branch, une fois terminée/testée, est fusionnée **dans develop**. Ces branches ne vont jamais directement dans main.
- **Branches de *release*** : lorsqu’on estime que develop est prêt à être publié comme nouvelle version, on crée une branche `release/x.y.z` à partir de `develop`. Sur cette branche de release, on fige les fonctionnalités et on se concentre sur la stabilisation, la correction des derniers bugs, la documentation, etc. Une fois la release prête, on fusionne la branche release dans `main` (pour marquer la sortie) **et** dans `develop` si des commits de corrections s’y trouvent ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=%2A%20Feature,the%20bug%20is%20being%20fixed)) ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=parallel%20development%20among%20developers%2C%20usually,lived)) (afin que develop intègre ces corrections).
- **Branches de *hotfix*** : destinées aux correctifs rapides en production. Si un bug critique est découvert sur la version en production (donc sur `main`), on crée une branche `hotfix/x.y.z+1` à partir de main, on applique le correctif. Une fois validé, on fusionne la hotfix dans `main` (on tagge une version patch) **et** dans `develop` pour que la correction soit reportée sur la branche de dev ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=%2A%20Feature,the%20bug%20is%20being%20fixed)). Ceci évite que le bug corrigé ne réapparaisse dans la prochaine version.
- Par convention, `main` et `develop` sont des branches infinies (lifetime illimité), tandis que feature, release, hotfix sont éphémères (supprimées après fusion).
- GitFlow introduit ainsi un modèle assez structuré avec de nombreuses branches parallèles. Des outils (comme l’extension git-flow) existent pour faciliter la création/fin de ces branches.

**Avantages de GitFlow** :
- Permet un développement **parallèle** structuré (features en parallèle sur develop, corrections urgentes sur main).
- Le code sur la branche `main` est toujours prêt pour une sortie (versions déjà sorties).
- On peut maintenir plusieurs versions stables en parallèle (ex: on peut avoir une branche main pour v1.x, et peut-être une autre branche de support pour une ancienne version, gérée via hotfixes, etc.).
- La séparation claire develop/main aide à la stabilité (quelqu’un qui veut la version stable clone main, celui qui veut la dernière dev clone develop).
- Particulièrement adapté aux projets où les sorties sont moins fréquentes, mais nécessitent du soin (ex: applications déployées chez le client, release packaging).

**Inconvénients de GitFlow**  ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Gitflow%20est%20un%20workflow%20Git,Gitflow%20%C3%A0%20des%20fins%20historiques)):
- Complexité élevée : beaucoup de branches, beaucoup d’opérations de merges (une feature se merge sur develop, une release se merge sur main et develop, une hotfix se merge sur main et develop). Ça fait beaucoup de mouvements, propice aux erreurs (oublier de merger un hotfix dans develop par ex).
- Moins adapté à la livraison continue (CI/CD) : GitFlow suppose une phase de release distincte. Dans un monde d’intégration continue où on déploie très souvent, cette lourdeur n’est pas nécessaire. D’ailleurs GitFlow "a perdu en popularité au profit des workflows basés sur le tronc, considérés comme de bonnes pratiques pour le développement continu et DevOps" ([ Workflow Gitflow | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Gitflow%20est%20un%20workflow%20Git,Gitflow%20%C3%A0%20des%20fins%20historiques)).
- Peut introduire un délai : les fonctionnalités peuvent rester un certain temps sur develop avant d’être disponibles en prod (jusqu’à la prochaine release).
- Intégration de CI plus complexe : par ex, tester la branche develop en plus de main, gérer les builds de release séparément, etc.

**Quand utiliser GitFlow ?** Dans des projets versionnés "classiques", par ex un logiciel où on publie des versions majeures tous les X mois, avec potentiellement des patchs sur les versions précédentes. Des équipes qui préfèrent une approche méthodique avec phases de développement et de stabilisation distinctes apprécieront GitFlow. 

### 5.3 GitHub Flow

**GitHub Flow** est un workflow plus **simplifié** que GitFlow, popularisé par les pratiques de nombreux projets sur GitHub (bien que GitHub ne l’ait pas formalisé sous ce nom initialement, c’est un pattern courant). Il se base sur l’idée qu’on déploie fréquemment et qu’on n’a pas besoin de branches de release ou de develop à long terme. Les caractéristiques principales :
- Il n’y a généralement **qu’une branche principale** (souvent `main` ou `master`). Cette branche représente l’état actuellement déployé ou prêt à être déployé en production.
- Pour chaque nouvelle fonctionnalité ou bugfix, on crée une **branche dédiée** issue de main (souvent sur les dépôts GitHub, on travaille même sur un fork, mais conceptuellement c’est pareil : une branche).
- On effectue le travail sur cette branche. De façon continue, on peut pousser sur cette branche, et idealement, **déployer en environnement de test** pour cette branche (certaines équipes pratiquent des environnements de review app).
- Lorsqu’elle est prête, on ouvre une **Pull Request** vers `main`. Après revue et validation (tests passants, etc.), on **merge** cette PR dans `main`. Souvent, on supprime alors la branche de feature.
- Dès qu’un commit entre dans `main`, on le déploie en production (dans GitHub flow originel, on suppose un déploiement très fréquent, parfois automatique).
- Il n’y a pas de concept de `develop` séparé ou de `release` branch. Tout ce qui est validé va direct dans `main` et potentiellement en prod.
- S’il y a un bug urgent en prod, on peut faire une petite branche fix à partir de main, le corriger, PR, merge sur main, déploiement. (Ou on commit direct sur main si extrême urgence, mais en général on garde le process de branche + PR, juste rapidement).

En gros, GitHub Flow = **main + feature branches courts** ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Unlike%20GitFlow%2C%20this%20model%20doesn%E2%80%99t,feature%20branch%20is%20then%20deleted)) ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Github%20Flow%20focuses%20on%20Agile,production%20cycles%20and%20frequent%20releases)). 

**Avantages de GitHub Flow** ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Github%20Flow%20focuses%20on%20Agile,production%20cycles%20and%20frequent%20releases)):
- Simplicité : beaucoup moins de branches permanentes à suivre. On n’a que main et des branches éphémères. Le modèle mental est plus simple.
- **Déploiement rapide et fréquent** : Permet une culture DevOps où chaque merge sur main peut être déployé (souvent associé à l’intégration continue et déploiement continu). On ne retient pas les features trop longtemps.
- **Feedback rapide** : Puisqu’on sort de petites améliorations fréquemment, on détecte vite si quelque chose pose problème, et on peut corriger dans la foulée.
- Pas besoin de synchroniser develop et main. Tout se fait sur main, donc pas de risque d’oublier de reporter un fix.
- Travaille bien avec la plupart des plateformes d’hébergement (GitHub, GitLab Flow par défaut est similaire pour projets qui n’ont pas besoin de release branches complexes).
- Idéal pour les **applications web** ou services déployés en continu où avoir une "release" figée n’est pas obligatoire (on peut toujours déployer la dernière version).

**Inconvénients** :
- **Pas adapté si on doit maintenir plusieurs versions en parallèle** : par exemple, une lib dont on maintient la v1 et la v2 en même temps. GitHub Flow classique ne prévoit qu’une seule ligne (main). Si on doit faire un patch sur une ancienne version, on n’a pas de branche dédiée (on peut bien sûr créer une branche comme `v1.x` et l’utiliser, on hybride alors avec du GitFlow minimal).
- **Risque de main instable** : Comme on merge très souvent, si le processus de test/review n’est pas rigoureux, on peut introduire plus facilement des bugs en prod. GitFlow consacre une branche dev pour tester ensemble toutes les features avant release, ici c’est plus direct. Donc GitHub flow fonctionne bien si on a une bonne culture de tests automatisés et de code review. Le code qui arrive sur main doit quand même être stable, sinon c’est la prod qui trinque.
- **Manque de formalité de version** : Souvent, on tag quand même des releases (surtout si c’est un projet qui publie des versions numérotées, ex: une bibliothèque). GitHub Flow n’empêche pas de tagger. C’est juste qu’on n’a pas de "phase de release" où on stoppe les features, on tag la release et on continue. On peut tagger n’importe quel commit sur main comme nouvelle version quand on décide de publier.
- **Pas de visibilité de long-terme par une branche** : Dans GitFlow, la branche develop peut toujours refléter l’état en cours de dev de la prochaine version, c’est clair. En GitHub flow, main contient un mélange de features déjà finies (donc en production) ET potentiellement partiellement la prochaine version (puisqu’on y ajoute en continu). Mais en pratique, main reflète la prod courante, donc ce n’est pas un inconvénient dans un contexte de déploiement continu.

**Quand utiliser GitHub Flow ?** 
- Sur des projets web/agiles où la priorité est d’intégrer et déployer rapidement de petites améliorations. 
- Sur les projets open source sur GitHub, beaucoup opèrent plus ou moins de cette manière : main = stable, PRs pour toute nouvelle contribution. 
- Sur un produit en démarrage, où on ne veut pas la complexité de GitFlow, GitHub flow permet d’aller vite.
- Petites équipes ou même dev solo, c’est plus léger à gérer.

Une anecdote : GitHub flow a été souvent résumé en 5 étapes :
1. Créer une branche depuis main,
2. Commiter vos changements sur cette branche,
3. Ouvrir une Pull Request,
4. Merge la PR une fois validée,
5. Supprimer la branche.

Et toujours déployer après merge (ou même tester déployer la branche avant merge si possible).

### 5.4 Trunk-Based Development

Le **développement “basé sur le tronc”** (Trunk-Based Development, TBD) est une approche encore plus minimaliste et orientée CI/CD. Ici, il n’y a pour ainsi dire **aucune branche durable** en dehors du *tronc principal* (d’où le nom). Le "tronc" correspond à la branche principale (par ex. `main`). L’idée maîtresse est que les développeurs intègrent leurs changements dans ce tronc **au moins une fois par jour** ([ Développement basé sur le tronc | Atlassian ](https://www.atlassian.com/fr/continuous-delivery/continuous-integration/trunk-based-development#:~:text=Le%20d%C3%A9veloppement%20bas%C3%A9%20sur%20le,ainsi%20que%20les%20performances%20organisationnelles)) ([ Développement basé sur le tronc | Atlassian ](https://www.atlassian.com/fr/continuous-delivery/continuous-integration/trunk-based-development#:~:text=livraison,d%C3%A9veloppement%20bas%C3%A9%20sur%20le%20tronc)), évitant ainsi les branches divergentes. En d’autres termes :
- Tout le monde travaille sur une seule branche (trunk). On commit directement dans main ou via de très courtes branches locales (qui durent quelques heures maximum).
- S’il y a des branches, ce sont des "branchettes" de très courte durée (intégrées le jour même ou en quelques jours max, <=1 sprint).
- Le code sur trunk doit rester déployable à tout moment. On utilise pour cela des techniques comme les **feature flags** (drapeaux de fonctionnalités) pour intégrer du code pas encore actif ou terminé sans affecter l’application en prod. Les parties non finies sont gardées inactives jusqu’à ce qu’elles soient prêtes, mais elles résident déjà dans le tronc ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Since%20there%20is%20no%20development,for%20quick%20and%20continuous%20deployment)).
- Il n’y a pas de branch de release longue ou de develop. Parfois, on crée une branche de release juste le temps de stabiliser quelques jours, mais idéalement on évite, tout est géré sur trunk.
- Les versions sont découpées par tags sur le trunk ou par des mécanismes de déploiement continu (chaque commit peut potentiellement être une release).
- Les merge conflicts sont rarissimes car tout le monde intègre très souvent (donc on intègre avant d'avoir divergé significativement).
- Ce modèle encourage **les petits commits fréquents** et une discipline de tests et monitoring pour vite détecter tout problème introduit.

**Avantages** ([Git Branching Strategies: GitFlow, Github Flow, Trunk Based...](https://www.abtasty.com/blog/git-branching-strategies/#:~:text=Github%20Flow%20focuses%20on%20Agile,production%20cycles%20and%20frequent%20releases)) ([ Développement basé sur le tronc | Atlassian ](https://www.atlassian.com/fr/continuous-delivery/continuous-integration/trunk-based-development#:~:text=Le%20d%C3%A9veloppement%20bas%C3%A9%20sur%20le,ainsi%20que%20les%20performances%20organisationnelles)):
- C’est souvent considéré comme la méthode la plus alignée avec l’**intégration continue véritable** (vous ne pouvez pas vraiment faire de l’intégration continue si chacun travaille isolé pendant des semaines).
- **Déploiement en continu** plus facile : avec trunk qui est toujours proche de l’état courant du développement, on peut même automatiser le déploiement de chaque commit (après tests auto).
- **Moins de surcharge de gestion de branches** : pas de merges de release/hotfix compliqués, pas de maintenance de deux branches en parallèle, etc. Tout converge vite sur trunk.
- Permet aux grosses équipes de collaborer sans casser le code du voisin en isolant via feature flags au lieu d’isoler via branches. 
- Suivi simplifié : on regarde trunk pour savoir tout ce qui se passe.
- Favorise une culture qualité : comme trunk doit rester stable, les devs adoptent plus naturellement TDD, tests robustes, feature flags, monitoring, etc. (C’est plus organisationnel, mais c’est ce qu’on observe où trunk-based est pratiqué).

**Inconvénients** :
- **Nécessite une grande discipline** : Ce n’est pas trivial pour n'importe quelle équipe de committer direct sur main sans introduire de régressions. Il faut des tests automatisés excellents, des revues de code rapides ou pair programming, etc. 
- **Feature Flags complexity** : Gérer la désactivation sélective de code peut devenir complexe si de nombreuses features inachevées cohabitent. Il faut un système de toggles propre et éviter la dette technique (retirer les flags une fois la feature stable).
- **Pas idéal pour contributions externes** : Sur un gros projet open source, on ne peut pas avoir tout le monde commitant sur trunk. On utilise forcément des branches de PR (ce qui redevient un GitHub flow en pratique, bien que le mainteneur intègre rapidement).
- **Support multi-version** : Comme GitHub Flow, trunk-based assume un flux de livraison unique. Si vous devez maintenir une version N-1, on peut créer une branche de maintenance pour N-1, mais c'est du cas particulier (et c'est comme un hotfix branch).
- **Organisation** : Parfois, la politique d’entreprise ou le type de produit ne permettent pas trunk-based (ex: logiciels embarqués qui sortent en versions planifiées, les clients ne mettent pas à jour en continu, etc.). 
- **Intégration de code potentiellement pas fini** : Bien que masqué par flag, du code incomplet est quand même intégré. Il faut s'assurer qu'il n'a aucun effet de bord. Ce style convient bien aux devs qui savent développer par petites itérations non cassantes.

Trunk-based est très prisé dans le milieu DevOps/continuous delivery. Des entreprises tech comme Google, Facebook l’adoptent (Google a un trunk monorepo géant, FB déploie master en prod 2 fois par jour, etc.). Atlassian indique que trunk-based "simplifie les phases de merge et d'intégration, rendant possible la CI/CD, et améliorant la livraison logicielle ainsi que la performance organisationnelle" ([ Développement basé sur le tronc | Atlassian ](https://www.atlassian.com/fr/continuous-delivery/continuous-integration/trunk-based-development#:~:text=Le%20d%C3%A9veloppement%20bas%C3%A9%20sur%20le,ainsi%20que%20les%20performances%20organisationnelles)). 

**GitHub Flow vs Trunk-Based** : Ils sont très proches philosophiquement (les deux prônent du merge rapide et pas de branches longues). La nuance est dans l’échelle et l’automatisation :
- GitHub Flow supppose toujours de passer par PR (même courtes). Trunk-based dans son extrême pourrait dire commit direct sur main (ou des micro branches de quelques heures).
- GitHub Flow est souvent lié à l'idée de déploiement fréquent, trunk-based va jusqu'à déploiement continu automatique.
- En pratique, beaucoup combinent : par exemple sur GitLab ou GitHub, on bosse sur de très courtes branches de MR, qu’on intègre rapidement (plusieurs fois par jour potentiellement). C’est trunk-based dans l’esprit, avec l'outil de code review en plus. 

**GitLab Flow** : À noter, GitLab a communiqué leur propre version de flux, qui est modulable selon besoins. GitLab Flow combine le meilleur de GitHub Flow avec la possibilité d’avoir des branches de production/staging si besoin. Ils encouragent à aligner branches sur environnements (par ex une branche `production` reflète ce qui est en prod, `preprod` pour staging, etc., et on fusionne dans production pour déployer). C’est une variante non extrémiste.

### 5.5 Choisir une stratégie et bonnes pratiques de branchement

Il n’y a pas de solution unique pour tous, le choix dépend de la taille de l’équipe, du type de livrable, de la fréquence de déploiement, etc. Quelques conseils :

- **Mettez-vous d’accord en équipe** : Quel que soit le modèle, documentez-le pour que chacun sache comment nommer sa branche, vers où merger, etc. Avoir un schéma dans le README ou le wiki du repo peut aider.
- **Préférez la simplicité si possible** : Ne complexifiez pas inutilement. Si vous êtes une petite équipe qui déploie souvent, GitHub Flow ou un trunk-based allégé suffisent, pas besoin de GitFlow complet.
- **Adaptez si nécessaire** : Vous pouvez très bien utiliser un *mix*. Par exemple, beaucoup d'équipes utilisent une variante : une branche `develop` pour intégrer tout ce qui est validé via PR, et cette branche est déployée en continu sur un environnement staging, puis périodiquement on tagge et on déploie sur prod (ce n’est pas tout à fait GitFlow car pas de branches release/hotfix, mais pas non plus trunk pur). Ou un trunk-based mais où les features plus importantes se font sur branch pour permettre la review.
- **Communication** : Si vous partez sur GitFlow, assurez que tout le monde comprend les merges à faire (surtout bien reporter hotfix -> develop). Si trunk-based, formez sur l’utilisation de feature flags et l’importance de tests.
- **Outils** : Utilisez les outils comme les pull requests, la CI, et éventuellement les scripts (par ex l'outil git-flow en ligne de commande peut initier le repo avec branches main/develop et fournir commandes `git flow feature start X`, etc.). Sur Bitbucket ou GitLab on peut configurer des règles de merge selon la branche (ex: exiger un MR pour main).
- **Release branches** : Même si en temps normal vous faites trunk-based, il peut être utile lors d’une sortie majeure de créer une branch temporaire pour gel de code et tests (une sorte de release branch). Ce n'est pas interdit de combiner.
- **Documenter le cycle** : Par exemple, "on travaille sur des branches de feature basées sur develop, on ouvre une PR, on squash merge dans develop. La branche develop est déployée sur staging. Tous les 2 sprints, on crée une branche release/X, tests, puis on merge sur main pour release prod." Ce serait une doc type d’un hybride.
- **Branches de longue durée** : Evitez les branches de feature qui durent des mois sans merge. Si c’est une très grosse feature qui n’est pas active tant qu’entièrement finie, envisagez de la découper ou de l’intégrer derrière un flag progressivement. Plus une branche vieillit, plus le merge final sera dur et risqué.
- **Stratégie de merge** : Décidez si vous faites des merges *avec* commit de merge ou *fast-forward/squash*. Dans un flux style GitHub Flow ou trunk, on a souvent le choix. Squasher nettoie l’historique, Merge commit préserve. C’est un choix d’équipe. Squash merge a l’avantage de ne pas polluer main avec "Merge PR#42 ..." multiples.
- **Éviter les merges complexes** : Dans GitFlow, la multiplicité des merges peut générer plus de conflits. Assurez-vous de tester intégralement la branche develop avant de lancer une release, etc.
- **Garder main protégée** : Sur GitHub/GitLab, activez la protection de la branche main (pas de push direct, seulement via PR et avec approbation/test). Idem pour develop si vous en avez une.
- **Bon usage des hotfix** : Si vous avez un bug en production, ne commencez pas à commiter pleins de choses sur develop pendant la correction sur main, ou vice-versa. Concentrez-vous à corriger sur main/hotfix branch puis mergez correctement, sinon develop peut diverger. 

En synthèse, GitFlow offre un contrôle strict utile pour les releases planifiées, GitHub Flow offre vitesse et simplicité pour déploiement continu, et Trunk-Based va encore plus loin dans l’agilité en imposant l’intégration constante. L’important est de **trouver le bon équilibre** entre stabilité et rapidité pour votre projet et de bien communiquer sur le processus de branchement choisi.

## 6. Collaboration efficace : bonnes pratiques pour travailler en équipe avec Git

Travailler en équipe sur un projet Git requiert plus que connaître les commandes. Il faut adopter des pratiques collaboratives pour éviter les conflits, tirer parti de l’outil et maintenir un rythme productif. Voici un ensemble de recommandations pour une collaboration réussie :

### 6.1 Utiliser les plateformes et outils de collaboration

- **Hébergeur centralisé** : Hébergez votre dépôt sur une plateforme comme **GitHub**, **GitLab** ou **Bitbucket** (ou serveur Git interne type GitLab CE) plutôt que d’échanger des patches par email ou se pousser localement. Cela offre une source de vérité accessible à tous.
- **Issues & Tickets** : Servez-vous du gestionnaire d’issues de la plateforme pour suivre les bugs et demandes de fonctionnalités. Liez les commits/PR aux issues correspondantes (ex: mentionner "Fix #123" fermera l’issue #123 sur GitHub). Ainsi, tout le monde voit le contexte de chaque changement.
- **Pull Requests / Merge Requests** : Instituez la revue de code via PR/MR pour tout changement significatif. Cela encourage la discussion, la détection d’erreurs et le partage de connaissances. Même dans une petite équipe, une relecture croisée est bénéfique.
- **Code Review** : Lors des revues, soyez constructifs. Regardez non seulement le code, mais aussi la pertinence du changement, l’impact sur d’autres modules, etc. Utilisez les commentaires en ligne pour pointer des lignes précises.
- **Intégration Continue** : Mettez en place un pipeline (Travis CI, Jenkins, GitHub Actions, GitLab CI…) qui se déclenche à chaque push ou PR. Ce pipeline doit exécuter les tests, l’analyse statique, la compilation… Ainsi, l’équipe est rapidement informée si un commit en casse quelque chose. N’hésitez pas à exiger une CI verte avant de fusionner une PR.
- **Wiki/Documentation** : Si la plateforme fournit un wiki ou un dossier docs/, utilisez-le pour noter les procédures, conventions de code, etc., que tout le monde pourra consulter. Par exemple, comment configurer le repo la première fois, ou le style de commit à respecter.
- **Discussions d’équipe** : Utilisez des canaux de communication (Slack, Teams, mailing list) couplés à Git. Par ex, brancher la notification des PR ou commits sur un canal. Ainsi, tout le monde voit en temps réel ce qui bouge dans le repo et peut réagir. Favorisez une culture où on n'hésite pas à poser des questions sur le code de quelqu’un d’autre.

### 6.2 Convention de code et de commits

- **Style de code cohérent** : Adoptez des conventions de code (indentation, nommages, structure) uniformes pour éviter des diffs sans signification (formatage différent) et pour que le code soit lisible par tous comme s’il était écrit par une seule personne. Vous pouvez utiliser des linters ou formatters automatiques (Prettier, ESLint, Black, gofmt selon les langages) pour normaliser.
- **Conventional Commits (optionnel)** : Certaines équipes utilisent le format *Conventional Commits*, e.g., messages du type "feat: add login functionality", "fix(profile): handle null pointer". Ceci permet de générer des changelogs automatisés et de catégoriser les changements. Ce n’est pas obligatoire, mais avoir un standard de message (même simplement "Type: courte description") aide.
- **Commentaires de code** : Encouragez la documentation du code via commentaires ou README techniques, pour que les autres comprennent plus vite. Aussi, mettre à jour la documentation quand on modifie une fonctionnalité (collaboration ce n’est pas que Git).
- **Responsabilité partagée** : Évitez l’attitude "ce fichier est à moi seul". Tout membre devrait se sentir autorisé à corriger un bug ou améliorer un code, même écrit par un autre, tant que c’est coordonné (via PR et discussion). Git trace l’historique, donc personne ne "perd" la paternité de son code initial, mais le logiciel appartient à l’équipe, pas à un individu.

### 6.3 Synchronisation et mise à jour du code

- **Pull régulièrement** : Chaque développeur doit penser à récupérer les dernières modifications de la branche sur laquelle il travaille, afin de rester à jour. Par exemple, faites un `git pull origin develop` chaque matin si vous travaillez sur develop, ou avant de pousser un commit, pull d’abord pour intégrer d’éventuels commits d’autrui. Cela réduit l’ampleur des merges.
- **Éviter les "long-lived merges"** : Ne gardez pas localement des commits longtemps sans les pousser. Si vous avez du code fonctionnel, push-le sur votre branche (même si c’est un brouillon, sur une PR en WIP). Cela permet aux autres de voir votre progression (et d’éviter de faire le même travail par inadvertance).
- **Feature branches partagées** : Parfois, plusieurs devs collaborent sur une même feature branch. Dans ce cas, poussez vos commits fréquemment sur la branche distante pour que vos collègues puissent les récupérer. Communiquez bien sur qui fait quoi pour éviter d’éditer les mêmes morceaux simultanément (sinon conflit).
- **Testing local avant merge** : Avant de merger votre branche, synchronisez-la avec la cible (via rebase ou merge de la cible) afin de tester l’intégration. Cela garantit que quand on clique "merge", on sait déjà que ça marche avec la dernière version. De plus, ça évite de faire subir aux intégrateurs la résolution de conflit – faites-le dans la branche de feature si possible.
- **Prévenir les conflits** : Si vous anticipez que vos changements risquent de chevaucher ceux de quelqu’un (ex: deux devs modifient la même fonction dans deux branches), discutez-en tôt. Peut-être coordonnez l’ordre de merge (on merge A d’abord puis B adaptera le code). Ou scindez le travail pour réduire l’intersection de modifications.
- **Revue des merges** : Quand une PR est fusionnée, supprimez la branche de feature pour ne pas la laisser traîner (sauf si il y a une raison de la garder). Checkez juste après merge sur main/develop que tout est ok (CI ou tests manuels). En cas de problème en production, ne jouez pas au blâme individuel, traitez-le ensemble (post-mortem constructif).

### 6.4 Gestion des conflits interpersonnels avec Git

- **Répartition du travail** : Git facilite l’édition concurrente, mais essayez de répartir les tâches pour minimiser que deux personnes travaillent sur exactement la même portion du code en parallèle. Utilisez un tableau Kanban ou un sprint backlog pour savoir qui code quoi.
- **Fusionner fréquemment** : Rejoignant trunk-based dev, plus on intègre souvent, moins les conflits sont probables. Si deux branches modifient le même fichier différemment, un conflit sur 5 lignes est plus facile à résoudre qu’un conflit sur 500 lignes modifiées divergeant depuis un mois.
- **Conflits de merge = opportunité** : Plutôt que voir un conflit comme une contrainte, voyez le comme une chance de revoir le code ensemble. Impliquez les deux auteurs dans la résolution si possible – cela assure que personne ne perde son changement ou ne brise la logique de l’autre. On apprend souvent en résolvant un conflit, car on regarde ce qu’a fait l’autre.
- **Eviter "commit de victoire"** : Ne résolvez pas un conflit en supprimant arbitrairement le travail de l’autre. Prenez le temps de comprendre les deux côtés. Parfois, la solution est de combiner les deux modifications.
- **Garder une atmosphère ouverte** : Git trace tout, donc inutile de cacher ses erreurs ou d’avoir peur de faire mal. Favorisez une équipe où chacun peut push du code pas parfait, le dire ("Work in progress, please review particularly the X part"). Mieux vaut pousser et améliorer que bloquer un commit jusqu’à la perfection (qui peut retarder l’intégration et frustrer les autres si ça les bloque).
- **Pair programming sur rebase/merge** : Si un rebase est délicat, faites-le à deux, la personne qui a la branche et quelqu’un qui a beaucoup touché la cible. À deux on comprend mieux l’intention initiale de chaque partie et on choisit la bonne version de chaque chunk.

### 6.5 Partage des connaissances

- **Blame/Annotate** : Utilisez `git blame` ou l’interface web pour voir qui a écrit telle ligne en dernier, et n’hésitez pas à poser la question directement à l’auteur en cas de doute. Le but n’est pas de "blâmer" malgré le nom, mais de trouver la bonne personne qui peut expliquer un choix de code.
- **Rotation** : Si possible, changez parfois les zones de code sur lesquelles vous travaillez. Ainsi tout le monde se familiarise avec différentes parties du codebase (bus factor réduit). Git aide car l’historique permet de comprendre comment la partie a évolué.
- **Retours sur le processus** : Régulièrement, en rétro d’équipe par exemple, discutez du workflow Git : est-il adapté, y a-t-il trop de frictions (ex: PR trop longues, merges tardifs)? Ajustez en conséquence (par ex, décider de rendre les PR plus petites).
- **Mentorat** : Les membres plus expérimentés peuvent guider les novices sur la résolution de conflits, l’écriture de bons commits etc. On peut faire du pair programming sur un commit pour transmettre les bonnes pratiques.

En suivant ces conseils, l’utilisation de Git devient fluide et naturelle dans l’équipe, et on minimise les frustrations. L’important est que tout le monde comprenne que Git est là pour **aider la collaboration, pas la compliquer**. Quand l’outil est bien maîtrisé et les règles claires, l’équipe peut se concentrer sur le produit à développer.

## 7. Gestion des conflits et résolution des erreurs fréquentes avec Git

Malgré toutes les précautions, des conflits et erreurs Git surviennent inévitablement. L’important est de savoir comment les aborder sereinement et les résoudre méthodiquement. Nous allons couvrir ici :
- La gestion des **conflits de merge/rebase** (quand deux modifications entrent en collision).
- La résolution des erreurs et situations Git courantes, avec les solutions.

### 7.1 Gestion des conflits de fusion (merge conflicts)

**Quand se produisent les conflits ?** Lors d’un `git merge`, `git pull` (qui fait un merge), ou `git rebase/cherry-pick`, Git tente de combiner les changements. Si les mêmes lignes du même fichier ont été modifiées différemment dans les deux versions, Git ne peut décider automatiquement quelle version garder. Il marque alors un **conflit**.

**Reconnaître un conflit** : Git vous l’indique dans la sortie de la commande, par ex:
```
Auto-merging src/app.js
CONFLICT (content): Merge conflict in src/app.js
Automatic merge failed; fix conflicts and then commit the result.
```
Si vous faites `git status`, vous verrez les fichiers en conflit listés sous « Unmerged paths ». Si vous ouvrez l’un de ces fichiers dans un éditeur, vous verrez des marqueurs de conflit comme:
```diff
<<<<<<< HEAD
console.log("Hello world");
=======
printf("Hello world\n");
>>>>>>> feature/alternate-lang
```
Tout ce qui est entre `<<<<<<< HEAD` et `=======` correspond à votre version (HEAD, la branche courante), et ce qui est entre `=======` et `>>>>>>> feature/alternate-lang` est la version de l’autre branche (ici nommée feature/alternate-lang) ([ workflow git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows#:~:text=git%C2%A0push%C2%A0origin%C2%A0main)).

**Étapes pour résoudre un conflit** :
1. **Identifier tous les fichiers en conflit** : `git status` donne la liste. Ouvrez chacun dans un éditeur ou un outil de diff.
2. **Analyser le changement** : Comprenez la différence entre les deux versions. Parfois c’est trivial (même texte modifié différemment). Parfois plus conceptuel (deux personnes ont ajouté une fonction différente au même endroit).
3. **Décider quelle version garder** : 
   - Si l’un des changements est clairement correct ou plus complet, prenez-le.
   - Parfois, il faut conserver les deux modifications en les combinant manuellement.
   - Ou écrire une troisième version qui englobe les deux.
   - Par exemple, dans du code, peut-être que les deux modifications sont compatibles : on peut appeler les deux fonctions l’une après l’autre.
   - Dans un document texte, on peut vouloir garder des phrases des deux côtés.
4. **Éditer le fichier** : Retirez les marqueurs `<<<<<<<`, `=======`, `>>>>>>>` et modifiez le contenu pour arriver au résultat désiré. Après édition, le fichier ne doit plus contenir ces marqueurs.
5. **Tester** : Si c’est du code, recompilez/lancez les tests avec la version fusionnée. Assurez-vous d’avoir produit un résultat cohérent (pas juste compilable, mais fonctionnel).
6. **Marquer comme résolu** : Une fois satisfait, ajoutez le fichier résolu à l’index : `git add <fichier>`. Faites cela pour chaque fichier en conflit après résolution.
7. **Finaliser la fusion** : 
   - Si c’était un merge manuel (via `git merge` ou un pull), une fois tous les conflits résolus et ajoutés, il faut committer la fusion : `git commit`. Git a peut-être pré-rempli un message "Merge branch X into Y" que vous pouvez éditer ou laisser.
   - Si c’était un rebase ou cherry-pick, exécutez `git rebase --continue` ou `git cherry-pick --continue` une fois les conflits du commit courant résolus. Le processus reprendra pour le commit suivant, ou terminera si fini.
8. **Poursuivre** : Si d’autres conflits sur d’autres fichiers existent, répétez. S’il reste des conflits non résolus, Git ne vous laissera pas commit (il dira "you have unmerged paths"). Utilisez `git status` pour vérifier qu’il n’y a plus "*Unmerged*" restants.

**Outils pour aider** :
- Vous pouvez utiliser un **merge tool** graphique ou dans l’IDE. Par ex: `git mergetool` peut ouvrir chaque conflit dans un outil comme KDiff3, BeyondCompare, VS Code, etc., où l’on voit les deux versions et on peut cliquer pour choisir.
- Certains IDE (Visual Studio, IntelliJ, VS Code) détectent les conflits et offrent une interface avec des boutons "Accepter changement entrant / sortant / les deux".
- Ces outils minimisent les erreurs de suppression accidentelle de marqueurs.

**Conseils de résolution** :
- **Résoudre en faveur de HEAD ou de l’autre** : Si vous savez que l’une des versions doit complètement écraser l’autre, vous pouvez accélérer:
  - Pour garder la version HEAD entièrement : exécutez `git checkout --ours <fichier>` suivi de `git add <fichier>`. (--ours prend la version HEAD dans le contexte d’un merge en conflit).
  - Pour garder la version de l’autre branche : `git checkout --theirs <fichier>`.
  - Attention, `--ours/--theirs` ne fonctionnent que lors d’un conflit, et chacun prend l’un des côtés. 
- **Diviser le travail** : S’il y a beaucoup de conflits, partagez la résolution entre les devs concernés, surtout s’ils connaissent leurs modifications. Par exemple "toi occupe-toi du fichier X, moi du Y, puis on se relit".
- **Rechercher `<<<<`** : après résolution, faites une recherche dans le projet de `<<<<` pour être sûr qu’il ne reste pas de marqueurs non traités.
- **Comprendre la cause** : Un conflit signale souvent que deux devs ont travaillé sur la même zone. C’est un signe qu’il faut peut-être discuter de la conception ou modulariser pour éviter ça. N’hésitez pas à organiser un petit débrief après un gros conflit pour en tirer les leçons (ex: "peut-être qu’on aurait dû factoriser cette logique plus tôt, pour éviter d’écrire du code concurrent sur ce même fichier").
- **Conflits et historique** : Un conflit réglé manuellement résulte souvent dans un commit de merge. Ce commit de merge aura deux parents et contient la résolution. Ce qui compte, c’est que la branche résultante compile et passe les tests. L’histoire retient qu’il y a eu un conflit, c’est normal.

**Choix merge vs rebase :** ces deux méthodes intègrent les changements d’une branche dans une autre, mais différemment. Un merge crée un commit de fusion, conservant l’historique divergent tel quel. Un rebase, lui, réapplique vos commits sur la nouvelle base comme s’ils avaient été committés plus tard (réécriture d’historique). Pour une intégration en équipe, **préférez le merge pour les branches partagées** (afin de ne pas réécrire l’historique public), et gardez le rebase pour vos travaux locaux ou pour cleaner l’historique de votre branche *avant* de la partager. **Ne rebase *jamais* des commits déjà poussés sur un dépôt public** car cela crée des incohérences pour vos collègues ([ Git rebase | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/rewriting-history/git-rebase#:~:text=Ne%20pas%20rebaser%20l%27historique%20public)). En clair, si votre branche feature est publique et que d’autres l’ont récupérée, évitez de la rebaser (privilégiez un merge de `main` dans votre feature pour la mettre à jour). En revanche, rebaser vos commits locaux avant de pousser la PR (ou squash vos commits) peut donner un historique final plus linéaire. Adaptez-vous à la politique du projet : certaines équipes interdisent tout rebase sur des branches partagées, d’autres l’autorisent avec précaution (ex: en s’assurant d’utiliser `--force-with-lease` lors du push pour ne pas écraser des commits qui ne seraient pas à vous).



### 7.2 Erreurs fréquentes de Git et comment les corriger

Voici une liste de problèmes courants que vous pouvez rencontrer en utilisant Git, et les solutions ou explications associées :

**1. "fatal: Not a git repository (or any of the parent directories)"**  
*Contexte:* Vous lancez une commande Git dans un dossier qui n’est pas sous Git.  
*Solution:* Assurez-vous d’être dans le répertoire du projet où se trouve le dossier `.git`. Si vous avez oublié d’initialiser le dépôt, exécutez `git init`. Si vous êtes dans un sous-dossier, il remonte les parents pour chercher `.git`; peut-être êtes-vous sur un disque différent ou racine. Naviguez dans le bon dossier.

**2. "fatal: ambiguous argument 'HEAD': unknown revision or path not in the working tree."**  
*Contexte:* Vous exécutez une commande nécessitant HEAD (par ex, `git log`), mais Git ne trouve pas HEAD. Souvent c’est parce qu’il n’y a aucun commit dans le repo.  
*Solution:* Si c’est un nouveau repo vide, faites un commit initial (`git add` puis `git commit`) ou un `git fetch` (si cloné sans HEAD). Si c’est un clone tout juste fait d’un repo vide, il n’y a juste pas de HEAD défini. Commitez ou attendez qu’un commit existe.

**3. "error: failed to push some refs to <repo>... Updates were rejected because the tip of your current branch is behind its remote counterpart."**  
*Contexte:* Vous essayez de `git push`, mais le push est rejeté car votre branche locale n’est pas à jour par rapport au distant ([ workflow git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows#:~:text=Mais%20comme%20son%20historique%20local,un%20message%20d%27erreur%20d%C3%A9taill%C3%A9)). Quelqu’un a poussé des commits que vous n’avez pas.  
*Solution:* Faites un `git pull` pour intégrer les changements distants (ce qui peut provoquer un merge commit si vous avez aussi des commits locaux). Après cela, essayez `git push` à nouveau. Alternativement, vous pouvez rebase vos commits locaux sur la nouvelle base distante (`git pull --rebase`), ce qui évite un merge commit, puis push. L’important est de synchroniser. **Ne forcez pas le push** (avec `-f`) sauf si vous savez ce que vous faites et que c’est volontaire de réécrire l’historique distant (rarement approprié sur une branche partagée).

**4. "fatal: The current branch <name> has no upstream branch."**  
*Contexte:* Vous avez créé une nouvelle branche et essayé de `git pull` ou `git push` sans configurer de suivi distant.  
*Solution:* La première fois, poussez avec `git push -u origin <name>` pour définir l’upstream (la branche distante correspondante sur origin) ([ workflow git | Atlassian Git Tutorial ](https://www.atlassian.com/fr/git/tutorials/comparing-workflows#:~:text=git%C2%A0push%C2%A0origin%C2%A0main)). Ensuite, `git pull` et `git push` fonctionneront sans préciser la branche. L’erreur vous indique même la commande à utiliser en général. Pour une pull branch sans upstream, soit spécifier `git pull origin <name>`, soit mieux, d’abord push avec -u.

**5. "Merge conflict in <fichier> - fix conflicts and run 'git commit'"**  
*Contexte:* On a abordé ça plus haut. Vous avez un conflit.  
*Solution:* Suivez le processus de résolution des conflits détaillé en 7.1. Éditer, `git add`, `git commit` (ou rebase --continue). Tant que vous voyez ce message, c’est qu’il y a du travail de résolution non fini.  

**6. "error: you need to resolve your current index first"**  
*Contexte:* Vous avez essayé de faire un `git merge`, `git pull` ou de changer de branche alors qu’il y a des conflits en cours ou un merge non terminé.  
*Solution:* Terminez d’abord la résolution du merge en cours (voir précédent). Git empêche de changer de HEAD tant que des conflits ne sont pas réglés. Si pour une raison ou une autre vous voulez annuler le merge en cours : `git merge --abort` ramènera l’état pré-merge. (Pour rebase, `git rebase --abort`). Ensuite, vous pouvez retenter après avoir peut-être stashé ou autre.

**7. "fatal: refusing to merge unrelated histories"**  
*Contexte:* Vous tentez de `git merge` deux branches sans ancêtre commun. Cela arrive souvent quand vous essayez de merge deux dépôts initialisés séparément (ex: vous avez un repo local init, et vous essayez de pull un repo distant qui n’a aucun rapport).  
*Solution:* Si vraiment vous voulez combiner deux historiques disjoints, utilisez `git merge --allow-unrelated-histories -m "Merge unrelated histories"` lors du merge. Mais réfléchissez si c’est ça que vous voulez (souvent c’est un cas d’erreur, p.ex cloner un repo puis faire git init aussi localement). Pour une situation typique : vous créez un repo sur GitHub avec un README, vous avez un repo local sans ce commit, le premier pull cause ce message. La solution est juste `git pull origin main --allow-unrelated-histories` la première fois, ou mieux, cloner dès le départ.

**8. "warning: LF will be replaced by CRLF" / "CRLF will be replaced by LF"**  
*Contexte:* Sur Windows vs UNIX, Git vous informe qu’il va convertir les fins de ligne selon la configuration `core.autocrlf`. Ce n’est pas une erreur, juste un warning.  
*Solution:* Généralement, on laisse Git normaliser (autocrlf=true sur Windows). Si ce message spam trop, vous pouvez désactiver le warning en fixant `.gitattributes` ou la config. L’important est d’être cohérent. Pas vraiment un "conflit" mais bon, les novices sont parfois confus par ce message. Il n’empêche pas le commit/push, c’est juste informatif.

**9. "fatal: remote origin already exists."**  
*Contexte:* Vous faites `git remote add origin ...` mais un remote origin est déjà défini.  
*Solution:* Soit vous vouliez changer l’URL du remote origin : dans ce cas, utilisez `git remote set-url origin <nouvelle_url>`. Soit vous avez peut-être cloné donc l’origin était auto, pas besoin de re-add. Si vous vouliez ajouter un autre remote (par ex upstream), donnez-lui un autre nom.

**10. "error: Your local changes to the following files would be overwritten by checkout/merge:"**  
*Contexte:* Vous essayez de `git checkout` une autre branche, ou de `git pull`, mais vous avez des modifications locales non commitées qui entrent en conflit avec les différences arrivant. Git refuse de procéder pour ne pas écraser vos changements.  
*Solution:* Vous avez quelques choix:
   - Si vous ne voulez pas garder vos changements locaux : vous pouvez les abandonner via `git reset --hard` (ATTENTION ça supprime toutes vos modifs non commitées irrémédiablement) ou plus ciblé `git restore <fichier>` pour chaque fichier listé.
   - Si vous voulez les garder pour plus tard : faites un `git stash` pour les mettre de côté, puis relancez la commande (checkout ou pull). Après ça, appliquez le stash (`git stash pop`). Il peut y avoir un conflit lors de l'application du stash si le fichier a aussi changé dans la branche vers laquelle vous êtes allé, qu’il faudra résoudre.
   - Si vous voulez les committer avant de changer de branche : committez sur place (ou stash, checkout, cherry-pick plus tard). 
   - En somme, Git dit "je ne bouge pas tant que tu n'as pas traité ces changements". Decidez de leur sort avant de retenter.

**11. "fatal: branch '<branch>' is not fully merged. If you are sure, run 'git branch -D <branch>'."**  
*Contexte:* Vous tentez de supprimer une branche avec `git branch -d` et Git refuse car il voit que les commits de cette branche n’ont pas tous été fusionnés dans la branche courante (ou master).  
*Solution:* Vérifiez que vous n’oubliez pas un merge. Si c’est une branche que vous pensiez avoir mergée via une PR, peut-être avez-vous squashé (alors Git ne reconnaît pas le commit exact). Si vous êtes sûr que plus rien d’utile n’est dans la branche, supprimez avec `git branch -D <branch>`. Cela peut arriver quand on utilise squash merges, car la branche contient des commits qui ne sont pas "techniquement" dans main, bien que leur contenu y soit. On force la suppression.

**12. "git push origin :<branch>" (suppression de branche distante)**  
*Note:* Ce n'est pas une erreur, mais beaucoup de débutants cherchent comment supprimer une branche distante. La syntaxe `git push origin --delete <branch>` ou équivalente `git push origin :<branch>` supprime la branche du serveur. Par exemple, `git push origin :feature/foo` supprime feature/foo sur origin. Sur GitHub on peut aussi le faire via l’interface (après merge d'une PR par ex). 

**13. Commits perdus après un rebase ou reset**  
*Contexte:* Vous avez fait un rebase ou un reset et vous ne voyez plus certains commits dans `git log`.  
*Solution:* Ils ne sont pas forcément perdus ! Utilisez `git reflog` pour lister l’historique de vos HEAD. Vous y verrez par ex `HEAD@{2}: commit: ...` ou `HEAD@{5}: reset: moving to HEAD~1`. Recherchez le SHA du commit manquant et vous pourrez le récupérer (par exemple, si c’était un commit local non poussé, faites `git cherry-pick <sha>` sur la branche courante). Si c’était un commit poussé et qu’il a été réécrit, peut-être le bon flux était de revert plutôt que rebase.
   - Aussi, si vous avez accidentellement effacé des commits par reset --hard, ne faites aucune autre action (qui pourrait nettoyer reflog) et utilisez un outil comme `git fsck` ou l’exploration de reflog pour retrouver le commit.
   - D'une manière générale, **Git ne supprime réellement les commits qu’après un certain temps** (ou lors d’un garbage collect). Donc on peut souvent les récupérer via reflog ou `git log -g`.

**14. Problèmes de cas sensibles sous Windows**  
*Contexte:* Sous Windows (système insensible à la casse par défaut), renommer un fichier `Readme.md` en `README.md` ne change pas le nom sur le disque, Git peut ne pas détecter le changement ou créer des doublons étranges.  
*Solution:* Configurez `git config core.ignorecase false` si besoin. Ou renommez en deux étapes: renommer en un autre nom provisoire puis renommer à la casse désirée. Ce n’est pas une "erreur" Git en soi, mais l’OS. Sur macOS aussi parfois. On doit faire attention aux noms de fichiers.

**15. "git commit --amend" perds mon commit sur remote**  
*Contexte:* Vous avez amendé un commit déjà poussé et fait un push forcé, maintenant vos collègues ont un historique en décalage.  
*Solution:* Évitez d’amender ou rebaser des commits déjà pushés sur une branche partagée. Si c’est fait, informez l’équipe de tirer la nouvelle histoire (via `git pull --rebase` ou recloner s’ils sont perdus). C’est mieux de faire un commit correctif séparé. Ce problème met en lumière la règle "ne pas réécrire l’historique partagé".

**16. "git stash drop" le mauvais stash**  
*Contexte:* Vous avez plusieurs stashes et supprimez le mauvais.  
*Solution:* Sachez que `git stash pop` supprime automatiquement le stash appliqué. Si vous dropez par erreur un stash utile, il n’y a pas de untracked reflog facile, bien que techniquement stash sont des commits référencés dans refs/stash. Si vous le droppez, c’est comme supprimer une branche contenant ces commits, mais ils peuvent encore être dans reflog HEAD si vous l'aviez appliqué avant. Sinon, prévention: utilisez `git stash list` et droppez en nommant explicitement stash@{N}. Soyez prudent, pas de "undo" simple après un drop. Vous pourriez chercher dans `git fsck --unreachable` pour retrouver un stash commit perdu, ce qui est avancé.

**17. Conflits de rebase sur submodule (rare)**  
Si vous utilisez des submodules (dépôts imbriqués), résoudre des conflits ou des divergences de submodule peut être compliqué. Souvent la solution plus simple est de décider quelle référence de submodule garder, ou d’aller dans le submodule et régler les choses. 

En général, la plupart des erreurs git ont une solution et surtout, rien n’est vraiment irréversible dans l’immédiat. Gardez votre calme, lisez bien les messages d’erreur (ils donnent souvent la piste de la commande à exécuter). Cherchez dans la documentation ou en ligne avec le message précis. Et n’oubliez pas, en cas de gros doute, on peut toujours faire une sauvegarde du dossier (copie) avant de tenter une correction destructive, histoire de pouvoir revenir en arrière si on aggrave le problème.

Git étant complexe, il y aura d’autres situations, mais les principes pour résoudre:
- Cherchez l’état du repo (`git status`, `git log`, `git branch -vv`, `git remote -v`) pour diagnostiquer.
- Utilisez les commandes de nettoyage ou d’annulation avec précaution (stash, reset, revert).
- En dernier recours, si votre repo local est vraiment dans un état incohérent et que vous êtes sûr que tout ce que vous voulez garder est sur le remote, vous pouvez recloner le dépôt sainement et reporter vos modifications manuellement. Mais c’est rare de devoir en arriver là.

## 8. Sécurisation et gestion des accès dans un projet Git

La collaboration ne se limite pas au code : il faut aussi gérer **qui a accès** au dépôt et comment protéger certaines actions, surtout dans un contexte multi-développeurs ou open-source. Il faut également veiller à la sécurité du contenu du dépôt lui-même (éviter les fuites d’informations sensibles). Voici les aspects à considérer :

### 8.1 Gestion des accès utilisateur

- **Contrôle d’accès au dépôt** : Sur la plateforme git (GitHub, GitLab, etc.), configurez les droits appropriés.
  - Dans un projet privé d’entreprise, n’accordez le push qu’aux membres de l’équipe. Les externes en lecture seule si besoin.
  - Utilisez les équipes/groupes pour gérer les droits en lot (ex: équipe "Dev Backend" a accès en écriture au repo API).
  - Sur GitHub, vous pouvez inviter des collaborateurs (accès direct) ou utiliser une organisation et mettre les devs dans un team avec rôle.
  - Sur GitLab, vous avez des rôles (Guest, Reporter, Developer, Maintainer, Owner) avec privilèges croissants. Donnez par ex. *Developer* aux devs (peuvent push), *Maintainer* aux leads (peuvent configurer le repo).
- **Authentification** : Préférez l’usage des **clés SSH** ou des **Tokens d’accès** sur HTTPS, plutôt que les mots de passe simples. D’ailleurs, GitHub a désactivé l’auth par password direct pour les opérations Git : il faut un **Personal Access Token (PAT)** ou utiliser SSH. 
  - Les clés SSH sont très pratiques : chaque utilisateur génère une paire de clés, la publique est ajoutée sur son compte du serveur Git. Ça assure l’identité sans mot de passe à chaque fois, et on peut révoquer une clé si besoin.
  - Les tokens sur HTTPS ont l’avantage d’être plus fins (on peut limiter leur scope). À stocker dans un gestionnaire de credentials comme *Git Credential Manager* (sur Win/Mac) pour ne pas retaper à chaque fois.
- **Authentification à deux facteurs (2FA)** : Activez le 2FA sur vos comptes GitHub/GitLab s’ils contiennent du code sensible ou privé. Cela évite qu’une compromission de mot de passe suffise pour accéder au code. Surtout important pour les *owners* ou comptes admin.
- **Clés de déploiement** : Si un serveur (CI ou un serveur de prod) doit tirer le code, n’utilisez pas le compte perso de quelqu’un. Utilisez une *Deploy Key* (une clé SSH dédiée à ce repo) ou un *Deploy Token*, avec des droits restreints (ex: lecture seule).
- **Permissions sur branches** : Protégez les branches critiques comme `main` ou `master`. Sur GitHub/GitLab, on peut :
  - Interdire le push direct (exiger PR),
  - Exiger N approbations de PR avant merge,
  - Exiger que la CI passe avant merge,
  - Empêcher la suppression de la branche ou le force-push sur celle-ci ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=Visual%20Studio%20fournit%20une%20interface,vous%20connecter%20%C3%A0%20un%20fournisseur)).
  - Ceci évite les accidents (un dev qui force push main et efface l’historique par mégarde, ou un commit non validé qui slippe).
- **Dépôts publics (open-source)** : Pour les contributions externes, ne donnez pas l’accès direct en écriture à n’importe qui. Habituellement, les externes forment un *fork* et proposent des PR. Seuls les mainteneurs (trusted) ont le droit de merge. Si un projet devient gros, ajoutez plus de mainteneurs de confiance pour étaler la charge, mais toujours en vérifiant leur historique dans la communauté.
- **Revue de code obligatoire** : Même en interne, c’est bien de fonctionner en "Aller via PR", comme ça personne ne push du code non relu sur main. Cela a un double effet : code meilleur, et logs de qui a approuvé quoi (utile en audit).
- **Journalisation** : Git lui-même garde qui a commité (nom/email) mais ça peut être altéré. La plateforme, elle, logge qui a poussé, qui a mergé. Ces logs d’événements (accessible dans UI ou via GraphQL) peuvent être utiles en cas d’incident (savoir qui a introduit une faille, ou qui a modifié les droits d’accès quand).

### 8.2 Protection du contenu et sécurité interne du dépôt

- **Ne pas exposer de données sensibles** : Comme évoqué, ne versionnez pas les secrets (clés privées, mdp). Non seulement par risque qu’ils se retrouvent publics un jour (ex: le dépôt devient open source, ou un dev mal intentionné), mais aussi car même en privé, de nombreux yeux y ont accès, et une fuite interne est possible. Utilisez des outils externes (Vault, config non versionnée, etc.). Si un secret a été commité, changez-le (révoquer le token, etc.) immédiatement. Et retirez-le de l’historique si possible (via filter-branch ou BFG), surtout si le dépôt devient public. 
- **Scan de secrets** : En prévention, activez un outil de scan de secrets. GitHub propose "Secret Scanning" sur les dépôts publics (et pour les entreprises sur privés) qui détecte des patterns de clés AWS, Azure, etc. Il y a aussi des pre-commit hooks (Git Secrets, pre-commit frameworks) pour empêcher de committer des choses qui ressemblent à un secret connu (ex: une ligne qui contient "AWS_ACCESS_KEY_ID").
- **Signature des commits** : Pour s’assurer qu’un commit ou tag vient bien d’une personne de confiance et n’a pas été altéré, on peut utiliser la **signature GPG** ou **signature X.509**. Git permet de signer les commits (`git commit -S`) et les tags (`git tag -s`). Sur GitHub on voit "Verified" si la signature correspond à une clé connue de l’auteur. Ce n’est pas obligatoire pour tous les projets, mais pour les projets sensibles (kernel Linux, ou projets où du code malicieux pourrait être inséré) c’est un niveau de sécurité. En interne, ça peut assurer une trace anti-usurpation (par ex. s’assurer qu’un commit sur main vienne bien d’un mainteneur).
- **Git Hooks côté serveur** : Si vous hébergez vous-même Git (par ex sur un serveur Gitolite, ou GitLab CE sur-premise), vous pouvez configurer des hooks côté serveur. Par ex: un *pre-receive hook* qui rejette tout push contenant une certaine condition (fichier interdit, commit non signé, etc.), ou un *update hook* qui logge les push entrants. Les plateformes SaaS comme GitHub ne laissent pas mettre des hooks custom côté serveur, elles offrent à la place les *Protected Branch Rules* et *GitHub Actions* etc. Sur GitLab on peut créer des hooks custom si autohébergé.
- **Sauvegardes** : Un dépôt Git central devrait être sauvegardé (surtout s’il est sur un serveur interne). GitHub/GitLab cloud gèrent eux-mêmes la redondance. Mais par précaution, on peut garder un miroir du repo sur un autre serveur. Git étant distribué, chaque clone est une copie, donc la probabilité de tout perdre est faible. Néanmoins, en entreprise, c’est bien d’avoir un backup hors-site (ex: un `git push` miroir vers un stockage S3 ou autre).
- **Mises à jour des dépendances** : Sécurité, c’est aussi dans le code. Utilisez des outils (Dependabot, Renovate) qui scannent votre repo pour trouver les dépendances obsolètes ou avec vulnérabilités, et proposent des PR de mise à jour. Cela maintient le projet plus sûr.
- **Permissions sur tags/releases** : Parfois, on veut contrôler qui peut créer des tags (surtout s’ils déclenchent des déploiements). Sur GitLab, on peut limiter ça. Sur GitHub, pas natif, mais on peut via protection branch (une wildcard sur refs/tags?). Vérifiez que seulement les mainteneurs puissent publier une release (pour éviter qu’un dev non autorisé publie v1.0.1 officieusement).
- **Attaques par dépendance** : Faites attention à d’où viennent vos submodules ou autres. Si vous utilisez submodule, utilisez idéalement des versions fixes. Une attaque connue est de compromettre un repo de dépendance ou une container image. Ce n’est pas Git en lui-même, mais en collaboration, chacun peut ajouter une lib, il faut valider la provenance (est-ce un repo officiel, un fork malicieux?). Ayez un processus de revue pour ce genre de choses.

### 8.3 Environnements isolés et contributions externes

- **Forks externes** : Sur un projet open source, il est fréquent de recevoir des PR de fork de personnes non dans l’organisation. Ces contributeurs ne peuvent pas accéder aux secrets du CI (ex: tokens de déploiement) par sécurité. Conception de GitHub: les secrets n’exécutent pas sur les PR de fork ou alors en read-only. Il faut en être conscient: une PR malveillante ne peut pas facilement exfiltrer les secrets car ils ne sont pas fournis. Donc le modèle de contribution via fork est plus sécure.
- **Examen de code externe** : Traitez toute contribution comme potentiellement hostile (principe de sécurité zéro confiance). Vérifiez le code ajouté pour toute porte dérobée ou effet indésirable. Dans des projets très sensibles, on fait signer un CLA (Contributor License Agreement) aux contributeurs et on examine les contributions sous l’angle sécurité.
- **Historique immuable** : Une fois un commit poussé sur main, évitez de le réécrire (force push). Non seulement ça perturbe l’équipe, mais si quelqu’un a cloné entre-temps, il a une version divergeante. Laissez l’historique faire son rôle d’audit. S’il y a un commit "mauvais", faites un revert (quitte à squash plus tard via un outil de maintenance s’il le faut, mais en interne vous avez la trace).
- **Audit des logs** : Dans les organisations, surveillez la création/suppression de branches importantes, la configuration du repo. Par exemple, sur GitLab on peut voir si quelqu’un a désactivé la CI ou baissé la protection. Ce genre d’actions doit être contrôlé par un nombre restreint (Maintainers/Admin).

### 8.4 Sécurité du poste client

Chaque développeur qui accède au repo doit lui-même suivre des pratiques de sécurité :
- **Protéger ses clés** : Si une clé SSH est compromise, un attaquant peut push du code malveillant. Donc protéger la clé privée par un passphrase forte, et ne pas la laisser traîner.
- **Éviter de cloner sur un serveur public** : Cloner le repo sur un ordi perso non sécurisé ou sur un serveur accessible, c’est risqué. Utilisez VPN ou connexions chiffrées.
- **Mises à jour Git** : Gardez votre version de Git à jour, car il y a déjà eu des failles (notamment sur le parsing de chemins ou des hooks). Par exemple, des repos malveillants avec noms de branches spéciaux pouvaient exécuter du code si on faisait git clone. Donc patcher Git minimise ce risque.
- **Pré-commit hook tiers** : Si vous exécutez des hooks fournis par d’autres, soyez conscient qu’un hook est un script qui s’exécute sur votre machine quand vous commit/push/pull. N’exécutez pas n’importe quel hook non vérifié, ça peut contenir du code malveillant. (Git ne partage pas les hooks via clone, c’est local config, donc ce risque arrive si vous même placez un hook dangereux dans votre .git/hooks ou quelqu’un qui accède à votre poste).
- **Sérialisation du repo** : N’importe qui ayant accès au dossier `.git` complet a l’historique. Donc évitez de laisser traîner une copie du repo sur un serveur web (il existe des cas où le dossier .git d’un site web était accessible, permettant de récupérer tout le code source!). Par exemple, si on déploie un site web, on ne copie pas .git sur le serveur web ou on le sécurise pour qu’il ne soit pas accessible.

En appliquant ces mesures, votre utilisation de Git restera sûre, limitant les possibilités d’accès non autorisé ou de modifications non contrôlées du code source, ce qui est essentiel pour la qualité du produit et la propriété intellectuelle.

## 9. Intégration de Git dans Visual Studio et l'explorateur de fichiers Windows

Beaucoup de développeurs sur Windows utilisent des environnements intégrés (IDE) ou l’explorateur pour interagir avec Git, plutôt que la ligne de commande exclusivement. Heureusement, Git s’intègre bien dans Visual Studio (pour les dev .NET/C++ par ex) et via des outils comme TortoiseGit dans l’Explorateur Windows. Voyons comment ces intégrations fonctionnent et comment les utiliser efficacement.

### 9.1 Git dans Visual Studio (VS)

**Visual Studio** (à ne pas confondre avec VS Code) offre depuis les versions 2019+ une expérience Git complète intégrée ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=Visual%20Studio%20fournit%20une%20interface,vous%20connecter%20%C3%A0%20un%20fournisseur)). 

- **Initialiser un dépôt dans VS** : Si vous créez un nouveau projet Visual Studio, en bas à droite de la fenêtre de création de projet il y a souvent une case "Place solution and project in the same directory" et "Create new Git repository". Cochez pour init directement le repo. Sinon, plus tard, vous pouvez aller dans *Git > Create Git Repository...*. Visual Studio va exécuter `git init`, et si vous êtes connecté à GitHub/AzureDevOps, il peut même créer un repo distant et pousser le commit initial.
- **Cloner un dépôt** : Via *Git > Clone Repository...* vous entrez l’URL (ou choisissez parmi GitHub/Azure DevOps liés à VS). Visual Studio clone le repo et ouvre directement la solution s’il en trouve une.
- **Connexion aux services** : Dans VS, vous pouvez vous connecter à GitHub, Azure DevOps, Bitbucket, etc. via *File > Account Settings*. Une fois connecté, VS peut lister vos repos distants, vos PR, etc. Par exemple, *View > GitHub Pane* permet de voir les PR sur le repo actuel.
- **Fenêtre "Git Changes"** : Remplace l’ancienne "Team Explorer" pour Git. On y voit trois sections : *Changes*, *Staged* (appelés "Pending Changes" et "Staged Changes"), et le champ de message de commit ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=match%20at%20L363%20simplement%20votre,a)). Cochez des fichiers pour les stage (ou clic droit stage). On peut double-cliquer un fichier pour voir le diff. 
- **Faire un commit** : Tapez le message de commit dans l’encart en haut, puis cliquez *Commit All* (ou *Commit Staged*). Vous pouvez aussi faire *Commit All and Push* en une étape ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=simplement%20votre%20message%20de%20validation%2C,a)). Si commit réussi, Visual Studio affiche un toast ou met à jour l’historique.
- **Branch Management** : En bas de VS, la branche courante est affichée. Cliquez dessus pour la liste des branches, créer/supprimer/renommer. Ou via *Git > Branches...* qui ouvre une fenêtre latérale listant *Branches locales* et *Branches distantes*. Depuis là, on peut double-cliquer une branche distante pour la créer en local (checkout tracking) ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=match%20at%20L410%20%C3%A0%20cocher,branch)).
- **Pull, Push, Fetch** : Dans la barre d’outils de VS 2022, il y a des icônes pour Pull, Push (une flèche vers le bas, une vers le haut). Le *Git > Fetch* permet juste de rafraîchir. Visual Studio indique les *Incoming Commits* et *Outgoing Commits* dans la vue historique ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=match%20at%20L442%20validation%20de,d%E2%80%99extraire%20ou%20d%E2%80%99envoyer%20les%20commits)). Avant de push, vous pouvez fetch/pull depuis l’IDE. VS signale aussi si votre branche est en retard/avance (ex: "2↓ 3↑" pour 2 commits à tirer, 3 à pousser).
- **Résolution de conflits dans VS** : Lors d’un pull qui cause conflit, la fenêtre *Git Changes* le mentionne et liste les fichiers en conflit. Si on double-clique un fichier en conflit, VS ouvre le *Merge Editor* : on voit deux colonnes (Source et Target) et en bas la résultante. On peut accepter un côté ou fusionner. VS marque les conflits non résolus en rouge. Une fois résolu, on *Accept Merge*. Ensuite, on commit le merge.
- **Historique et comparaison** : VS a une fenêtre *Git Repository* (introduite dans VS2019 later, accessible via *Git > Manage Branches* ou *Git Repository*). Elle montre un graphe des commits ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=match%20at%20L447%20,branche%20dans%20Visual%20Studio)) avec références. On peut cliquer un commit pour voir le diff dans *Git History* window ([À propos de Git dans Visual Studio | Microsoft Learn](https://learn.microsoft.com/fr-fr/visualstudio/version-control/git-with-visual-studio?view=vs-2022#:~:text=aper%C3%A7u%20de%20son%20historique%20de,validation%20dans%20le%20volet%20droit)). Egalement, *Git > View History* sur un fichier donne l’historique de ce fichier (commits où il a changé).
- **Tags** : Via VS UI, on peut créer un tag : dans la liste de branches, il y a une section *Tags*, ou clic-droit sur un commit *Create Tag*. Pousser les tags n’est pas encore automatique, on peut le faire via *Git > Push -> Push Tags* ou push depuis CLI.
- **Stash** : Visual Studio 2022 inclut le support du stash. Dans *Git Changes*, menu *Stash -> Stash All* (et pop). On peut voir la liste des stashes via *Git Repository* window.
- **Intégration avec Azure DevOps (ex TFS)** : Visual Studio supporte Git nativement. (Les anciens TFS "Team Foundation Version Control" sont distincts). On peut utiliser Git sur Azure Repos; VS propose de connecter. On peut associer les commits aux work items en mentionnant #ID etc. Azure DevOps peut imposer du *branch policies* (ex: require linked work item).
- **Extension tierces** : Il existe des extensions VS comme *Git Extensions*, *Git Tools*, mais VS 2022 en a moins besoin, son support natif est bon. On peut aussi configurer un *Diff/Merge external* (par ex Araxis) dans *Tools > Options > Source Control > Git*.
- **Limites** : Visual Studio ne prend pas en charge 100% des commandes. Par ex, rebase interactif, bisect, cherry-pick multiple, submodules management, etc., sont plus aisés en ligne de commande. Il y a bien *Git > Cherry-Pick...* sur un commit ou *Rebase > Pick base* sur branch, mais pour du complexe, utilisez CLI ou un outil dédié.

En somme, Visual Studio permet aux développeurs d'effectuer la plupart des opérations Git sans quitter l'IDE, ce qui améliore la productivité sur Windows pour ceux qui préfèrent le tout intégré.

### 9.2 Intégration dans l’explorateur de fichiers Windows

Pour les développeurs qui aiment interagir via l’explorateur Windows, **TortoiseGit** est un outil très populaire (analogue à TortoiseSVN à l’époque). Il s’intègre au shell Windows en ajoutant des options de menu contextuel et des icônes.

- **Installation de TortoiseGit** : Téléchargez l’installateur sur <https://tortoisegit.org/>. Il faut également Git for Windows en backend (mais vous l’avez déjà si vous suivez ce guide). Pendant l’installation, associez-le bien avec votre version de Git. TortoiseGit se place comme une interface.
- **Menu contextuel** : Après installation, clic-droit sur n'importe quel dossier:
  - Si le dossier est un repo Git, vous verrez des options comme *Git Commit -> "branch name"*, *Git Pull*, *Git Push*, *TortoiseGit >* (sous-menu complet) ([Git/Windows — Wikilivres](https://fr.wikibooks.org/wiki/Git/Windows#:~:text=TortoiseGit)).
  - Si le dossier n’est pas un repo, vous avez *Git Clone* et *Git Create repository here*.
- **Icônes d’état** : Sur les dossiers/fichiers versionnés, TortoiseGit affiche des icônes overlays (une coche verte si pas de modif, un point rouge si modifié, un signe plus pour nouveau, un point d'exclamation jaune si en conflit) ([Git/Windows — Wikilivres](https://fr.wikibooks.org/wiki/Git/Windows#:~:text=TortoiseGit%20est%20un%20client%20pour,ses%20d%C3%A9p%C3%B4ts%20depuis%20l%27explorateur%20Windows)) ([Git/Windows — Wikilivres](https://fr.wikibooks.org/wiki/Git/Windows#:~:text=,droit%20d%27un%20dossier)). Ces indicateurs dans l’explorateur permettent de voir d'un coup d'œil les modifications. Si vous ne les voyez pas, il y a une limite Windows de 15 overlays, assurez-vous que TortoiseGit est prioritaire (réglable dans TortoiseGit > Settings > Icon Overlays).
- **Commit via Explorer** : 
  - Naviguez dans votre dépôt, faites clic droit dans la racine -> *Git Commit -> "branch"* ([Git/Windows — Wikilivres](https://fr.wikibooks.org/wiki/Git/Windows#:~:text=)). 
  - Une fenêtre "Commit" s'ouvre (TortoiseGit dialog) listant tous les fichiers modifiés, avec cases à cocher pour sélectionner lesquels committer. On peut double-cliquer un fichier pour voir le diff.
  - Rédigez le message en haut. Bouton *Commit* pour valider. S’il y a des fichiers non ajoutés, TortoiseGit propose d'ajouter automatiquement.
- **Push/Pull** : Clic droit dossier -> *Git Sync...* ouvre un panneau de synchronisation où on peut Fetch, Pull, Push d'un clic. Ou alors directement *Git Pull* et *Git Push* entrées séparées. 
  - Le *Git Sync* est pratique car il centralise et montre la progression, et permet le Rebase également.
- **Résolution de conflit** : En cas de conflit, TortoiseGit affiche un dialogue lors du pull/merge indiquant les fichiers en conflit. Vous pouvez cliquer *Launch Merge Tool* pour chaque fichier, qui ouvre TortoiseGitMerge (un outil dédié) en trois volets (base, local, remote). Vous choisissez les changements, puis sauvegardez. Ensuite, dans la fenêtre de conflit, vous pouvez marquer comme résolu. 
- **Log / Historique** : Clic droit sur le repo -> *Git Show log*. Une fenêtre affiche la liste des commits (avec graph en relief). Vous pouvez filtrer par texte, par auteur, etc. Clic-droit sur un commit offre options (create branch, create tag, revert this commit, cherry-pick this, etc.). Double-cliquer un commit montre le détail et la diff dans l’UI.
- **Configuration** : TortoiseGit a une interface de paramètres accessible via *TortoiseGit > Settings*. On y configure son nom, email (normalement il détecte du global config). On peut aussi paramétrer les outils de diff externe, la langue, etc.
- **Git Bash Here** : En plus de TortoiseGit, l’installation de Git fournit "Git Bash Here" dans le menu contextuel ([git avec Windows](https://profgra.org/lycee/presentation_git_avec_Windows.html#:~:text=git%20avec%20Windows%20Nous%20allons,avec%20Git%20Bash%20Here)). Cela ouvre un terminal Bash à l’endroit cliqué. Utile si une commande n’est pas couverte par TortoiseGit (ex: running a complex git filter-branch).
- **Autres clients graphiques** : 
  - **GitHub Desktop** : destiné aux repos GitHub, interface très simple pour commit/push/PR, moins d’options avancées que TortoiseGit mais plus user-friendly pour débutant. 
  - **GitKraken** : client Git multi-plateforme, interface agréable (graphe visuel, gestion de conflits facile). Il est gratuit pour usage non commercial, payant sinon. 
  - **SourceTree** (Atlassian) : gratuit, longtemps populaire, mais certains le trouvent lent maintenant. 
  - **Fork** : client payant (gratuit essai) Mac/Windows, bien noté.
- **Explorer natif** : Il y a de légères intégrations Windows 11 qui reconnaissent un dossier git (affiche le nom de branch dans Explorateur?), mais c’est limité. Mieux vaut TortoiseGit pour l’Explorer.
- **Utilisation mixte** : On peut très bien utiliser Visual Studio ET TortoiseGit ensemble. Par ex, Visual Studio pour committer pendant qu’on code, et TortoiseGit pour une opération ponctuelle hors VS ou pour gérer un patch quick hors solution. Ils se basent tous sur le même repo, donc c’est synchronisé. De même, on peut toujours ouvrir une console et utiliser la CLI en parallèle de l’IDE.
- **Attention aux line endings** : Parfois, l’IDE peut être configuré CRLF, l’UI Explorer va montrer modifié car conversion. Assurez-vous que core.autocrlf est cohérent. Visual Studio tient compte de .gitattributes, TortoiseGit aussi, donc en principe pas de souci.
- **Case des fichiers** : Sur Windows, renommer une casse via l’UI Explorer ne déclenchera pas un event case-change. TortoiseGit a une fonction "rename" dans son menu context (dans les commandes avancées) qui aide. Ou renomme via VS (il ajoute une git mv).
- **Bureau** : Windows 10 et + ont "Windows Terminal" qui peut intégrer un prompt custom (via oh-my-posh etc) pour afficher branch, statut. Installer Windows Terminal et configurer un PS1 git prompt peut être utile pour devs CLI.

## 9. Pull requests et revues de code

Les **pull requests (PR)** sont un élément central de la collaboration avec Git, particulièrement dans les workflows GitHub Flow et similaires. Une PR est à la fois une demande de fusion de votre branche dans la branche cible (souvent `main` ou `develop`) et un espace de discussion pour examiner le code. Voici quelques bonnes pratiques :

- **Faites des PR dès que votre travail est prêt à être revu**, même si la fonctionnalité n’est pas encore parfaite. Il est possible de marquer une PR comme “brouillon” (*draft*) si ce n’est pas finalisé, tout en sollicitant un retour précoce de vos pairs. Ne gardez pas indéfiniment une grosse branche privée : intégrez tôt et souvent, cela rejoint l’idée des commits et branches atomiques.

- **Limitez la taille des PR** : idéalement une PR ne doit traiter qu’une fonctionnalité ou un bug. Évitez les “usine à gaz” de 50 fichiers modifiés et 10 000 lignes – c’est décourageant à relire et propice aux erreurs. Si vous vous rendez compte que votre branche a gonflé excessivement, envisagez de la découper en plusieurs PR plus petites (par exemple, extraire quelques changements de refactoring annexes dans une PR séparée). Des PR courtes sont plus faciles à comprendre et à tester, et seront revues plus rapidement par vos collègues.

- **Soignez la description de la PR** : le titre doit être explicite (par ex. *“Ajout de l’authentification OAuth2”* plutôt que *“Corrections diverses”*). Dans le descriptif, résumez le **contexte** (pourquoi ce changement ? quel problème résout-on ou quelle fonctionnalité ajoute-t-on ?), éventuellement **comment tester** ou reproduire le bug corrigé, et mentionnez les points d’attention. Liez la PR à un ticket d’issue si applicable (via des mots-clés *fix #num* ou en utilisant l’UI de votre forge logicielle). Une bonne description permet au relecteur de comprendre les tenants et aboutissants sans avoir à deviner. C’est aussi utile plus tard pour la traçabilité.

- **Assignez des reviewers et demandez des retours** : ne fusionnez pas vos propres PR sans regard extérieur (sauf cas très trivial). Même un petit patch bénéficie d’une deuxième paire d’yeux – cela permet de repérer d’éventuelles régressions, d’améliorer la qualité du code ou simplement de partager la connaissance du changement avec l’équipe ([How to correctly review a pull request | JimBobBennett](https://jimbobbennett.dev/blogs/how-to-review-a-pr/#:~:text=How%20to%20correctly%20review%20a,check%20the%20code%20for%20correctness)). Cultivez une culture de relecture bienveillante et constructive : les commentaires doivent viser le **code**, pas la personne, et chercher à améliorer la solution. En tant qu’auteur, soyez ouvert aux critiques et prêt à expliquer vos choix. En tant que relecteur, saluez ce qui est bien, posez des questions si quelque chose n’est pas clair, et suggérez des modifications si nécessaire.

- **Testez et validez avant de merger** : la PR est l’endroit où s’exécute généralement l’intégration continue (CI). Assurez-vous que la pipeline passe (tests unitaires OK, linter OK, build sans erreur). N’hésitez pas à tirer la branche sur votre machine pour la tester manuellement si besoin (la commande `git pull origin feature/ma-branche` vous permet de récupérer la branche du contributeur et de l’exécuter localement). En équipe, vous pouvez convenir qu’au moins un reviewer approuve (**“LGTM”** pour *Looks Good To Me*) avant de pouvoir fusionner.

- **Choisissez le mode de fusion adapté** : selon la politique du projet, vous pourriez faire un **merge commit** classique, un **squash merge** (écraser tous les commits de la PR en un seul commit sur la cible, utile pour garder un historique linéaire), ou un **rebase merge** (rebaser les commits de la PR sur la cible puis les intégrer sans commit de merge). Chacune a ses avantages ; l’important est de le définir par convention. Par exemple, beaucoup de projets open-source utilisent le **squash** pour que chaque PR apparaisse comme un seul commit dans `main`, ce qui simplifie le log. D’autres préfèrent garder les commits individuels pour faciliter le suivi des modifications fines.

- **Supprimez la branche** une fois la PR fusionnée (sauf si elle doit être conservée pour d’autres raisons). Comme évoqué, cela garde le dépôt propre.

En appliquant ces pratiques, les pull requests deviennent un outil efficace de partage de connaissance, de relecture de code et de maintien de la qualité logicielle. Une bonne revue de code attrape non seulement les bugs, mais peut aussi améliorer la lisibilité, la performance, la sécurité, etc., du code. **Respectez le temps de vos relecteurs** : évitez de sortir une PR énorme le vendredi 18h, ou de demander une review urgente sur du code non testé. Si vous êtes relecteur, essayez de faire le retour dans un délai raisonnable (idéalement sous un jour ou deux), pour que l’auteur puisse intégrer vos retours rapidement ([Best Practices for Reviewing Pull Requests in GitHub](https://rewind.com/blog/best-practices-for-reviewing-pull-requests-in-github/#:~:text=A%20good%20code%20review%20process,hours%20after%20its%20first%20submission)).



En résumé, l’intégration de Git dans l’écosystème Windows est très bonne désormais. Visual Studio permet aux développeurs de gérer le contrôle de version dans leur flux de travail habituel, et TortoiseGit rend les interactions via l’explorateur très accessibles, même pour ceux peu familiers avec la ligne de commande. L’important est de trouver l’outil ou la combinaison d’outils qui rendent votre utilisation de Git la plus efficace et confortable possible.

## 10. Annexe : Checklists détaillées pour chaque action Git

Pour conclure ce guide, voici des **checklists** qui récapitulent les étapes clés et bonnes pratiques à suivre pour les actions Git courantes. Utilisez-les comme référence rapide pour vous assurer de n'oublier aucune étape importante.

### Checklist : Configurer son environnement Git (une fois par machine)
- [ ] Installer Git (et un outil comme Git Bash sur Windows).
- [ ] Configurer son nom d’utilisateur : `git config --global user.name "Prénom Nom"`.
- [ ] Configurer son email : `git config --global user.email "email@domaine.com"`.
- [ ] (Optionnel) Configurer l'éditeur par défaut, ex: `git config --global core.editor "code --wait"` pour VS Code.
- [ ] (Optionnel) Configurer la couleur : `git config --global color.ui auto`.
- [ ] Vérifier que la gestion des fins de ligne est correcte : sur Windows, `git config --global core.autocrlf true`.
- [ ] Ajouter des alias utiles (optionnel): ex: `git config --global alias.st status`, alias log, etc.
- [ ] Générer une **clé SSH** et l'ajouter à votre compte GitHub/GitLab si vous comptez utiliser SSH (vérifier avec `ssh -T git@github.com`).
- [ ] (Optionnel) Configurer le gestionnaire de credentials (sur Windows, Git for Windows installe Git Credential Manager par défaut, donc ok).
- [ ] Cloner un dépôt de test ou créer un dépôt pour s'assurer que tout fonctionne.

### Checklist : Initialiser un nouveau dépôt Git
- [ ] Créer un dossier pour votre projet et y placer les fichiers initiaux (code, README, etc.).
- [ ] Exécuter `git init` dans ce dossier. (Un sous-dossier `.git` apparaît).
- [ ] Créer un fichier `.gitignore` adapté à votre projet (avant le premier commit idéalement) pour ignorer les fichiers non voulus.
- [ ] Utiliser `git status` pour voir les fichiers non suivis.
- [ ] Ajouter les fichiers pertinents au suivi : `git add .` (ou sélectionner précisément).
- [ ] Configurer la branche par défaut si besoin (par défaut Git utilise `master` jusqu’à 2.28, sinon paramétré par init.defaultBranch).
- [ ] Faire le **commit initial** : `git commit -m "Initial commit"`.
- [ ] Si un **dépôt distant** existe déjà (par ex. créé sur GitHub):
  - Ajouter le remote : `git remote add origin <URL>`.
  - Pousser la branche principale : `git push -u origin main` (ou master selon nom).
- [ ] Sinon, si pas de distant encore, éventuellement créer le repo distant via l'UI et pousser.
- [ ] Noter que HEAD est sur votre commit initial, `git log` affiche celui-ci.

### Checklist : Cloner un dépôt existant
- [ ] Obtenir l’URL du dépôt (HTTPS ou SSH).
- [ ] Dans le répertoire parent désiré, exécuter `git clone <URL> [nom_dossier]`.
- [ ] S’il s’agit d’un dépôt privé en HTTPS, s'authentifier (via token ou credential manager).
- [ ] Assurer-vous que le clone est terminé sans erreur.
- [ ] Aller dans le dossier du projet cloné (`cd nom_dossier`).
- [ ] Vérifier la configuration du remote : `git remote -v` (devrait montrer origin).
- [ ] Vérifier les branches disponibles : `git branch -a`. La branche par défaut est généralement checkoutée.
- [ ] Configurer les accès (ex: ajouter votre propre origin si c’est un fork).
- [ ] (Optionnel) Exécuter `git lfs pull` si le projet utilise Git LFS, pour télécharger les gros fichiers manquants.
- [ ] Lancer le build ou la configuration du projet suivant README pour s'assurer que tout est correct.

### Checklist : Effectuer des commits (workflow de base)
- [ ] Avant de commencer à éditer, assurez-vous d’être sur la bonne branche (`git branch` ou vue de l’IDE).
- [ ] Écrire/modifier le code ou les fichiers.
- [ ] Utiliser `git status` pour voir les modifications.
- [ ] Ne commiter que les changements pertinents et finalisés. Si des fichiers sont en cours, soit ne pas les ajouter, soit les stash si besoin de committer séparément.
- [ ] Ajouter les fichiers à committer :
  - `git add <file1> <file2>` pour ajouter sélectivement, 
  - ou `git add -p` pour ajouter par portions si vous voulez fractionner un fichier.
  - ou `git add .` si tout est prêt à partir.
- [ ] Revérifier avec `git status` que la zone de staging contient exactement ce que vous voulez committer.
- [ ] (Optionnel) Faire un `git diff --staged` pour revoir les changements exacts qui seront commités.
- [ ] Lancer le commit : `git commit -m "Message concis qui décrit le changement"`.
- [ ] Si l'éditeur s'ouvre (pas de -m), écrire un message clair, sauvegarder et quitter.
- [ ] Vérifier que le commit est enregistré : `git log -1` devrait montrer votre commit avec le bon message.
- [ ] Si c’est un commit lié à un ticket ou une tâche, mentionner l’ID (ex: "fix bug #123 ...").
- [ ] Si vous réalisez après coup que vous avez oublié un fichier, soit faire un second commit, soit utiliser `git commit --amend` si le commit n’est pas encore poussé (puis ajouter le fichier et amender).
- [ ] Si vous avez des tests automatisés, exécutez-les avant de pousser, pour attraper les erreurs localement.

### Checklist : Pusher des modifications vers le dépôt distant
- [ ] S’assurer que la branche locale est à jour par rapport au distant : 
  - Exécuter `git fetch origin` puis `git log origin/<branch>..HEAD` pour voir si votre HEAD est en avance uniquement, ou 
  - plus simple, `git pull --rebase` pour intégrer d'abord les changements distants (évite un push rejeté).
- [ ] Résoudre tout conflit en local s’il y a eu un `pull`.
- [ ] Exécuter les tests/compilation pour valider que tout fonctionne après l’intégration.
- [ ] Pousser : `git push` (si le upstream est configuré, sinon `git push -u origin <branch>` la première fois).
- [ ] Observer la sortie pour s'assurer qu’il n’y a pas d’erreurs. 
  - Si "rejected", suivre la procédure de synchronisation (pull puis push).
- [ ] Vérifier sur la plateforme (GitHub/GitLab) que vos commits apparaissent bien sur la branche distante.
- [ ] (Optionnel) Si c’est une branche de feature qui doit être mergée via PR, peut-être créer la PR tout de suite après push.
- [ ] Informer l’équipe si c’était un changement important ou potentiellement impactant (par ex sur Slack: "J’ai poussé la refonte du module X, merci de pull sur develop").

### Checklist : Mettre à jour son dépôt (pull/sync)
- [ ] Avant de commencer la journée ou une nouvelle tâche, `git fetch` ou `git pull` la branche cible pour avoir les derniers changements.
- [ ] Si vous avez des modifications locales non commit, décidez :
  - soit les committer,
  - soit les stasher, 
  - soit elles seront fusionnées pendant le pull (si non conflictuelles et vous utilisez commit -a).
  - Sinon Git vous empêchera de pull (si risque d'écrasement).
- [ ] Exécuter `git pull` (ou `git pull --rebase` selon vos préférences).
- [ ] En cas de conflit, résoudre comme vu (modification manuelle puis commit).
- [ ] Une fois le pull terminé, compiler/exécuter le projet pour vérifier que tout marche avec les nouveaux changements intégrés.
- [ ] Si le pull a introduit de nouvelles dépendances ou modifications de config, suivre les instructions (par ex: "npm install" si package.json a changé).
- [ ] Communiquer s’il y a des changements notables (ex: "Après pull, n’oubliez pas de refaire les migrations de DB, car j’ai modifié le schéma").

### Checklist : Créer et utiliser une branche de fonctionnalité
- [ ] Basculer sur la branche à partir de laquelle vous voulez diverger (souvent `develop` ou `main`), et assurez-vous qu’elle est à jour (`git pull`).
- [ ] Créer la nouvelle branche : `git checkout -b feature/ma-feature` (depuis la base à jour).
- [ ] Publier la branche si d’autres doivent y accéder : `git push -u origin feature/ma-feature`.
- [ ] Travailler dessus : commits fréquents, push fréquents (au moins en fin de journée) pour backup et partage.
- [ ] Garder la branche synchronisée avec la base : régulièrement `git merge develop` (ou rebase) dans votre feature pour intégrer d’autres changements récents, afin de détecter tôt les conflits.
- [ ] Quand la feature est terminée :
  - S’assurer que la branche fonctionne (tests passants).
  - Mettre à jour la branche par rapport à base une dernière fois (`git merge origin/develop`).
  - Résoudre conflits si présents, retester.
  - Push final.
- [ ] Ouvrir une Pull Request vers la branche base (develop) en décrivant bien la feature et tout point à revoir.
- [ ] Réviser avec les collègues, faire les ajustements nécessaires (commits supplémentaires sur la branche).
- [ ] Une fois approuvée, fusionner la PR (souvent on squash merge ou rebase merge).
- [ ] Après merge, **supprimer la branche** distante (via l’UI ou `git push origin --delete feature/ma-feature`).
- [ ] Supprimer la branche locale si plus nécessaire : `git branch -d feature/ma-feature`.

### Checklist : Fusionner une branche (merge)
- [ ] Contexte : On va merger branchX dans branchY (ex: feature dans develop, ou develop dans main).
- [ ] Basculer sur la branche cible (ex: `git checkout develop`) et s’assurer qu’elle est à jour (pull).
- [ ] Optionnel : créer un commit de merge vide test (non, on va direct).
- [ ] Lancer la fusion : `git merge feature/branchX`.
- [ ] Si fast-forward : pas de conflit, tout va bien, la branche pointera sur les commits de branchX.
- [ ] S’il y a un **conflit** :
  - Suivre la procédure de résolution (ouvrir chaque fichier conflictuel, choisir versions, etc.).
  - Une fois résolu, `git add` puis `git commit` pour finaliser le merge (message auto de Git).
- [ ] Vérifier que le merge commit (si créé) est bien dans l’historique : `git log --graph --oneline --decorate` le montrera.
- [ ] Push branchY mise à jour : `git push origin develop`.
- [ ] Si c’était une PR via interface, la fusion se fait généralement via l’UI (crée un commit de merge ou squash).
- [ ] **Après merge** : 
  - Nettoyer la branche source si pas utile : supprimer la branche de feature locale et distante (sauf si on la garde pour d’autres merges).
  - Notifier l’équipe du merge, surtout si c’est un gros changement ("Feature X merged into develop, pull to get latest changes.").
  - Sur base de code, peut-être marquer l’intégration (update du CHANGELOG, etc.).

### Checklist : Rebaser une branche (pour mise à jour ou intégration propre)
- [ ] Contexte : On veut rebaser la branche `featureX` sur la branche `main`/`develop`.
- [ ] S’assurer qu’il n’y a pas de travail non commité sur featureX (committez ou stash).
- [ ] Passer sur la branche concernée : `git checkout featureX`.
- [ ] Mettre à jour la branche cible (ex: `git fetch origin && git checkout develop && git pull`).
- [ ] Revenir sur featureX : `git checkout featureX`.
- [ ] Lancer `git rebase develop` (en supposant develop comme cible).
- [ ] Git va rejouer vos commits un à un sur la base de develop.
- [ ] En cas de **conflit sur un commit** :
  - Git stoppe avec message, résoudre les fichiers conflictuels, `git add` une fois résolus.
  - `git rebase --continue` pour poursuivre.
  - Répéter s’il y a d’autres conflits sur les commits suivants.
  - En cas de gros problème, on peut faire `git rebase --abort` pour revenir à l’état initial.
- [ ] Une fois rebase terminé (succès), la branche featureX a maintenant de nouveaux commits (nouveaux SHAs).
- [ ] Lancer votre suite de tests pour vérifier que tout est toujours OK après rebase.
- [ ] Force push la branche rebasée vers le remote : `git push -f origin featureX`. (Car l’historique a changé, un push normal serait rejeté).
- [ ] Prévenir éventuellement les collègues qui avaient cette branche que l’historique a été réécrit (ils devront peut-être recloner ou reset leur branche local).
- [ ] Maintenant, vous pouvez ouvrir/mettre à jour la PR avec la branche rebasée, ou la fusionner directement s’il n’y a plus de conflit.
- [ ] (Optionnel) Supprimer les sauvegardes de rebase (Git en garde dans reflog HEAD@{...} au cas où, mais elles partiront plus tard).

### Checklist : Résoudre un conflit de merge/rebase
- [ ] Identifier les fichiers en conflit (`git status` vous les donne).
- [ ] Pour chaque fichier conflictuel:
  - Ouvrir le fichier et localiser `<<<<<<<` marques.
  - Décider du contenu final (prendre une version ou combiner).
  - Éditer le fichier pour obtenir le résultat voulu, enlever tous les `<<<<<<< ======= >>>>>>>`.
  - Sauvegarder le fichier.
  - Option : utiliser un outil de merge (graphique) pour faciliter cette étape, puis sauvegarder.
- [ ] Marquer le fichier comme résolu : `git add <fichier>`.
- [ ] Vérifier `git status` : le fichier ne devrait plus être "unmerged" mais dans les modifications prêtes.
- [ ] Répéter pour tous les fichiers en conflit.
- [ ] Une fois tout résolu, compléter le processus:
  - Si c’était un merge : `git commit` (le message par défaut est pré-écrit, modifiez-le éventuellement pour préciser la résolution majeure).
  - Si c’était un rebase/cherry-pick : `git rebase --continue` ou `git cherry-pick --continue`.
- [ ] Faire un build/test complet après la fusion pour s’assurer qu’aucune partie du code n’a été perdue ou mal fusionnée.
- [ ] S’il y a un problème, vous pouvez consulter le commit précédent ou stash de backup:
  - Le commit de merge est modifiable via un commit ultérieur correctif.
  - En rebase, vous pouvez `git rebase --abort` et re-réfléchir la stratégie (par ex: peut-être merge normal plutôt que rebase).
- [ ] Ajouter éventuellement dans la description du commit de merge ce qui a été fait en résolution (par ex: "Resolved conflict by keeping X version of function Y").
- [ ] Continuer le workflow normal (push etc).

### Checklist : Revenir en arrière sur un commit (revert)
- [ ] Identifier le commit fautif à annuler (`git log` pour trouver le SHA et s’assurer du contenu).
- [ ] Assurez-vous d’être sur la branche où vous voulez appliquer le revert (souvent la même où le commit est).
- [ ] Exécuter `git revert <sha_du_commit>` (vous pouvez revert plusieurs en mettant une plage ou en répétant la commande).
- [ ] Git ouvre l’éditeur pour un message de revert. Documentez-le si nécessaire (ex: "Revert X because it caused Y").
- [ ] Enregistrer/quitter pour créer le commit de revert. Si conflits:
  - Résoudre les conflits (comme une fusion).
  - `git revert --continue` une fois fait.
- [ ] Le nouveau commit de revert est créé. Examinez `git show` dessus pour vérifier qu’il supprime/ajuste bien ce qui était attendu (il devrait inverser le diff du commit initial).
- [ ] Poussez la branche avec ce nouveau commit de revert.
- [ ] Optionnel : référencez dans l’issue tracker que ce commit a été revert (souvent mention auto sur GitHub "This reverts commit abc123").
- [ ] Si plus tard on veut réintroduire le commit original (après corrections), on pourra revert le revert ou cherry-pick l'original sur la branche.
- [ ] Communiquez à l’équipe ou aux stakeholders que la fonctionnalité/bugfix X a été annulée (surtout si c’était en production et visible, pour qu’ils en tiennent compte).

### Checklist : Gérer un stash (sauvegarde temporaire)
- [ ] Vous avez des modifications en cours que vous ne voulez pas commiter tout de suite mais devez changer de contexte (branch ou pull).
- [ ] Vérifiez que tout ce que vous ne voulez pas stasher est commité (le stash va prendre modifications tracked par défaut).
- [ ] Optionnel : si vous avez des nouveaux fichiers non suivis que vous voulez inclure, soit les `git add` (sans commit) pour qu’ils soient dans l’index (le stash inclut l’index et le WD par défaut), soit utilisez `git stash -u`.
- [ ] Exécuter `git stash push -m "Description du travail en cours"` (le -m est optionnel mais utile si plusieurs stashes).
- [ ] Vérifier `git stash list` que stash@{0} est créé avec votre message. `git status` doit maintenant dire "rien à commit, WD propre".
- [ ] Changez de branche ou faites le pull, etc., l’arbre de travail étant clean.
- [ ] Quand prêt à récupérer, assurez-vous d’être dans le bon contexte (branche sur laquelle appliquer les changements).
- [ ] Exécuter `git stash pop` pour appliquer le stash le plus récent et le supprimer de la liste. Si vous voulez garder le stash (par ex le réutiliser ailleurs aussi), faites `git stash apply` à la place (puis plus tard `git stash drop` si plus besoin).
- [ ] En cas de conflit à l’application du stash, résoudre les conflits comme normalement (les conflits se présentent entre vos changements stashés et ce qu’il y a dans la branche actuelle).
- [ ] Une fois appliqué, vos fichiers modifiés doivent être de retour. Confirmez avec `git status`.
- [ ] Committez les changements ou continuez votre travail.
- [ ] S’il restait le stash (si on a fait apply sans drop), nettoyez-le une fois sûr : `git stash drop stash@{n}`.
- [ ] Évitez de laisser traîner des stashes trop longtemps car on les oublie. Pensez à les documenter (d’où l’utilité du message).
- [ ] À la fin d’un sprint, passez en revue `git stash list` et videz ceux qui sont obsolètes (`git stash clear` pour tout supprimer si vraiment plus besoin).

### Checklist : Publier une version (tag & release)
- [ ] S’assurer que la branche cible (main par ex) est dans l’état exact qu’on veut publier (tous les commits de la version présents, CI verte, etc.).
- [ ] Choisir un numéro de version suivant votre convention (MAJOR.MINOR.PATCH).
- [ ] Mettre à jour éventuellement le fichier version/changelog du projet dans un commit séparé ("Bump version to X").
- [ ] Créer un tag annoté : `git tag -a vX.Y.Z -m "Release version X.Y.Z"`.
- [ ] Pousser le tag vers le remote : `git push origin vX.Y.Z` (ou `git push origin --tags` si convenu).
- [ ] Vérifier sur la plateforme que le tag est bien présent (sur GitHub, onglet "Tags" ou "Releases").
- [ ] Créer une Release (sur GH par ex, créer un Release depuis le tag vX.Y.Z, en ajoutant notes de version). Sur GitLab, pourrait être généré aussi.
- [ ] Informer l’équipe ou la communauté que la version X.Y.Z est publiée.
- [ ] Si applicable, déployer la version (si c’est un produit à déployer).
- [ ] (Optionnel) Si on utilise une branche de release ou qu’il faut reporter sur d’autres branches:
  - Merge tag dans branche stable, ou créer branch maintenance à partir du tag si on va maintenir cette version.
  - Rediriger les correctifs de cette version en double: sur main + sur branch support.
- [ ] Archiver ou marquer la livraison selon vos besoins (ex: un commit de tag signale le freeze).
- [ ] Incrémenter ensuite la version en développement sur la branche de dev (ex: passez de 1.2.3 released à 1.2.4-dev ou 1.3.0-dev).
- [ ] Continuer le cycle de dev.

### Checklist : Ouvrir une Pull Request (et la mener à bien)

- [ ] **Préparer la branche :** Assurez-vous que votre branche de travail contient **tous les commits liés à la fonctionnalité/bug** que vous voulez proposer. Nettoyez l’historique si nécessaire (par ex., via rebase interactif pour squash les commits “bruit” du genre fix typo) – *optionnel*. *(*Pourquoi ?*)* – Une PR est plus facile à accepter si l’historique est clair. Cependant, ce nettoyage n’est pas obligatoire ; c’est souvent accepté d’avoir plusieurs petits commits tant qu’ils sont bien nommés. Si vous débutez, ne passez pas trop de temps à réécrire l’historique, concentrez-vous sur le code en lui-même.

- [ ] **Mettre à jour avec la branche cible :** Synchronisez votre branche avec la dernière version de la branche vers laquelle vous ferez la PR (souvent `main` ou `develop`). Faites `git pull origin main` dans votre branche (ou rebase sur main). *(*Pourquoi ?*)* – Cela garantit que votre code intègre les changements récents et réduit les risques de conflits. De plus, vous pourrez tester l’intégration en local. Si vous mergez une PR et qu’entre-temps main a bougé, la plateforme essaiera de merger mais peut signaler un conflit – autant le résoudre avant d’ouvrir la PR.

- [ ] **Vérifier que tout passe (tests, lint) :** Exécutez la suite de tests en local si elle existe, ou au minimum compilez/vérifiez le fonctionnement. *(*Pourquoi ?*)* – Une PR qui échoue direct les tests CI ou qui casse l’application aura du mal à être acceptée. Montrez que vous avez validé votre travail. 

- [ ] **Pousser la branche sur le remote :** Voir checklist “Push” précédente. Si c’est la première fois pour cette branche, `git push -u origin ma-branche` pour créer la branche distante et la lier. *(*Pourquoi ?*)* – Évidemment, la PR se base sur la branche distante, pas votre dépôt local. 

- [ ] **Aller sur l’interface Git de votre forge pour créer la PR :** Sur GitHub, un bouton “Compare & pull request” apparaît généralement après un push récent. Sinon, cliquez sur “New pull request” et choisissez votre branche comme source et la branche cible (ex: base: main <- compare: feature/branchement). *(*Pourquoi ?*)* – C’est là que vous démarrez officiellement la PR. 

- [ ] **Rédiger le titre et la description de la PR :** Comme conseillé, écrivez un **titre concis** (par ex. “[Feature] Ajout de l’authentification Google”) et une description détaillée. Mentionnez le problème résolu (“Closes #123 si vous voulez fermer une issue liée), le contexte, éventuellement une capture d’écran si c’est une UI, ou des étapes de reproduction pour un bug. *(*Pourquoi ?*)* – Plus vous facilitez le travail du relecteur, plus la review sera effica ([7 Git Mistakes a Developer Should Avoid | Tower Blog](https://www.git-tower.com/blog/7-git-mistakes-a-developer-should-avoid/#:~:text=A%20lot%20can%20be%20said,each%20update%20to%20the%20project))0】. C’est l’occasion d’expliquer les choix techniques si besoin (“J’ai utilisé telle lib car…, J’ai dû changer telle API, etc.”).

- [ ] **Assigner des reviewers/labels/projet :** Suivant votre organisation, assignez la PR à un ou plusieurs développeurs pour relecture. Ajoutez des labels (ex: “backend”, “urgent”, …) ou liez à une milestone si votre outil le permet et que c’est pertinent. *(*Pourquoi ?*)* – Assigner formalise la demande de relecture. Sans destinataire, il y a un risque que personne ne prenne en charge la review. 

- [ ] **Participer activement à la revue :** Une fois la PR ouverte, surveillez les commentaires. Répondez aux questions, engagez la discussion si on vous suggère des changements. *(*Pourquoi ?*)* – Une PR est collaborative. Si un reviewer prend du temps pour un retour, traitez-le avec attention. S’il pointe un problème, corrigez-le dans de nouveaux commits (ou amendez les commits existants si la politique le permet) puis poussez ces changements. GitHub les ajoutera à la PR automatiquement.

- [ ] **Gérer les retours et les tests CI :** Si la CI signale des erreurs, corrigez-les (ajoutez des commits). Si un reviewer demande des modifications, effectuez-les puis **faites un push**. Eventuellement, marquez la conversation comme résolue ou laissez un commentaire. *(*Pourquoi ?*)* – La PR n’est pas statique, elle évolue jusqu’à ce qu’elle soit acceptable. Montrez que vous prenez en compte les retours. 

- [ ] **Obtenir l’approbation :** Après corrections, un ou plusieurs relecteurs devraient approuver la PR (review “Approved” sur GitHub). Assurez-vous que personne d’autre ne voulait la relire (selon vos règles, peut-être 2 approbations requises). *(*Pourquoi ?*)* – C’est le feu vert pour intégrer. Sans approbation, ne fusionnez pas (sauf cas d’auto-merge autorisé et trivial, mais en général on attend au moins un avis).

- [ ] **Fusionner la PR (merge)** : Utilisez le bouton “Merge” via l’interface (ou “Rebase and merge” / “Squash and merge” selon le mode choisi). Si vous avez l’option de supprimer la branche après coup, cochez-la le cas échéant. *(*Pourquoi ?*)* – La fusion via l’interface enregistre automatiquement les références de la PR (sur GitHub, ça ajoute un message “Merge pull request #Num”). Votre code est maintenant dans la branche cible. 

- [ ] **Supprimer la branche** : Si ce n’est pas fait auto, supprimez la branche distante depuis l’interface ou via `git push origin --delete nom-branche`. Et en local, vous pouvez aussi la supprimer : `git branch -d nom-branche`. *(*Pourquoi ?*)* – Pour nettoyer comme évoqué. La branche ne sert plus, on la retire pour ne pas polluer.

- [ ] **Communiquer si nécessaire** : Annoncez sur le canal adéquat (Slack, etc.) que la feature est mergée, surtout si cela nécessite une action (déploiement manuel, information aux QA, etc.). *(*Pourquoi ?*)* – Tout le monde ne suit pas en temps réel les merges Git. Une petite note “Feature X est maintenant merge dans main, sera déployée ce soir” aligne l’équipe.

*Explications :* Ce processus peut sembler lourd mais c’est la routine d’une bonne collaboration. Les étapes 1-4 sont techniques (préparer votre code). Les étapes 5-7 sont la création formelle de la PR. 8-9 couvrent l’itération pendant la review. Et 10-13 la conclusion/mise en prod. En suivant cela, vous aurez des PR bien tenues, ce qui accélère les revues et améliore la qualité globale du code intégré.


---

Ces checklists visent à assurer qu'à chaque étape vous appliquez les bonnes pratiques et n’oubliez rien d’important. Bien entendu, elles peuvent être adaptées selon le contexte (par exemple, un projet peut ne pas utiliser de branching model complexe, ou skip des steps). Elles servent de garde-fou pour opérer Git avec confiance et propreté.

---

Ce guide complet vous a présenté tous les aspects essentiels de Git, des bases aux techniques avancées. En le suivant, vous devriez être capable d’utiliser Git de manière efficace, d’éviter la plupart des écueils classiques et de collaborer sereinement avec votre équipe sur n’importe quel projet versionné. Git est un outil puissant : sa maîtrise vous apportera un gain de productivité et de sécurité non négligeable dans vos développements. Bonne gestion de versions !
