# GKE Infrastructure with Terraform — Portfolio Case

## Overview

This project demonstrates the design and implementation of a Google Kubernetes Engine infrastructure using Terraform, with a focus on reusable modules, environment-based configuration, private networking, and CI/CD readiness.

The goal of this case is to show how a cloud-native infrastructure repository can be structured to provision and manage a production-ready GKE cluster in a controlled, scalable, and maintainable way.

> This is a portfolio project based on real-world DevOps and Platform Engineering practices. Sensitive company data, internal names, and private configurations were intentionally removed or replaced with generic examples.

---

## Objective

The main objective was to create a Terraform-based structure capable of provisioning a GKE cluster using a reusable module approach.

The solution was designed to support:

* Google Kubernetes Engine cluster provisioning
* Shared VPC integration
* Environment-based configuration using `.tfvars`
* Private cluster configuration
* Node pool management
* Secondary IP ranges for Pods and Services
* CI/CD pipeline execution readiness
* Clear repository structure for future add-ons and platform components

---

## Technologies Used

* Google Cloud Platform
* Google Kubernetes Engine
* Terraform
* Terraform Modules
* GCS Remote State
* Kubernetes
* Git
* CI/CD Pipeline Integration
* Shared VPC
* IAM and Service Accounts

---

## Repository Structure

```bash
.
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
├── backend.tf
├── dev.tfvars
├── hml.tfvars
├── prod.tfvars
└── versions.tf
```

---

## Architecture Summary

The repository was designed to consume a reusable Terraform module responsible for creating and managing the GKE cluster.

The environment repository contains only the configuration required for each environment, while the reusable module contains the core logic for cluster provisioning.

This separation helps maintain a cleaner infrastructure architecture and makes it easier to reuse the same module across multiple clusters or environments.

---

## Key Implementation Points

### 1. Environment-Based Configuration

Each environment has its own `.tfvars` file, allowing the same Terraform codebase to be reused across different stages.

Example:

```hcl
environment = "dev"

project_id   = "example-dev-project"
region       = "us-central1"
zones        = ["us-central1-a"]
cluster_name = "gke-data-platform-dev"

network_name       = "shared-vpc-dev"
subnetwork_name    = "subnet-gke-dev"
network_project_id = "shared-vpc-host-project"

pods_secondary_range_name     = "gke-pods-range"
services_secondary_range_name = "gke-services-range"

deletion_protection = false
```

---

### 2. Shared VPC Integration

The cluster was designed to consume an existing Shared VPC, where the network is managed in a separate host project.

This approach is commonly used in enterprise environments to centralize network governance while allowing service projects to deploy workloads.

The configuration includes:

* Host project reference
* Existing VPC name
* Existing subnet name
* Secondary ranges for Kubernetes Pods and Services

---

### 3. Reusable Terraform Module

The environment repository consumes a reusable GKE module using a remote source.

Example:

```hcl
module "gke" {
  source = "git::https://github.com/example-org/tf-module-gke-cluster.git?ref=0.1.0"

  project_id   = var.project_id
  region       = var.region
  zones        = var.zones
  cluster_name = var.cluster_name

  network_name       = var.network_name
  subnetwork_name    = var.subnetwork_name
  network_project_id = var.network_project_id

  pods_secondary_range_name     = var.pods_secondary_range_name
  services_secondary_range_name = var.services_secondary_range_name

  deletion_protection = var.deletion_protection
}
```

This approach improves standardization, reduces duplication, and allows multiple clusters to follow the same infrastructure pattern.

---

### 4. Terraform Code Corrections

During the implementation, several Terraform issues were identified and corrected, including:

* Incorrect variable references
* Mismatched variable types
* Invalid output references
* Workspace-based logic replaced by `.tfvars` configuration
* Missing provider configurations
* Module source and versioning adjustments
* Plan failures caused by inconsistent inputs

These corrections improved the reliability of the Terraform plan and made the repository easier to maintain.

---

### 5. CI/CD Readiness

The repository was structured to be executed through a CI/CD pipeline.

The expected pipeline flow includes:

1. Pull Request creation
2. Terraform format validation
3. Terraform initialization
4. Terraform validation
5. Terraform plan
6. Manual approval
7. Terraform apply

This allows infrastructure changes to be reviewed, validated, and applied in a controlled way.

---

## Challenges

Some of the main challenges addressed in this project were:

* Adapting an infrastructure pattern from another cloud provider to GCP
* Keeping the repository simple while maintaining enterprise standards
* Integrating with an existing Shared VPC model
* Handling Terraform module versioning
* Fixing Terraform plan failures caused by inconsistent variables and module expectations
* Preparing the repository for pipeline-based execution

---

## Results

The final structure provides a clean foundation for managing a GKE cluster using Terraform.

Main outcomes:

* Reusable Terraform module consumption
* Environment-specific configuration through `.tfvars`
* Improved Terraform plan reliability
* Clear separation between environment repository and reusable module
* Initial structure ready for CI/CD pipeline execution
* Foundation prepared for future Kubernetes add-ons such as monitoring, secrets management, ingress, and autoscaling components

---

## What This Project Demonstrates

This case demonstrates practical experience with:

* Infrastructure as Code
* GKE cluster provisioning
* Terraform module design
* Cloud networking concepts
* Shared VPC usage
* CI/CD-oriented infrastructure delivery
* Kubernetes platform engineering
* Troubleshooting Terraform plan and module issues
* Enterprise-style repository organization

---

## Next Improvements

Possible future improvements include:

* Add Kubernetes add-ons using Terraform Helm provider
* Add observability stack integration
* Add workload identity configuration
* Add cluster autoscaling configuration
* Add policy and security components
* Add automated pipeline examples
* Add architecture diagram

---

## Author

DevOps Engineer focused on Kubernetes, Terraform, Cloud Infrastructure, CI/CD, and Platform Engineering.

Experience with AWS, GCP, Kubernetes, Terraform, observability, automation, and infrastructure troubleshooting in enterprise environments.

