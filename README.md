# Vue Todo App — CI/CD Pipeline Project

This repository demonstrates a complete **Automated CI Pipeline** for a Vue 3 application using **GitHub Actions** and a **Self-hosted Runner**, submitted as part of the Module 5 Assignment: GitHub Actions Fundamentals.

---

## Project Links

- **GitHub Repository:** [https://github.com/Motawash-Semeen/CI-CD-Pipeline-Project](https://github.com/Motawash-Semeen/CI-CD-Pipeline-Project)
- **CI Workflow:** [.github/workflows/ci.yml](.github/workflows/ci.yml)
- **Actions Dashboard:** [https://github.com/Motawash-Semeen/CI-CD-Pipeline-Project/actions](https://github.com/Motawash-Semeen/CI-CD-Pipeline-Project/actions)
- **Branch:** `development`

---

## Concepts

### What is CI/CD?

- **CI (Continuous Integration):** The practice of automatically building and testing code every time a developer pushes changes to the repository. It ensures new code integrates cleanly with the existing codebase and catches bugs immediately — before they reach other developers or production.

- **CD (Continuous Delivery/Deployment):** The automated process of delivering the tested and built application to a staging or production environment after all CI checks pass. Together, CI/CD eliminates slow, error-prone manual build and deployment steps, allowing teams to ship faster with higher confidence.

**Key Benefits:**
- Bugs caught at push time, not days later
- Every build is consistent and reproducible
- No "works on my machine" problems
- Teams can release smaller, safer changes more frequently

---

### What is a Self-hosted Runner?

A **self-hosted runner** is a machine that you own, configure, and manage to execute jobs from GitHub Actions workflows. Unlike GitHub-hosted runners (temporary cloud VMs), a self-hosted runner:

- Runs on **your own hardware** (local PC, server, or VM)
- Gives full control over the OS, installed tools, and environment
- Can access **local network resources** (databases, internal APIs)
- Has **no usage minute limits** like GitHub-hosted free tiers

In this project, a runner named **`CI-CD-RUNNER`** is registered on a Windows x64 machine (v2.334.0). It listens continuously and picks up jobs the moment a push to `development` is detected.

> **Screenshot:** `screenshots/self-hosted-runner.png`

---

### Workflow Execution Process

1. **Trigger** — A `push` event occurs on the `development` branch
2. **GitHub reads** `.github/workflows/ci.yml` and queues a job
3. **Self-hosted runner** receives the job from GitHub
4. **Checkout** — Runner clones the repository into its `_work/` directory
5. **Environment Setup** — Node.js 18 is installed and npm cache is restored
6. **Install** — `npm install` downloads all project dependencies
7. **Build** — `npm run build` compiles the Vue 3 app via Vite into `dist/`
8. **Verify** — `ls dist/` confirms build artifacts were generated
9. **Result** — GitHub marks the run ✅ Success or ❌ Failed with full logs

---

## Workflow YAML

```yaml
name: CI Pipeline - Vue Todo App

on:
  push:
    branches:
      - development

jobs:
  build:
    name: Install & Build
    runs-on: self-hosted

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm install

      - name: Build application
        run: npm run build

      - name: Confirm build output
        run: |
          echo "Build completed successfully."
          ls dist/
```

| Element | Value | Purpose |
|---------|-------|---------|
| `on: push: branches: [development]` | Trigger | Runs on every push to `development` |
| `runs-on: self-hosted` | Runner | Uses the local registered machine |
| `actions/checkout@v4` | Action | Clones repo onto the runner |
| `actions/setup-node@v4` | Action | Installs Node.js 18 with caching |
| `npm install` | Command | Installs all dependencies |
| `npm run build` | Command | Builds Vue app via Vite into `dist/` |
| `ls dist/` | Command | Verifies build output exists |

---

## ✅ Pipeline Verification

### 1. Successful Pipeline Execution

The screenshot below shows the GitHub Actions dashboard where the pipeline successfully completed all steps — Checkout, Setup Node, Install, Build, and Verify.

> **Screenshot:** `screenshots/success-pipeline.png`

---

### 2. Failed Pipeline Debugging

To demonstrate pipeline debugging, a non-existent step (`npm run test`) was intentionally added to the workflow and pushed. The pipeline failed with a clear error message in the logs:

```
npm error Missing script: "test"
```

The screenshot below shows the failed run and the exact error visible in the GitHub Actions log — demonstrating how failures are identified and debugged.

> **Screenshot:** `screenshots/failed-pipeline.png`

---

### 3. Self-hosted Runner Executing a Job

The screenshot below shows the runner terminal confirming it connected to GitHub and picked up the job:

```
√ Connected to GitHub
Current runner version: '2.334.0'
Listening for Jobs
Running job: Install & Build
```

> **Screenshot:** `screenshots/runner-executing.png`

---

## Local Development

### Prerequisites
- Node.js 18+
- npm

### Setup

```bash
git clone https://github.com/Motawash-Semeen/CI-CD-Pipeline-Project.git
cd CI-CD-Pipeline-Project
npm install
npm run dev
```

### Triggering the CI Pipeline

Any push to the `development` branch automatically triggers the pipeline:

```bash
git add .
git commit -m "your commit message"
git push origin development
```

---

## Self-hosted Runner Setup

```powershell
cd actions-runner
.\config.cmd --url https://github.com/Motawash-Semeen/CI-CD-Pipeline-Project --token <TOKEN>
.\run.cmd

# Optional: run as Windows service
.\svc.cmd install
.\svc.cmd start
```

*Submitted as part of the Module 5 Assignment — GitHub Actions Fundamentals.*