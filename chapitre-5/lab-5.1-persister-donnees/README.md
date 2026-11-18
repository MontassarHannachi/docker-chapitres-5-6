# Lab 5.1 — Persister des données avec Docker

## 1. Objectifs

Ce laboratoire a pour objectif de comprendre la persistance des données dans Docker et le rôle des volumes.

- Observer la perte des données après la suppression d'un conteneur.
- Utiliser un volume nommé pour conserver les données.
- Comprendre l'indépendance entre les données et le cycle de vie du conteneur.

## 2. Environnement technique

- Système : Ubuntu
- Moteur : Docker
- Image : `postgres:16-alpine`
- SGBD : PostgreSQL
- Utilisateur : `etudiant`
- Base de données : `cours`

## 3. Expérience 1 — Sans volume nommé

Un conteneur PostgreSQL a été lancé sans volume nommé. Une table `notes` a été créée et une première donnée a été insérée.

| id | texte |
|---:|---|
| 1 | premiere note |

Après suppression puis recréation du conteneur, la requête sur la table `notes` a retourné :

```text
ERROR: relation "notes" does not exist
**Observation :** les données stockées dans le système de fichiers du conteneur ont été perdues après sa suppression.

## 4. Expérience 2 — Avec un volume nommé

Un volume nommé `pgdata` a été utilisé lors du TP pour stocker les données PostgreSQL dans `/var/lib/postgresql/data`.

Après suppression et recréation du conteneur en réutilisant ce volume, la donnée précédemment insérée était toujours disponible :

| id | texte |
|---:|---|
| 1 | premiere note |

**Observation :** le volume nommé conserve les données indépendamment du cycle de vie du conteneur.

## 5. Comparaison

| Sans volume nommé | Avec volume nommé |
|---|---|
| Données liées au conteneur | Données indépendantes du conteneur |
| Suppression du conteneur : perte des données | Suppression du conteneur : données conservées dans le volume |
| Persistance non garantie | Persistance assurée par le volume |

## 6. Conclusion

Ce TP montre l'importance des volumes Docker pour les applications qui doivent conserver leurs données, notamment les bases de données.

Un volume nommé permet de séparer les données du cycle de vie des conteneurs et facilite leur conservation lors des opérations de remplacement ou de recréation.
