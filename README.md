# SpaceX Lancements - Frontend Test

Ce projet est une application frontend réalisée avec **VueJS**, **Typescript** et **Tailwind CSS** qui présente les lancements SpaceX en se basant sur l’**API SpaceX v5**.

---

## Table des Matières

- [Introduction](#introduction)
- [Fonctionnalités](#fonctionnalités)
- [Technologies Utilisées](#technologies-utilisées)
- [Installation](#installation)
- [Déploiement](#déploiement)
- [Utilisation](#utilisation)
- [Remarques et Difficultés](#remarques-et-difficultés)
- [Ressources](#ressources)

---

## Introduction

Ce projet a été développé dans le cadre d’un test frontend afin d’évaluer la capacité à :

- Développer une application VueJS 3 avec Composition API et Typescript.
- Intégrer une API tierce (l’API SpaceX v5) et manipuler ses données.
- Utiliser Tailwind CSS pour un design moderne, responsive et animé.
- Structurer le code en composants réutilisables (NextLaunch, LaunchFilter, LaunchList, LaunchModal).

L’application affiche le prochain lancement avec un compte à rebours, permet de filtrer et de lister les lancements via une timeline alternée, et affiche un modal détaillé pour chaque lancement, incluant une intégration de vidéo YouTube.

---

## Fonctionnalités

- **Prochain lancement** : Affichage dynamique du prochain lancement avec un compte à rebours en temps réel.
- **Filtrage** : Sélection entre "Tous les lancements", "Lancements réussis" et "Lancements échoués".
- **Timeline alternée** : Affichage des 10 derniers lancements sous forme de timeline où chaque élément alterne entre la gauche et la droite.
- **Modal détaillé** : Détails du lancement (nom, date formatée, description, image, lien vers un article, lieu, payloads, clients) avec une option pour afficher une vidéo YouTube intégrée.
- **Animations et Design Responsive** : Transitions fluides, animations de soulignement sur le menu de filtre, et un design adaptatif pour différents écrans.

---

## Technologies Utilisées

- **VueJS**
- **Typescript**
- **Tailwind CSS**
- **Axios** (pour les appels API)
- **API SpaceX v5**
- **YouTube Iframe API**

---

## Installation

### Prérequis

- Node.js (version LTS recommandée)
- npm ou yarn

### Instructions

1. **Cloner le dépôt Git :**

   ```bash
   git clone <URL_DU_DEPOT>
   cd <NOM_DU_DEPOT>
   ```

2. **Installer les dépendances :**

   ```bash
   npm install
   # ou
   yarn install
   ```

3. **Configurer l'environnement (si nécessaire) :**

   Si des variables d’environnement sont requises, créez un fichier `.env` à la racine du projet et ajoutez-y les variables nécessaires.

---

## Déploiement

### Mode Développement

Pour démarrer l’application en mode développement :

```bash
npm run dev
# ou
yarn dev
```

L’application sera accessible par défaut à l’adresse [http://localhost:3000](http://localhost:3000).

### Construction et Déploiement en Production

1. **Construire le projet :**

   ```bash
   npm run build
   # ou
   yarn build
   ```

   Cela génère un dossier `dist` contenant les fichiers optimisés pour la production.

2. **Déploiement sur une plateforme statique :**

   Le dossier `dist` peut être déployé sur des plateformes telles que Vercel, Netlify, GitHub Pages, ou tout autre hébergeur de sites statiques.

---

## Utilisation

- **Prochain lancement** : La page d’accueil affiche le prochain lancement avec un compte à rebours mis à jour en temps réel.
- **Filtrage** : Utilisez le menu de filtrage pour choisir les lancements à afficher (Tous, Réussis, ou Échoués). Le contenu se met à jour dynamiquement.
- **Timeline** : Les 10 derniers lancements sont affichés sous forme de timeline alternée. Cliquez sur un lancement pour ouvrir le modal de détails.
- **Modal** : Le modal affiche le nom du lancement, la date (format jour/mois/année), la description (ou un message par défaut), une image d’illustration, un lien vers l’article de présentation, et des informations complémentaires (lieu, payloads, clients). Vous pouvez également activer l’affichage de la vidéo YouTube associée. Le modal se ferme en cliquant sur le bouton "X" ou en cliquant à l’extérieur du contenu.

---

## Remarques et Difficultés

- **Intégration de l’API SpaceX** :  
  L’API SpaceX renvoie parfois des données partielles pour certains lancements (ex. : absence de description, payloads sous forme d’IDs, etc.). Des valeurs par défaut ont été mises en place pour pallier ces manques.

- **Gestion des données imbriquées** :  
  Pour certains champs comme `launchpad` et `payloads`, l’API ne renvoyait rien. J'ai du m'assurer que l'appel de l'API était correct.

- **Design et Animation** :  
  La mise en place d’une timeline alternée, l’animation des boutons du filtre (soulignement animé) et les transitions du modal ont nécessité des ajustements CSS et l’utilisation des classes utilitaires de Tailwind CSS.

- **Intégration de la vidéo YouTube** :  
  L’intégration d’iframes YouTube a demandé de vérifier que l’URL soit bien en HTTPS et de gérer le cas où la vidéo n’est pas disponible.

- **Modal Interactif** :  
  La gestion de la fermeture du modal via le clic à l’extérieur et la propagation d’événements a été implémentée avec `@click.self`.

---

## Ressources

- [VueJS Documentation](https://vuejs.org/)
- [API SpaceX v5 Documentation](https://github.com/r-spacex/SpaceX-API)
- [Axios Documentation](https://axios-http.com/)
- [Tailwind CSS Documentation](https://tailwindcss.com/)
- [YouTube Iframe API Documentation](https://developers.google.com/youtube/iframe_api_reference)
- [Space X](https://www.spacex.com/)
