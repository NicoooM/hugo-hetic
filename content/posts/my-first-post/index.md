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

## Créer un article

Pour créer un article, il suffit de créer un fichier **.md** dans le dossier **content**. Hugo va automatiquement générer une page HTML à partir de ce fichier.

```
hugo new posts/my-first-post.md
```

A l'intérieur, on peut utiliser du Markdown pour mettre en forme le contenu. Hugo va automatiquement générer une page HTML à partir de ce fichier.

```
---
title: "Mon premier article"
date: 2023-11-15
featured_image: "first-post.jpeg"
slug: creer-une-app-hugo
---

## Mon premier article

Suite de l'article...
```

## Override les éléments d'un thème

Pour personnaliser un thème, il suffit de créer un dossier **layouts** (qui est peut être déjà créé) à la racine du projet. Hugo va automatiquement utiliser les fichiers de ce dossier pour générer le site.

Il est possible de créer des fichiers avec le même nom que ceux du thème pour les remplacer. Par exemple, pour modifier le fichier **single.html** du thème, il suffit de créer un fichier **layouts/single.html**.

## Déployer le site sur GitHub Pages

Pour déployer le site sur GitHub Pages, il faut d'abord initialiser un dépôt Git sur Github.
Ensuite, dans les paramètres Github Pages, on peut sélectionner Github Actions comme source et choisir le workflow Hugo déjà pré-configuré.
Il ressemble à ça :

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.114.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb
          https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

Avec ce workflow, à chaque push sur la branche main, le site est automatiquement généré et déployé sur Github Pages à l'url https://username.github.io/repository-name.

## Conclusion

Hugo est un outil très puissant pour créer un site statique. Il est très facile à prendre en main et permet de créer un site rapidement. Il est possible de personnaliser un thème ou de créer son propre thème. Il est également possible d'ajouter des fonctionnalités avec des shortcodes ou des templates. Avec Github Pages, il est ensuite très facile de déployer le site.

## Liens utiles

- [Documentation Hugo](https://gohugo.io/documentation/)
- [Documentation Github Pages](https://docs.github.com/en/pages)
- [Thèmes Hugo](https://themes.gohugo.io/)
