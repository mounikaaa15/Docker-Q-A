# Docker-Q-A

1. What is Docker?

Answer:
Docker is a containerization platform that packages an application along with its dependencies, libraries, and runtime into a container, ensuring it runs consistently across different environments.

Why Docker?
Eliminates "works on my machine" issues.
Faster deployment.
Better resource utilization than VMs.
Easy scalability.


2. Docker vs Virtual Machine

Interview line:| Docker                    | VM                        |
| ------------------------- | ------------------------- |
| Shares host OS kernel     | Has its own OS            |
| Lightweight               | Heavy                     |
| Starts in seconds         | Starts in minutes         |
| Less resource consumption | More resource consumption |
| Container isolation       | Full OS isolation         |

"Containers virtualize the OS layer, while VMs virtualize the hardware layer."


3. Docker Architecture
Components:

Docker Client

Runs commands:

docker build
docker run
docker pull
Docker Daemon (dockerd)

Responsible for:

Building images
Running containers
Managing networks
Managing volumes
Docker Registry

Stores images.

Examples:

Docker Hub
Azure Container Registry
Amazon ECR

5. Docker Image vs Container
“A Docker image is a read-only immutable blueprint containing application code and dependencies, while a container is a running instance of that image with an added writable layer. Multiple containers can be created from the same image, each running in isolation.”


6.Important Dockerfile Instructions 
1. FROM – Base Image (Most Critical)

Defines the starting point of the image.

FROM ubuntu:22.04
🔥 Senior insight:
Always prefer minimal base images (alpine, distroless)
Base image impacts:
security surface
image size
CVE exposure

👉 Interview line:

“Base image selection is a security and performance decision, not just a dependency choice.”

2. RUN – Build-Time Execution

Executes commands while building image.

RUN apt-get update && apt-get install -y curl
🔥 Best practices:
Combine commands to reduce layers
Clean cache to reduce image size
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

👉 Senior point:

Each RUN creates a new layer
More layers = larger image + slower builds
3. COPY vs ADD (Very commonly asked)
COPY (preferred)
COPY app.py /app/
ADD (advanced behavior)
Can extract tar files
Can fetch URLs (not recommended)
🔥 Interview answer:

“We prefer COPY over ADD because ADD has hidden behaviors like auto-extraction and remote URL support, which reduces predictability.”

4. WORKDIR – Sets working directory
WORKDIR /app
🔥 Why important:
Avoids multiple RUN cd /app
Improves readability
Ensures consistent execution context
5. CMD vs ENTRYPOINT (VERY IMPORTANT)
CMD (default command)
CMD ["nginx", "-g", "daemon off;"]
ENTRYPOINT (fixed executable)
ENTRYPOINT ["nginx"]
🔥 Key difference:
Feature	CMD	ENTRYPOINT
Overridable	Yes	Limited
Purpose	Default args	Main process
🔥 Senior explanation:

“ENTRYPOINT defines the main container process, while CMD provides default arguments. ENTRYPOINT is preferred for executables.”

6. ENV – Environment Variables
ENV NODE_ENV=production
🔥 Use cases:
Configuring runtime behavior
Avoid hardcoding values
⚠️ Risk:
Sensitive data should NOT be stored here

👉 Better alternatives:

Docker secrets
CI/CD injected env vars
7. EXPOSE – Documentation only
EXPOSE 8080
🔥 Important insight:
Does NOT actually publish port
Only documentation for runtime

Actual exposure happens at runtime:

docker run -p 8080:8080
8. VOLUME – Persistent storage
VOLUME /data
🔥 Senior insight:
Used for stateful data separation
Helps avoid data loss when container is destroyed
9. ARG – Build-time variables
ARG VERSION=1.0
🔥 Difference from ENV:
Feature	ARG	ENV
Build time	Yes	No
Runtime	No	Yes
10. HEALTHCHECK – Production readiness
HEALTHCHECK CMD curl -f http://localhost || exit 1
🔥 Why it matters:
Used by orchestrators (Docker/Kubernetes)
Helps detect unhealthy containers

👉 Interview line:

“Healthcheck enables self-healing systems in production environments.”

11. USER – Security hardening
USER appuser
🔥 Why critical:
Avoid running as root
Reduces attack surface
12. LABEL – Metadata tagging
LABEL maintainer="devops@example.com"
🔥 Use cases:
Image versioning
Ownership tracking
CI/CD metadata


Senior-Level Dockerfile Practices
1. Layer optimization
Combine RUN commands
Order changes from least to most frequently changed
2. Multi-stage builds
Separate build and runtime environments
Reduce final image size
3. .dockerignore usage
Avoid copying unnecessary files (node_modules, logs)
4. Distroless / Alpine images
Reduce CVE exposure

ENTRYPOINT = “what should always run”
CMD = “default behavior, but flexible”

“CMD defines default arguments that can be overridden at runtime, while ENTRYPOINT defines the main executable of the container. In production, ENTRYPOINT is commonly used to enforce the primary process, and CMD is used to provide default parameters. Most real-world Dockerfiles combine both to allow flexibility while maintaining control over execution.”


7.Docker Networking
Bridge Network

Default.

docker network ls

Containers communicate within same host.

Host Network

Uses host network directly.

docker run --network host
Overlay Network

Used across multiple nodes.

Mostly in Docker Swarm.


10. Port Mapping
docker run -p 8080:80 nginx

Meaning:

Host:8080 -> Container:80
11. Docker Volumes

Problem:

Container data disappears when container is deleted.

Solution:

Volumes.

docker volume create mydata
docker run -v mydata:/var/lib/mysql mysql

Used for:

Databases
Logs
Persistent storage
12. Bind Mount vs Volume
Bind Mount
-v /host/data:/app/data

Uses host path.

Volume
-v myvolume:/data

Managed by Docker.

Production recommendation:
Use volumes whenever possible.

13. Multi-Stage Build

Reduces image size.

Example:

FROM golang:1.24 AS build

WORKDIR /app

COPY . .

RUN go build -o app

FROM alpine

COPY --from=build /app/app .

CMD ["./app"]

Benefit:

Smaller image
Better security
Faster deployments
14. Docker Layer Caching

Each Dockerfile instruction creates a layer.

Bad:

COPY . .
RUN npm install

Any code change rebuilds everything.

Good:

COPY package.json .

RUN npm install

COPY . .

Dependencies are cached.

15. How to Reduce Docker Image Size?

Interview favorite.

Use Alpine images.
Multi-stage builds.
Remove unnecessary packages.
Use .dockerignore.
Minimize layers.
Use slim images.

Example:

FROM python:3.11-slim
16. Docker Security Best Practices
Don't run as root
RUN adduser appuser

USER appuser
Scan images

Examples:

Trivy
Grype
Use minimal base images
FROM alpine
Keep images updated

Patch vulnerabilities regularly.

17. Docker Logs
docker logs container_name

Follow logs:

docker logs -f container_name
