
## Concepts de bases:

**PostgreSql** utilise un modèle client/serveur:
• Un processus serveur, qui gère les fichiers de la base de données, accepte les connexions à la base de la part des applications clientes et effectue sur la base les actions des clients. Le programme serveur est appelé postgres. 

• L'application cliente (l'application de l'utilisateur), qui veut effectuer des opérations sur la base de données. Les applications clientes peuvent être de natures très différentes : un client peut être un outil texte, une application graphique, un serveur web qui accède à la base de données pour afficher des pages web ou un outil spécialisé dans la maintenance de bases de données. Certaines applications clientes sont fournies avec PostgreSQL ; la plupart sont développées par les utilisateurs.

## Connexion au serveur postgreSQL

- **Accéder au client `psql`**
    `sudo -u postgres psql `
- **Création de l'utilisateur:**
    `CREATE USER {nom_user} WITH PASSWORD {PASSWORD};`
- **Création de la base données**:
    `CREATE DATABASE {nom_de_la_base} OWNER {nom_user};`
- **Connéxion à la base:**
    `\c {nom_de_la_base}`
- **Création d'une table:**
```
CREATE TABLE utilisateurs (
    id SERIAL PRIMARY KEY,
    nom VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    mdp TEXT
);
```

## Manipulation d'une table:

- **Ajouts de colonnes:**
    `INSERT INTO {nom_de_la_table} (nom, email,mdp) VALUES ('donnée1', 'donnée2', 'donnée3'); `
- **Effacer une ligne:**
    `DELETE FROM {nom_de_la_table} WHERE id=1;`
- **Mise à jour de la valeur d'une colonne:**
    `UPDATE {nom_de_la_table} SET {nom_colonne} = 'nouvelle valeur' WHERE {condition};` 
- **Entrer une clé étrangère dans une table:**
    `{nom_de_la_colonnes} {types de la donnée} REFERENCES {nom_de_la_table_reference(clé primaire)};`
- **Effacer une colonne d'une table**:
	`ALTER TABLE {nom_de_table} DROP COLUMN {nom_colonne};`
- **Ajout d'une nouvelle colonne**:
	`ALTER TABLE {nom_de_table} ADD COLUMN {nom_colonne} {type_donnee};`
- **Afficher une colonne de la table avec une colonne d'une table reliée à elle grâce à une clé étrangère**:
	`SELECT {nom_table.nom_collone_à_afficher}, {nom_table_reliée.nom_collone_à_afficher} AS {nom_colonne_affiché} FROM {nom_table} JOIN {nom_table_reliée} ON {nom_table.clé_étrangère = {nom_table_reliée.clé_primaire};`
	
	**exemple**: SELECT livres.titre, auteurs.nom AS auteur FROM livresJOIN auteurs ON livres.auteur_id = auteurs.id;


## Jointure

```
SELECT utilisateurs.nom, commandes.produit
FROM utilisateurs
INNER JOIN commandes
ON utilisateurs.id = commandes.utilisateur_id;
```




SELECT c.nom
FROM client c
JOIN location l ON c.id_client = l.id_client
GROUP BY c.id_client, c.nom
HAVING COUNT(DISTINCT l.id_vehicule) > 1;


SELECT 
    v.id_vehicule,
    v.marque,
    v.modele,
    SUM(date_fin - date_debut) AS duree_totale_en_jours
FROM 
    location l
JOIN 
    vehicule v ON l.id_vehicule = v.id_vehicule
GROUP BY 
    v.id_vehicule, v.marque, v.modele;



BEGIN;

-- 1. Insertion de la location
INSERT INTO location (id_client, id_vehicule, date_debut, date_fin, montant)
VALUES (2, 4, '2025-06-01', '2025-06-10', 300000);

-- 2. Mise à jour de la disponibilité du véhicule
UPDATE vehicule
SET disponible = FALSE
WHERE id_vehicule = 4;

COMMIT;
