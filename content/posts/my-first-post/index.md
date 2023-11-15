---
title: "Retour d'Expérience : GitHub Pages avec Hugo"
date: 2023-11-15
featured_image: "first-post.jpeg"
slug: creer-une-app-hugo
---

## La création d'un projet Hugo

La création d'un projet Hugo est très rapide. Il suffit d'utiliser ces commandes pour initialiser la structure de base d'un site.

```
sudo snap install hugo // installer hugo
hugo new site quickstart
cd quickstart
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
hugo server
```

Hugo propose une organisation claire des dossiers, avec le répertoire **content** destiné au contenu principal. En quelques commandes, on est prêt à personnaliser un site avec un thème de base et à rédiger son contenu en Markdown.

## Override les éléments d'un thème

Pour personnaliser un thème, il suffit de créer un dossier **layouts** à la racine du projet. Hugo va automatiquement utiliser les fichiers de ce dossier pour générer le site.

Il est possible de créer des fichiers avec le même nom que ceux du thème pour les remplacer. Par exemple, pour modifier le fichier **single.html** du thème, il suffit de créer un fichier **layouts/single.html**.

```yaml
name: Déploiement vers GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Récupération du Dépôt
        uses: actions/checkout@v2

      - name: Configuration de Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"

      - name: Construction
        run: hugo --minify

      - name: Déploiement
        uses: peaceiris/actions-gh-pages@v3
        with:
          publish_dir: ./public
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
```
