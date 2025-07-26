# DevOps-Master-Project

AWS Cloud Assignment: CodePipeline using Terraform & DevSecOps with GitHub Actions and
Kubernetes Sealed Secrets
#### Task 1: CodePipeline using Terraform
Use Terraform to provision an AWS CodePipeline.
The pipeline must include:
• Source stage connected to GitHub or CodeCommit
• Build stage using AWS CodeBuild
• Deploy stage using AWS CodeDeploy or EC2
Define all required infrastructure using Terraform:
• CodePipeline, CodeBuild, CodeDeploy
• IAM roles and policies
• S3 buckets for artifact storage
 Use Terratest to write infrastructure tests and validate the Terraform modules.
Apply Terraform to create the pipeline and confirm successful setup.

#### Task 2: DevSecOps Integration using GitHub Actions and Sealed Secrets
Use GitHub Actions to automate the CI/CD workflow.
Integrate security scanning into the workflow using:
• `tfsec` for Terraform code
• `Trivy` for Docker image scanning
• Use Sealed Secrets to manage Kubernetes secrets securely before deploying workloads.
• Store secrets in encrypted SealedSecrets format and apply them to the Kubernetes cluster
as part of the deployment pipeline.
Ensure GitHub Actions workflow:
• Executes on each code push
• Runs security scans
• Applies SealedSecrets
• Triggers deployment to Kubernetes or updates infrastructure via Terraform


---


# 🚀 AWS CodePipeline with Terraform (CI/CD) | DevSecOps Project

This repository provisions a full AWS CI/CD pipeline using **Terraform** for infrastructure as code (IaC) and **Terratest** for infrastructure testing. The pipeline includes:

- Source stage (GitHub or CodeCommit)
- Build stage (AWS CodeBuild)
- Deploy stage (AWS CodeDeploy or EC2)
- IAM roles and policies
- S3 bucket for pipeline artifacts

## 📌Task 1: CodePipeline using Terraform

### ✅ Features

- AWS CodePipeline with GitHub trigger
- AWS CodeBuild to build & package app
- AWS CodeDeploy to EC2
- IAM roles with least privilege
- S3 bucket for artifact storage
- Terraform infrastructure as code
- Infrastructure tests using Terratest (Golang)
  
```markdown
## 🧱 Project Structure

DevSecOps-Masters-Project/
├── terraform/
│   ├── main.tf                # Main Terraform configuration
│   ├── variables.tf           # Input variables
│   ├── outputs.tf             # Output values
│   ├── backend.tf             # S3 remote state (optional)
│   └── versions.tf            # Required provider versions
├── test/
│   └── pipeline\_test.go       # Terratest validation
├── buildspec.yml              # Build configuration for CodeBuild
└── README.md

```
---

## 🔧 Prerequisites

- AWS CLI configured
- Terraform >= 1.4
- Golang (for Terratest)
- GitHub Repository connected via CodeStar
- EC2 instance with CodeDeploy Agent installed
- IAM user with admin access or fine-grained permissions

---

## 🚀 Deployment Guide

### Step 1: Clone the Repository

```bash
git clone https://github.com/<your-username>/DevSecOps-Masters-Project.git
cd DevSecOps-Masters-Project/terraform
````

---

### Step 2: Update Terraform Variables

Edit `variables.tf` or create a `terraform.tfvars` file with:

```hcl
github_repo_name   = "your-repo-name"
github_owner       = "your-github-username"
codestar_connection_arn = "arn:aws:codestar-connections:..."
ec2_instance_id    = "i-xxxxxxx"
region             = "ap-south-1"
```

---

### Step 3: Initialize Terraform

```bash
terraform init
```

---

### Step 4: Plan & Apply Infrastructure

```bash
terraform plan
terraform apply -auto-approve
```

This creates:

* S3 bucket for artifacts
* IAM roles for CodePipeline, CodeBuild, CodeDeploy
* CodePipeline with 3 stages: Source → Build → Deploy

---

### Step 5: Trigger the Pipeline

Push code to your GitHub repository. This automatically triggers the pipeline via CodeStar connection.

---

## 🧪 Infrastructure Testing with Terratest

### Step 1: Go to the `test/` directory

```bash
cd ../test
```

### Step 2: Run Tests

```bash
go test -v pipeline_test.go
```

The test will:

* Validate Terraform initialization and apply
* Check if CodePipeline and other resources exist

---

## 📦 Cleanup Resources

```bash
cd ../terraform
terraform destroy -auto-approve
```

---

## 📚 References

* [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
* [AWS CodePipeline Docs](https://docs.aws.amazon.com/codepipeline/)
* [Terratest Docs](https://terratest.gruntwork.io/)

---

---
## 📌Task 2: DevSecOps Integration using GitHub Actions and Sealed Secrets

# 🔐 DevSecOps Integration using GitHub Actions & Sealed Secrets

This part of the project integrates **DevSecOps practices** into the CI/CD workflow using **GitHub Actions**, focusing on security, automation, and secret management for Kubernetes deployments.

---


| Feature                     | Tool/Tech Used              |
|----------------------------|-----------------------------|
| CI/CD Workflow             | GitHub Actions              |
| Terraform Security Scanning| `tfsec`                     |
| Docker Image Scanning      | `Trivy`                     |
| Secrets Management         | Sealed Secrets (kubeseal)   |
| Kubernetes Deployment      | kubectl + GitHub Actions    |
| IaC Deployment             | Terraform via CI/CD         |

---
---
```markdown
### 📂 Project Structure

