# Progression Bloc 2 — Conception et développement d'applications (SLAM)

> BTS2 SIO SLAM — Semestre 2 — Février à Mai 2026
> Cours : **Lundi matin** et **Mardi après-midi**
> Projet SP : **BiblioTech** — Gestion de bibliothèque (Laravel 12 / PHP 8.3 / SQLite)

## Compétences du Bloc 2

| Réf. | Compétence |
|------|-----------|
| A2.1 | Concevoir et développer une solution applicative |
| A2.2 | Assurer la maintenance corrective ou évolutive d'une solution applicative |
| A2.3 | Gérer les données |

## Rappel : acquis du Semestre 1 (Séances SP 1 à 3)

| Séance SP | Thème | Acquis | Validation |
|-----------|-------|--------|------------|
| **S1** | Fondations Laravel & MVC | Routes (`web.php`), Contrôleurs, Blade (`@extends`, `@section`, `@foreach`), architecture MVC, `php artisan serve` | ≥ 3 routes, flux URL → Contrôleur → Vue compris |
| **S2** | BDD SQLite & Eloquent | Migrations (`php artisan migrate`), Modèles Eloquent, Relations (`belongsTo`/`hasMany`), Seeders, Scopes (`disponible()`, `recherche()`) | Base SQLite fonctionnelle, données dynamiques, relations OK |
| **S3** | CRUD & Vues avancées | Contrôleurs Resource (7 actions), Route Model Binding, Validation (`required\|min:3`), Messages flash, Composants Blade, Bootstrap 5 | CRUD complet, validation serveur, interface responsive |

**Seuil de passage** : ≥ 12/20 à chaque évaluation de séance pour progresser.

---

## Phase 1 — SP Laravel BiblioTech (février → 1er avril)

### S9 — Mardi 24 février
**Retour de stage — Reprise BiblioTech (rappel S1-S3)**
- Bilan technique du stage
- Réinstallation / vérification environnement : PHP 8.3, SQLite, Git, `php artisan serve`
- Reprise des acquis S1-S3 :
  - Routes et contrôleurs (MVC) → `routes/web.php`
  - Eloquent : `Livre::all()`, `Livre::find($id)`, relations `$livre->categorie->nom`
  - CRUD : les 7 actions Resource (index, create, store, show, edit, update, destroy)
  - Validation : `'titre' => 'required|min:3|max:255'`
- Vérification : l'appli tourne, les données s'affichent depuis SQLite, le CRUD fonctionne
- Point Git : état du dépôt GitHub, branches, historique des commits

**Compétences :** B2.1.4, B2.1.5, B2.1.6, B2.3

---

### S10 — Lundi 2 mars
**Séance SP 4 — Authentification & Autorisations (partie 1)**
- **Authentification vs Autorisation** : vérifier l'identité ≠ vérifier les permissions
- Système d'authentification Laravel : Login / Register / Logout
- **Sessions utilisateur** : token de session → cookie → `Auth::user()`
- **Hashage des mots de passe** : bcrypt, `Hash::check()` — JAMAIS de mot de passe en clair
- **Protection CSRF** : directive `@csrf` dans chaque formulaire
- Formulaires sécurisés d'inscription et de connexion

**Compétences :** B2.1.6, S4.2 (Sécurité informatique)

### S10 — Mardi 3 mars
**Séance SP 4 — Authentification & Autorisations (partie 2)**
- **Middleware `auth`** : protection des routes — redirection si non connecté
- **Système de rôles** : 3 niveaux Admin / Bibliothécaire / Utilisateur
  - Admin : accès total (gestion utilisateurs, configuration)
  - Bibliothécaire : gestion livres et emprunts
  - Utilisateur : consultation catalogue, emprunt
- Affichage conditionnel Blade selon le rôle (`@auth`, `@guest`, conditions sur rôle)
- Fonctionnalité « Se souvenir de moi »
- Déconnexion sécurisée
- **Évaluation Séance 4** (validation ≥ 12/20)

**Compétences :** C2.2, C2.3, C4.1, S2.1, S2.2, S4.2

---

### S11 — Lundi 9 mars
**Séance SP 5 — Tests automatisés (partie 1)**
- **Pourquoi tester ?** Le filet de sécurité du développeur
- **Tests unitaires** (`tests/Unit/`) : tester un modèle, une méthode isolée
  - Ex : `Livre` calcule correctement si un livre est disponible
- **Tests fonctionnels** (`tests/Feature/`) : tester une route de bout en bout
  - Ex : GET `/livres` → assertStatus(200), assertSee('Livres')
- **Assertions principales** :
  - `assertStatus(200)`, `assertSee('texte')`, `assertDatabaseHas(...)`, `assertRedirect(...)`
- **Trait `RefreshDatabase`** : base remise à zéro avant chaque test
- Objectif : couverture > 70% (`php artisan test --coverage`)

**Compétences :** B2.1.9 (Tests), A2.2

### S11 — Mardi 10 mars
**Séance SP 5 — CI/CD GitHub Actions & SonarCloud (partie 2)**
- **CI/CD** : Continuous Integration / Continuous Deployment
- **Fichier `.github/workflows/ci.yml`** : pipeline automatisé
  - Étapes : install PHP 8.3 → Composer → `.env` → migrations SQLite → tests
  - Badge vert/rouge sur le README
