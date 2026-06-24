# Multi-Application Parallel Deployment Platform

## 🎯 Project Overview

Designed a deployment framework capable of building source code once and deploying multiple applications simultaneously.

The solution eliminates redundant builds and dramatically reduces deployment duration by executing deployments in parallel using GitHub Actions Matrix Strategy.

---

## 🚨 Problem Statement

Traditional deployment process:

App 1 Build → Deploy
App 2 Build → Deploy
App 3 Build → Deploy

Multiple builds increased deployment time significantly.

---

## ✅ Solution

Single Build
↓
Shared Artifact
↓
Parallel Deployments

App 1
App 2
App 3
App N

---

## Features

* Single build process
* Shared build artifact
* Parallel deployments
* Matrix strategy
* Faster releases
* Reduced resource usage
* Zero downtime deployments

---

## ⚙️ How It Works


* build job runs exactly once: builds and pushes a single shared Docker image, tagged with the commit SHA. Its output (image URI) is passed downstream.
* set-matrix job reads apps.json and exposes it as a dynamic matrix so the deploy job knows exactly which apps to target — no hardcoding.
* deploy job fans out using strategy.matrix: GitHub Actions spins up one parallel runner per app, all referencing the same already-built image.
* Each runner renders its app-specific task definition, deploys to its own ECS cluster/service, and waits independently for service stability.
* fail-fast: false ensures that if App 2 fails its health check, Apps 1 and 3 still complete successfully — failures are isolated per app.

---

## 🧩 Matrix Strategy Explained

### GitHub Actions' strategy.matrix normally varies build parameters (OS, language version). Here it's repurposed to vary deployment targets:

```yaml
strategy:
  matrix:
    app: [ {name: app1, ...}, {name: app2, ...}, {name: app3, ...} ]
  max-parallel: 10
```

Each matrix combination becomes its own isolated job execution — run on its own runner, in parallel, with its own logs, but all sharing the same upstream build artifact via needs.build.outputs.image.

---

## 🛠️ GitHub Actions Features Used

* Matrix Strategy
* Parallel Jobs
* Workflow Dependencies
* Shared Artifacts
* Environment Variables

---

## ✅ Benefits

* Significant deployment time reduction
* Improved scalability
* Consistent deployments
* Operational efficiency
