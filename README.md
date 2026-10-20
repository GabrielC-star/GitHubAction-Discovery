# TP DevOps Correction Docker

TP GitHubAction

What is it supposed to do?

Ce projet présente une API Java Spring Boot pour gérer des étudiants et des départements. Docker Compose lance l’API, une base de données PostgreSQL et un serveur HTTP Apache. GitHub Actions construit l’API et exécute ses tests lors des push et des pull requests vers `main`.

## 2-1 What are Testcontainers?

Testcontainers est une bibliothèque Java qui démarre des conteneurs Docker temporaires pour les tests. Elle permet de tester l’application avec de vrais services, comme PostgreSQL, puis supprime les conteneurs à la fin des tests.

## 2-2 For what purpose do we need to use secured variables in CD?

Les variables sécurisées servent à stocker des informations sensibles nécessaires au déploiement, comme les identifiants Docker Hub. Elles évitent d’écrire ces informations dans le code ou de les rendre visibles dans le dépôt. Dans GitHub, on les ajoute dans **Settings → Secrets and variables → Actions**, puis on les utilise dans le workflow avec `${{ secrets.NOM_DU_SECRET }}`.

Pour Docker Hub, il est préférable de créer un jeton d’accès et de l’enregistrer dans un secret nommé `DOCKERHUB_TOKEN`, avec le nom d’utilisateur dans `DOCKERHUB_USERNAME`.

## 2-3 Why did we put needs: build-and-test-backend on this job? Maybe try without this and you will see!

`needs` impose un ordre entre les jobs : la construction et la publication des images attendent la réussite des tests. Sans cette dépendance, les deux jobs peuvent s’exécuter en même temps. Si les tests échouent, les images ne sont pas publiées. La valeur de `needs` doit correspondre exactement au nom du job de tests ; dans ce workflow, ce nom est `test-backend`.

## 2-4 For what purpose do we need to push docker images?

Publier les images sur Docker Hub permet de les stocker et de les partager. Un serveur peut ensuite les télécharger pour déployer l’application, sans avoir à reconstruire les images à partir du code source.
