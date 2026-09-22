# Module 9: Containerizing a Service with Docker

## Objective
Move from running a service directly on the server (Module 6's Nginx setup) to
running it as a container — the standard way modern applications are packaged
and deployed, including in cloud environments.

## Prerequisites
- Comfortable with the core server (Modules 1-8 complete)
- Ubuntu server with internet access via the NAT adapter

## Planned Steps (fill in once you reach this module)

### 1. Install Docker
```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

### 2. Run a basic container
```bash
docker run hello-world
```

### 3. Containerize the site from Module 6
Write a simple Dockerfile for the static site, build it, and run it.

### 4. Explore Docker basics
`docker ps`, `docker images`, `docker logs`, volumes vs. no persistence,
port mapping (`-p 8080:80`).

## Verification
_(fill in once completed)_

## Issues & Troubleshooting
_(fill in)_

## Key Takeaways
_(fill in — how this compares to running services directly on the host, and
why this matters for cloud/DevOps work)_
