# GCP Kubernetes API Deployment with Terraform

This project demonstrates how to create and deploy a simple API to Google Cloud Platform (GCP) using Kubernetes (GKE) with Infrastructure as Code (IaC) exclusively through Terraform. The API returns the current time in JSON format when accessed via a GET request. Continuous Deployment (CD) is implemented using GitHub Actions.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Configure Google Cloud SDK](#2-configure-google-cloud-sdk)
  - [3. Build and Push Docker Image](#3-build-and-push-docker-image)
  - [4. Initialize and Apply Terraform Configuration](#4-initialize-and-apply-terraform-configuration)
  - [5. Verify Deployment](#5-verify-deployment)
- [CI/CD Pipeline](#cicd-pipeline)
- [Clean Up](#clean-up)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before starting, ensure you have the following tools installed:
- [Docker](https://www.docker.com/get-started)
- [Google Cloud SDK](https://cloud.google.com/sdk/docs/install)
- [Terraform](https://www.terraform.io/downloads.html)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [gke-gcloud-auth-plugin](https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl#install_plugin)

## Project Structure

```plaintext![API Time](https://github.com/user-attachments/assets/1c219fee-0096-486d-8110-ea1e63880cee)

├── main.tf                   # Terraform configuration for GKE and Kubernetes resources
├── kubernetes.tf             # Kubernetes Deployment and Service configuration
├── Dockerfile                # Dockerfile for building the API container
├── app.py                    # Python Flask API that returns the current time
├── requirements.txt          # Python dependencies
├── .github/workflows/deploy.yml  # GitHub Actions workflow for CI/CD
└── README.md                 # Project README file














Setup Instructions

1. Clone the Repository
Clone this repository to your local machine:

bash
Copy code
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
2. Configure Google Cloud SDK
Authenticate with Google Cloud:
bash
Copy code
gcloud auth login
Set up kubectl to use your GKE cluster:
bash
Copy code
gcloud container clusters get-credentials <cluster-name> --region <region> --project <project-id>
Ensure the gke-gcloud-auth-plugin is installed:
bash
Copy code
gcloud components install gke-gcloud-auth-plugin
3. Build and Push Docker Image
Build the Docker image:
bash
Copy code
docker build -t gcr.io/<project-id>/my-api:latest .
Push the image to Google Container Registry (GCR):
bash
Copy code
docker push gcr.io/<project-id>/my-api:latest
4. Initialize and Apply Terraform Configuration
Initialize Terraform:
bash
Copy code
terraform init
Apply the Terraform configuration:
bash
Copy code
terraform apply
Confirm the action when prompted.
5. Verify Deployment
Once Terraform has completed, verify that the API is running:
bash
Copy code
kubectl get pods
kubectl get svc
Access the API:
Use the external IP from the kubectl get svc output and access the API endpoint: http://<external-ip>:8080/time
CI/CD Pipeline

This project uses GitHub Actions to automate the deployment process. The pipeline:

Builds the Docker image
Pushes the image to Google Container Registry
Deploys the Kubernetes resources using Terraform
Setting up Secrets in GitHub
Ensure that the following secrets are set in your GitHub repository:

GCP_PROJECT_ID
GCP_SA_KEY (Service Account Key JSON)
Clean Up

To clean up resources created by Terraform:

bash
Copy code
terraform destroy
