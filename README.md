# TaskMaster: Task Scheduler in Go  
**TaskMaster Hero**

TaskMaster is a robust and efficient task scheduler made for educational purposes and written in **Go**.  
It is designed to handle a high volume of tasks and distribute them across multiple workers for execution.

---

## 🚀 System Components  

TaskMaster is composed of several key components that work together to schedule and execute tasks:

- **Scheduler**  
  - Front-end server of the system.  
  - Receives tasks from clients and schedules them for execution.  

- **Coordinator**  
  - Selects tasks that need to be executed at a given instant based on their schedules.  
  - Handles worker registration and decommissioning.  
  - Distributes tasks across available workers.  

- **Worker**  
  - Executes tasks assigned by the coordinator.  
  - Reports completion status back to the coordinator.  
  - Registers automatically with the coordinator and sends **heartbeats** to indicate liveness.  

- **Client**  
  - Submits tasks to the scheduler via an **HTTP endpoint**.  
  - Queries the scheduler for task status.  

- **Database (PostgreSQL)**  
  - Stores task details: ID, task info, scheduling info, and completion info.  
  - Accessed by both the **scheduler** and **coordinator**.  

🔗 All components are implemented as separate services and communicate using **gRPC**.  
This enables high **scalability** and **fault tolerance**.

---

## 🔄 Life of a Task  

### 1. Scheduling  
- User sends an HTTP request to the **scheduler**.  
- Scheduler persists task info and scheduling details into the **database**.  

### 2. Execution  
- Coordinator periodically scans the DB for scheduled tasks.  
- Coordinator distributes tasks to available workers.  
- Workers enqueue tasks in memory and execute them as soon as resources are available.  

### 3. Retries  
- Coordinator retries task execution only if task submission to a worker failed.  
- Tasks that fail **inside a worker** are not retried.  

---

## ⚠️ Limitations  
- This project is for **educational purposes** only.  
- Tasks are simple **strings** representing arbitrary data.  
- Coordinator uses a **push-based** approach → may overload workers if not enough are available.  

---

## 📂 Directory Structure  

cmd/ # Entry points for scheduler, coordinator, and worker
pkg/ # Core logic of scheduling, coordination, and execution
data/ # SQL scripts for database initialization
tests/ # End-to-end integration tests
*-dockerfile # Dockerfiles for each service
docker-compose.yml # Deployment configs for containerized cluster
