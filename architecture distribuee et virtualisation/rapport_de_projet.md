# Compte rendu du projet d'architecutre distribuée et virtualisation.

Ce rapport documente la manière dont j'ai réalisé le projet du cours sur l'architecture distribuée et la virtualisation.
Voici le lien du repository du projet : https://github.com/WilliamsiNFINITE/projet_architecture_distribuee_virtualisation

## Sommaire

* [TD 1](#td-1)


## TD 1

- 1 : Mettez en place votre environnement de travail

Pour ce premier TD j'ai installé les éléments requis pour le bon fonctionnemenbt du projet (docker, fly.io, wsl).
Dans un second temps, j'ai initialisé le repository en y installant les dépendances avec npm, puis, j'ai executé les tests.

- 2 : Que pouvez-vous dire sur le fichier `package.json` ? Sur le fichier `package-lock.json` ?

Le fichier `package.json` contient le nom des dépendances du projets. Le fichier `package-lock.json` désigne les éléments nécessaires à tous les packages et dépendances avec plus de détail.

- 3 : L'installation de systeminformation avec la commande `npm install systeminformation` a ajouté cette dépendance dans le repository. 

```json
  "dependencies": {
    "systeminformation": "^5.12.13"
  }
```

`devDependency` designe les dépendances qui sont utilisées en local dans le cadre du développement de l'application et qui peuvent ne pas être utilisées en production. `dependency` sont les dépendances qu'on utilise pendant la production de l'application. 

- 4 : 

Difficulté : Appréhender les nouveaux outils. 

- 5 Testez le fonctionnement de votre application. Vous pouvez utiliser l’outil curl. À votre avis, pourquoi utilise-t-on ce formalisme pour construire l’URL de l’API ? 
A mon avis cet URL désigne une API dont la version est la numéro 1 et qui sert à recevoir les informations du système. 

- 6 Écrivez un jeu de tests pour votre application avec Jest, et vérifiez son exécution. Pourquoi écrit-on un tel jeu de tests ?
On écrit un jeu de test pour s'assurer du compretement de notre application. 


## TD 2

- 8 Déployez un nouveau conteneur à partir de votre image publiée. Quelle commande utilisez-vous ?
On utilise la commande [`docker create`](https://docs.docker.com/engine/reference/commandline/create/#options)




