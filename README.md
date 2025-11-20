# AWS ECS Fargate – Containerized Web App with ALB & Autoscaling

This project demonstrates how to deploy a **containerized web application** on  
**Amazon ECS (Fargate)** using a **public Docker image**, fronted by an  
**Application Load Balancer (ALB)** with **auto-scaling** enabled.

The architecture includes:
- A custom VPC with public & private subnets
- ALB in public subnets
- ECS tasks in private subnets
- NAT Gateway for outbound internet access
- IAM roles for ECS task execution
- CloudWatch Logs integration

---

## 🚀 Architecture Diagram

The generated architecture diagram is included here:

**File:**
<img width="1024" height="1024" alt="awsecs" src="https://github.com/user-attachments/assets/fb38c2f9-73f9-4d98-a32a-40501756208c" />


---

## 🏗️ Architecture Overview

### Components

| Component | Purpose |
|----------|---------|
| **VPC** | Isolated network environment |
| **Public Subnets** | Host ALB |
| **Private Subnets** | Host ECS Fargate tasks |
| **Internet Gateway** | Public traffic access |
| **NAT Gateway** | Allow private tasks to pull images / updates |
| **Application Load Balancer** | Routes traffic to ECS tasks |
| **Target Group (IP mode)** | Registers Fargate task IPs |
| **ECS Cluster (Fargate)** | Runs containerized application |
| **Task Definition** | Defines container image + resources |
| **IAM Roles** | Task execution & app permissions |
| **Auto Scaling** | Scales tasks based on CPU/Metrics |
| **CloudWatch Logs** | Collects container logs |

---

## 🐳 Container Image

You can use any **public Docker image**, including:

Example:
```bash
nginx:latest

//your own
docker.io/yourname/yourimage:latest

```

📘 Deployment Steps (Console)
1. Create VPC (recommended 2 public + 2 private subnets)
Use VPC → Create VPC → VPC and more:
Public subnets for ALB
Private subnets for ECS tasks
NAT gateway enabled

2. Create IAM Roles
ecsTaskExecutionRole (required)
ecsTaskRole (optional)

3. Create Application Load Balancer
Internet-facing
Listener on port 80
Target group (IP mode)

4. Create Security Groups
sg-alb → allow 80 from 0.0.0.0/0
sg-ecs-tasks → allow from ALB SG on container port

5. Create ECS Cluster (Fargate)
“Networking Only” cluster.

6. Create Task Definition
Launch type: Fargate
Image: public Docker Hub / ECR Public
Port mapping: 80
Logs via CloudWatch

7. Create ECS Service
Attach ALB & target group
Deploy in private subnets
Enable auto-scaling (CPU target = 50%)

8. Test Application
Use ALB DNS name:
http://<alb-dns-name>

🧹 Cleanup (Teardown)
Delete in this exact order to avoid dependency errors:
Delete ECS Service
Delete ECS Task Definitions
Delete ALB
Delete Target Group
Delete Security Groups
Delete CloudWatch Log Group (optional)
Delete IAM roles (optional)
Delete ECS Cluster
Delete NAT Gateway
Release Elastic IP
Delete Subnets
Delete Route Tables
Detach/Delete Internet Gateway
Delete VPC

