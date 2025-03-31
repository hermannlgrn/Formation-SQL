# TP9

# Les données fornies 

### la table article
| id | nom | prix | 
|----|---|---|
| 1 | PlayStation 5 | 400.00 |
| 2 | X box | 350.00 |
| 3 | Machine à café | 300.00 |
| 4 | PlayStation 3 | 100.00 |

### la table client
| id | nom | prenom | 
|----|---|---|
| 1 | Brad | PITT |
| 2 | George | Cloney |
| 3 | Jean | DUJARDIN |


# Ce que je dois faire : 

:one: Créer la base de données   
:two: Créer les tables  
:three: Ajouter les données  
:four: Afficher la commande de Brad PITT  
|prenom|nom|date_achat|nom|prix|nb|total|
|---|---|---|---|---|---|---|
|PITT|Brad|2024-09-08 10:15:00|X box|350|1|350|
|PITT|Brad|2024-09-08 10:15:00|Machine à café|300|1|300|
|PITT|Brad|2024-09-08 10:15:00|PlayStation 3|100|2|200|

# Travail

## Code pour répondre aux questions :one: Créer la base de données, :two: Créer les tables, :three: Ajouter les données

```sql
DROP DATABASE IF EXISTS e_commerce;
CREATE DATABASE e_commerce CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE e_commerce;

CREATE TABLE client (
  id INT AUTO_INCREMENT,
  nom VARCHAR(100) NOT NULL,
  prenom VARCHAR(100) DEFAULT NULL,
  PRIMARY KEY (id)
) ENGINE=INNODB;

CREATE TABLE commande (
  id INT AUTO_INCREMENT,
  date_achat DATETIME NOT NULL,
  client_id INT NOT NULL,
  PRIMARY KEY (id),
  CONSTRAINT fk_client_commande FOREIGN KEY (client_id) REFERENCES client(id)
) ENGINE=INNODB;

CREATE TABLE article (
  id INT AUTO_INCREMENT,
  nom VARCHAR(100) NOT NULL,
  prix FLOAT NOT NULL,
  PRIMARY KEY (id)
) ENGINE=INNODB;

CREATE TABLE ligne (
  article_id INT NOT NULL,
  commande_id INT NOT NULL,
  nombre INT NOT NULL,
  prix FLOAT NOT NULL,
  PRIMARY KEY (article_id, commande_id),
  CONSTRAINT fk_ligne_article FOREIGN KEY (article_id) REFERENCES article(id),
  CONSTRAINT fk_ligne_commande FOREIGN KEY (commande_id) REFERENCES commande(id)
) ENGINE=INNODB;

-- Insertion des articles
INSERT INTO article (nom, prix) VALUES
('PlayStation 5', 500.00),
('Xbox', 350.00),
('Machine à café', 400.00),
('PlayStation 3', 100.00);

-- Insertion des clients
INSERT INTO client (prenom, nom) VALUES
('Brad', 'PITT'),
('George', 'CLOONEY'),
('Jean', 'DUJARDIN');

-- Insertion des commandes
INSERT INTO commande (date_achat, client_id) VALUES ('2024-09-08 10:15:00', 1);

-- Insertion des lignes de commande
INSERT INTO ligne (article_id, commande_id, nombre, prix) VALUES
(4, 1, 2, 100.00),
(3, 1, 1, 300.00),
(2, 1, 1, 350.00);
```

## Code pour répondre a la question :four: Afficher la commande de Brad PITT

```sql
USE e_commerce;
SELECT 
client.prenom AS prenom,
client.nom AS nom,
commande.date_achat as date_achat,
article.nom AS nom,
ligne.prix AS prix,
ligne.nombre AS nb,
ligne.prix*ligne.nombre AS total
FROM
commande
INNER JOIN ligne ON commande.id = ligne.commande_id
INNER JOIN article ON article_id= article.id
INNER JOIN client ON client.id = commande.client_id
WHERE commande_id=1;
```

## Code pour afficher le total

```sql
USE e_commerce;
-- total
SELECT 
 SUM(ligne.prix*ligne.nombre) AS total_ht,
 SUM(ligne.prix*ligne.nombre*0.2) AS total_tva,
 SUM(ligne.prix*ligne.nombre*1.2) AS total_ttc
FROM
commande
INNER JOIN ligne ON commande.id = ligne.commande_id
WHERE commande_id=1;
```