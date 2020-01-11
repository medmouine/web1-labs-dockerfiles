Les Dockerfiles présents dans ce repository seront utilisés pour la correction des laboratoires. 

Pour vous assurer du bon fonctionnement de votre application, vous pouvez les utiliser librement mais aucune connaissance en rapport avec Docker ou toute autre outil de containerization ne fait partie de la matière du cours et vous ne serez dans aucunement jugés là-dessus.


# Dockerfiles pour les laboratoires du cours Développement d'applications Web GLO-3102


## Guide

### Installer docker
Se référer à la [documentation](https://docs.docker.com/install/)

D'autres outils de containerization peuvent être utilisés (ex: [Podman](https://podman.io/)) mais ne seront pas couverts par ce tutoriel.

### Avant de commencer
Dans le cas d'une application statique ne demandant aucune librairie externe (ex: Laboratoire 1-3), utiliser le fichier ce trouvant dans [`/static`](/static).

Pour les applications clients nécessitant des librairies externes ou des scripts de démarrage (ex: application Vuejs ou autre), utiliser les fichiers ce trouvant dans [`/dynamic`](/dynamic).

Pour les applications serveur utilisants Nodejs, utiliser les fichiers ce trouvant dans [`/node`](/node).

Il suffit de copier les fichiers en question dans le root de votre projet (dossier contenant `package.json` ou `index.html` dans le cas d'une app statique)

**Note importante : Il est important que les applications utilisants des librairies externes utilisent [NPM](https://www.npmjs.com/) et non [YARN](https://yarnpkg.com/) et qu'elles aient un script pour construire le dossier de distribution nommé `dist`, ce script doit être appelable avec la commande `npm run build`**


**Note importante 2: Il est important que les applications Node utilise une variable d'environnement pour l'acquisition du port. Vous pouvez utiliser le port par defaut de votre choix, celui que nous utiliserons pour la correction est le port `3000`.**

Pour obtenir une variable d'environnement en Nodejs :
```javascript
const port = process.env.PORT || 3000;
```
**Note importante 3: Il est important que les applications Node démarrent en utilisant un script `start` appelable avec la commande `npm start`**

Pour ajouter ce script dans votre projet, modifier le ficher `package.json` et rajouter dans la section `scripts` la commande `start` avec la commande associée pour démarrer le projet. 
Exemple :

```json
"scripts": {
  "start": "node app.js"
}
```

### Construire et lancer un container
Pour construire l'image du container utiliser la commande suivante dans le dossier contenant le ficheir `Dockerfile`:
```bash
docker build -t <NOM_DE_L'IMAGE> .
```
Pour démarrer le container utiliser la commande suivante :
```bash
docker run -it -p 8080:80 <NOM_DE_L'IMAGE>
```

Cette commande démarrera l'application sur le port `8080` de votre machine en mappant le port `80` de l'image.

Dans le cadre des application Nodejs roulant sur un port différent du port http defaut `80`, il faudrat utiliser la commande suivante :
```bash
docker run -it -p 3000:3000 <NOM_DE_L'IMAGE>
```

Cette commande démarrera l'applciation sur le port `3000`.

Pour lancer l'application en arrière plan, remplacer l'argument `-it` par `-d` dans la commande de lancement du container.
