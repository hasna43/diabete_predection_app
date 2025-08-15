# 🩺 Application de Prédiction du Diabète - FastAPI + PostgreSQL + ML

Ce projet est une application web permettant :
- L’authentification sécurisée des médecins
- La prédiction du diabète à partir de données médicales grâce à un modèle Machine Learning
- Une interface web simple pour la gestion des patients et la visualisation des résultats

---

## Structure du projet

project/
│
├── main.py                 
├── ml
    ├── model.pkl               
├── requirements.txt        
├── templates/
│   ├── login.html           
│   ├── register.html          
    ├── home.html 
    ├── base.html 
    ├── add_patient.html
    ├── patients.html  
└── detabase.py                  # CSS, JS, images

---

## 1. Prérequis

- **Python** ≥ 3.10
- **PostgreSQL** ≥ 14
- **pip** installé
- **virtualenv** (optionnel)

---

## 2. Installation

### 2.1 Cloner le projet
```bash
git clone https://github.com/votre-repo/diabete-fastapi.git
cd diabete-fastapi
```

### 2.2 Créer et activer un environnement virtuel
```bash
pythocn -m venv env
# Windows :
env\Scripts\activate
# macOS / Linux :
source env/bin/activate
```

### 2.3 Installer les dépendances
```bash
pip install -r requirements.txt
```


## 🗄 3. Configuration PostgreSQL

### 3.1 Lancer PostgreSQL
Windows :
```bash
net start postgresql-x64-14
```

Linux / macOS :
```bash
sudo service postgresql start
```

### 3.2 Créer la base de données et la table
```sql
psql -U postgres
CREATE DATABASE diabetoweb;
\c diabete


CREATE TABLE medecins (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE patients (
    id SERIAL PRIMARY KEY,
    doctorid INTEGER REFERENCES medecins(id),
    name TEXT,
    age INTEGER,
    sex TEXT,
    glucose FLOAT,
    bmi FLOAT,
    bloodpressure FLOAT,
    pedigree FLOAT,
    result INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE predictions (
    id SERIAL PRIMARY KEY,
    patientid INTEGER REFERENCES patients(id),
    result INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

---

## 4. Ajouter un médecin (compte utilisateur)

Hacher un mot de passe avec Python :
```python
import bcrypt
hashed = bcrypt.hashpw("monmotdepasse".encode(), bcrypt.gensalt())
print(hashed.decode())
```

Insérer dans PostgreSQL :
```sql
INSERT INTO medecins (username, password) 
VALUES ('admin', 'mot_de_passe_haché_ici');
```

---

## 5. Modèle Machine Learning

Le fichier `model.pkl` contient déjà un modèle entraîné.  

## 6. Lancer l’application

```bash
uvicorn main:app --reload
```

Accédez à :
```
http://127.0.0.1:8000
```

---

## 7. Notes importantes

- Modifier dans `main.py` :
```python
DATABASE_URL = "dbname=diabete user=postgres password=VOTRE_MOT_DE_PASSE host=localhost"
```
- PostgreSQL doit être démarré avant `uvicorn`.
- Si `psql` n’est pas reconnu, ajouter PostgreSQL au `PATH` de Windows.

---

## Licence
Projet libre d’utilisation et d’adaptation.
