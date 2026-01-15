# Conduit Deployment (Docker & CI/CD)

## Table of Contents

1. [Introduction](#introduction)
2. [Project Structure](#project-structure)
3. [Quickstart](#quickstart)
4. [Environment Variables](#environment-variables)
5. [CI/CD Deployment](#cicd-deployment)
6. [Usage](#usage)
7. [Logging](#logging)
8. [Checklist](checklist.pdf)
9. [Contact](#contact)

---

## Introduction

This repository contains the **deployment and CI/CD setup** for the  
**Conduit full-stack application** (Angular frontend + Django REST backend).

The main focus of this project is:

- containerization using Docker
- orchestration with Docker Compose
- automated build & deployment via GitHub Actions
- secure handling of configuration and secrets

The frontend and backend are integrated as **Git submodules** and are built
automatically during the CI pipeline.

---

---

## Project Structure

```
conduit-deployment/
├── docker-compose.yaml
├── .gitmodules
├── backend/ # Django backend (git submodule)
├── frontend/ # Angular frontend (git submodule)
├── .github/
│ └── workflows/
│ └── deployment.yaml # GitHub Actions CI/CD pipeline
├── .env.template
└── README.md
```

---

## Quickstart

### 1. Clone the repository (including submodules)

```bash
git clone --recurse-submodules https://github.com/BenjaminTietz/conduit-deployment.git
cd conduit-deployment
```

If submodules were not cloned:

```bash
git submodule update --init --recursive
```

# The submodule configuration does not rely on the default branches of the included repositories

---

### 2. Create your environment file

All backend and frontend configuration values are defined in a single `.env`
file located in the project root.

```sh
cp .env.template .env
```

Modify if needed.

---

### 3. Start the application locally

```sh
docker-compose up
```

Backend → http://localhost:8000  
Frontend → http://localhost:8282

---

## Environment Variables

The application is configured via a `.env` file.

An example configuration is provided in  
👉 [`env.template`](./env.template)

### Setup

### Setup

Copy the template and adjust the values as needed:

```bash
cp env.template .env

---

## CI/CD Deployment

The deployment pipeline is implemented using **GitHub Actions**.

**Workflow responsibilities:**

- Build frontend and backend Docker images
- Inject frontend configuration at build time
- Push images to GitHub Container Registry (GHCR)
- Deploy containers to a remote VM via SSH
- Start services using Docker Compose in detached mode

No build steps are executed on the target VM.

## Usage

### Backend (Django)

Run inside the backend container:

```

docker exec -it conduit_backend bash
python manage.py createsuperuser

```

---

### Frontend (Angular)

The production build is embedded in an NGINX container and served at:

```

http://localhost:8282

````

---

## Logging

All services log to stdout/stderr and are managed by Docker's json-file logging driver.
Log rotation is enabled to prevent excessive disk usage.

Logs can be accessed via:

```bash
docker logs conduit_backend
docker logs conduit_frontend
````

Logs can optionally be persisted by redirecting Docker logs to a file.

```
docker logs conduit_backend > conduit_backend-logs.txt
docker logs conduit_frontend > conduit_frontend-logs.txt
```

## Contact

### 👤 Personal

Benjamin Tietz

- Portfolio: https://benjamin-tietz.com
- Mail: mail@benjamin-tietz.com

### 🌍 Social

- LinkedIn: https://www.linkedin.com/in/benjamin-tietz/

### 💻 Project Repository

- https://github.com/BenjaminTietz/conduit-deployment