DevSecOps-Masters-Project/
├── .github/workflows/
│   └── devsecops-pipeline.yml       # GitHub Actions CI/CD
├── terraform/                       # Terraform IaC
├── docker/
│   └── Dockerfile                   # Containerized app
├── k8s/
│   ├── manifests/                   # Kubernetes YAMLs
│   │   └── deployment.yaml
│   └── secrets/
│       ├── db-creds.yaml            # Encrypted SealedSecret
│       └── sealed-secret.yaml
└── README.md
````

---

## ⚙️ GitHub Actions CI/CD Pipeline

Located at: `.github/workflows/devsecops-pipeline.yml`

### Workflow Steps:

1. **Trigger**
   - On every `push` to `main` branch
2. **Terraform Security Scan**
   - Run `tfsec` on Terraform codebase
3. **Docker Image Security Scan**
   - Use `Trivy` to scan image vulnerabilities
4. **Build & Push Docker Image**
   - Build image from Dockerfile and push to Docker Hub
5. **Sealed Secrets Deployment**
   - Decrypt and apply Kubernetes `SealedSecrets`
6. **Kubernetes Deployment**
   - Apply K8s manifests to the cluster
7. **Terraform Apply (Optional)**
   - Apply infra changes if any

---

## 🔒 Sealed Secrets Setup

### Step 1: Encrypt Kubernetes Secret

```bash
kubectl create secret generic db-creds \
  --from-literal=MONGO_URL="your-mongo-url" \
  --dry-run=client -o yaml > k8s/secrets/db-creds.yaml
```

### Step 2: Seal the Secret

```bash
kubeseal --cert cert.pem \
  --format yaml < k8s/secrets/db-creds.yaml > k8s/secrets/sealed-secret.yaml
```

### Step 3: Apply Sealed Secret in CI

```yaml
- name: Apply Sealed Secrets
  run: kubectl apply -f k8s/secrets/sealed-secret.yaml
```

---

## 🛡️ Security Scanning Tools

### `tfsec`: Terraform Security

* **Install locally:** `brew install tfsec`
* **In CI:**

  ```yaml
  - name: tfsec scan
    run: tfsec ./terraform
  ```

### `Trivy`: Docker Image Scan

* **Install locally:** `brew install aquasecurity/trivy/trivy`
* **In CI:**

  ```yaml
  - name: Trivy scan
    run: trivy image your-dockerhub/image:tag
  ```

---

## 🚀 Deployment

### On Code Push:

* The CI/CD pipeline runs all scans
* Deploys sealed secrets
* Applies K8s manifests
* Optionally runs `terraform apply`

---

## 🧪 Local Testing (Optional)

```bash
# Terraform security
tfsec ./terraform

# Trivy Docker image scanning
docker build -t my-image .
trivy image my-image
```

---

## 🧼 Cleanup

```bash
kubectl delete -f k8s/secrets/sealed-secret.yaml
kubectl delete -f k8s/manifests/deployment.yaml
```
Here’s a clean **DevSecOps CI/CD Workflow Flowchart** for **Task 2**, covering all components: GitHub Actions, security scanning (tfsec, Trivy), Sealed Secrets, Terraform, and Kubernetes deployment.

---

### 📊 **Final DevSecOps Workflow Diagram**

```plaintext
DevSecOps-Masters-Project/

├── .github/

│   └── workflows/

│       └── devsecops-pipeline.yml        # GitHub Actions CI/CD

├── terraform/                            # All Terraform code here

│   ├── main.tf

│   ├── variables.tf

│   ├── outputs.tf

│   └── test/

│       └── pipeline\_test.go              # Terratest for pipeline

├── docker/

│   └── Dockerfile                        # Dockerfile for app image

├── k8s/

│   ├── manifests/

│   │   └── deployment.yaml               # K8s Deployment YAML

│   └── secrets/

│       └── sealed-secret.yaml            # Sealed Secrets

├── README.md

└── .gitignore

```

---

### 🔍 Flowchart Description

| Step                     | Description                                         |
| ------------------------ | --------------------------------------------------- |
| **1. Git Push**          | Developer pushes code to GitHub                     |
| **2. Trigger CI**        | GitHub Actions workflow starts                      |
| **3. tfsec Scan**        | Scans Terraform code for security misconfigurations |
| **4. Trivy Scan**        | Checks Docker image for vulnerabilities             |
| **5. Sealed Secrets**    | Decrypt and apply Kubernetes secrets securely       |
| **6. Docker Build**      | Build and push image to Docker Hub                  |
| **7. Kubernetes Deploy** | Apply manifests to deploy app on cluster            |


## 🙋‍♂️ Author

**Kasturi Pattnayak**



---

## 📄 License

This project is licensed under the MIT License.

