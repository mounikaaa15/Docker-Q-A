# Docker Q&A 🚀

A concise interview-focused Docker cheat sheet covering core concepts, architecture, Dockerfile instructions, networking, volumes, and best practices.

---

## 1. What is Docker?

Docker is a containerization platform that packages an application along with its dependencies, libraries, and runtime into a container, ensuring it runs consistently across different environments.

### Why Docker?
- Eliminates "works on my machine" issues  
- Faster deployment  
- Better resource utilization than VMs  
- Easy scalability  

---

## 2. Docker vs Virtual Machine

| Docker                        | Virtual Machine (VM)        |
|-----------------------------|-----------------------------|
| Shares host OS kernel       | Has its own OS              |
| Lightweight                 | Heavy                       |
| Starts in seconds           | Starts in minutes           |
| Less resource consumption   | More resource consumption   |
| Container isolation         | Full OS isolation           |

👉 Containers virtualize the OS layer, while VMs virtualize the hardware layer.

---

## 3. Docker Architecture

### Components

**Docker Client**
- Runs commands like:
  - `docker build`
  - `docker run`
  - `docker pull`

**Docker Daemon (dockerd)**
- Builds images  
- Runs containers  
- Manages networks and volumes  

**Docker Registry**
- Stores images  
- Examples:
  - Docker Hub
  - Azure Container Registry
  - Amazon ECR  

---

## 4. Docker Image vs Container

A **Docker image** is a read-only blueprint containing application code and dependencies, while a **container** is a running instance of that image with a writable layer.

---

## 5. Dockerfile Instructions

### 1. FROM
Defines base image.

```dockerfile
FROM ubuntu:22.04
