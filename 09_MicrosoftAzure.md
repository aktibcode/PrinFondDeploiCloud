# Chapitre : MICROSOFT AZURE


# Comment déployer une application MERN Stack sur Azure via l'intégration continue

![](https://i.imgur.com/vj7Y3MK.png)

Ce didacticiel explique comment configurer une application MERN pour qu'elle fonctionne sur Azure Web Services et comment la déployer via GitHub + intégration continue. Le didacticiel fonctionne à partir d'une application MERN compatible Azure que j'ai créée.

Remarque : ce tutoriel n'explique pas comment configurer une application CRUD MERN complète ni comment développer en React. Les seules données que nous enverrons de notre backend à notre frontend sont un petit objet dictionnaire.

### Commençons

Prérequis

* Compte Azure
* Compte Git + GitHub (pour l'intégration continue)
* Compte MongoDB Atlas (niveau gratuit) (vous pouvez également utiliser CosmosDB d'Azure au lieu de MongoDB Atlas, mais ce tutoriel ne le couvre pas)

# Premiers pas

1. Clonez le dépôt sur [https://github.com/Sogutt/Azure-MERN-Boilerplate]. Il s'agit du modèle d'application MERN compatible Azure.
2. Exécutez « npm install » dans le répertoire racine.
3. Exécutez « yarn install » dans le répertoire client.
4. Ajustez l'URL de connexion MongoDB dans new-index.js à l'URL de connexion que vous obtenez de MongoDB Atlas. Si vous ne savez pas comment procéder, accédez à votre cluster sur MongoDB Atlas, cliquez sur le bouton « Connecter » et suivez les étapes ci-dessous.

![](https://i.imgur.com/hyonymS.png)

![](https://i.imgur.com/ziYsNt6.png)

# 1. Structure et logique du projet

La structure est juste une application (frontend-React) à l'intérieur d'une autre (backend-Node/Express). Le répertoire racine contient notre logique backend ainsi que tous les modules nécessaires. Le répertoire client contient l'intégralité de l'application frontend avec tous les packages nécessaires à l'exécution de notre application React. Pour souligner qu'il s'agit essentiellement de deux projets distincts, j'utilise npm pour gérer l'application backend et yarn pour gérer l'application react.

Étant donné que la valeur ajoutée de ce modèle est de fournir un environnement compatible Azure, la logique de l'API (communication backend-frontend) est très simple. Notre MongoDB contient un dictionnaire simple (comme illustré ci-dessous). Nous récupérons ces données et les envoyons à notre application React frontend pour les restituer.

# 2. Environnement de développement et de production

Il existe trois différences principales entre un environnement de développement local et un environnement de production Azure.

Tout d’abord, nous devons nous assurer que nous ne codons pas en dur notre port dans server.js, car cela ne fonctionnera pas sur Azure. Au lieu de cela, nous déterminons le port de la manière suivante.

app.set('port', process.env.PORT || 5000);
console.log("++++++++++++++++" + app.get('port'));
La deuxième différence est que dans Azure, notre frontend est servi comme une build React statique. Cela signifie que nous devons exécuter « npm run build » dans notre répertoire client, puis inclure la ligne de code suivante dans notre server.js. Cela permet à Express de servir la build que nous venons de créer, ce qui est la façon dont nous servons notre frontend sur Azure.

Troisièmement
, nous devons pointer notre route GET/ dans server.js vers l'index.html dans notre build.

app.get("*", (req, res) => {
res.sendFile(path.resolve(__dirname, "client", "build",
"index.html"));
});
Nous pouvons maintenant déployer.

# 3. Mise en place de l'intégration continue

Remarque : vous devez configurer une application Web Azure avant cette étape. Il s'agit d'un processus très simple, je ne pense donc pas qu'il y ait grand-chose à couvrir. Cependant, assurez-vous que votre environnement d'exécution est Node 12.0 LTS. Si vous avez déjà configuré votre application Web avec un environnement d'exécution différent, vous pouvez le modifier dans Configuration -> Paramètres généraux.

Une fois votre application Web configurée, accédez au « Centre de déploiement ».
