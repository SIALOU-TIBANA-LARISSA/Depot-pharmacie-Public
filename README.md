# 💊 Dépôt Pharmacie

Application web de gestion interne d'un dépôt de pharmacie permettant de centraliser les principales opérations : gestion des produits, suivi des stocks et des lots, ventes en caisse, achats fournisseurs, gestion des utilisateurs, notifications et rapports.

L'application repose sur une architecture séparant un backend Django REST Framework et un frontend React/TypeScript. Elle intègre notamment la méthode **FEFO** (*First Expired, First Out*), qui privilégie les lots dont la date de péremption est la plus proche lors des ventes.

## 📸 Captures d'écran

Les captures d'écran présentant les différentes interfaces et fonctionnalités de l'application sont disponibles dans le dossier `captures-ecran/`, situé à la racine du projet.

## 🛠️ Technologies utilisées

### Backend

* Python
* Django 5.2
* Django REST Framework
* SimpleJWT pour l'authentification par jetons
* SQLite en développement
* PostgreSQL selon la configuration de `DATABASE_URL`
* WeasyPrint pour la génération des factures PDF

### Frontend

* React 19
* TypeScript
* Vite
* React Router
* CSS personnalisé
* Icônes SVG réalisées sur mesure, sans dépendance UI externe

## 🏗️ Architecture du projet

Le backend est organisé en plusieurs applications, chacune dédiée à une partie de la gestion du dépôt.

| Application     | Responsabilités                                              |
| --------------- | ------------------------------------------------------------ |
| `accounts`      | Gestion des utilisateurs, authentification, rôles et profils |
| `catalog`       | Gestion des catégories, fournisseurs, produits et lots       |
| `inventory`     | Suivi des mouvements et ajustements de stock                 |
| `customers`     | Gestion des clients                                          |
| `purchases`     | Gestion des achats et réceptions fournisseurs                |
| `sales`         | Gestion des ventes, prélèvement FEFO et factures PDF         |
| `notifications` | Gestion des alertes et notifications                         |
| `core`          | Paramètres généraux du dépôt                                 |
| `reports`       | Tableau de bord, statistiques et rapports                    |

## ✨ Fonctionnalités principales

### 1. Tableau de bord

* Consultation des principaux indicateurs d'activité.
* Suivi du chiffre d'affaires et du bénéfice.
* Nombre de ventes et suivi des achats.
* Affichage des produits les plus vendus.
* Consultation des alertes liées au stock et aux péremptions.

### 2. Gestion des produits

* Création, modification et consultation des produits.
* Organisation des produits par catégories.
* Association des fournisseurs.
* Suivi des prix d'achat et de vente.
* Importation des produits à partir d'un fichier Excel.

### 3. Gestion du stock et des lots

* Suivi des quantités disponibles.
* Gestion des lots et de leurs dates de péremption.
* Traçabilité des entrées, sorties et ajustements.
* Application de la méthode FEFO lors des ventes.
* Alertes de rupture de stock et de péremption.

### 4. Ventes et caisse

* Recherche des produits.
* Constitution d'un panier de vente.
* Association facultative d'un client.
* Application de remises.
* Prise en charge de plusieurs modes de paiement.
* Calcul de la monnaie à rendre pour les paiements en espèces.
* Déstockage automatique selon la méthode FEFO.
* Impression du ticket de vente.
* Génération de factures au format PDF.

### 5. Historique des ventes

* Consultation des ventes enregistrées.
* Recherche et filtres par période et par caissier.
* Consultation du détail des ventes.
* Téléchargement des factures PDF.
* Annulation des ventes par l'administrateur, avec réintégration du stock.

### 6. Achats fournisseurs

* Création de bons d'achat comportant plusieurs produits.
* Gestion des achats en brouillon.
* Réception totale ou partielle des commandes.
* Gestion des livraisons échelonnées.
* Aide à la saisie des prix avec alerte en cas d'écart par rapport au dernier prix connu.
* Annulation des achats avec gestion des mouvements de stock.
* Suppression des achats selon les règles prévues par l'application.

### 7. Gestion des clients

* Enregistrement des clients.
* Consultation des informations clients.
* Association facultative d'un client à une vente.

### 8. Gestion des utilisateurs et des rôles

* Création et gestion des comptes utilisateurs.
* Modification des profils.
* Ajout d'une photo de profil.
* Gestion des rôles et des permissions.
* Réinitialisation des mots de passe par l'administrateur.
* Désactivation des comptes.

