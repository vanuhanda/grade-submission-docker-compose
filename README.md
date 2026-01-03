# grade-submission-docker-compose
Docker Compose–based deployment of a multi-container grade submission application (Frontend, Backend, MongoDB) with persistent storage, running on AWS EC2.

# Grade Submission Application – Docker Compose on AWS EC2

## Overview
This project demonstrates the deployment of a multi-container **Grade Submission Application** using **Docker Compose**.  
The application consists of a frontend portal, a backend API, and a MongoDB database, all running as separate containers with persistent storage and deployed on an AWS EC2 instance.

The project highlights the transition from **manual Docker-based container management** to **Docker Compose–based orchestration**, along with networking, persistence, and cloud deployment concepts.

---

## Application Components
- **Frontend (grade-submission-portal)**  
  Web interface for submitting and viewing grades

- **Backend (grade-submission-api)**  
  REST API responsible for handling grade data

- **Database (MongoDB)**  
  Persistent data store for grade records

---

## Manual Docker vs Docker Compose

### Manual Docker Approach
Initially, the application was deployed using individual Docker commands:

- A custom Docker network was created manually
- Containers were started using multiple `docker run` commands
- Environment variables were passed explicitly
- Startup order had to be managed manually

Example workflow:
- Create Docker network
- Run backend container
- Run frontend container
- Ensure correct container names for communication

While this approach works, it becomes difficult to manage as the number of services increases.

---

### Docker Compose Approach
Docker Compose simplifies the deployment by defining the entire application stack in a single YAML file.

Key benefits:
- One-command deployment (`docker compose up`)
- Automatic network creation
- Built-in service discovery
- Easier configuration management
- Repeatable and consistent deployments

Docker Compose replaces multiple manual commands with a declarative configuration.

---

## Networking

Docker Compose automatically creates a **private bridge network** for all services defined in the compose file.

Key networking concepts demonstrated:
- Containers communicate using **service names**, not IP addresses
- Frontend connects to backend using the backend service name
- Backend connects to MongoDB using the database service name
- No hard-coded IPs or localhost dependencies

This approach enables:
- Service discovery via Docker DNS
- Clean separation between services
- Portability across environments (local, EC2, cloud)

---

## Persistence

The MongoDB container uses a **named Docker volume** to ensure data persistence.

Why this matters:
- Containers are ephemeral by design
- Removing or recreating containers does not delete stored data
- Database data survives:
  - `docker compose down`
  - Container restarts
  - Application redeployments

This project verifies persistence by:
- Submitting data via the frontend
- Stopping the stack
- Restarting the stack
- Confirming previous data remains available

---

## AWS EC2 Deployment

The Docker Compose stack is deployed on an **AWS EC2 instance** running Ubuntu Server.

Deployment highlights:
- Docker and Docker Compose installed on EC2
- Application images pulled directly from Docker Hub
- Security Group configured to allow:
  - SSH access
  - Application access via exposed ports
- Application accessed using EC2 public IP

This demonstrates:
- Running containerized workloads on cloud infrastructure
- Separation between build (local) and run (EC2)
- Real-world deployment workflow

---

## Docker Compose Usage

Start the application stack:
```bash
docker-compose up -d
```
---

Stop the stack 
```bash
docker-compose down
```

View running containers
```bash
docker ps
```

View Logs
```bash
docker-compose logs
```
