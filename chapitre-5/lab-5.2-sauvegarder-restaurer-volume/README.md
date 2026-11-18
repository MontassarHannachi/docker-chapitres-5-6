# Lab 5.2 — Sauvegarder et restaurer un volume Docker

## 1. Objectifs

- Comprendre les sauvegardes des données PostgreSQL.
- Identifier une sauvegarde logique et une sauvegarde physique.
- Vérifier les données présentes dans le volume utilise par un conteneur.

## 2. Environnement

- Systeme : Ubuntu
- Conteneurisation : Docker
- Image PostgreSQL : postgres:16-alpine
- Base : cours
- Utilisateur : etudiant

## 3. Sauvegarde logique

Le fichier `~/sauvegardes/cours.sql` a ete identifie comme un export PostgreSQL produit avec pg_dump version 16.15.

L inspection a confirme la presence de la table notes et des enregistrements suivants :

| id | texte |
|---:|---|
| 1 | premiere note |
| 34 | deuxieme note |
| 35 | troisieme note |

Le fichier se termine par la mention PostgreSQL database dump complete.

## 4. Sauvegarde physique

L archive `~/sauvegardes/pgdata.tar.gz` a ete retrouvee. Sa taille verifiee est de 6 666 209 octets. Son contenu a ete inspecte avec `tar -tzf` et contient notamment les repertoires pg_wal, global et pg_logical ainsi que le fichier pg_hba.conf.

Cette inspection confirme la presence de fichiers de donnees PostgreSQL dans l archive.

## 5. Verification du volume

Le conteneur `db` utilise le volume Docker `pgdata-restaure`, monte dans `/var/lib/postgresql/data`.

La commande suivante a permis de verifier les donnees :

```bash
docker exec db psql -U etudiant -d cours -c "SELECT * FROM notes;"
```

Les trois enregistrements observes sont les memes que ceux presents dans le fichier SQL.

## 6. Limites de verification

Les verifications confirment l existence du dump SQL, de l archive physique et des donnees dans le volume actuellement utilise.

Elles ne permettent toutefois pas de prouver si le volume pgdata-restaure provient de l archive physique ou du fichier SQL. La methode exacte de restauration ne peut donc pas etre affirmee.

## 7. Conclusion

Ce laboratoire presente deux approches de sauvegarde PostgreSQL : l export logique avec pg_dump et l archivage physique des fichiers de donnees. Il montre egalement l importance de verifier le contenu des sauvegardes et les donnees disponibles apres restauration.