### 9. Notifications et alertes

* Alertes de rupture de stock.
* Alertes de péremption.
* Notifications liées aux ventes et aux achats.
* Consultation et suppression des notifications.

### 10. Rapports et statistiques

* Suivi du chiffre d'affaires.
* Suivi du bénéfice.
* Analyse des ventes.
* Identification des produits les plus et les moins vendus.
* Rapports liés à l'activité et au stock.
* Export de données au format Excel.

## 🔐 Gestion des rôles et permissions

L'application distingue trois profils d'utilisateurs. Les autorisations sont contrôlées côté backend et prises en compte dans l'interface frontend.

| Rôle                  | Principaux accès                                                                              |
| --------------------- | --------------------------------------------------------------------------------------------- |
| Administrateur        | Accès complet à l'application, aux paramètres et à la gestion des utilisateurs                |
| Caissier              | Ventes, caisse, historique de ses ventes, consultation des produits, clients et notifications |
| Gestionnaire de stock | Produits, stock, achats et fournisseurs                                                       |

Les permissions sont définies selon les écrans et les opérations autorisées. Le contrôle est appliqué côté backend à travers les permissions Django REST Framework et côté frontend à travers `routes/permissions.ts`.

## ⚙️ Démarrage local

### Prérequis

* Python
* Node.js et npm
* Git
* Un navigateur web

### 1. Backend

Créer un environnement virtuel :

```bash
python -m venv venv
```

Activer l'environnement virtuel.

**Windows :**

```bash
venv\Scripts\activate
```

**macOS / Linux :**

```bash
source venv/bin/activate
```

Installer les dépendances :

```bash
pip install -r requirements.txt
```

Créer le fichier d'environnement :

```bash
cp .env.example .env
```

Sous Windows, il est également possible de copier manuellement `.env.example` et de renommer la copie en `.env`.

Appliquer les migrations :

```bash
python manage.py migrate
```

Créer un compte administrateur :

```bash
python manage.py createsuperuser
```

Démarrer le serveur backend :

```bash
python manage.py runserver
```

Le backend est accessible par défaut à l'adresse :

`http://localhost:8000/`

L'API est accessible sous :

`http://localhost:8000/api/`

En l'absence de `DATABASE_URL`, le projet utilise SQLite par défaut.

### 2. Frontend

Dans un second terminal :

```bash
cd frontend
```

Installer les dépendances :

```bash
npm install
```

Créer le fichier d'environnement :

```bash
cp .env.example .env
```

Configurer notamment `VITE_API_BASE_URL` pour indiquer l'adresse du backend.

Démarrer le frontend :

```bash
npm run dev
```

L'interface est accessible par défaut à l'adresse :

`http://localhost:5173`

Le backend et le frontend doivent fonctionner simultanément pour permettre à l'application de communiquer avec les données.

## 👥 Profils utilisateurs

Après la création d'un compte administrateur ou avec des comptes déjà présents dans une base de démonstration, il est possible de se connecter depuis la page `/connexion`.

Pour vérifier le fonctionnement des permissions, utiliser un compte correspondant à chaque rôle :

* **Administrateur :** accès à l'ensemble des fonctionnalités.
* **Caissier :** accès aux opérations de vente et aux fonctionnalités autorisées pour la caisse.
* **Gestionnaire de stock :** accès à la gestion des produits, du stock, des achats et des fournisseurs.

L'administrateur peut gérer les comptes depuis la page « Utilisateurs ».

## 🧪 Tests

Les tests backend peuvent être exécutés avec la commande :

```bash
python manage.py test
```

Sous Windows, certains tests liés à la génération de PDF avec WeasyPrint peuvent être ignorés (`skipped`) lorsque les bibliothèques natives nécessaires ne sont pas installées.

WeasyPrint nécessite notamment des bibliothèques telles que Pango, Cairo et GObject.

## 🎯 Objectif du projet

Dépôt Pharmacie est une application destinée à faciliter la gestion quotidienne d'un dépôt de pharmacie, à améliorer le suivi des stocks et à centraliser les opérations de vente et d'achat.

Ce projet met en œuvre plusieurs compétences en développement web : conception d'une API REST, développement d'une interface React, gestion d'une base de données, authentification, permissions par rôle et génération de documents PDF.
