# divyam-syself-devops

## Requirement Gathering
- Helm chart to deploy a sample application to a Kubernetes cluster
- Single Helm Chart to deploy all components to a desired namespace with best production practises.
- Database deployment integration and considerations.

## SPRINT 1: Choosing an application with backend logic and database and manual deployment for image verification and understanding the variables.
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




