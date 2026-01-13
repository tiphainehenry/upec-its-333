# TP – Architecture Microservices autour du mini-projet (4h)

## 1. Contexte et objectifs

Ce TP a pour objectif de transformer le **mini-projet** en une **architecture microservices**.

Au lieu de développer une application Flask monolithique, vous allez concevoir **plusieurs services indépendants**, chacun responsable d’une fonctionnalité métier précise.

Ce TP met l’accent sur :
- la décomposition fonctionnelle,
- l’indépendance des services,
- la communication inter-services via HTTP,
- la compréhension concrète de la notion de microservice (et non seulement d’API).

---

## 2. Compétences visées

À l’issue du TP, vous devez être capable de :

- Concevoir une architecture basée sur des microservices
- Implémenter plusieurs applications Flask indépendantes
- Associer une base de données spécifique à chaque service
- Mettre en place une communication synchrone entre services
- Expliquer les choix architecturaux réalisés

---

## 3. Architecture cible

L’architecture minimale attendue est composée de **deux microservices**.

### 3.1 Service Personne

**Responsabilité**  
Gestion de l’existence des personnes (identité, création, suppression).

- Port : `5001`
- Stockage : SQLite

### 3.2 Service Santé

**Responsabilité**  
Gestion des paramètres de santé d’une personne.

Attention, ce service ne crée pas de personnes et ne gère pas leur identité.

- Port : `5002`
- Stockage : JSON ou SQLite

### Règle fondamentale

Un microservice n’a **jamais le droit** d’accéder aux données d’un autre service autrement que par HTTP.

---

## 4. Structure du projet

```text
mini-projet/
├── person-service/
│   ├── app.py
│   └── database.db
├── health-service/
│   ├── app.py
│   └── data.json
```

---

## 5. Travail demandé

1. Implémenter le service Personne (CRUD minimal). Une personne contient au minimum : (id : entier, clé primaire;
name : chaîne de caractères)

API à implémenter:
* POST	/persons	Créer une personne
* GET	/persons/<id>	Récupérer une personne
* DELETE	/persons/<id>	Supprimer une personne

L’identifiant est généré par le service
Une personne inexistante doit retourner HTTP 404

2. Implémenter le service Santé
Les paramètres de santé peuvent inclure poids, taille, fréquence cardiaque, tension artérielle

* GET	/health/<person_id>	Lire les données de santé
* POST	/health/<person_id>	Ajouter des données
* PUT	/health/<person_id>	Modifier des données
* DELETE	/health/<person_id>	Supprimer les données


Avant toute opération sur les données de santé, le service Santé doit :
* Appeler le service Personne
* Vérifier que la personne existe
* Continuer uniquement si la réponse est 200 OK


3. Rajouter service d'authentification par token jwt 

4. lancer les services via docker

