# 🌾 Système d'Irrigation Intelligente

Application web basée sur une architecture microservices pour la gestion automatisée de l'irrigation agricole.

---

## 📖 Description du Projet

Le **Système d'Irrigation Intelligente** optimise l'utilisation de l'eau en agriculture grâce à l'analyse de données environnementales collectées par des capteurs IoT.

### Objectifs

- Collecte automatisée des données environnementales (humidité du sol, température, pluviométrie)
- Analyse en temps réel des mesures des capteurs
- Génération automatique de recommandations d'irrigation personnalisées
- Optimisation de la consommation d'eau basée sur des données réelles

### Architecture

L'application est composée de 7 services :

**Infrastructure :**
- **Config Server** (8888) - Configuration centralisée
- **Eureka Server** (8761) - Service Discovery
- **API Gateway** (8080) - Point d'entrée unique
- **Kafka + Zookeeper** (9092, 2181) - Communication asynchrone

**Microservices :**
- **Microservice Collecte** (8081) - Gestion capteurs et observations
- **Microservice Analyse** (8082) - Génération des recommandations

**Frontend :**
- **Angular** (4200) - Interface utilisateur web

---

## 🛠️ Technologies Utilisées

### Backend
- **Java 21** - Langage de programmation
- **Spring Boot 3.2** - Framework backend
- **Spring Cloud** - Microservices (Config, Eureka, Gateway, OpenFeign)
- **Apache Kafka 7.5** - Message Broker / Event Streaming
- **Spring Data JPA** - Accès base de données
- **H2 Database** - Base de données en-mémoire
- **Maven** - Gestion dépendances et build

### Frontend
- **Angular 17** - Framework frontend
- **TypeScript** - Langage
- **RxJS** - Programmation réactive
- **Nginx** - Serveur web (production)

### DevOps
- **Docker** - Conteneurisation
- **Docker Compose** - Orchestration multi-conteneurs

---

## � Installation et Exécution

### Prérequis

- **Docker Desktop** (version 20.10+)
- **Docker Compose** (version 2.0+)
- **8 GB RAM minimum**

### Démarrage

**Windows :**
```powershell
git clone <url-du-repo>
cd projet_micro
.\build-all.ps1
docker-compose up -d
```

**Linux/macOS :**
```bash
git clone <url-du-repo>
cd projet_micro
chmod +x build-all.sh
./build-all.sh
docker-compose up -d
```

### Vérification

```bash
# Vérifier le statut
docker-compose ps

# Consulter les logs
docker-compose logs -f
```

### Accès aux Services

| Service | URL |
|---------|-----|
| **Application** | http://localhost:4200 |
| **Eureka Dashboard** | http://localhost:8761 |
| **API Gateway** | http://localhost:8080 |

**⏱️ Temps de démarrage : 2-3 minutes**

### Arrêt

```bash
docker-compose stop      # Arrêter
docker-compose down      # Arrêter et supprimer
```

---

### Description des Dossiers

| Dossier | Description |
|---------|-------------|
| `backend/` | Contient tous les microservices Spring Boot |
| `frontend/` | Application Angular 17 avec composants et services |
| `config-repo/` | Fichiers de configuration pour Config Server |
| `docker/` | Scripts et configurations Docker additionnels |


---

## 📋 Prérequis

Avant d'installer et d'exécuter le projet, assurez-vous d'avoir les outils suivants installés :

### Obligatoires

