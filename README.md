# Kubernetes Deployment for Node.js Hello World App

## Overview
This repository contains Kubernetes manifests and a Helm chart to deploy a simple Node.js Hello World application. The setup prioritizes high availability and security best practices, using the following Kubernetes resources:

- **ConfigMap**: Stores application settings and mounts them inside the container.
- **Deployment**: Runs three replicas of the application on different nodes to ensure uptime and load distribution.
- **Ingress**: Exposes the application through a specified hostname and path.
- **Service**: Handles internal networking and load balancing.
- **Pod Disruption Budget (PDB)**: Ensures that at least two instances of the application remain running during voluntary disruptions.

## Prerequisites
Before deploying, make sure you have:
- Docker installed
- `kubectl` installed and configured
- A running Kubernetes cluster (for local testing, use **Minikube** or **Kind**)
- (Optional) `Helm` installed if you plan to use Helm for deployment

## Running Locally
If you want to test the application locally without Kubernetes, you can run it with Docker:

1. Pull the Docker image:
   ```sh
   docker pull pvermeyden/nodejs-hello-world
   ```
2. Run the container:
   ```sh
   docker run -p 8080:80 pvermeyden/nodejs-hello-world
   ```
3. Open your browser or use curl to access the app:
   ```sh
   curl http://localhost:8080
   ```

## Deploying with Kubernetes Manifests
1. Start a local Kubernetes cluster (if needed):
   ```sh
   minikube start
   ```
2. Apply the ConfigMap:
   ```sh
   kubectl apply -f manifests/configmap.yaml
   ```
3. Deploy the application:
   ```sh
   kubectl apply -f manifests/deployment.yaml
   ```
4. Create the service:
   ```sh
   kubectl apply -f manifests/service.yaml
   ```
5. Set up ingress to expose the application:
   ```sh
   kubectl apply -f manifests/ingress.yaml
   ```
6. Apply the Pod Disruption Budget:
   ```sh
   kubectl apply -f manifests/pdb.yaml
   ```
7. If using Minikube, enable the ingress controller:
   ```sh
   minikube addons enable ingress
   ```

## Deploying with Helm
If you prefer Helm, follow these steps:
1. Install the Helm chart:
   ```sh
   helm install hello-world helm-chart/
   ```
2. Upgrade the deployment when updating configurations:
   ```sh
   helm upgrade hello-world helm-chart/
   ```
3. Remove the deployment if needed:
   ```sh
   helm uninstall hello-world
   ```

## Accessing the Application
For local testing with Minikube, use:
```sh
minikube service nodejs-hello-world-service --url
```

## Cleaning Up
To remove all resources, use:
```sh
kubectl delete -f manifests/
```
Or, if you deployed using Helm:
```sh
helm uninstall hello-world
```

## Additional Information
- The application runs on the image: `pvermeyden/nodejs-hello-world`
- It follows security best practices by running as a non-root user.
- Readiness and liveness probes help maintain application health and reliability.
