p'tit tuto d'entreprise...

# Git - c'est comme Svn, mais en mieux !

## Installation

 1. **La majorité des IDE intègrent déjà Git dans leur package**

	- Eclipse : [http://www.eclipse.org/egit/?gclid](http://www.eclipse.org/egit/?gclid)
	- Netbeans : [https://netbeans.org/kb/docs/ide/git.html](https://netbeans.org/kb/docs/ide/git.html)
	- Phpstorm : [https://www.jetbrains.com/phpstorm/help/using-git-integration.html](https://www.jetbrains.com/phpstorm/help/using-git-integration.html)

 2.  **Sinon on peut aussi installer Git localement...**

...pour travailler en ligne de commande ou avec l'utilitaire graphique inclus dans le package à cette adresse : [https://git-for-windows.github.io/](https://git-for-windows.github.io/)

Le package contient **Git Bash** une invite de commande bien pensée pour Git (utile pour les administrateurs de repo à la limite, sinon passes ton chemin) et **Git UI**, une interface graphique plus sympa surtout pour gérer les branches (c'est genre **Tortoise** pour SVN).
Laissez toutes les options par défaut lors de l'installation, à la fin vous pourrez gérer Git à partir du menu contextuel windows sur vos projet (clic-droit > "GUI here") :

![git UI et git Bash](https://raw.githubusercontent.com/webSebLabbe/mini-help-git/master/images/gitui_gitbash.jpg)


----------

**ATTENTION : certains IDE intègrent toutes les fonctionnalités de Git mais demandent quand même d'installer Git sur votre machine pour interpréter les commandes.
Dans ce cas installez Git comme dans le 2ème point ci-dessus et renseignez ce chemin comme interpréteur Git dans votre IDE => *C:\Program Files\Git\bin\git.exe* .**

Pour l'utilisation en ligne de commande => [super tuto](https://www.atlassian.com/git/tutorials/).

## Le "ignore" de Git
Sous SVN, ignore est une commande, sous Git c'est un fichier à la racine du projet qui contient les fichiers et dossiers qu'on veut ignorer : `.gitignore`. C'est dans ce fichier qu'on renseigne 

![gitignore](https://raw.githubusercontent.com/webSebLabbe/mini-help-git/master/images/gitignore.jpg)

*exemple d'un fichier :*

    # Fichiers de config des IDE
	.idea
	.project

	# Fichiers des gestionnaires de dépendances (node/composer/bower...)
	bower_components/*
	vendor/*
	node_modules/*
	*.lock

	# Fichiers de log
	*.log


## Utilisation avec un IDE

 - **Importer** le projet dans le dossier de workflow créera automatiquement un dossier avec le nom du repo et les fichiers à l’intérieur.
   ![enter image description here](https://raw.githubusercontent.com/webSebLabbe/mini-help-git/master/images/clone-git-phpstorm.jpg)

 - Pour **update** un projet, sous Git c'est "**pull**" mais certains IDE gardent le terme d'update.

 - Avant de pouvoir commit des fichiers, il faut les ajouter au repo (**add**), en général les IDE proposent d'ajouter les fichiers soit à la création, soit lors du commit.

 - avec Git le **commit** est juste un enregistrement local des modification avec un message. Passage obligatoire pour envoyer ensuite au serveur distant...

 - ...avec un **push** ("pousser" la modif sur le serveur pour l'envoyer définitivement dans le repo distant). Certains IDE proposent une option pour faire le commit et le push dans la foulée (exemple Phpstorm) : 

![commit and push phpstorm](https://raw.githubusercontent.com/webSebLabbe/mini-help-git/master/images/commit&push_phpstorm.jpg)

### En résumé - après des modifs...

 1. on pense à **ajouter** les nouveaux fichiers dans Git
 2. on **commit** régulièrement (sauvegarde + message)
 3. on **push** (le commit seul ne suffit pas) pour envoyer au serveur, en vue de mettre en ligne par exemple

## Les branches

### Explications
Git inclut un système gestion de branches de développement.
L'idée c'est de garder une base saine et utilisable en prod qu'on appelle le tronc commun (branche **Master**) et de développer sur des copies de cette base (par exemple **Develop**, **Feature1**, ce qu'on veut du moment que c'est compréhensible). 

*Exemple d'organigramme des branches Git :*
![organigramme des branches](https://raw.githubusercontent.com/webSebLabbe/mini-help-git/master/images/git-branches.png)

**La visualisation des branches d'un projet ressemble en général à ça :** 

 - *Master* : la branche principale, en prod si le projet est en ligne, **on ne doit pas développer sur cette branche**
 - *Dev (ou Developp)* : créée pour tout développement. On peut aussi la nommer avec le nom du dev en cas d'interventions différentes (Dev_Lucas / Dev_Justine / ...)
 - *Feature1, Bugfix3,...* : copie de(s) branche(s) dev pour l'ajout d'une fonctionnalité particulière ou juste pour travailler sur un bug à résoudre

Lorsque le travail est terminé, on peut fusionner les branches concernées pour au final copier les changements sur la branche de prod.
Exemple : **Develop > Master** ou **Feature1 > Developp**

### Utilisation
Sur un projet classique, le premier dev n'aura qu'une seule branche disponible, **Master**.
Pour créer une branche il suffit d'être sur celle que l'on veut copier (ici Master donc) et à partir de ton IDE dans le menu Git, sélectionnes New Branch... (ou Create Branch...).

*Exemple sous Eclipse :*
![](https://raw.githubusercontent.com/webSebLabbe/mini-help-git/master/images/gestion-branches-eclipse.jpg)

Une fois créée, assures-toi que tu as switché dessus pour commencer à travailler. Ce changement de branche s'appelle **checkout** sous Git (récupérer une branche). Les IDE proposent une option pour checkout la branche qu'on s'apprête à créer.

*Exemple sous Eclipse, pour vérifier, en rouge le nom de la branche courante. Info visible aussi dans le menu sur l'image du dessus*
![branche courante](https://raw.githubusercontent.com/webSebLabbe/mini-help-git/master/images/nom_branches_eclipse.jpg)

**/!\ Pour valider un changement sur une branche, il faut au moins faire un commit**

### Fusionner des branches
Copier les changements d'une branche A vers une branche B sous Git, ça s'appelle un **merge**.

Imaginons que tu as terminé tes modifs sur **Developp** (pour reprendre l'organigramme). Pour envoyer les changement sur **Master**, il faut **checkout** la branche de destination (ici Master) et sélectionner "**Merge...**" dans ton IDE. Une interface apparaît pour sélectionner la(les) sources (ici Develop).
A la validation, il se peut qu'une interface de **conflit** apparaisse, pas de panique, **annules le merge**, fais un **update** (pull) et **recommences**.
S'il y a toujours un conflit, vérifies les changements dans l'interface qui s'affiche... **et bon courage** !!
(c'est rare si tu penses bien **à update la branche de destination avant de merge**)

On peut de la même manière créer **Feature1** à partir de **Develop** pour créer un slider (par exemple) puis merge le résultat sur la branche de destination (Develop donc) et chaque branche peut comme ça être copiée pour ajouter une fonctionnalité au développement en cours (attention à ne pas créer un sac de nœuds impossible à maintenir quand même...)

### Suppression
Une branche devenue inutile peut être supprimée, la commande est prévue dans le menu Git de ton IDE/Logiciel.
Attention à ne pas supprimer une branche en cours de développement, mais normalement le logiciel donnera une alerte.
La meilleure solution c'est de ne jamais supprimer à moins d'être sur (pour faire du ménage si beaucoup de branches, par exemple).


## Sources et articles

On va surement n'utiliser que 20-25% des capacités de Git dans la majorité des cas, mais c'est la base et c'est suffisant. Si tu veux aller plus loin ou mieux comprendre le fonctionnement, v'là quelques ressources sympas :

 - [http://git-scm.com/docs/gittutorial](http://git-scm.com/docs/gittutorial) : tuto officiel...
 - [https://www.atlassian.com/git/tutorials](https://www.atlassian.com/git/tutorials) : mon préféré !
 - [https://openclassrooms.com/courses/gerez-vos-codes-source-avec-git](https://openclassrooms.com/courses/gerez-vos-codes-source-avec-git) : en frenchy'
 - [http://www.grafikart.fr/formations/git](http://www.grafikart.fr/formations/git) : série de vidéos plutôt bien faite, french' again.
 - [http://git-scm.com/book/en/v2](http://git-scm.com/book/en/v2) : et ce cours officiel ultra complet mais long... très long ! Plutôt pour les administrateurs.
