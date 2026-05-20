# Three-Tier App: Docker & Kubernetes Deployment

A containerized ticket-tracking application built with a three-tier architecture and deployed on both Docker Compose and Kubernetes. Each service runs in its own container and communicates over a defined network — demonstrating real-world container orchestration and infrastructure practices.

## Architecture

| Tier | Technology | Role |
|---|---|---|
| Frontend | Apache | Serves the dashboard UI |
| Backend | Flask (Python) | REST API for ticket operations |
| Database | MariaDB | Stores and persists ticket data |

## Features

- View, create, and manage tickets through the UI or API
- Full stack wired together with Docker Compose
- Kubernetes deployment using manifest files in `k8s/`
- Secret management via shell script (`create-secret.sh`)
- Automated verification script (`check-lab13.sh`)
- Environment variables managed through `.env` (not committed)

## File Overview

| File / Folder | Description |
|---|---|
| `app/` | Flask backend application |
| `apache/` | Apache frontend configuration |
| `k8s/` | Kubernetes manifests |
| `create-secret.sh` | Creates Kubernetes secrets from `.env` values |
| `check-lab13.sh` | Verifies the full stack is running correctly |
| `.gitignore` | Excludes `.env` and other sensitive files |

## Getting Started

**Prerequisites**
- Docker Engine and Docker Compose v2
- Git
- Apache2 disabled on the host to free port 80

**Clone the repo**

```bash
git clone https://github.com/yxbsra/inet4031-testlab13.git
cd inet4031-testlab13
```

**Set up environment variables**

Create a `.env` file in the root directory with your database credentials:

```
MYSQL_ROOT_PASSWORD=yourpassword
MYSQL_DATABASE=tickets
MYSQL_USER=youruser
MYSQL_PASSWORD=yourpassword
```

**Start the stack**

```bash
docker compose up -d
```

**Verify everything is running**

```bash
docker compose ps
bash check-lab13.sh
```

## Kubernetes Deployment

Apply all manifests:

```bash
kubectl apply -f k8s/
```

Create secrets before deploying:

```bash
bash create-secret.sh
```

Check pod status:

```bash
kubectl get pods
kubectl get services
```

## Technologies

`Docker` `Docker Compose` `Kubernetes` `Flask` `Apache` `MariaDB` `Python` `Shell` `DevOps`
