# Java Backend Microservices

## **Overview**
This repository contains my implementation of a production-style Java microservices architecture developed while completing an advanced backend engineering course. Beyond the guided implementation, I extended the project with additional features including Redis caching and pagination, and continue to use it as a sandbox for learning backend architecture and cloud-native development.
## **Architecture**
### Basic CRUD
```mermaid
graph LR
Frontend["Frontend (Client)"]:::frontend
Controller["controller<br/>Handles HTTP Request &<br/>Response"]:::controller
Service["service<br/>handles business logic, including<br/>conversions between DTOs and<br/>domain models"]:::service
Repository["repository<br/>Handles interactions with the<br/>database"]:::repository
DB[("db")]:::database

    Frontend -->|"Makes POST<br/>Request"| Controller
    Controller -->|"Returns<br/>Response"| Frontend
    Controller -->|"Passes DTO"| Service
    Service -->|"Return DTO"| Controller
    Service -->|"Passes<br/>Model"| Repository
    Repository -->|"Returns<br/>Model"| Service
    Repository -->|"Runs Queries"| DB
    
    classDef frontend fill:#8B4513,stroke:#654321,color:#fff
    classDef controller fill:#2C3E50,stroke:#1a252f,color:#fff
    classDef service fill:#34495E,stroke:#1a252f,color:#fff
    classDef repository fill:#34495E,stroke:#1a252f,color:#fff
    classDef database fill:#000,stroke:#333,color:#fff
```
### gRPC
```mermaid
graph LR
    subgraph DockerNetwork["Docker Network"]
        PatientService["Patient Service<br>(gRPC client)"]
        BillingService["Billing Service<br>(gRPC server)"]
        BillingProto["billing_service.proto"]
    end
    Frontend["Frontend<br>Client"] -- REST Request<br>JSON --> PatientService
    PatientService -- GRPC Request<br>Protobuf --> BillingService
    BillingProto -. describes .-> BillingService

    PatientService:::service
    BillingService:::service
    BillingProto:::proto
    Frontend:::frontend
    classDef frontend fill:#8B4513,stroke:#654321,color:#fff
    classDef service fill:#34495E,stroke:#1a252f,color:#fff
    classDef proto fill:#000,stroke:#333,color:#fff
    classDef container stroke:#a78bfa,fill:#f5f3ff
```
### Kafka
```mermaid
graph LR
subgraph DockerNetwork["Docker Network"]
PatientService["Patient Service<br>(gRPC client)(Kafka producer)"]
BillingService["Billing Service<br>(gRPC server)"]
BillingProto["billing_service.proto"]
KafkaTopic["Kafka Topic<br>(Patients)"]
AnalyticsService["Analytics Service"]
NotificationService["Notification Service"]
end
Frontend["Frontend<br>Client"] -- REST Request<br>JSON --> PatientService
PatientService -- produces --> KafkaTopic
PatientService -- gRPC Request: Protobuf --> BillingService
KafkaTopic -- Kafka consumer --> AnalyticsService & NotificationService
BillingProto -. describes .-> BillingService
PatientService:::service
BillingService:::service
KafkaTopic:::topic
AnalyticsService:::service
NotificationService:::service
BillingProto:::proto
Frontend:::frontend
classDef frontend fill:#8B4513,stroke:#654321,color:#fff
classDef service fill:#34495E,stroke:#1a252f,color:#fff
classDef proto fill:#000,stroke:#333,color:#fff
classDef container stroke:#a78bfa,fill:#f5f3ff
classDef topic fill:#6B6512,stroke:#1a252f,color:#fff
```
### API Gateway
```mermaid
graph LR
subgraph DockerNetwork["Docker Network"]
PatientService["Patient Service<br>(gRPC client)(Kafka producer)"]
BillingService["Billing Service<br>(gRPC server)"]
AuthService["Auth Service"]
BillingProto["billing_service.proto"]
KafkaTopic["Kafka Topic<br>(Patients)"]
ApiGateway["API Gateway"]
AnalyticsService["Analytics Service"]
NotificationService["Notification Service"]
end
Frontend["Frontend<br>Client"] -- GET ../api/analytics --> ApiGateway
Frontend["Frontend<br>Client"] -- GET ../api/patients --> ApiGateway
Frontend["Frontend<br>Client"] -- POST ../api/patients/ --> ApiGateway
ApiGateway -- redirect --> PatientService
ApiGateway -- redirect --> AuthService
PatientService -- produces --> KafkaTopic
PatientService -- gRPC Request: Protobuf --> BillingService
KafkaTopic -- Kafka consumer --> AnalyticsService & NotificationService
BillingProto -. describes .-> BillingService
PatientService:::service
BillingService:::service
KafkaTopic:::topic
AnalyticsService:::service
NotificationService:::service
BillingProto:::proto
Frontend:::frontend
classDef frontend fill:#8B4513,stroke:#654321,color:#fff
classDef service fill:#34495E,stroke:#1a252f,color:#fff
classDef proto fill:#000,stroke:#333,color:#fff
classDef container stroke:#a78bfa,fill:#f5f3ff
classDef topic fill:#6B6512,stroke:#1a252f,color:#fff
```
### Authentication
```mermaid
graph LR
 subgraph DockerNetwork["Docker Network"]
    PatientService["Patient Service<br>(gRPC client)(Kafka producer)"]
    BillingService["Billing Service<br>(gRPC server)"]
    AuthService["Auth Service"]
    BillingProto["billing_service.proto"]
    KafkaTopic["Kafka Topic<br>(Patients)"]
    ApiGateway["API Gateway"]
    AnalyticsService["Analytics Service"]
    NotificationService["Notification Service"]
  end
    Frontend["Frontend<br>Client"] -- "GET ../api/patients" --> ApiGateway
    Frontend -- "POST ../auth/login/" --> ApiGateway
    ApiGateway -- validates token --> AuthService
    AuthService -- returns JWT token --> ApiGateway
    ApiGateway -- Routes the request --> PatientService
    PatientService -- produces --> KafkaTopic
    PatientService -- gRPC Request: Protobuf --> BillingService
    KafkaTopic -- Kafka consumer --> AnalyticsService & NotificationService
    BillingProto -. describes .-> BillingService

     PatientService:::service
     BillingService:::service
     BillingProto:::proto
     KafkaTopic:::topic
     AnalyticsService:::service
     NotificationService:::service
     Frontend:::frontend
    classDef frontend fill:#8B4513,stroke:#654321,color:#fff
    classDef service fill:#34495E,stroke:#1a252f,color:#fff
    classDef proto fill:#000,stroke:#333,color:#fff
    classDef container stroke:#a78bfa,fill:#f5f3ff
    classDef topic fill:#6B6512,stroke:#1a252f,color:#fff
```

## Features

✅ RESTful APIs

✅ Spring Boot

✅ Spring Validation

✅ Global Exception Handling

✅ PostgreSQL

✅ Docker

✅ Docker Compose

✅ JWT Authentication

✅ API Gateway

✅ gRPC

✅ Apache Kafka

✅ Integration Testing

✅ LocalStack

✅ AWS CloudFormation

## Tech Stack

##### Backend

* Java 21
* Spring Boot
* Spring Security
* Spring Validation

##### Communication

* REST
* gRPC
* Kafka

##### Database

PostgreSQL

##### Infrastructure

* Docker
* Docker Compose
* LocalStack
* CloudFormation

##### Testing

* JUnit
* Integration Testing
## Project Structure
* analytics-service/  
* api-requests/  
* billing-service/  
* grpc-requests/   
* integration-tests/  
* patient-service/
* api-gateway/        
* auth-service/  
* infrastructure/
## Getting Started

## Running with Docker

## Future Improvements

## Learning Outcomes