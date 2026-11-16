[![Academic Year](https://img.shields.io/badge/Epitech-Tek1-blue?style=for-the-badge&logo=epitech&logoColor=white)](https://www.epitech.eu/)
[![Docker](https://img.shields.io/badge/Docker-Containers-blue.svg?style=for-the-badge&logo=docker)](https://www.docker.com/)
[![Docker Compose](https://img.shields.io/badge/Orchestration-Docker_Compose-blueviolet.svg?style=for-the-badge&logo=docker)](https://docs.docker.com/compose/)
[![Module](https://img.shields.io/badge/Module-B--DOP--200-informational?style=for-the-badge)](#)

# 🚢 Popeye — Architecture Microservices Conteneurisée avec Docker Compose

**Popeye** est un projet d'infrastructure et d'orchestration DevOps axé sur la conteneurisation d'une application multi-niveaux complète (application de vote en ligne). Il met en œuvre les bonnes pratiques Docker : Dockerfiles multi-étapes optimisés, réseaux de ponts isolés et persistance des données.

---

## 🏗️ Architecture des Microservices

L'application est orchestrée en 5 services communicants :

1. **`poll` (Front-End de Vote)** :
   - Application web en **Python / Flask** permettant aux utilisateurs de voter entre deux choix.
   - Exposée sur le port public de l'hôte : `http://localhost:5000`.
2. **`redis` (Broker de Messages)** :
   - File d'attente in-memory collectant et tamponnant les votes soumis en temps réel.
3. **`worker` (Consommateur d'Arrière-Plan)** :
   - Service d'arrière-plan en **Java / .NET** extrayant les votes de Redis pour les agréger et les insérer dans la base PostgreSQL.
4. **`db` (Base de Données Relationnelle)** :
   - Serveur **PostgreSQL** hébergeant le schéma relationnel et garantissant la persistance des suffrages dans un volume Docker dédié.
5. **`result` (Front-End des Résultats)** :
   - Application web en **Node.js / Express** avec interface interactive affichant le décompte des votes en direct.
   - Exposée sur le port : `http://localhost:5001`.

---

## 🌐 Réseaux & Isolation

- **`poll-tier`** : Isole la communication entre le service de vote `poll` et le cache `redis`.
- **`back-tier`** : Réseau privé reliant le `worker`, le cache `redis` et la base `db`.
- **`result-tier`** : Réseau reliant le service d'affichage `result` et la base `db`.

---

## 🚀 Démarrage de l'Infrastructure

```bash
# Construire les images et démarrer la stack en arrière-plan
docker compose up --build -d

# Vérifier l'état de santé de chaque conteneur
docker compose ps

# Consulter les journaux applicatifs en direct
docker compose logs -f

# Stopper et purger l'environnement (avec suppression des volumes)
docker compose down -v
```
