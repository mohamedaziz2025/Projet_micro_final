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

## 🔌 Endpoints API

### Via API Gateway (Port 8080)

Toutes les requêtes frontend passent par l'API Gateway qui route vers les microservices.

#### 🎛️ Microservice Collecte

**Base URL** : `http://localhost:8080/api/collecte`

##### Capteurs

| Méthode | Endpoint | Description | Body |
|---------|----------|-------------|------|
| `GET` | `/capteurs` | Liste tous les capteurs | - |
| `GET` | `/capteurs/{id}` | Détail d'un capteur | - |
| `POST` | `/capteurs` | Créer un capteur | JSON Capteur |
| `PUT` | `/capteurs/{id}` | Modifier un capteur | JSON Capteur |
| `DELETE` | `/capteurs/{id}` | Supprimer un capteur | - |

**Exemple de création de capteur :**
```bash
curl -X POST http://localhost:8080/api/collecte/capteurs \
  -H "Content-Type: application/json" \
  -d '{
    "nom": "Capteur-Humidité-1",
    "type": "Humidité",
    "localisation": "Zone A",
    "actif": true
  }'
```

##### Observations

| Méthode | Endpoint | Description | Body |
|---------|----------|-------------|------|
| `GET` | `/observations` | Liste toutes les observations | - |
| `GET` | `/observations/{id}` | Détail d'une observation | - |
| `POST` | `/observations` | Créer observation → **Kafka** | JSON Observation |
| `PUT` | `/observations/{id}` | Modifier une observation | JSON Observation |
| `DELETE` | `/observations/{id}` | Supprimer une observation | - |

**Exemple de création d'observation :**
```bash
curl -X POST http://localhost:8080/api/collecte/observations \
  -H "Content-Type: application/json" \
  -d '{
    "valeur": 28.5,
    "unite": "%",
    "capteurId": 1
  }'
```

#### 📊 Microservice Analyse

**Base URL** : `http://localhost:8080/api/analyse`

##### Recommandations

| Méthode | Endpoint | Description | Body |
|---------|----------|-------------|------|
| `GET` | `/recommandations` | Liste toutes les recommandations | - |
| `GET` | `/recommandations/{id}` | Détail d'une recommandation | - |
| `POST` | `/recommandations` | Créer recommandation manuelle | JSON Recommandation |
| `PUT` | `/recommandations/{id}` | Modifier une recommandation | JSON Recommandation |
| `DELETE` | `/recommandations/{id}` | Supprimer une recommandation | - |

**Exemple de récupération des recommandations :**
```bash
curl http://localhost:8080/api/analyse/recommandations
```

### Accès Direct aux Microservices (Développement)

#### Config Server (Port 8888)

```bash
# Récupérer la configuration d'un service
curl http://localhost:8888/api-gateway/default

# Healthcheck
curl http://localhost:8888/actuator/health
```

#### Eureka Server (Port 8761)

```bash
# Dashboard Web
http://localhost:8761

# API REST - Liste des services enregistrés
curl http://localhost:8761/eureka/apps
```

---

## 🔄 Communication entre Services

### Communication Synchrone (HTTP/REST)

**Via OpenFeign + Eureka**

```java
// Dans Microservice-Analyse
@FeignClient(name = "MICROSERVICE-COLLECTE")
public interface CollecteClient {
    @GetMapping("/observations/{id}")
    ObservationDto getObservation(@PathVariable Long id);
}
```

**Flux :**
```
Microservice-Analyse → Eureka (découverte)
                     → Microservice-Collecte
                     → Réponse synchrone
```

### Communication Asynchrone (Kafka)

**Event-Driven Architecture**

**Producer (Microservice-Collecte) :**
```java
@Service
public class ObservationService {
    @Autowired
    private KafkaTemplate<String, Observation> kafkaTemplate;
    
    public Observation create(Observation obs) {
        Observation saved = repository.save(obs);
        kafkaTemplate.send("observations-topic", saved);
        return saved; // Retour immédiat
    }
}
```

**Consumer (Microservice-Analyse) :**
```java
@Service
public class RecommandationService {
    @KafkaListener(topics = "observations-topic", 
                   groupId = "analyse-group")
    public void consumerObservation(ObservationDto obs) {
        // Analyse et génération recommandation
        Recommandation reco = genererRecommandation(obs);
        repository.save(reco);
    }
}
```

