# divyam-syself-devops

## Requirement Gathering
- Helm chart to deploy a sample application to a Kubernetes cluster
- Single Helm Chart to deploy all components to a desired namespace with best production practises.
- Database deployment integration and considerations.

## SPRINT 1: Choosing an application with backend logic, database, and manual deployment for image verification and understanding the variables.
I have chosen to use the stocks application developed by CloudAcademy. This application is a stock data management system designed to handle and store stock market data using a Spring Boot backend and a MySQL database. It provides a RESTful API for CRUD operations on stock data.

- Completed running the containers manually with docker-compose and the application is working as expected on localhost. Faced errors related to mysql connection failure , did troubleshooting using
  `docker logs containerid`
  

![image](https://github.com/user-attachments/assets/dd62f82c-c9e1-4d24-88e3-08e96e69b85b)
![image](https://github.com/user-attachments/assets/99ad8435-f079-45d3-afce-7674dc495520)
![image](https://github.com/user-attachments/assets/cc86f3e1-4389-49a3-bc8c-94c2b160e43e)

## SPRINT 2: Creating Kubernetes Manifests and Helm chart directory structure for the Trading Application

Kubernetes Manifests Creation and Deployment without Helm
![image](https://github.com/user-attachments/assets/76e933c3-d850-47c1-9d32-017e0f00a91f)
![image](https://github.com/user-attachments/assets/0d9545fa-9845-4e33-9861-7abc890de710)

While working with minikube I faced issues making the connection between the API and app pods work because of the way minikube exposes the service to any program running on the host operating system.

## SPRINT 3: Creating Helm Chart, Packaging and Installing on Minikube cluster

![image](https://github.com/user-attachments/assets/79d9e341-c67a-4818-945b-f3816d0ca493)




## PROJECT DOCUMENTATION AND INSTALLATION STEPS

This Helm chart deploys a multi-tiered application that includes an API service, a frontend service, and a MySQL database. Each service is deployed as a Kubernetes Deployment and exposed via a Service resource.

## HELM STRUCTURE

The chart consists of the following components:

Namespace: Defines the namespace where the application will run.
API Service: Deploys the API backend as a Kubernetes Deployment and exposes it via a NodePort Service.
Frontend Service: Deploys the frontend application as a Kubernetes Deployment and exposes it via a NodePort Service.
Database Service: Deploys a MySQL database as a Kubernetes Deployment and exposes it via a ClusterIP Service.

**INSTALLATION**
`helm install <release-name> ./<path>`

**UNINSTALLTION**
`helm uninstall <release-name>`

**Global Parameters**
namespace.name: The name of the namespace to create. 

**API Service Parameters**
- api.replicas: Number of API replicas. 
- api.image: API container image. 
- api.dbConnStr: Database connection string.
- api.dbUser: Database username.
- api.dbPassword: Database password.
- api.nodePort: NodePort for exposing the API service. 

**Frontend Service Parameters**
- app.replicas: Number of frontend replicas.
- app.image: Frontend container image.
- app.apiHostPort: API host and port for frontend access.
- app.nodePort: NodePort for exposing the frontend service.
- 
**Database Service Parameters**
- db.replicas: Number of database replicas. 
- db.image: Database container image. 
- db.rootPassword: MySQL root password.








