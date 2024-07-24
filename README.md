# Wisecow Application Deployment on Kubernetes

## Objective

To containerize and deploy the Wisecow application, hosted in the GitHub repository, on a Kubernetes environment with secure TLS communication. In this deployment, we are using a local Kubernetes cluster, and the application will run on localhost.

## Getting Started

To run this project on your local machine, follow these steps:

### Installation

Install `fortune-mod` and `cowsay` on Debian-based systems like Ubuntu using the following commands:

```bash
sudo apt update
sudo apt install fortune-mod cowsay -y
```
## Dockerization

### Create Dockerfile

Create a `Dockerfile` and navigate to the path of the file. Use the following command to build the Docker image:

```sh
docker build -t wisecow-image .
```
### Push Docker Image to Docker Hub

Create a repository/container registry on Docker Hub and push the image with a proper tag:

```sh
docker push yourRepositoryName/wisecow_image:latest
```
## Kubernetes Deployment

### Create Deployment and Service Manifests

Create Kubernetes deployment manifest files for deploying the Wisecow application in a Kubernetes environment, for example:

#### `wisecow-app-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wisecow-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wisecow
  template:
    metadata:
      labels:
        app: wisecow
    spec:
      containers:
      - name: wisecow-container
        image: yourRepositoryName/wisecow_image:latest
        ports:
        - containerPort: 80
```
#### `wisecow-app-service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: wisecow-service
spec:
  selector:
    app: wisecow
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```
### Deploy to Kubernetes

Apply the deployment and service using the following commands:

```sh
kubectl apply -f wisecow-app-deployment.yaml
kubectl apply -f wisecow-app-service.yaml
```
### Check deployments and services running

Use the following commands to check the status of deployments and services:

```sh
kubectl get deployments
kubectl get services
```
## Continuous Integration and Deployment

1. Create a GitHub workflow:
   - Create a YAML file in `./github/workflows`.
   - Set the secrets `DOCKER_USERNAME` and `DOCKER_PASSWORD` with your Docker Hub username and Docker image name.

Example workflow:

```yaml
name: CI/CD

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Build Docker image
      run: docker build -t yourRepositoryName/wisecow_image:${{ github.sha }} .

    - name: Log in to Docker Hub
      run: echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin

    - name: Push Docker image
      run: docker push yourRepositoryName/wisecow_image:${{ github.sha }}
```
## TLS Implementation

1. Install OpenSSL:

   ```sh
   sudo apt-get install openssl
```
2.Generate Private Key:
   
    ```sh
    openssl genrsa -out server.key 2048
```
3.Generate Certificate Signing Request (CSR):

      ```sh
    openssl req -new -key server.key -out server.csr
```
4.Self-Sign Certificate:   

     ```sh
     openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```
5.Create Kubernetes Secret:

   ```sh
   kubectl create secret tls tls-secret --cert=server.crt --key=server.key
```
### Output 

After following the above steps, the Wisecow application should be running on your local Kubernetes cluster with secure TLS communication.
     


