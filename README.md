# 🚀 E-Commerce Production Infrastructure on AWS (CloudFormation)

This project defines a **fully automated, production-ready AWS infrastructure** for an e-commerce application using **AWS CloudFormation (Infrastructure as Code)**.

It provisions networking, compute, storage, security, monitoring, scaling, and load balancing components in a single template.

---

## 📌 Architecture Overview

The infrastructure includes:

- Custom VPC with public and private subnets
- Internet Gateway & NAT Gateways
- Application Load Balancer (ALB)
- EC2 instances (private subnets)
- Auto Scaling Group with Launch Template
- DynamoDB table
- S3 bucket (versioned)
- IAM roles & instance profiles
- CloudWatch monitoring & alarms
- SNS alerts
- VPC endpoints for S3 & DynamoDB

---

## 🏗️ AWS Services Used

- Amazon VPC
- Amazon EC2
- Elastic Load Balancing (ALB)
- Auto Scaling Groups
- Amazon DynamoDB
- Amazon S3
- AWS IAM
- Amazon CloudWatch
- Amazon SNS
- VPC Endpoints

---

## 🌐 Network Design

### VPC
- CIDR: `10.0.0.0/16`
- DNS Support: Enabled
- DNS Hostnames: Enabled

### Subnets
| Type       | Subnet | CIDR         |
|------------|--------|-------------|
| Public 1   | AZ-1   | 10.0.1.0/24 |
| Public 2   | AZ-2   | 10.0.2.0/24 |
| Private 1  | AZ-1   | 10.0.3.0/24 |
| Private 2  | AZ-2   | 10.0.4.0/24 |

---

## 🖥️ Compute Layer

### EC2 Instances
- Instance Type: `t2.micro`
- AMI: Amazon Linux (AMI: `ami-04b4f1a9cf54c11d0`)
- Deployed in private subnets
- Protected by Security Groups

### Auto Scaling Group
- Min: 1
- Max: 3
- Desired: 1
- Uses Launch Template for consistency
- Automatically registers with ALB

---

## ⚖️ Load Balancing

### Application Load Balancer (ALB)
- Internet-facing
- Spans 2 public subnets
- Routes traffic to EC2 instances via Target Group
- Health check enabled on `/`

---

## 🔐 Security

### Security Group Rules
- SSH (22): Open to all (0.0.0.0/0)
- HTTP (80): Open to all (0.0.0.0/0)
- All outbound traffic allowed

### IAM Role
EC2 instances are granted:
- Full DynamoDB access
- S3 access (GetObject, PutObject, ListBucket)

---

## 📦 Storage

### DynamoDB
- Table: `ecom-prod-dynamodb`
- Primary Key: `id` (String)
- On-demand billing mode
- Server-side encryption enabled

### S3 Bucket
- Bucket Name: `ecom-prod-s3-bucket`
- Versioning enabled
- Private access only

---

## 📊 Monitoring & Alerts

### CloudWatch Alarms
- CPU > 75% → Scale Up
- CPU < 30% → Scale Down
- ALB requests > 100,000 → Alert

### SNS Notifications
- Email alerts sent to:
  22ug2-0190M@sltc.edu.lk

---

## 📈 Auto Scaling Policies

- Scale Up: +1 instance
- Scale Down: -1 instance
- Cooldown: 300 seconds

---

## 🔗 VPC Endpoints

- S3 Gateway Endpoint (secure S3 access without internet)
- DynamoDB Gateway Endpoint (private access from VPC)

---

## 📤 Outputs

After deployment, CloudFormation provides:

- ALB URL
- DynamoDB Table Name
- S3 Bucket Name

---

## 🚀 Deployment Instructions

### Deploy Stack
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name ecom-prod-stack \
  --capabilities CAPABILITY_NAMED_IAM

---

## 🧠 Key Features

- Fully Infrastructure-as-Code (IaC)
- Highly scalable architecture
- Secure private subnet compute layer
- Production-grade monitoring
- Automated scaling based on load
- Centralized logging & alerts

---

## ⚠️ Notes

- Ensure the AMI ID is valid in your AWS region
- Replace email in SNS subscription before deployment
- SSH access is open for demo purposes (restrict in production)

---

## 👨‍💻 Author

Charith Hewage  
Cloud and DevOps Architect 

Malindi Ratnayake
Cloud and DevOps Co-Architect

[mali ratnayake github](https://github.com/MaliRatnayake)

Achin Ovinda
Cloud networking 
[Achin Ovinda](https://github.com/ovinda123)

---

## 📜 License

This project is for educational and demonstration purposes.
