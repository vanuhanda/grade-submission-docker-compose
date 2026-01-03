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

The following diagram illustrates the Docker Compose–based architecture deployed on AWS EC2:

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/0f4178fd-0f3d-4338-a6aa-4605f0f809e4" />

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

## Docker Images

The application images are built locally and published to **Docker Hub**, allowing the full stack to be deployed without rebuilding images on the target host.

### Backend API Image
- **Image:** `vanuhanda/grade-submission-api`
- **Docker Hub:** https://hub.docker.com/r/vanuhanda/grade-submission-api
- **Description:** Backend REST API responsible for processing grade submissions and interacting with MongoDB.

Pull image:
```bash
docker pull vanuhanda/grade-submission-api
```

### Frontend Portal Image
- **Image:** `vanuhanda/grade-submission-portal`
- **Docker Hub:** https://hub.docker.com/r/vanuhanda/grade-submission-portal
- **Description:** Frontend web application providing a user interface for submitting and viewing grades.

Pull image:
```bash
docker pull vanuhanda/grade-submission-portal
```
## Screenshots Walkthrough

### Application UI – Grade Submission Form
This screenshot shows the frontend portal running inside a Docker container,
accessible via the EC2 public IP. User input is submitted to the backend API.

![Grade Submission Form](screenshots/1a Grade Submission UI.png)

---

### Application UI – Stored Grades
This screenshot confirms successful data persistence.
Grades submitted via the frontend are stored in MongoDB and retrieved via the backend API.

![Grades View](screenshots/1b Grade Submission UI.png)

---

### Running Docker Containers
This screenshot shows all application containers running simultaneously,
confirming successful orchestration using Docker Compose.

![Docker Containers](screenshots/Docker Containers Running.png)

---

### Docker Compose Configuration
This screenshot shows the `docker-compose.yml` file defining all services,
networks, environment variables, and volumes.

![Docker Compose File](screenshots/Docker Compose File.png)

---

### Application State After EC2 Reboot
This screenshot confirms that the application stack continues to function
after an EC2 instance reboot, validating container restart behavior and volume persistence.

![After Reboot](screenshots/4 After EC2 Rebbot.png)