| Outil | Version Minimale | Vérification | Installation |
|-------|-----------------|--------------|--------------|
| **Docker Desktop** | 20.10+ | `docker --version` | [docker.com](https://www.docker.com/products/docker-desktop) |
| **Docker Compose** | 2.0+ | `docker-compose --version` | Inclus avec Docker Desktop |
| **Git** | 2.x | `git --version` | [git-scm.com](https://git-scm.com/) |

### Configuration Système

| Ressource | Minimum | Recommandé |
|-----------|---------|------------|
| **RAM** | 8 GB | 16 GB |
| **Espace Disque** | 10 GB | 20 GB |
| **CPU** | 4 cores | 8 cores |

### Optionnels (pour développement local sans Docker)

| Outil | Version | Utilité |
|-------|---------|---------|
| **Java JDK** | 21+ | Développement backend |
| **Maven** | 3.8+ | Build backend |
| **Node.js** | 18+ | Développement frontend |
| **npm** | 9+ | Gestionnaire paquets frontend |
| **IDE** | - | IntelliJ IDEA, VS Code, Eclipse |

---

## 🚀 Installation et Exécution

### Méthode 1 : Démarrage Rapide avec Docker (Recommandé)

Cette méthode construit et lance automatiquement tous les services en un seul script.

#### Windows (PowerShell)

```powershell
# 1. Cloner le projet
git clone <url-du-repo>
cd projet_micro

# 2. Builder tous les services
.\build-all.ps1

# 3. Lancer tous les services avec Docker Compose
docker-compose up -d

# 4. Vérifier le statut des conteneurs
docker-compose ps
```

#### Linux / macOS (Bash)

```bash
# 1. Cloner le projet
git clone <url-du-repo>
cd projet_micro

# 2. Rendre le script exécutable
chmod +x build-all.sh

# 3. Builder tous les services
./build-all.sh

# 4. Lancer tous les services avec Docker Compose
docker-compose up -d

# 5. Vérifier le statut des conteneurs
docker-compose ps
```

### Méthode 2 : Démarrage Manuel Étape par Étape

Si vous préférez un contrôle complet sur chaque étape :

#### Étape 1 : Cloner le Projet

```bash
git clone <url-du-repo>
cd projet_micro
```

#### Étape 2 : Initialiser Config Repository (si nécessaire)

```bash
cd config-repo
git init
git add .
git commit -m "Initial configuration"
cd ..
```

#### Étape 3 : Builder les Images Docker

```bash
# Build tous les services en une fois
docker-compose build

# OU builder individuellement
docker-compose build config-server
docker-compose build eureka-server
docker-compose build api-gateway
docker-compose build microservice-collecte
docker-compose build microservice-analyse
docker-compose build frontend
```

#### Étape 4 : Lancer les Services

```bash
# Lancer tous les services en arrière-plan
docker-compose up -d

# OU lancer avec logs visibles (pour débogage)
docker-compose up

# OU lancer des services spécifiques
docker-compose up -d config-server eureka-server
```

### 📊 Ordre de Démarrage

Docker Compose gère automatiquement l'ordre grâce aux dépendances définies :

```
1. 🟢 Zookeeper        (Port 2181)
2. 🟢 Kafka            (Port 9092)
3. 🟢 Config Server    (Port 8888) → Healthcheck
4. 🟢 Eureka Server    (Port 8761) → Attend Config Server
5. 🟢 API Gateway      (Port 8080) → Attend Eureka
6. 🟢 Microservice Collecte (8081) → Attend Eureka & Kafka
7. 🟢 Microservice Analyse (8082)  → Attend Eureka & Kafka
8. 🟢 Frontend         (Port 4200) → Attend API Gateway
```

⏱️ **Temps de démarrage total : 2-3 minutes**

### 🔍 Vérifier le Démarrage

#### Vérifier l'état des conteneurs

```bash
docker-compose ps
```

Résultat attendu :
```
NAME                      STATUS         PORTS
config-server             Up (healthy)   0.0.0.0:8888->8888/tcp
eureka-server             Up             0.0.0.0:8761->8761/tcp
api-gateway               Up             0.0.0.0:8080->8080/tcp
kafka                     Up             0.0.0.0:9092->9092/tcp
zookeeper                 Up             0.0.0.0:2181->2181/tcp
microservice-collecte     Up             0.0.0.0:8081->8081/tcp
microservice-analyse      Up             0.0.0.0:8082->8082/tcp
frontend                  Up             0.0.0.0:4200->80/tcp
```

#### Vérifier les logs

```bash
# Logs de tous les services
docker-compose logs -f

# Logs d'un service spécifique
docker-compose logs -f api-gateway
docker-compose logs -f microservice-collecte

# Dernières 100 lignes
docker-compose logs --tail=100
```

#### Vérifier les services via navigateur

| Service | URL | Statut Attendu |
|---------|-----|----------------|
| **Frontend** | http://localhost:4200 | Interface Angular |
| **Eureka Dashboard** | http://localhost:8761 | Affiche services enregistrés |
| **Config Server** | http://localhost:8888/actuator/health | `{"status":"UP"}` |
| **API Gateway** | http://localhost:8080/actuator/health | `{"status":"UP"}` |
| **Microservice Collecte** | http://localhost:8081/actuator/health | `{"status":"UP"}` |
| **Microservice Analyse** | http://localhost:8082/actuator/health | `{"status":"UP"}` |

### 🛑 Arrêter les Services

```bash
# Arrêter tous les services (préserve les données)
docker-compose stop

# Arrêter et supprimer les conteneurs
docker-compose down

# Supprimer conteneurs + volumes + réseaux
docker-compose down -v

# Supprimer + images
docker-compose down --rmi all
```

### 🔄 Redémarrer un Service Spécifique

```bash
# Redémarrer un service
docker-compose restart api-gateway

# Reconstruire et redémarrer
docker-compose up -d --build api-gateway

# Voir les logs en temps réel
docker-compose logs -f api-gateway
```

---

## 💡 Utilisation

### Accès à l'Application

Une fois tous les services démarrés, accédez à :

🌐 **Application Web** : http://localhost:4200

### Scénario d'Utilisation Complet

#### 1️⃣ **Créer des Capteurs**

1. Accédez à l'onglet **"Capteurs"**
2. Cliquez sur **"Nouveau Capteur"**
3. Remplissez le formulaire :
   - **Nom** : Capteur-Humidité-Zone-A
   - **Type** : Humidité
   - **Localisation** : Parcelle Nord
   - **Actif** : Oui
4. Cliquez sur **"Enregistrer"**

**Types de capteurs disponibles :**
- 💧 **Humidité** : Mesure l'humidité du sol (%)
- 🌡️ **Température** : Mesure la température ambiante (°C)
- 🌧️ **Pluviométrie** : Mesure les précipitations (mm)

#### 2️⃣ **Ajouter des Observations**

1. Accédez à l'onglet **"Observations"**
2. Cliquez sur **"Nouvelle Observation"**
3. Remplissez le formulaire :
   - **Capteur** : Sélectionnez un capteur
   - **Valeur** : 25.5
   - **Unité** : % (pour humidité)
   - **Date/Heure** : Auto (timestamp actuel)
4. Cliquez sur **"Enregistrer"**

**Ce qui se passe en arrière-plan :**
```
Frontend → API Gateway → Microservice-Collecte
                              ↓
                    Sauvegarde en base H2
                              ↓
                    Publication sur Kafka
                              ↓
              Microservice-Analyse (consomme)
                              ↓
                    Génère recommandation
                              ↓
                    Sauvegarde recommandation
```

#### 3️⃣ **Consulter les Recommandations**

1. Accédez à l'onglet **"Recommandations"**
2. Visualisez les recommandations générées automatiquement
3. Chaque recommandation affiche :
   - 💧 **Type d'irrigation** : Goutte-à-goutte, Aspersion, etc.
   - 📊 **Quantité d'eau** : Litres recommandés
   - ⏱️ **Durée** : Minutes d'irrigation
   - 🚨 **Priorité** : HAUTE, MOYENNE, BASSE
   - 📝 **Motif** : Raison de la recommandation

**Exemple de recommandation :**
```
Type : Irrigation Goutte-à-Goutte
Quantité : 50 litres
Durée : 30 minutes
Priorité : HAUTE
Motif : Humidité du sol faible (25.5%) - Irrigation urgente requise
```

#### 4️⃣ **Tableau de Bord**

1. Accédez au **"Dashboard"**
2. Visualisez :
   - 📊 Statistiques globales
   - 📈 Graphiques d'observations
   - 🔔 Alertes et notifications
   - 📍 État des capteurs actifs

---

## 📝 Licence

Projet académique - **Libre d'utilisation à des fins éducatives**


