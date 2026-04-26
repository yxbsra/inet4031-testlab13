# Docker Lab: Containerizing a Three-Tier Application
**INET 4031 - Introductions to Systems**

This lab introduces Docker and Docker Compose by having you containerize a
real, multi-service application. You will package three components: Apache,
Flask, and MariaDB. These will be packaged into separate containers and wired together so they function as a complete application.

The application code and scaffolding are provided. Your job is to complete the Dockerfiles, verify the stack runs correctly, and document your work below.

> **Directions and explanations for this lab are on the repository Wiki.**
> Refer to the Wiki pages for step-by-step instructions.

---

*The sections below are for you to fill out. Replace each placeholder with your own content before submitting. Having a detailed README is the best practice for showing your work in future GitHub repositories.*

---

# Project Overview

This project deploys a simple ticket‑tracking system using a three‑tier architecture.
Apache serves the frontend dashboard, Flask provides the backend API, and MariaDB stores ticket data.
Users can view existing tickets, check API health, and create new tickets through the UI or API.
The purpose of the lab is to containerize each service, connect them with Docker Compose, and ensure the full stack runs correctly

# Prerequisites

- Docker Engine installed and running
- Docker Compose v2
- Git
- Apache2 disabled on the host VM to free port 80
- SSH access to the VM
- Internet access to pull base images

# Getting Started

- Clone the repository 
- Create .env file and fill in placeholders
- Create .gitingore

# Configuration

The .env file stores credentials and configuration values that should not be committed to GitHub.
Docker Compose automatically loads these values and injects them into the containers.

# Verification

To confirm the stack is running correctly:
- Run: docker compose ps
- Open forntend dashboard in a browser
- Test from CLI
- Create a new ticket
- Test persistence 
- Run the check script (all tests must pass)

# Kubernetes Deployment
- The application now runs on Kubernetes instead of Docker Compose
- Deploy using: kubectl apply -f k8s/
- Access the dashboard at: http://172.16.198.131:30080 

