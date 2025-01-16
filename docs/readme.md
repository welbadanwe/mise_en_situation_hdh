# Projet OMOP - Transformation des données de santé

## Contexte du projet

Dans ce projet, la mission était de transformer des données de santé hétérogènes en un format standardisé à l'aide du modèle "OMOP", afin de standardiser les données de santé et ainsi assurer leur cohérence et leur compatibilité avec des outils d'analyse.

### Données fournies

Le projet repose sur plusieurs fichiers de données synthétiques du SNDS :
- Les données des assurés (**ir_ben_r.csv**)
- Les prestations remboursées (**er_prs_f.csv**)
- Les établissements de santé (**t_mcoaae.csv**)
- Les professionnels de santé (**ir_act_v.csv**, **ir_spe_v.csv**)

### Objectif

L’objectif est de remplir les tables suivantes selon le modèle OMOP :
1. "Person" (avec Python - pandas)
2. "Care Site" (avec SQL - SQLite)
3. "Provider" (avec Apache Spark)

## Étapes du projet

### 1. Table "Person" sur Python, en utilisant uniquement Pandas
- **But** : Créer une table des personnes avec les colonnes suivantes : `person_id`, `gender_concept_id`, `year_of_birth`, `month_of_birth`, `person_source_value`, `location_id`, `gender_source_value`.
- **Méthode** :
  - Les données des assurés ont été chargées à partir du fichier `ir_ben_r.csv`.
  - Un identifiant unique pour chaque personne a été créé et des colonnes ont été ajoutées pour les informations demandées (sexe, date de naissance, localisation).
  - Les données ont été exportées dans un fichier CSV `omop_person.csv`.

### 2. Table "Care Site" (SQL - SQLite)
- **But** : Créer une table des établissements de santé avec les colonnes suivantes : `cc_site_id`, `care_site_name`, `location_id`, `care_site_source_value`.
- **Méthode** :
  - Les données des établissements ont été extraites du fichier `t_mcoaae.csv`.
  - Les prestations remboursées ont été fusionnées avec les établissements pour ajouter les informations de localisation.
  - La table a été créée et remplie dans une base de données SQLite appelée `omop.db`.

### 3. Table "Provider" (Apache Spark)
- **But** : Créer une table des fournisseurs de soins avec les colonnes suivantes : `provider_id`, `specialty_source_value`, `specialty_concept_id`, `provider_source_value`.
- **Méthode** :
  - Les données des spécialités et des actes ont été extraites respectivement des fichiers `ir_spe_v.csv` et `ir_act_v.csv`.
  - Un identifiant unique `provider_id` a été généré pour chaque fournisseur.
  - La table a été enregistrée au format "Parquet" sous le nom `provider_table.parquet`.

## Exécution du projet

### Prérequis
- **Python 3.x**
- **Pandas** : Pour la manipulation des données et la génération de fichiers CSV.
- **Apache Spark** : Pour le traitement des données volumineuses et la génération de la table "Provider".
- **SQLite** : Pour la gestion de la base de données des établissements de santé.

### Instructions d'exécution

1. **Table Person** :
   - Exécuter le script `Script omop_person.ipynb` dans un environnement Python avec "pandas" installé.
   - Le fichier de sortie `omop_person.csv` sera généré.

2. **Table Care Site** :
   - Exécuter le script `Script table_care_site.ipynb` dans un environnement Python avec "SQLite" installé.
   - Le fichier SQLite `omop.db` sera créé avec la table "care_site".

3. **Table Provider** :
   - Exécuter le script `Script table_provider.ipynb` dans un environnement avec "Apache Spark" installé.
   - Le fichier "Parquet" `provider_table.parquet` sera généré.

## Remarques
- Le script pour la table "Care Site" utilise une valeur par défaut (0) pour "location_id" lorsqu'aucune information de localisation n'est disponible pour les établissements.
- Pour la table "Provider", la colonne "specialty_concept_id" est remplie avec la valeur 0, car une difficulté à été rencontrée au remplissage de cette dernière
- Un dossier documents utilisé permets de stocker les datas qui ont été utilisées
- Un fichier reponses aux questions existe égalemennt pour repondre aux questions posées dans le document de mise en situation du HDH, et est dans le dossier /docs
