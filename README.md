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

### Ressources supplémentaires

- [Documentation officielle de Git](https://git-scm.com/doc)
- [Tutoriels Git par Atlassian](https://www.atlassian.com/git/tutorials)
