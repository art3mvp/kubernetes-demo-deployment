# Project Overview

This repository sets up a microservices architecture using AWS EKS and MongoDB, supporting user authentication and task management. Data is stored in MongoDB, and logs are saved in an AWS EFS volume.

## Setup Instructions

### 1. Add `.env` File
Create a `.env` file in the root directory with the necessary environment variables, including MongoDB credentials.

### 2. Create MongoDB Cluster
Set up a MongoDB cluster (e.g., using [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)) and configure the connection details in the `.env` file.

### 3. AWS EKS Cluster Setup

#### a. Create VPC for EKS Cluster
Use AWS VPC to create the necessary networking components for your EKS cluster.

#### b. Create EKS Cluster
Create an EKS cluster using the AWS console or CLI.

#### c. Create Node Group
Provision a node group in the EKS cluster using `t3.small` spot instances for cost efficiency.

### 4. Set Up EFS for Storage
Create an AWS Elastic File System (EFS) with a security group (SG) that allows access from your EKS cluster.

### 5. Deploy to Kubernetes

#### a. Create Kubernetes Secrets
Store MongoDB credentials as a Kubernetes secret:

```bash
kubectl create secret generic mongo-secret --from-env-file=.env
```

#### b. Apply Kubernetes Manifests
Deploy the following services using `kubectl`:

- **Auth Service**: Handles user authentication (signup, login).
- **Task Service**: Manages tasks for each user.

Use the following command to apply the manifests:

```bash
kubectl apply -f auth-service.yaml
kubectl apply -f auth-deployment.yaml
```

### 6. Load Balancers
The services will create AWS LoadBalancers for:

- **Task Service**: Manages tasks for users.
- **User Service**: Permits signup, login, and log storage in the EFS volume.

### 7. Data Storage
- **Users and Tasks**: Stored in MongoDB.
- **Logs**: Stored in AWS EFS using CSI pv.

### 8. Local Testing with Docker
You can test the services locally using `docker-compose`:

```bash
docker-compose up
```
