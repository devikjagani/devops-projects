# AWS CodePipeline CI/CD Pipeline

## Project Overview

Implemented a native AWS CI/CD solution using CodePipeline, CodeBuild, Amazon ECR, and Amazon ECS.

The pipeline automatically detects GitHub commits, builds Docker images, pushes them to ECR, creates ECS task revisions, and deploys updated services using rolling deployments.

---

## Architecture

GitHub
↓
AWS CodeConnections
↓
AWS CodePipeline
↓
AWS CodeBuild
↓
Amazon ECR
↓
Amazon ECS

---

## Key Features

* Automatic GitHub integration
* Docker image build automation
* ECR image repository management
* ECS service deployment
* Rolling updates
* Zero downtime releases
* Deployment health validation

---

## Build Process

### Source Stage

CodePipeline detects GitHub commit.

### Build Stage

CodeBuild:

* Builds Docker image
* Tags image
* Pushes image to ECR

### Deploy Stage

Pipeline:

* Creates ECS task revision
* Updates ECS service
* Waits for healthy tasks
* Removes old tasks

---

## Technologies Used

* AWS CodePipeline
* AWS CodeBuild
* AWS CodeConnections
* AWS ECS
* AWS ECR
* Docker
* IAM

---

## Results

* Fully managed AWS CI/CD
* Automated production deployments
* Reduced manual intervention
* Reliable rollout strategy
