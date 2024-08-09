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



## DATABASE CONSIDERATIONS AND OTHER BEST PRACTICES

- In this setup, I am using a pre-built MySQL Docker image developed by cloudacademy that's included in our Helm chart to deploy the database directly within the Kubernetes cluster. This method is straightforward since the image is already defined, making it easy to manage everything in one place.
If the MySQL image wasn't pre-defined, we could have used Bitnami's Helm charts. Bitnami provides well-maintained, production-ready Helm charts for various applications, including MySQL. These charts come with advanced features like automatic backups, high availability, and easy scaling, which are ideal for more complex or large-scale deployments.

- In the current setup, I have deployed MySQL without persistent storage, so data stored in the database would be lost if the pod is restarted or deleted. To avoid this, Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) can be used to provide a stable, long-term storage solution.

- Currently, I am storing sensitive information like database passwords directly in the deployment YAML. While this works, it's not secure. A better approach is to use Kubernetes Secrets.

## PRODUCTION GRADE KUBERNETES ENVIRONMENT

**ARCHITECTURE:**

Kubernetes high-level components can be categorized in 
- Control plane: This is the brain of the Kubernetes cluster. It is responsible for making global decisions about the cluster, such as scheduling new pods, creating load balancers, and configuring network rules.
- Worker nodes: These are the machines that run the applications. Each worker node has a set of components that provide the Kubernetes runtime environment.

Kubernetes low-level details are as follows:
Worker node components:

- Kubelet: This agent runs on each worker node and is responsible for communicating with the control plane, managing pods, and ensuring that containers are running and healthy.
- Kube-proxy: This network proxy runs on each worker node and implements service networking and load balancing.
Container runtime: This is a software package that is responsible for running containers on the worker node. The most common container runtimes are Docker and contained.

Control plane components:
- Kube-apiserver: This is the front end of the Kubernetes control plane. It exposes the Kubernetes API, which can be used by users and other components to interact with the cluster.
- Etcd: distributed key-value store that is used by Kubernetes to store cluster state.
- Kube-scheduler: This component is responsible for deciding which node to place new pods on.
- Kube-controller-manager: This component runs a set of controller processes that perform various cluster management tasks, such as creating endpoints, managing replication controllers, and cleaning up terminated pods.
- Cloud controller manager: This component integrates Kubernetes with cloud-specific features and services.


My implementation of a self-managed Kubernetes environment would be something like this:

I would start by selecting Ubuntu 24.04 as the base operating system for all nodes. This choice ensures compatibility and stability with Kubernetes v1.30.3 and meets our requirements.

For the control plane, I would deploy at least three control plane nodes to ensure high availability and fault tolerance. These nodes will manage the overall cluster operations, including scheduling and handling API requests. I would install the necessary Kubernetes components on these nodes: the API Server to manage cluster state and user requests, the Controller Manager to maintain desired cluster states, the Scheduler to allocate workloads to worker nodes, and etcd as the distributed key-value store for cluster data and configuration.

On the worker nodes, which are responsible for running application workloads, I would set up the kubelet to ensure containers are running as expected. Additionally, I would install kube-proxy to handle networking rules and facilitate communication between pods and services. For container management, I would select and configure a container runtime such as Docker, containerd, or CRI-O.

I would then look into the networking. I would ensure that all nodes, both control plane and worker nodes, are connected via a private network for secure and efficient communication. I would also choose and configure a Container Networking Interface (CNI) plugin to manage internal cluster networking.

For storage, I would use a Container Storage Interface (CSI) to manage persistent storage, integrating with external storage systems to provide volumes for pods. I would configure a default storage class to allow dynamic provisioning of these volumes as needed by applications.

To address high availability and scalability, I would deploy multiple control plane nodes and configure an external load balancer to distribute incoming requests across these nodes. This setup ensures that the cluster remains operational even if one node fails and handles increased load effectively.

I would also secure my environment. I would implement Role-Based Access Control (RBAC) to manage access to Kubernetes resources and control user actions within the cluster. I would also apply network policies to regulate pod communication, enhancing security and isolation within the cluster.




