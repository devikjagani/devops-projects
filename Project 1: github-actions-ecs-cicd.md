# End-to-End CI/CD Pipeline using GitHub Actions, Docker, AWS ECR and AWS ECS

## Project Overview

Designed and implemented a fully automated CI/CD pipeline that deploys containerized applications to AWS ECS with zero downtime.

The solution automatically builds Docker images whenever code is pushed to GitHub, stores images in Amazon ECR, creates new ECS task revisions, and performs rolling deployments to production services.

---

## Architecture

GitHub
↓
GitHub Actions
↓
Docker Build
↓
Amazon ECR
↓
ECS Task Revision
↓
ECS Service Update
↓
Rolling Deployment
↓
Production

---

## Key Features

* Fully automated deployments
* Docker image versioning
* Amazon ECR image storage
* ECS rolling deployments
* Zero downtime releases
* Automated health validation
* Task revision automation
* Production-ready workflow

---

## Workflow Process

### Step 1: Source Code Push

Developer pushes code to GitHub repository.

### Step 2: Build Phase

GitHub Actions:

* Checkout source code
* Configure AWS credentials
* Build Docker image
* Tag image using github action run number

### Step 3: Push Phase

Docker image pushed to Amazon ECR.

Example:

app:latest
app:run-number
app:20
app:21

### Step 4: Deployment Phase

Pipeline automatically:

* Creates new ECS task definition revision
* Updates ECS service
* Starts new tasks

### Step 5: Health Validation

Pipeline waits for:

* Container health checks
* ECS task health
* Target Group health

### Step 6: Traffic Shift

ECS rolling deployment:

* New tasks become healthy
* Traffic shifts automatically
* Old tasks terminated

Result: Zero downtime deployment

---

## Technologies Used

* GitHub Actions
* Docker
* AWS ECS
* AWS ECR
* AWS IAM
* AWS CloudWatch

---

## Achievements

* Zero downtime deployments
* Fully automated release process
* Faster deployment cycles
* Reduced operational effort
* Production-grade deployment automation
