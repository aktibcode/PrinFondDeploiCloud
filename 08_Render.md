# Chapitre : RENDER


# Déployer une application Web sur Render

![](https://i.imgur.com/0o4avhc.png)

Une fois que nous n'avons plus uniquement des fichiers statiques comme HTML et CSS, notre site Web cesse d'être un site Web et devient une application Web qui nécessite un serveur pour fonctionner. Une application Web est dynamique, évolutive et contient une logique en arrière-plan.

Par le passé, l'une des meilleures méthodes pour déployer une application Web était de l'héberger sur Heroku. Cependant, Heroku ayant abandonné son offre gratuite, ce n'est peut-être plus une option viable pour beaucoup d'entre nous. Heureusement, je suis tombé sur une alternative fantastique appelée Render. Render héberge non seulement des sites Web statiques, mais également des applications Web, des tâches cron et bien plus encore, le tout gratuitement (jusqu'à une certaine limite). Maintenant, laissez-moi vous guider à travers ces quatre étapes faciles à suivre !

## Étape 1 — Serveur dynamique :

Avant même de commencer, nous devons nous assurer que notre serveur n'est pas une valeur codée en dur, car une fois déployé, Render attribuera notre application Web à un port aléatoire. Pour nous assurer que cela fonctionne comme il se doit, nous devons faire en sorte que notre application écoute process.env.PORT ou un serveur local :

```
//make the following change in your index.js file

app.listen(process.env.PORT || 3000);
```

## Étape 2 — Masquer les fichiers sensibles :

Étant donné que nous devons télécharger notre référentiel local sur Github Pages avant de le déployer sur Render, il existe un risque d'exposer des informations privées et sensibles au public. Pour éviter cela, créons un nouveau fichier .gitignore, qui ignorera les fichiers spécifiés et empêchera le suivi des modifications qui y sont apportées.

```
//once created, simply add all the files you want to hide to .gitignore

node_modules/
.env

//the .env file is where you store your environmental variables, for instance
//you would assign your API key to an environmental variable to keep it secure
```

## Étape 3 — Envoi sur Github :

Tout d'abord, créons un nouveau dépôt Github, qu'il soit privé ou public, et veillons à ne pas ajouter le fichier README.
Une fois cela fait, nous devons retourner à notre terminal et initialiser Git. Pour ce faire, nous exécutons les commandes suivantes :

```
git init //initializes git
git add . //adds all files, the dot is for root directory
git commit -m "Initial commit" //makes an initial commit


//alternatively, we could also initialize Git through Visual Studio Code's
//source control tab
```

Maintenant, copions le code fourni depuis Github pour importer un référentiel existant et collons-le dans notre terminal :

```
git remote add origin https://github.com/[username]/[project-name].git
git branch -M main
git push -u origin main

//of course instead of [username]/[project-name] you will have your own
//username and project name as part of your https url
```

## Étape 4 — Déploiement sur rendu

Vous vous demandez peut-être comment notre serveur reconnaîtra que nous avons utilisé certains packages si notre fichier node_modules n'est pas rendu. C'est là que package.json et package-lock.json viennent à la rescousse. Render analysera ces fichiers et installera tous les packages nécessaires pour activer notre code.

En supposant que vous ayez déjà créé votre compte Render, procédons en connectant Github et en sélectionnant le référentiel que vous souhaitez déployer sur Render.

![](https://i.imgur.com/jNrqwdX.png)