- **SonarCloud** : analyse qualité de code (gratuit pour l'open-source)
  - 4 types de problèmes : Bugs, Vulnérabilités, Code Smells, Dette technique
  - **Quality Gate** : verdict final Passed ✅ / Failed ❌
  - Fichier `sonar-project.properties` + secret `SONAR_TOKEN`
- Captures d'écran dashboard SonarCloud pour le dossier E6
- Badges GitHub Actions + SonarCloud sur le README

**Compétences :** B2.1.8 (CI/CD), A2.2, A1.4 (Travailler en mode projet)

---

### S12 — Lundi 16 mars
**Séance SP 5 — Déploiement production & Monitoring (partie 3)**
- **Configuration production** :
  - `APP_DEBUG=false` (obligatoire — messages d'erreur détaillés = faille de sécurité)
  - HTTPS obligatoire (Let's Encrypt)
- **Optimisations Laravel** :
  - `php artisan config:cache`, `route:cache`, `view:cache`
  - `composer install --optimize-autoloader --no-dev`
- **Options de déploiement** : hébergement mutualisé, VPS, Heroku
- **Processus** : clone → Composer → `.env` prod → migrations → optimisations
- **Monitoring** :
  - Logs Laravel (`storage/logs/laravel.log`)
  - Health Check (`/health/deep`) : vérification BDD, cache, disque
  - Sentry (error tracking, alertes)
- **Rollback** : tags Git (v1.0, v1.1…), retour à version stable

**Compétences :** A2.1, A2.2, S2.2 (Conception et développement), A1.5

### S12 — Mardi 17 mars
**Documentation & Préparation E5/E6**
- **Documentation technique** :
  - Architecture MVC BiblioTech
  - Schéma BDD (MCD/MLD) : Livres, Catégories, Utilisateurs, Emprunts
  - Choix technologiques argumentés (Laravel, SQLite, GitHub Actions, SonarCloud)
- **Documentation utilisateur** : guide d'utilisation BiblioTech
- **Fiches descriptives** de réalisation professionnelle
- Captures d'écran : dashboard SonarCloud, pipeline GitHub Actions, app déployée
- Complétion du **tableau de synthèse E5**
- Vérification couverture compétences A2.1, A2.2, A2.3

**Compétences :** B2.1.10, A2.1–A2.3, A1.6

---

### S13 — Lundi 23 mars
**Entraînement blanc E6 (partie 1)**
- Simulation conditions réelles CCF :
  - **Préparation sur table** (30 min) : analyse expression des besoins
  - **Entretien E1** (20 min) : explicitation de la réalisation BiblioTech
- Débriefing collectif : points forts, axes d'amélioration
- Points de vigilance : justification des choix (pourquoi Laravel ? pourquoi SQLite ? pourquoi SonarCloud ?)

**Compétences :** A2.1–A2.3

### S13 — Mardi 24 mars
**Entraînement blanc E6 (partie 2)**
- Simulation conditions réelles CCF :
  - **Réalisation en environnement technologique** (60 min) : développement sur BiblioTech
  - **Entretien E2 — Recette** (20 min) : démonstration, validation, tests
- Débriefing collectif
- Derniers ajustements avant le CCF

**Compétences :** A2.1–A2.3

---

### 🔴 S14 — Lundi 30 mars → Mercredi 1er avril — CCF E6 SLAM (3 jours)

**Déroulement de l'épreuve E6 SLAM :**

| Phase | Durée | Contenu |
|-------|-------|---------|
| Préparation sur table | 30 min | Analyse de l'expression des besoins |
| Entretien 1 (E1) | 20 min | Explicitation de la réalisation |
| Réalisation en environnement technologique | 60 min | Production sur l'environnement du candidat |
| Entretien 2 (E2) | 20 min | Recette de la solution |

---

## Phase 2 — Révisions EDC SLAM (après CCF, avril–mai)

### 🌸 S15-S16 — 4 au 19 avril : VACANCES DE PRINTEMPS

### S17 — Lundi 20 avril
Révisions EDC SLAM — Algorithmique, structures de données, BDD (SQL, normalisation)

### S17 — Mardi 21 avril
Révisions EDC SLAM — Cybersécurité (RGPD, OWASP), annales E7

### S18 — Lundi 27 avril
Révisions EDC SLAM — Annales chronométrées

### S18 — Mardi 28 avril
Révisions EDC SLAM — Annales chronométrées

### S19 — Lundi 4 mai
Révisions EDC SLAM — Dernières annales

### S19 — Mardi 5 mai
Révisions EDC SLAM — Dernières annales E7

### S20 — Lundi 11 mai
Dernière ligne droite — Questions/réponses

### S20 — Mardi 12 mai
Dernières révisions EDC SLAM

### 🔴 S21 — Épreuves écrites
- **Lundi 18 mai** : E3 Maths info (16h–18h)
- **Jeudi 21 mai** : E7 Cybersécurité (14h–18h)

---

## Récapitulatif SP BiblioTech — 5 séances

| Séance | Semestre | Thème | Concepts clés | Livrable |
|--------|----------|-------|---------------|----------|
| **S1** | S1 | Fondations Laravel & MVC | Routes, Contrôleurs, Blade | App avec 3+ routes fonctionnelles |
| **S2** | S1 | BDD SQLite & Eloquent | Migrations, Modèles, Relations, Seeders, Scopes | Base SQLite dynamique |
| **S3** | S1 | CRUD & Vues avancées | Contrôleurs Resource, Validation, Flash, Composants Blade | CRUD complet + interface responsive |
| **S4** | S2 | Auth & Autorisations | Login/Register, Middleware, Rôles, CSRF, bcrypt | Système auth avec 3 niveaux |
| **S5** | S2 | Production & Déploiement | Tests auto, CI/CD GitHub Actions, SonarCloud, Deploy, Monitoring | App testée, analysée, déployée |