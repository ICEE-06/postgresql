**Noms et prénoms des binômes**:
- **LAHINIRIKO Mara Sylvain** L2 ARSB 247/LA/24-25
-  **MBOLANIRINA Stephano** Kevin L2 ARSB 258/LA/24-25 


## Partie1: Normalisation Relationnelle

### 1- Deux anomalies possible dans la table:
- **Anomalie d'insertion**: la colonnes `produits_commandés` contient plusieurs produits dans un seule cellule.
- **Anomalie de suppression**: si on supprime une commande de la table, les informations sur les clients sont aussi perdues. 



## Partie2: Requêtes SQL avancé

### 1- Requêtes SQL qui affiche le noms des clients ayant loué plus d'un vehicule

```
SELECT c.nom
FROM client c
JOIN location l ON c.id_client = l.id_client
GROUP BY c.id_client, c.nom
HAVING COUNT(DISTINCT l.id_vehicule) > 1;
```

![[Pasted image 20250519143411.png]]

![[Pasted image 20250519143438.png]]![[Pasted image 20250519143533.png]]

```
gestion_location=# SELECT c.nom FROM client c JOIN LOCATION l ON c.id_client=l.id_client GROUP BY  c.id_client, c.nom HAVING COUNT (DISTINCT l.id_vehicule) > 1;
   nom   
---------
 Sylvain
(1 ligne)
```

### 2-Afficher pour chaque véhicule loué, la durée total en jours

```
gestion_location=# SELECT v.id_vehicule, v.marque, v.modele, SUM(date_fin-date_debut) AS duree_total_jour FROM location l JOIN vehicule v ON l.id_vehicule=v.id_vehicule GROUP BY v.id_vehicule, v.marque, v.modele;
 id_vehicule | marque | modele | duree_total_jour 
-------------+--------+--------+------------------
           4 | toyota | V8     |               19
           2 | Ford   | Raptor |               31
           1 | Lambo  | A546   |               49
           3 | Audi   | GT60   |               19

```

### 3- Transaction SQL qui fait les opérations dites dans le sujet:

```
BEGIN;

INSERT INTO location (id_client, id_vehicule, date_debut, date_fin, montant)
VALUES (2, 4, '2025-06-01', '2025-06-10', 300000);

UPDATE vehicule
SET disponible = FALSE
WHERE id_vehicule = 4;

COMMIT;

```

![[Pasted image 20250519145614.png]]