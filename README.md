# Distributed Task Execution System  


## Project Overview  

A **scalable task scheduler** built in **Go**, designed to handle high volumes of tasks and distribute them efficiently across multiple workers using **gRPC**.  

- Demonstrates **distributed scheduling, execution, and monitoring**  
- Backed by **PostgreSQL** for persistence  
- Includes **Docker-based deployment** for easy cluster setup  

---

## System Architecture  

The system consists of five key components:  

| Component   | Role |
|-------------|------|
| **Scheduler** | Receives task requests from clients and stores them in the database |
| **Coordinator** | Selects due tasks from DB and distributes to workers; handles worker registration & heartbeats |
| **Worker** | Executes assigned tasks, queues them in memory, reports completion |
| **Client** | Submits tasks & queries status via HTTP endpoints |
| **Database (PostgreSQL)** | Stores task metadata & scheduling information |

📡 **Communication:** All services use **gRPC**, enabling scalability and fault tolerance.  

---

## Task Lifecycle  

1. **Scheduling**  
   - Clients send tasks to **Scheduler** via `HTTP POST`  
   - Scheduler saves task details & scheduled execution time in DB  

2. **Execution**  
   - **Coordinator** polls DB for due tasks  
   - Distributes tasks to **Workers**  
   - Workers execute & report status back  

3. **Retries**  
   - Coordinator retries tasks only if **worker assignment fails**  
   - Execution failures are **not auto-retried** 

---

## Directory Structure  

| Directory / File       | Description |
|-------------------------|-------------|
| `cmd/` | Entry points for scheduler, coordinator, and worker |
| `pkg/` | Core service logic |
| `data/` | SQL scripts for DB initialization |
| `tests/` | Integration tests |
| `*-dockerfile` | Dockerfiles for each service |
| `docker-compose.yml` | Compose file for cluster setup |

---

## Running the Cluster  

Run with **Docker Compose**:  

```bash
docker-compose up --build --scale worker=3
