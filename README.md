# Three-Tier App: Docker & Kubernetes Deployment

[![CI](https://github.com/yxbsra/three-tier-app-docker-kubernetes/actions/workflows/ci.yml/badge.svg)](https://github.com/yxbsra/three-tier-app-docker-kubernetes/actions/workflows/ci.yml)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=yxbsra_inet4031-testlab13&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=yxbsra_inet4031-testlab13)

A containerized ticket-tracking application built with a three-tier architecture and deployed on both Docker Compose and Kubernetes. Each service runs in its own container and communicates over a defined network — demonstrating real-world container orchestration and infrastructure practices.

Every push to `main` runs a CI/CD pipeline in GitHub Actions that lints the code, validates the Kubernetes manifests, builds both Docker images, and runs two security scans: CodeQL (SAST) and SonarQube Cloud.

## Architecture

| Tier | Technology | Role |
|---|---|---|
| Frontend | Apache | Serves the dashboard UI and reverse-proxies API requests to Flask |
| Backend | Flask (Python) | REST API for ticket operations |
| Database | MariaDB | Stores and persists ticket data |

## Features

- View, create, and manage tickets through the UI or API
- Full stack wired together with Docker Compose
- Kubernetes deployment (k3s) using manifest files in `k8s/`
- Only the web tier is exposed outside the cluster (NodePort `30080`); the database uses an internal ClusterIP Service
- Secret management via shell script (`create-secret.sh`) — credentials are stored in a Kubernetes Secret, not in the manifests
- Automated verification script (`check-lab13.sh`)
- Environment variables managed through `.env` (not committed)
- CI/CD pipeline with automated linting, manifest validation, image builds, and security scanning

## CI/CD Pipeline

Defined in `.github/workflows/ci.yml`. Runs on every push and pull request to `main`.

| Job | What it does |
|---|---|
| **Lint** | Checks Python for syntax errors and undefined names (flake8) and checks shell scripts (ShellCheck) |
| **Validate Kubernetes manifests** | Validates every file in `k8s/` against the Kubernetes schema (kubeconform) |
| **Build Docker images** | Builds the Flask API and Apache web images; runs only after lint and validation pass |
| **CodeQL SAST** | Static application security testing on the Python code; results appear under the Security tab |
| **SonarQube Cloud scan** | Code quality and security analysis with a quality gate; results on SonarQube Cloud |

Pipeline security practices:

- The SonarQube token is stored as an encrypted GitHub Actions secret (`SONAR_TOKEN`), never in the workflow file
- Workflow permissions default to read-only; only the CodeQL job is granted `security-events: write`
- The SonarQube scan action is pinned to a full commit SHA to protect against supply-chain tampering

## Security Fixes

**Information exposure through an exception (CodeQL, 3 alerts, Medium).** The Flask error handlers returned the raw exception message (`str(e)`) to the client, which could reveal database details. The handlers now log the full error on the server and return a generic message to the client. After the fix was pushed, CodeQL re-scanned and all three alerts closed.

## File Overview

| File / Folder | Description |
|---|---|
| `app/` | Flask backend application and Dockerfile |
| `apache/` | Apache frontend configuration, dashboard, and Dockerfile |
| `k8s/` | Kubernetes manifests |
| `.github/workflows/ci.yml` | CI/CD pipeline |
| `sonar-project.properties` | SonarQube Cloud configuration |
| `create-secret.sh` | Creates Kubernetes secrets from your credential values |
| `check-lab13.sh` | Verifies the full stack is running correctly |
| `.gitignore` | Excludes `.env` and other sensitive files |

## Getting Started

### Prerequisites

- Docker Engine and Docker Compose v2
- Git
- Apache2 disabled on the host to free port 80

### Clone the repo

```bash
git clone https://github.com/yxbsra/three-tier-app-docker-kubernetes.git
cd three-tier-app-docker-kubernetes
```

### Set up environment variables

Create a `.env` file in the root directory with your database credentials:

```
MYSQL_ROOT_PASSWORD=yourpassword
MYSQL_DATABASE=tickets
MYSQL_USER=youruser
MYSQL_PASSWORD=yourpassword
```

### Start the stack

```bash
docker compose up -d
```

### Verify everything is running

```bash
docker compose ps
```

## Kubernetes Deployment

Create the namespace:

```bash
kubectl create namespace ticket-app
```

Create secrets before deploying:

```bash
bash create-secret.sh
```

Apply all manifests:

```bash
kubectl apply -f k8s/
```

Check pod status:

```bash
kubectl get pods -n ticket-app
kubectl get services -n ticket-app
```

Run the verification script:

```bash
bash check-lab13.sh
```

The dashboard is available at `http://localhost:30080`.

## Technologies

Docker · Docker Compose · Kubernetes (k3s) · Flask · Apache · MariaDB · Python · Shell · GitHub Actions · CodeQL · SonarQube Cloud · DevSecOps
