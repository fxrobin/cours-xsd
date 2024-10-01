# IVTR : COURS XML-SCHEMA

Ce projet contient le support de cours pour le cours XML-Schema format Markdown.

Pour l'afficher dans le navigateur, il faut utiliser reveal.js, comme indiqué ci-dessous.

> **Note**: Le contenu de ce cours est suceptible d'évoluer. Pour être sûr d'avoir la dernière version, il est recommandé de cloner le dépôt.

## Affichage du cours

Avec Docker l'affichage du cours est très simple. Il suffit de lancer la commande suivante:

```bash
$ docker run --rm \
  -v $(pwd)/presentation:/reveal/docs/slides \
  -e TITLE='COURS XSD IVTR' \
  -p 8000:8000 -p 35729:35729 \
  cloudogu/reveal.js:dev
```	

Ouvrez ensuite votre navigateur à l'adresse http://localhost:8000

## Travaux pratiques

L'exercice décrit dans le code est disponible (fichier `games.xml`)avec son corrigé (fichier `games.xsd`).

```bash
ls -l exercice/
total 8
-rw-r--r-- 1 robin robin  472 Oct  1 15:44 games.xml
-rw-r--r-- 1 robin robin 1989 Oct  1 15:27 games.xsd
```