# Coworking Space Service - Analytics API

## Overview
The Coworking Space Service Analytics API provides business analysts with basic analytics data on user activity. This API is deployed as a microservice on AWS EKS using a CI/CD pipeline with AWS CodeBuild and Amazon ECR.

## Architecture
- **Database:** PostgreSQL deployed on Kubernetes with persistent storage (gp2 EBS)
- **Application:** Python Flask API containerized with Docker
- **Container Registry:** Amazon ECR hosts the Docker images
- **CI/CD:** AWS CodeBuild automatically builds and pushes Docker images on code changes
- **Orchestration:** AWS EKS manages the Kubernetes cluster
- **Monitoring:** AWS CloudWatch Container Insights provides logging and monitoring

## Deployment Process
1. Code is pushed to the GitHub repository
2. AWS CodeBuild detects changes and triggers a build using `buildspec.yml`
3. CodeBuild builds the Docker image and pushes it to Amazon ECR with semantic versioning
4. Kubernetes deployment is updated with the new image URI from ECR
5. Running `kubectl apply -f` on the deployment YAML deploys the updated application

## Releasing New Builds
To release a new version, push code changes to the main branch. Trigger a CodeBuild build, then update the image tag in `deployment/coworking.yaml` and apply with `kubectl apply -f deployment/coworking.yaml`.

## Configuration
- **ConfigMap:** Stores DB_HOST, DB_PORT, DB_USERNAME, DB_NAME
- **Secret:** Stores DB_PASSWORD securely
- **Environment variables** are injected into the container via `envFrom` and `env` in the deployment YAML

## Stand-Out Suggestions
**Resource Allocation:** The deployment specifies resource requests (cpu: 256m, memory: 128Mi) and limits (cpu: 500m, memory: 500Mi) to ensure reasonable resource consumption per pod.

**Recommended Instance Type:** A t3.small instance is ideal for this workload as it provides 2 vCPUs and 2 GiB memory, balancing cost and performance for a lightweight analytics API with moderate traffic.

**Cost Savings:** Use EC2 Spot Instances for non-critical workloads, right-size node instances based on actual usage, scale down nodes during off-hours, and leverage AWS Savings Plans for sustained usage discounts.