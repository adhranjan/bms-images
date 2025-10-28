# Ranjan Adhikari's DevOps & Dev Environment Docker Setup

This repository contains a Docker Compose setup for a comprehensive development and testing environment. It includes commonly used services such as Redis, Kafka, RabbitMQ, Jenkins, MySQL, Mailpit, NFS, and related tools.

---

## Website
Check out more about me and my projects: [https://ranjan.fyi](https://ranjan.fyi)

---

## Services

### 1. **Redis**
- **Image:** `redis:latest`
- **Ports:** `6379:6379`
- **Volumes:** `redis_data:/data`
- **Description:** In-memory key-value store for caching and message brokering.

### 2. **Zookeeper**
- **Image:** `zookeeper:3.7`
- **Ports:** `2181:2181`
- **Healthcheck:** Ensures Zookeeper is up before Kafka starts.
- **Description:** Required by Kafka for managing broker metadata.

### 3. **Kafka**
- **Image:** `wurstmeister/kafka:2.13-2.8.1`
- **Ports:** `9092:9092`
- **Depends on:** Zookeeper
- **Environment Variables:**  
  - `KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092`  
  - `KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092`  
  - `KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181`  
- **Description:** Distributed streaming platform for event-driven architectures.

### 4. **RabbitMQ**
- **Image:** `rabbitmq:3-management`
- **Ports:** `5672:5672`, `15672:15672`
- **Volumes:** `rabbitmq_data:/var/lib/rabbitmq`
- **Environment Variables:**  
  - `RABBITMQ_DEFAULT_USER=guest`  
  - `RABBITMQ_DEFAULT_PASS=guest`  
- **Description:** Message broker with management UI.

### 5. **Jenkins**
- **Image:** `jenkins/jenkins:lts`
- **Ports:** `8080:8080`, `50000:50000`
- **Volumes:** `jenkins_data:/var/jenkins_home`
- **Environment Variables:**  
  - `JAVA_OPTS=-Djenkins.install.runSetupWizard=false` (skip setup wizard)
- **Description:** CI/CD automation server.

### 6. **MySQL Workbench**
- **Image:** `lscr.io/linuxserver/mysql-workbench:latest`
- **Ports:** `7000:3000`, `7001:3001`
- **Volumes:** `mysql_workbench_data:/config`
- **Description:** GUI tool to manage MySQL databases.

### 7. **MySQL Client**
- **Image:** `mysql:8.0`
- **Usage:** For manual MySQL operations.
- **Profiles:** `manual` (start only when needed)

### 8. **Mailpit**
- **Image:** `axllent/mailpit:latest`
- **Ports:** `1025:1025` (SMTP), `8025:8025` (UI)
- **Volumes:** `~/Docker/mailpit-data:/data`
- **Environment Variables:**  
  - `MP_DATABASE=/data/mailpit.db` (persist emails)
- **Description:** Testing SMTP server and webmail UI for development.

### 9. **NFS Server**
- **Image:** `itsthenetwork/nfs-server-alpine`
- **Ports:** `2049:2049`
- **Volumes:** `/home/ranjan/Docker/nfsshare:/nfsshare`
- **Profiles:** `manual` (start explicitly)
- **Description:** NFS server for sharing files across containers.

### 10. **NFS Nginx**
- **Image:** `nginx:alpine`
- **Ports:** `7080:80`
- **Volumes:** `/home/ranjan/Docker/nfsshare:/usr/share/nginx/html:ro`
- **Profiles:** `manual` (start explicitly)
- **Description:** Simple Nginx server to serve files from NFS share.

### 11. **Redis Commander**
- **Image:** `rediscommander/redis-commander:latest`
- **Ports:** `8081:8081`
- **Environment Variables:** `REDIS_HOSTS=local:redis:6379`
- **Depends on:** Redis
- **Description:** Web UI for managing Redis databases.

---

## Networks
- `app_network`: All main services are connected to this bridge network.

---

## Volumes
| Volume                | Purpose                         |
|-----------------------|---------------------------------|
| `redis_data`          | Redis persistence               |
| `rabbitmq_data`       | RabbitMQ persistence            |
| `postgres_data`       | Placeholder for PostgreSQL data |
| `mysql_workbench_data`| MySQL Workbench configuration   |
| `jenkins_data`        | Jenkins home directory          |

---

## Usage

### Start All Services
```bash
docker-compose up -d
