# k8s-jkns-Ans
 Dockerized Application Deployment with Jenkins and Kubernetes

# Overview
 This project automates the process of building, tagging, and deploying a Dockerized application to a Kubernetes cluster using Jenkins and Ansible. The application consists of an Nginx web server, and the process involves building a Docker image, pushing it to Docker Hub, preparing Kubernetes deployment files, and deploying the app to a Kubernetes cluster.

## Tools Used

### Docker
Used to containerize the application (Nginx web server).
Docker images are built and pushed to Docker Hub for later use in Kubernetes.


### Jenkins
Automates the build, push, and deployment pipeline.
Jenkins parameters allow flexibility in building and deploying different versions of the application.

### Ansible
Handles the deployment on Kubernetes, ensuring the application is deployed correctly based on the prepared Kubernetes configuration.


### Kubernetes (Minikube)
The cluster where the application is deployed.
Minikube is a lightweight Kubernetes solution used for local development and testing.
