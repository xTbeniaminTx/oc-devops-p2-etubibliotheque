# EtuBibliothèque

Application de gestion des étudiants d'une bibliothèque.
Les agents de la bibliothèque créent leur compte, se connectent, puis gèrent la liste des étudiants (ajout, consultation, modification, suppression).

## Structure du dépôt

- [`back/`](back/README.md) : API REST Spring Boot et base MySQL
- [`front/`](front/README.md) : application Angular

## Stack technique

- **Back-end** : Java 21, Spring Boot 3.5, Spring Security, Spring Data JPA, MapStruct, Lombok
- **Base de données** : MySQL 9.7, lancée dans un conteneur Docker
- **Front-end** : Angular 19, Angular Material
- **Tests** : JUnit, Mockito, Testcontainers (back), Jest (front)

## Prérequis

- **JDK 21**
- **Docker** avec Docker Compose :
  - Linux : Docker Engine
  - Windows ou macOS : Docker Desktop, démarré avant de lancer le back
- **Node.js 22** et npm
- Maven n'est pas nécessaire : le projet fournit le Maven Wrapper (`mvnw`)

## Variables d'environnement

Les accès à la base sont lus dans un fichier `back/.env`.

1. Copier le modèle :

   ```
   cp back/.env.example back/.env
   ```

2. Remplacer les valeurs fictives :

   | Variable | Rôle | Exemple |
   |---|---|---|
   | `DB_USER` | utilisateur de l'application | `etudiant_app` |
   | `DB_PASSWORD` | mot de passe de cet utilisateur | à générer |
   | `DB_ROOT_PASSWORD` | mot de passe root de MySQL | à générer |
   | `DB_HOST` | hôte de la base | `localhost` |
   | `DB_PORT` | port de la base | `3306` |
   | `DB_NAME` | nom de la base | `etudiant_db` |

   Pour générer un mot de passe : `openssl rand -base64 24`

3. Sous Linux, protéger le fichier : `chmod 600 back/.env`

## Démarrage

Le back-end doit être démarré **avant** le front-end.

### 1. Back-end

Depuis le dossier `back/` (le fichier `.env` est lu dans le dossier courant) :

```
cd back
./mvnw spring-boot:run
```

Sous Windows : `mvnw.cmd spring-boot:run`

Spring Boot démarre lui-même le conteneur MySQL décrit dans `compose.yaml`, puis l'API sur le port **8080**.

Les données sont conservées dans le volume Docker `back_db_data`.

### 2. Front-end

Dans un second terminal :

```
cd front
npm install
npm run start
```

L'application est disponible sur http://localhost:4200.
Les appels `/api` sont redirigés vers le back-end sur le port 8080 (`proxy.conf.json`).

## Vérifier que tout fonctionne

- Back-end :

  ```
  curl http://localhost:8080/actuator/health
  ```

  Réponse attendue : `{"status":"UP"}`

- Front-end : ouvrir http://localhost:4200/register et créer un compte agent.

## Sécurité

- `back/.env` ne doit jamais être commité : il est listé dans `.gitignore`.
- `back/.env.example` ne contient que des valeurs fictives.
- Seul le point de supervision `/actuator/health` est exposé, sans authentification.
