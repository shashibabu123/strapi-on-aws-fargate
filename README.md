### Documentation: Deploying Strapi Application on AWS Using ECS Spot Fargate Managed by Terraform

---

#### **Objective:**
This task aims to deploy a Strapi application on AWS using **ECS Spot Fargate**. The application is containerized using **Docker**, pushed to **Amazon ECR**, and managed entirely through **Terraform**. The infrastructure includes a **VPC**, **subnets**, **ECS Cluster**, **Security Groups**, **Application Load Balancer (ALB)**, and an **ECS service** configured for **Fargate launch type**.

---

### **Steps to Complete the Task:**

1. **Containerize the Strapi Application:**
   - The Strapi application is containerized using a **Dockerfile**. The Docker image is built, tested, and pushed to **Amazon ECR** (Elastic Container Registry).
   - **Docker Commands Used:**
     - Build the Docker image:
       ```bash
       docker build -t strapi-app .
       ```
     - Login to ECR:
       ```bash
       aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com
       ```
     - Tag and push the Docker image to ECR:
       ```bash
       docker tag strapi-app:latest <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/strapi-app
       docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/strapi-app
       ```

2. **Terraform Configuration:**
   The Terraform code is structured to provision all necessary AWS resources. Below are the key components:

   - **VPC, Subnets, and Networking:**
     - A **VPC** is created, along with **subnets**, **Internet Gateway**, and **Route Tables**.
   
   - **ECS Cluster:**
     - The **ECS Cluster** is created with **Fargate launch type** to run the Strapi application.

   - **IAM Role and Task Definition:**
     - An **ECS Task Definition** is written in Terraform using the Docker image pushed to ECR. The **IAM execution role** (for task execution) is specified to allow ECS to pull images from ECR and manage logs.

   - **ECS Service:**
     - The ECS service is configured to run the Strapi application with a **Spot Fargate** capacity provider. It uses the **ALB** (Application Load Balancer) for public access.

   - **Security Groups and ALB:**
     - **Security Groups** are created to control traffic to/from the application.
     - An **ALB** is created with listeners and target groups to route traffic to the ECS service.

   - **Outputs:**
     - The **public URL of the ALB** is output to access the Strapi application.

---

### **Terraform Code Breakdown:**

1. **VPC and Networking (vpc.tf):**
   - Creates the VPC, subnets, and necessary networking resources for ECS and ALB.

2. **ECS Cluster and Task Definition (ecs.tf):**
   - Defines the ECS cluster and ECS task definition to run the Strapi Docker container.
   - Specifies the **execution role ARN**, **Docker image** from ECR, and the container port `1337` (for Strapi).

3. **ECS Service (ecs-service.tf):**
   - Sets up the ECS service with **Fargate Spot** capacity provider.
   - Configures the security groups and networking for the service.

4. **ALB and Security Groups (alb.tf):**
   - Creates the ALB and its associated listener and target groups.
   - Configures the security groups to allow traffic from the internet (ports 80 and 1337).

---

### **Key Terraform Files:**

- **`alb.tf`**: Configures the **Application Load Balancer**, security groups, and listeners.
- **`ecs.tf`**: Defines the **ECS Task Definition** and **ECS Service**.
- **`variables.tf`**: Contains the input variables used across Terraform configuration files.
- **`main.tf`**: The entry point for Terraform configuration, including the resources for VPC, subnets, ECS cluster, and service.
- **`iam.tf`**: Configures **IAM roles and policies** for ECS task execution.
- **`providers.tf`**: Configures the AWS provider for Terraform.

---

### **Official Documentation Referenced:**

- **Amazon ECS Documentation:**
  - [ECS Overview](https://docs.aws.amazon.com/ecs/latest/developerguide/Welcome.html)
  - [Fargate Launch Type](https://docs.aws.amazon.com/ecs/latest/developerguide/AWS_Fargate.html)
  - [ECS Task Definition](https://docs.aws.amazon.com/ecs/latest/developerguide/task_definitions.html)

- **Terraform AWS Provider Documentation:**
  - [AWS ECS Service Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecs_service)
  - [AWS Security Group Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)
  - [AWS Load Balancer (ALB) Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lb)

- **ECR Documentation:**
  - [Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

---

### **Output:**
After running `terraform apply`, the public URL of the ALB is output, and you can access your Strapi application through the browser at `http://<ALB-public-DNS>:1337`.

---

### **Conclusion:**
By following this approach, you can efficiently deploy a Strapi application on AWS using ECS Fargate Spot with the entire infrastructure managed via Terraform. This solution leverages various AWS services such as ECS, ECR, ALB, and IAM roles, enabling a scalable and cost-effective deployment.


