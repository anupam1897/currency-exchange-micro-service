# Currency Exchange Microservice

This project sets up a complete CI/CD pipeline for deploying a Currency Exchange Microservice using Azure DevOps, Terraform, and Kubernetes. The microservice provides currency exchange rate information and is designed to be scalable and resilient.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Configure Azure DevOps](#2-configure-azure-devops)
  - [3. Setup Terraform](#3-setup-terraform)
  - [4. Configure Kubernetes](#4-configure-kubernetes)
  - [5. Deploy the Microservice](#5-deploy-the-microservice)
- [Usage](#usage)

## Overview

This repository contains everything you need to deploy a Currency Exchange Microservice using a modern DevOps pipeline. It leverages the following technologies:

- **Azure DevOps**: For CI/CD pipelines
- **Terraform**: For infrastructure as code
- **Kubernetes**: For container orchestration

## Architecture

![Architecture Diagram](path_to_architecture_diagram)

1. **Source Code Management**: Managed in GitHub.
2. **CI/CD Pipelines**: Managed in Azure DevOps.
3. **Infrastructure Provisioning**: Handled by Terraform, creating necessary Azure resources.
4. **Container Orchestration**: Managed by Kubernetes, deploying the microservice as a set of pods.

## Prerequisites

Before you begin, ensure you have the following:

- An Azure account
- Azure DevOps organization setup
- Kubernetes cluster (AKS recommended)
- Terraform installed on your local machine
- Kubectl installed on your local machine

## Setup Instructions

### 1. Clone the Repository

Clone this repository to your local machine:

```sh
git clone https://github.com/yourusername/currency-exchange-microservice.git
cd currency-exchange-microservice
```

### 2. Configure Azure DevOps

1. **Create a new project** in Azure DevOps.
2. **Import the repository** into Azure DevOps Repos.
3. **Set up service connections** for Azure and Kubernetes.
4. **Create a new pipeline** using the `azure-pipelines.yml` file provided in this repository.

### 3. Setup Terraform

1. Navigate to the `terraform` directory:

    ```sh
    cd terraform
    ```

2. Initialize Terraform:

    ```sh
    terraform init
    ```

3. Create a `terraform.tfvars` file and add your Azure credentials and other required variables:

    ```hcl
    subscription_id = "your_subscription_id"
    client_id       = "your_client_id"
    client_secret   = "your_client_secret"
    tenant_id       = "your_tenant_id"
    resource_group  = "your_resource_group"
    location        = "your_azure_location"
    ssh_public_key  = "path_to_your_ssh_public_key"
    ```

4. Or Apply the Terraform configuration to provision resources using the following command in Azure DevOps pipeline:

    ```sh
    terraform apply -var client_id=$(client_id) -var client_secret=$(client_secret) -var ssh_public_key=$(publickey.secureFilePath)
    ```

### 4. Configure Kubernetes

1. Ensure your Kubernetes context is set to the correct cluster:

    ```sh
    kubectl config use-context your-cluster-name
    ```

2. Apply Kubernetes manifests:

    ```sh
    kubectl apply -f kubernetes/
    ```

### 5. Deploy the Microservice

Once the infrastructure is in place and Kubernetes is configured, the Azure DevOps pipeline will handle the build, test, and deployment of the microservice.

## Usage

After deployment, the Currency Exchange Microservice will be accessible via the external IP of the Kubernetes service. You can find this IP using:

```sh
kubectl get svc currency-exchange-service
```

- Access the service at
http://[service-ip]/currency-exchange/from/USD/to/INR

- Example response:
```
{
  "id": 10001,
  "from": "USD",
  "to": "INR",
  "conversionMultiple": 65.00,
  "environmentInfo": "NA"
}
```