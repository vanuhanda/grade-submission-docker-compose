## Architecture

The application is deployed as a **multi-container Docker Compose stack** running on an **AWS EC2 instance**.  
Each major component runs in its own container and communicates over a private Docker network.

---

### High-Level Architecture

User Browser  
↓  
Frontend Container (grade-submission-portal)  
↓  
Backend API Container (grade-submission-api)  
↓  
MongoDB Container (Persistent Storage)

---

### Component Responsibilities

**Frontend – Grade Submission Portal**
- Runs as a containerized web application
- Exposes port **5001** to the host
- Accepts user input and sends requests to the backend API
- Communicates with the backend using Docker service discovery

**Backend – Grade Submission API**
- Runs as a containerized REST API
- Exposes port **3000** (optional for debugging)
- Handles business logic and database operations
- Connects to MongoDB using environment-based configuration

**Database – MongoDB**
- Runs as a dedicated database container
- Stores grade records persistently
- Uses a named Docker volume for data durability
- Not exposed publicly

---

### Docker Networking

- Docker Compose automatically creates a **private bridge network**
- Containers communicate using **service names** as hostnames
- No hard-coded IP addresses are used
- Networking is isolated from the host system

Examples:
- Frontend → Backend: `node-server`
- Backend → Database: `mongo`

---

### Data Persistence

- MongoDB data is stored in a **named Docker volume**
- Volume is mounted to `/data/db` inside the MongoDB container
- Data persists across:
  - Container restarts
  - `docker compose down` / `up`
  - EC2 instance reboots

---

### Cloud Deployment

- The entire Docker Compose stack runs on a single AWS EC2 instance
- Application is accessed using the EC2 public IP and exposed port
- Docker images are pulled directly from Docker Hub
- Security Groups control external access to the application

---

### Architecture Characteristics

- **Stateless application containers**
- **Stateful database container with persistent volume**
- **Loose coupling via environment variables**
- **Portable and reproducible deployment**

This architecture mirrors real-world containerized application deployments and provides a foundation for scaling or migration to container orchestration platforms such as Kubernetes.