**Flux :**
```
POST /observations
      ↓
Microservice-Collecte sauvegarde
      ↓
Publication Kafka (non-bloquant)
      ↓
Réponse immédiate au client
      ...
Microservice-Analyse consomme (asynchrone)
      ↓
Génère recommandation
```

---

## 🐛 Troubleshooting

### Problèmes Courants et Solutions

#### ❌ Problème : Services ne démarrent pas

**Symptôme :**
```bash
docker-compose ps
# Certains services en état "Exited" ou "Restarting"
```

**Solutions :**

1. **Vérifier les logs :**
   ```bash
   docker-compose logs config-server
   docker-compose logs eureka-server
   ```

2. **Attendre le healthcheck du Config Server :**
   ```bash
   docker-compose logs -f config-server
   # Attendre : "Started ConfigServerApplication"
   ```

3. **Vérifier les ressources Docker :**
   - Docker Desktop → Settings → Resources
   - RAM minimum : 8 GB
   - CPU minimum : 4 cores

4. **Nettoyer et redémarrer :**
   ```bash
   docker-compose down -v
   docker system prune -f
   docker-compose up -d
   ```

#### ❌ Problème : Port déjà utilisé

**Symptôme :**
```
Error: bind: address already in use (port 8080)
```

**Solutions :**

**Windows :**
```powershell
# Trouver le processus
netstat -ano | findstr :8080

# Tuer le processus (remplacer PID)
taskkill /PID <PID> /F
```

**Linux/Mac :**
```bash
# Trouver et tuer le processus
lsof -ti:8080 | xargs kill -9
```

#### ❌ Problème : Eureka n'affiche pas les services

**Symptôme :**
- Dashboard Eureka vide ou services manquants

**Solutions :**

1. **Attendre 30-60 secondes** (temps d'enregistrement)

2. **Vérifier les logs du service :**
   ```bash
   docker-compose logs api-gateway
   # Chercher : "DiscoveryClient registering service"
   ```

3. **Vérifier la configuration Eureka :**
   ```bash
   curl http://localhost:8761/eureka/apps
   ```

#### ❌ Problème : Frontend ne charge pas les données

**Symptôme :**
- Erreur CORS ou Network Error dans la console

**Solutions :**

1. **Vérifier que l'API Gateway est accessible :**
   ```bash
   curl http://localhost:8080/api/collecte/capteurs
   ```

2. **Vérifier la configuration CORS dans API Gateway**

3. **Inspecter la console navigateur (F12) :**
   - Network tab → Voir les requêtes échouées

#### ❌ Problème : Kafka ne reçoit pas les messages

**Symptôme :**
- Observations créées mais pas de recommandations générées

**Solutions :**

1. **Vérifier Kafka et Zookeeper :**
   ```bash
   docker-compose logs kafka
   docker-compose logs zookeeper
   ```

2. **Vérifier les logs Producer :**
   ```bash
   docker-compose logs microservice-collecte
   # Chercher : "Observation publiée sur Kafka"
   ```

3. **Vérifier les logs Consumer :**
   ```bash
   docker-compose logs microservice-analyse
   # Chercher : "Observation consommée depuis Kafka"
   ```

4. **Tester Kafka manuellement :**
   ```bash
   docker exec -it kafka kafka-topics --list --bootstrap-server localhost:9092
   ```

#### ❌ Problème : Build Maven échoue

**Symptôme :**
```
[ERROR] Failed to execute goal: compilation failure
```

**Solutions :**

1. **Nettoyer le cache Maven :**
   ```bash
   docker-compose build --no-cache config-server
   ```

2. **Vérifier la version Java :**
   - Le projet nécessite Java 21

3. **Vérifier la connexion Internet :**
   - Maven télécharge les dépendances depuis Maven Central

### Commandes de Diagnostic Utiles

```bash
# État de tous les conteneurs
docker-compose ps

# Ressources utilisées
docker stats

# Inspecter un conteneur
docker inspect config-server

# Accéder au shell d'un conteneur
docker exec -it api-gateway sh

# Vérifier les réseaux Docker
docker network ls
docker network inspect projet_micro_irrigation-network

# Voir toutes les images
docker images

# Espace disque utilisé
docker system df
```

---



## 📝 Licence

Projet académique - **Libre d'utilisation à des fins éducatives**


