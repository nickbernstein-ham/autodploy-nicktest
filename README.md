# Autodeploy Nick Test Project

This repository contains a simple NGINX application that is automatically deployed to a Kubernetes cluster using Argo CD.  
The deployment is triggered by updating the `build-id` annotation in `manifests/deployment.yaml`.  
When the `build-id` changes, the GitHub Actions workflow builds a new Docker image, pushes it to Docker Hub, and updates the annotation.  
Argo CD detects the change, pulls the new image, and redeploys the application.

## Key Points

- **Updating the `build-id`**  
  Edit the `build-id` line in `manifests/deployment.yaml` to a new value (e.g., a timestamp or commit hash).  
  This change will cause the CI pipeline to rebuild the image and push it to Docker Hub.

- **Image Pull Policy**  
  The container image must be pulled on every deployment.  
  Ensure `imagePullPolicy: Always` is set in the deployment spec (it is already set in the provided manifest).

- **Ingress**  
  The application is exposed via an Ingress resource (`manifests/ingress.yaml`) that routes traffic from `autodeploy.hri.testinglab.tech` to the NGINX service.

- **Argo CD**  
  The Argo CD Application (`argo-deployment/autodeploy-argo.yaml`) watches the `manifests` directory in this repository and automatically syncs changes to the `nick-test` namespace.

## Usage

1. Commit a new `build-id` value to `manifests/deployment.yaml`.  
2. Push the change to GitHub.  
3. The CI pipeline will build and push a new Docker image.  
4. Argo CD will detect the updated image and redeploy the application.

Happy deploying!  
