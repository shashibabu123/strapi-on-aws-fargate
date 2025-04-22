# 🚀 Strapi Deployment on AWS Fargate using Terraform

This project automates the deployment of a **Strapi CMS** application using **AWS Fargate**, managed entirely with **Terraform**. It is designed for **beginners** to learn how to use Terraform for deploying containerized applications on AWS with best practices.

---

## 📁 Project Structure

strapi-on-aws-fargate/ 
│ ├── .gitignore # Ignore Terraform state and cache files
├── README.md # This documentation
│ ├── main.tf # Root Terraform configuration ├── providers.tf # AWS provider configuration ├── variables.tf # Input variables ├── outputs.tf # Output values │ ├── iam.tf # IAM roles for ECS and task execution ├── ecs.tf # ECS Cluster, Task Definition, Fargate Service ├── alb.tf # Application Load Balancer, Target Group, Listener │ ├── terraform.tfstate* # Terraform state file (should not be committed) ├── terraform.tfstate.backup* # Backup of previous state (should not be committed) └── terraform.tfstate..backup # Auto backups by Terraform



## 🧠 Prerequisites

Before you begin, ensure you have the following installed:

- ✅ [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- ✅ [Terraform](https://developer.hashicorp.com/terraform/downloads)
- ✅ [Docker](https://www.docker.com/products/docker-desktop/)
- ✅ A running **Strapi Docker image** pushed to AWS ECR
- ✅ Basic understanding of AWS, Docker, and Terraform

---

## 🔐 AWS Setup

1. **Configure your AWS credentials**:

```bash
aws configure
Enter your:

AWS Access Key ID

AWS Secret Access Key

Default region (e.g. us-east-1)

Output format (e.g. json)

Ensure your ECR repository and Docker image are ready:

Example ECR URL:

118273046134.dkr.ecr.us-east-1.amazonaws.com/strapi-app
🔧 How It Works
This Terraform configuration will:

Create a VPC (if needed) or use an existing one

Create an ECS Cluster with Fargate

Define IAM roles and policies for ECS tasks

Configure a Load Balancer (ALB) with target groups and listeners

Deploy your Strapi Docker container as a Fargate service

Expose your service on port 80 via ALB

🚀 Deployment Steps
1. Clone the Repository

git clone https://github.com/shashibabu123/strapi-on-aws-fargate.git
cd strapi-on-aws-fargate
2. Initialize Terraform

terraform init
3. Review and Apply the Plan

terraform plan
terraform apply
Confirm with yes when prompted.

⚙️ Customize Variables
Edit the variables.tf file or use -var flags to provide:


variable "region" {
  default = "us-east-1"
}

variable "ecr_image_url" {
  description = "Your ECR Docker image URL"
  default     = "118273046134.dkr.ecr.us-east-1.amazonaws.com/strapi-app"
}
🌐 Accessing the App
Once Terraform finishes applying:

It will output the ALB DNS name.

Visit the DNS URL in your browser.

You should see the Strapi CMS running!

🧽 Clean Up
To destroy the resources and avoid charges:


terraform destroy
🛠️ Troubleshooting
Large Files: Avoid committing .terraform/ folder. It contains provider binaries that exceed GitHub’s 100MB limit.

Authentication Issues: Use git config --global credential.helper store or use SSH URLs for secure GitHub access.

ECR Access: Ensure the ECS Task Role has ECR pull permissions.

📚 Useful References
Strapi Documentation

Terraform AWS Provider Docs

AWS ECS Fargate

Docker to ECR Push Guide

🙌 Contributing
This is a learning project — feel free to fork it, improve it, or suggest enhancements!

📩 Author
Made with 💻 by Shashi Babu

