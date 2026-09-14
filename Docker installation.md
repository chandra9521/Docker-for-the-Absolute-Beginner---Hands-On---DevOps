
# Install Docker Engine on Ubuntu

In this lab, we install Docker Engine on an Ubuntu Linux VM.

The goal is to install Docker, verify that the service is running, and run our first container.

## 1. Docker Engine vs Docker Desktop

For this demo, we install **Docker Engine** only.

- Docker Engine: Core Docker platform used to run containers.
- Docker Desktop: Desktop application that includes Docker Engine and additional developer tools.

We will focus on Docker Engine first to understand the basics.

---

## 2. Prerequisites

Before installing Docker Engine on Ubuntu, check the following:

- Ubuntu 64-bit operating system.
- Supported Ubuntu version.
- Supported CPU architecture.
- Required firewall configuration.
- No conflicting older Docker packages.

Official installation guide:

https://docs.docker.com/engine/install/ubuntu/

### Check Ubuntu Version

Run:

```bash
cat /etc/*release*
```

Example output:

```text
NAME="Ubuntu"
VERSION="24.04.3 LTS (Noble Numbat)"
VERSION_CODENAME=noble
```

This command displays information about the Linux distribution and version.

### Check System Architecture

Run:

```bash
dpkg --print-architecture
```

Example output:

```text
arm64
```

Common architectures include:

- amd64 — 64-bit x86 architecture.
- arm64 — 64-bit ARM architecture.

Docker supports multiple architectures, but the selected Ubuntu version and architecture must be supported by the Docker installation guide.

---

## 3. Firewall Considerations

Docker uses Linux networking and firewall rules.

The official Docker documentation includes requirements and limitations related to firewall configuration.

Important points:

- Review Docker's firewall requirements before installation.
- Be careful with existing firewall rules.
- Do not blindly change firewall settings on a production VM.

If the VM already hosts applications, verify that installing Docker will not affect existing networking or security rules.

---

## 4. Uninstall Conflicting Old Packages

Before installing Docker Engine, check for older or conflicting Docker packages.

Common packages mentioned in the official documentation include:

```text
docker.io
docker-compose
docker-compose-v2
docker-doc
podman-docker
containerd
runc
```

**Important:** Do not uninstall packages blindly on an existing production VM.

Some packages may already be required by other applications.

For a fresh lab VM, follow the official Docker instructions.

---

## 5. Install Docker Engine Using the APT Repository

Docker recommends installing Docker Engine using its official APT repository.

The general process is:

```text
Update APT
    |
Install required packages
    |
Add Docker's GPG key
    |
Configure Docker APT repository
    |
Install Docker Engine
```

### Step 1: Update Package Information

```bash
sudo apt update
```

This updates the package information from configured repositories.

### Step 2: Install Required Packages

```bash
sudo apt install ca-certificates curl
```

These packages are used to securely download and verify repository information.

### Step 3: Add Docker's Official GPG Key

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Create the directory for repository signing keys.

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  -o /etc/apt/keyrings/docker.asc
```

Download Docker's GPG key.

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Make the key readable by APT.

### Step 4: Add Docker APT Repository

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

This adds Docker's official Ubuntu repository.

The command automatically detects:

- System architecture.
- Ubuntu release codename.

### Step 5: Update APT Again

```bash
sudo apt update
```

Now APT can retrieve package information from the Docker repository.

---

## 6. Install Docker Engine

Install Docker Engine and related components:

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Packages Installed

| Package | Purpose |
|---|---|
| docker-ce | Docker Engine |
| docker-ce-cli | Docker command-line interface |
| containerd.io | Container runtime component |
| docker-buildx-plugin | Docker image building |
| docker-compose-plugin | Docker Compose support |

### What is Docker CE?

Docker CE stands for Docker Community Edition.

It is the Docker Engine package name commonly used in Linux installation commands.

---

## 7. Verify Docker Service

After installation, check whether Docker is running:

```bash
sudo systemctl status docker
```

Expected status:

```text
Active: active (running)
```

This confirms that the Docker service is running.

### Useful Service Commands

Check Docker status:

```bash
sudo systemctl status docker
```

Start Docker:

```bash
sudo systemctl start docker
```

Restart Docker:

```bash
sudo systemctl restart docker
```

Enable Docker at boot:

```bash
sudo systemctl enable docker
```

---

## 8. Run the Hello World Container

To verify the installation, run:

```bash
sudo docker run hello-world
```

### What Happens?

```text
docker run hello-world
        |
        v
Check local image
        |
        v
Pull image from Docker Hub
        |
        v
Create container
        |
        v
Start container
        |
        v
Display Hello from Docker
```

If the image is not available locally, Docker pulls it from Docker Hub.

Expected output includes:

```text
Hello from Docker!
```

This confirms that Docker can:

- Communicate with the Docker daemon.
- Pull an image from Docker Hub.
- Create a container.
- Run the container successfully.

---

## 9. Important Docker Components

The installation includes several components that we will learn later.

### Docker Engine

Runs and manages containers.

### Docker CLI

Used to interact with Docker.

Example:

```bash
docker run nginx
```

### containerd

Manages container lifecycle operations.

### Buildx

Used for building Docker images.

### Docker Compose

Used to define and run multi-container applications.

---

## Quick Revision

1. Docker Engine is the core Docker platform.
2. Docker Desktop is a separate desktop application.
3. Check Ubuntu version using `cat /etc/*release*`.
4. Check architecture using `dpkg --print-architecture`.
5. Install Docker Engine using the official Docker APT repository.
6. `docker-ce` is the Docker Engine package.
7. `containerd.io` is a container runtime component.
8. Check Docker service using `systemctl status docker`.
9. Run `docker run hello-world` to verify the installation.
10. Docker pulls the `hello-world` image from Docker Hub if needed.

## Lab Result

```text
Ubuntu VM
    |
Docker Engine Installed
    |
Docker Service Active
    |
Hello World Container Ran Successfully
```
