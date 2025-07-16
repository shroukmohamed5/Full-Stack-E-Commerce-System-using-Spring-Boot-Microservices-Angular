 
# Full-Stack E-Commerce System using Microservices Architecture

This graduation project presents the design and implementation of a full-stack e-commerce application that demonstrates the practical application of modern software engineering concepts, including microservices architecture, container orchestration, and secure authentication.

The system is developed using Spring Boot for the backend services and Angular 18 for the frontend, with inter-service communication managed through Apache Kafka. Security is enforced via Keycloak, and the application is deployed on a Kubernetes cluster with integrated observability through Grafana and Prometheus.

The project aims to simulate a real-world scalable e-commerce platform, showcasing skills in distributed systems, DevOps, and frontend-backend integration.


## Microservices Overview


- Product Service
- Order Service
- Inventory Service
- Notification Service
- API Gateway using Spring Cloud Gateway MVC
- Shop Frontend using Angular 18

## Tech Stack

The technologies used in this project are:

- Spring Boot
- Angular
- Mongo DB
- MySQL
- Kafka
- Keycloak
- Test Containers with Wiremock
- Grafana Stack (Prometheus, Grafana, Loki and Tempo)
- API Gateway using Spring Cloud Gateway MVC
- Kubernetes


## Application Architecture:
The system follows a microservices-based architecture with the following components and communication flows:

- Frontend (Angular):
The user interacts with the system through an Angular-based frontend UI. This frontend communicates exclusively with the backend via the API Gateway.

- API Gateway:
Built using Spring Cloud Gateway, it acts as a single entry point to route client requests to the appropriate backend services. It also integrates with Resilience4J to ensure service reliability and fault tolerance.

- Auth Server:
The authentication and authorization mechanism is implemented using Keycloak, which is integrated with the API Gateway to manage secured access for users.

- Product Service:
Handles product-related operations. It communicates with a MongoDB instance for product data storage. Communication is secured and goes through the API Gateway with Resilience4J.

- Order Service:
Responsible for processing orders. It interacts directly with a MySQL database to manage order data. It also serves as the central orchestrator for both synchronous and asynchronous communications:

- Synchronous Communication:
Communicates with Inventory Service to check and reserve product stock. This interaction is enhanced with Resilience4J for circuit breaking and fallback logic.

- Asynchronous Communication:
Sends messages to Notification Service via Apache Kafka to send order confirmation or updates.

- Inventory Service:
Manages the product stock. It uses a MySQL database and responds to sync requests from the Order Service.

- Notification Service:
Listens to Kafka events and sends notifications (email messages). This service is containerized like others and secured.

- Databases:

 MongoDB is used by the Product Service.

 MySQL is used by both Order and Inventory Services.

- Kafka:
Used for asynchronous messaging between services, especially between Order Service and Notification Service.

- Resilience4J:
Implemented for service-to-service reliability, such as timeouts, retries, and circuit breakers in synchronous calls.

- Observability Stack:

-  OpenTelemetry

-  Prometheus

-  Grafana Loki

-  Grafana Tempo

-  Grafana Dashboard
These tools provide metrics, logs, and tracing data for monitoring the system health.

- Kubernetes:
All services are containerized (Docker) and deployed in a Kubernetes cluster for scalability, orchestration, and infrastructure management

## How to run the frontend application


Running the frontend application requires the following tools to be available in the development environment:

- Node.js
- NPM
- Angular CLI

Run the following commands to start the frontend application

```shell
cd frontend
npm install
npm run start
```
## How to build the backend services

Run the following command to build and package the backend services into a docker container

```shell
mvn spring-boot:build-image -DdockerPassword=<your-docker-account-password>
```

The above command will build and package the services into a docker container and push it to your docker hub account.

## How to run the backend services

Make sure you have the following installed on your machine:

- Java 21
- Docker
- Kind Cluster - https://kind.sigs.k8s.io/docs/user/quick-start/#installation

### Start Kind Cluster
    
Run the k8s/kind/create-kind-cluster.sh script to create the kind Kubernetes cluster

```shell
./k8s/kind/create-kind-cluster.sh
```
This will create a kind cluster and pre-load all the required docker images into the cluster, this will save you time downloading the images when you deploy the application.

### Deploy the infrastructure

Run the k8s/manifests/infrastructure.yaml file to deploy the infrastructure

```shell
kubectl apply -f k8s/manifests/infrastructure.yaml
```

### Deploy the services

Run the k8s/manifests/applications.yaml file to deploy the services

```shell
kubectl apply -f k8s/manifests/applications.yaml
```

### Access the API Gateway

To access the API Gateway, you need to port-forward the gateway service to your local machine

```shell
kubectl port-forward svc/gateway-service 9000:9000
```

### Access the Keycloak Admin Console
To access the Keycloak admin console, you need to port-forward the keycloak service to your local machine

```shell
kubectl port-forward svc/keycloak 8080:8080
```

### Access the Grafana Dashboards
To access the Grafana dashboards, you need to port-forward the grafana service to your local machine

```shell
kubectl port-forward svc/grafana 3000:3000
```

## Note:
This project was developed as part of a university practical training program to demonstrate the implementation of a real-world full-stack e-commerce system using modern technologies and best practices in microservices architecture.
