# 🏥 Sunvista - HealthCare Cloud Infrastructure Automation

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?style=for-the-badge&logo=sonarqube&logoColor=white)

---

## 📌 Executive Project Overview

**Sunvista** is an automated, highly available, and secure multi-environment (Staging & Production) AWS infrastructure provisioning engine designed for a Healthcare SaaS platform. Built using **Terraform Infrastructure as Code (IaC)**, this project enforces industry-standard security best practices, cost optimization governance, and zero-downtime microservices delivery via **AWS ECS Fargate**.

Key highlights of this implementation include dynamic multi-tenant VPC networking, automated SSL/TLS termination, automated database management with Amazon RDS PostgreSQL, lifecycle-managed DICOM medical record storage on Amazon S3, and integrated DevSecOps tooling.

---

## 📐 Architecture Diagram & Workflow

```text
                     ┌─────────────────────────────────────────────────────────────┐
                     │                        AWS ROUTE 53                         │
                     └──────────────────────────────┬──────────────────────────────┘
                                                    │
                                                    ▼
                     ┌─────────────────────────────────────────────────────────────┐
                     │                Application Load Balancer (ALB)              │
                     └──────────────┬──────────────────────────────┬───────────────┘
                                    │                              │
                     ┌──────────────▼──────────────┐  ┌────────────▼──────────────┐
                     │     Public Subnet (Web)     │  │    Public Subnet (CI/CD)    │
                     │  - NAT Gateways             │  │  - Jenkins Master          │
                     │  - Internet Gateways        │  │  - SonarQube Server        │
                     └──────────────┬──────────────┘  └────────────────────────────┘
                                    │
                                    ▼
┌──────────────────────────────────────────────────────────────────────────────────────────┐
│                                   Private Subnet (App)                                   │
│  ┌───────────────────────┐   ┌───────────────────────┐   ┌────────────────────────────┐  │
│  │  ECS Fargate Backend  │   │  ECS Fargate Frontend │   │ Celery Background Workers  │  │
│  └───────────────────────┘   └───────────────────────┘   └────────────────────────────┘  │
└───────────────────────────────────────────┬──────────────────────────────────────────────┘
                                            │
                                            ▼
┌──────────────────────────────────────────────────────────────────────────────────────────┐
│                                 Isolated Subnet (Database)                               │
│  ┌───────────────────────────┐   ┌───────────────────────────┐   ┌────────────────────┐  │
│  │   Amazon RDS PostgreSQL   │   │   ElastiCache Redis       │   │  Amazon EFS        │  │
│  └───────────────────────────┘   └───────────────────────────┘   └────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────────────────────┘
