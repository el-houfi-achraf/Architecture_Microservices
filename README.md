<p align="center">
  <img src="https://img.shields.io/badge/Spring%20Boot-3.2.0-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white" alt="Spring Boot"/>
  <img src="https://img.shields.io/badge/Spring%20Cloud-2023.0.0-6DB33F?style=for-the-badge&logo=spring&logoColor=white" alt="Spring Cloud"/>
  <img src="https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java"/>
  <img src="https://img.shields.io/badge/Consul-Service%20Discovery-F24C53?style=for-the-badge&logo=consul&logoColor=white" alt="Consul"/>
  <img src="https://img.shields.io/badge/MySQL-Database-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL"/>
</p>

<h1 align="center">Architecture Microservices</h1>
<h3 align="center">Système de Gestion de Location de Voitures</h3>

<p align="center">
  <i>Une architecture microservices complète avec Spring Cloud, Consul Discovery et API Gateway</i>
</p>

---

## 📋 Table des Matières

- [🎯 Présentation](#-présentation)
- [🏗️ Architecture](#️-architecture)
- [🔧 Technologies Utilisées](#-technologies-utilisées)
- [📂 Structure du Projet](#-structure-du-projet)
- [⚙️ Configuration des Services](#️-configuration-des-services)
- [🚀 Installation et Démarrage](#-installation-et-démarrage)
- [🔌 Endpoints API](#-endpoints-api)
- [📊 Modèles de Données](#-modèles-de-données)
- [🌐 Communication Inter-Services](#-communication-inter-services)
- [📝 Auteur](#-auteur)

---

## 🎯 Présentation

Ce projet illustre la mise en œuvre d'une **architecture microservices** pour un système de gestion de location de voitures. L'application est composée de plusieurs microservices indépendants qui communiquent entre eux via une **API Gateway** et s'enregistrent automatiquement auprès d'un **Service Registry (Consul)**.

### Objectifs Pédagogiques

- ✅ Comprendre les principes de l'architecture microservices
- ✅ Implémenter la découverte de services avec **HashiCorp Consul**
- ✅ Configurer une **API Gateway** avec Spring Cloud Gateway
- ✅ Maîtriser la communication inter-services avec **RestTemplate**
- ✅ Gérer plusieurs bases de données indépendantes

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT HTTP                                     │
│                         (Navigateur / Postman)                               │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         🌐 API GATEWAY (Port 8888)                          │
│                         Spring Cloud Gateway                                 │
│                    Route dynamique des requêtes                              │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                    ┌─────────────────┴─────────────────┐
                    │                                   │
                    ▼                                   ▼
┌───────────────────────────────┐     ┌───────────────────────────────┐
│   🚗 SERVICE-CAR (Port 8082)  │────▶│ 👤 SERVICE-CLIENT (Port 8081) │
│                               │     │                               │
│   • Gestion des voitures      │     │   • Gestion des clients       │
│   • CRUD Voitures             │     │   • CRUD Clients              │
│   • Association Client-Car    │     │                               │
└───────────────────────────────┘     └───────────────────────────────┘
           │                                       │
           │                                       │
           ▼                                       ▼
┌───────────────────────────────┐     ┌───────────────────────────────┐
│   🗄️ carservicedb (MySQL)     │     │  🗄️ clientservicedb (MySQL)  │
└───────────────────────────────┘     └───────────────────────────────┘

                                      │
                    ┌─────────────────┴─────────────────┐
                    │                                   │
                    ▼                                   ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                     🔍 CONSUL SERVICE REGISTRY (Port 8500)                  │
│                    Découverte et enregistrement des services                 │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                     🎯 EUREKA SERVER (Port 8761) [Optionnel]                │
│                    Serveur de registre Netflix Eureka                        │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔧 Technologies Utilisées

| Technologie | Version | Rôle |
|-------------|---------|------|
| **Spring Boot** | 3.2.0 | Framework principal |
| **Spring Cloud** | 2023.0.0 | Écosystème microservices |
| **Spring Cloud Gateway** | - | API Gateway réactive |
| **Consul Discovery** | - | Service Registry et Discovery |
| **Netflix Eureka** | - | Service Registry alternatif |
| **Spring Data JPA** | - | Persistance des données |
| **Spring Data REST** | - | Exposition REST automatique |
| **MySQL** | 8.x | Base de données relationnelle |
| **Lombok** | - | Réduction du boilerplate code |
| **Maven** | 3.x | Gestion des dépendances |
| **Java** | 17 | Langage de programmation |

---

## 📂 Structure du Projet

```
TP23/
├── 📁 car/                          # Microservice de gestion des voitures
│   ├── 📁 src/main/java/com/example/car/
│   │   ├── 📄 CarApplication.java          # Point d'entrée
│   │   ├── 📁 controllers/
│   │   │   └── 📄 CarController.java       # REST Controller
│   │   ├── 📁 entities/
│   │   │   ├── 📄 Car.java                 # Entité JPA Car
│   │   │   └── 📄 Client.java              # DTO Client
│   │   ├── 📁 models/
│   │   │   └── 📄 CarResponse.java         # Response DTO
│   │   ├── 📁 repositories/
│   │   │   └── 📄 CarRepository.java       # Repository JPA
│   │   └── 📁 services/
│   │       └── 📄 CarService.java          # Logique métier
│   ├── 📁 src/main/resources/
│   │   └── 📄 application.yml              # Configuration
│   └── 📄 pom.xml
│
├── 📁 client/                       # Microservice de gestion des clients
│   ├── 📁 src/main/java/com/example/client/
│   │   ├── 📄 ClientApplication.java       # Point d'entrée
│   │   ├── 📁 controllers/
│   │   │   └── 📄 ClientController.java    # REST Controller
│   │   ├── 📁 entities/
│   │   │   └── 📄 Client.java              # Entité JPA Client
│   │   ├── 📁 repositories/
│   │   │   └── 📄 ClientRepository.java    # Repository JPA
│   │   └── 📁 services/
│   │       └── 📄 ClientService.java       # Logique métier
│   ├── 📁 src/main/resources/
│   │   └── 📄 application.yml              # Configuration
│   └── 📄 pom.xml
│
├── 📁 gateway/                      # API Gateway
│   ├── 📁 src/main/java/com/fateway/gateway/
│   │   └── 📄 GatewayApplication.java      # Configuration Gateway
│   ├── 📁 src/main/resources/
│   │   └── 📄 application.yml              # Routes et configuration
│   └── 📄 pom.xml
│
├── 📁 server_eureka/                # Serveur Eureka (optionnel)
│   ├── 📁 src/main/java/com/server/servereureka/
│   │   └── 📄 ServerEurekaApplication.java # Serveur Eureka
│   ├── 📁 src/main/resources/
│   │   └── 📄 application.yml              # Configuration Eureka
│   └── 📄 pom.xml
│
└── 📄 README.md                     # Documentation
```

---

## ⚙️ Configuration des Services

### 🚗 SERVICE-CAR (Port 8082)

```yaml
server:
  port: 8082

spring:
  application:
    name: SERVICE-CAR
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: SERVICE-CAR
  datasource:
    url: jdbc:mysql://localhost:3306/carservicedb?createDatabaseIfNotExist=true
    username: root
    password: ""
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

### 👤 SERVICE-CLIENT (Port 8081)

```yaml
server:
  port: 8081

spring:
  application:
    name: SERVICE-CLIENT
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: SERVICE-CLIENT
  datasource:
    url: jdbc:mysql://localhost:3306/clientservicedb?createDatabaseIfNotExist=true
    username: root
    password: ""
  jpa:
    hibernate:
      ddl-auto: update
```

### 🌐 GATEWAY (Port 8888)

```yaml
server:
  port: 8888

spring:
  application:
    name: GATEWAY
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: GATEWAY
    gateway:
      discovery:
        locator:
          enabled: true
          lower-case-service-id: true
```

### 🎯 EUREKA SERVER (Port 8761)

```yaml
server:
  port: 8761

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```

---

## 🚀 Installation et Démarrage

### Prérequis

- ☕ **Java 17** ou supérieur
- 📦 **Maven 3.6+**
- 🐬 **MySQL 8.x** 
- 🔍 **HashiCorp Consul** (installé et en cours d'exécution)

### 1️⃣ Installation de Consul

```bash
# Téléchargement et installation de Consul
# Windows (avec Chocolatey)
choco install consul

# macOS (avec Homebrew)
brew install consul

# Linux
wget https://releases.hashicorp.com/consul/1.17.0/consul_1.17.0_linux_amd64.zip
unzip consul_1.17.0_linux_amd64.zip
sudo mv consul /usr/local/bin/
```

### 2️⃣ Démarrage de Consul

```bash
# Démarrer Consul en mode développement
consul agent -dev
```

> 📍 Accédez à l'interface Consul : http://localhost:8500

### 3️⃣ Configuration MySQL

```sql
-- Les bases de données sont créées automatiquement grâce à :
-- createDatabaseIfNotExist=true
```

### 4️⃣ Ordre de Démarrage des Services

```bash
# Terminal 1 - Démarrer Consul
consul agent -dev

# Terminal 2 - Server Eureka (Optionnel)
cd server_eureka
mvn spring-boot:run

# Terminal 3 - Gateway
cd gateway
mvn spring-boot:run

# Terminal 4 - Service Client
cd client
mvn spring-boot:run

# Terminal 5 - Service Car
cd car
mvn spring-boot:run
```

### 5️⃣ Vérification

| Service | URL | Description |
|---------|-----|-------------|
| Consul UI | http://localhost:8500 | Interface de gestion Consul |
| Eureka Dashboard | http://localhost:8761 | Dashboard Eureka |
| Gateway | http://localhost:8888 | Point d'entrée unique |
| Service Client | http://localhost:8081 | API Clients |
| Service Car | http://localhost:8082 | API Voitures |

---

## 🔌 Endpoints API

### 👤 Service Client

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/api/client` | Récupérer tous les clients |
| `GET` | `/api/client/{id}` | Récupérer un client par ID |
| `POST` | `/api/client` | Créer un nouveau client |

**Via Gateway :**
```bash
# Récupérer tous les clients
curl http://localhost:8888/service-client/api/client

# Récupérer un client spécifique
curl http://localhost:8888/service-client/api/client/1

# Créer un client
curl -X POST http://localhost:8888/service-client/api/client \
  -H "Content-Type: application/json" \
  -d '{"nom": "Achraf", "age": 25}'
```

### 🚗 Service Car

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/api/car` | Récupérer toutes les voitures avec clients |
| `GET` | `/api/car/{id}` | Récupérer une voiture par ID avec son client |

**Via Gateway :**
```bash
# Récupérer toutes les voitures
curl http://localhost:8888/service-car/api/car

# Récupérer une voiture spécifique
curl http://localhost:8888/service-car/api/car/1
```

---

## 📊 Modèles de Données

### 👤 Client

```java
@Entity
public class Client {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nom;
    private Float age;
}
```

### 🚗 Car

```java
@Entity
public class Car {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String brand;
    private String model;
    private String matricule;
    private Long client_id;  // Référence vers le client
}
```

### 📦 CarResponse (DTO)

```java
@Builder
public class CarResponse {
    private Long id;
    private String brand;
    private String model;
    private String matricue;
    private Client client;  // Client enrichi via appel inter-service
}
```

---

## 🌐 Communication Inter-Services

Le **Service Car** communique avec le **Service Client** pour enrichir les données des voitures avec les informations des clients propriétaires.

### Mécanisme

```java
@Service
public class CarService {
    @Autowired
    private RestTemplate restTemplate;
    
    private final String URL = "http://localhost:8888/SERVICE-CLIENT";
    
    public List<CarResponse> findAll() {
        // 1. Récupérer les voitures localement
        List<Car> cars = carRepository.findAll();
        
        // 2. Appeler le service client via la Gateway
        ResponseEntity<Client[]> response = 
            restTemplate.getForEntity(URL + "/api/client", Client[].class);
        
        // 3. Mapper et enrichir les données
        return cars.stream()
            .map(car -> mapToCarResponse(car, response.getBody()))
            .toList();
    }
}
```

### 🔄 Flux de Communication

```
[Client Request] 
       │
       ▼
[API Gateway :8888]
       │
       ├──────► [SERVICE-CAR :8082]
       │              │
       │              ▼
       │        [RestTemplate Call]
       │              │
       │              ▼
       └──────► [SERVICE-CLIENT :8081]
                      │
                      ▼
              [Response enrichie]
```

---

## 📝 Bonnes Pratiques Implémentées

| Pratique | Description |
|----------|-------------|
| ✅ **Séparation des responsabilités** | Chaque service a une responsabilité unique |
| ✅ **Base de données par service** | Isolation des données (Database per Service) |
| ✅ **Discovery Pattern** | Enregistrement automatique via Consul |
| ✅ **API Gateway** | Point d'entrée unique avec routage dynamique |
| ✅ **DTO Pattern** | Utilisation de DTOs pour les réponses API |
| ✅ **Lombok** | Réduction du boilerplate code |
| ✅ **Configuration externalisée** | Utilisation de fichiers YAML |

---

## 🐛 Dépannage

| Problème | Solution |
|----------|----------|
| Consul non accessible | Vérifiez que Consul est démarré : `consul agent -dev` |
| Service non enregistré | Vérifiez la configuration consul dans `application.yml` |
| Erreur de connexion MySQL | Vérifiez les credentials et que MySQL est démarré |
| Gateway ne route pas | Vérifiez que `lower-case-service-id: true` est configuré |

---

## 📝 Auteur

<p align="center">
  <strong>Achraf</strong><br>
  <
</p>

---
