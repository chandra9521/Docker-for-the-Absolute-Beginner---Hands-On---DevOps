
# Docker CLI Commands

This section covers the basic Docker commands used to create, view, stop, remove, and interact with containers and images.

## 1. `docker run`

The `docker run` command is used to create and start a container from an image.

```bash
docker run nginx
```

### What happens?

1. Docker checks if the Nginx image is available locally.
2. If the image is not available, Docker pulls it from Docker Hub.
3. Docker creates a container from the image.
4. The container starts running.

The image is downloaded only when it is not already available locally. Subsequent runs can reuse the same local image.

```bash
docker run redis
docker run ubuntu
docker run nginx
```

### Docker Hub pull limits

Docker Hub applies rate limits to unauthenticated image pulls.

For current limits, check the official documentation:

https://docs.docker.com/docker-hub/usage/

If you are pulling many images, authenticate with Docker Hub:

```bash
docker login
```

This is especially relevant when working with CI/CD pipelines.

---

## 2. `docker ps`

Lists currently running containers.

```bash
docker ps
```

Example output:

```text
CONTAINER ID   IMAGE   COMMAND   STATUS   NAMES
a1b2c3d4e5f6   nginx   ...       Up       silly_samet
```

Important fields:

| Field | Meaning |
|---|---|
| CONTAINER ID | Unique ID of the container |
| IMAGE | Image used to create the container |
| COMMAND | Command running inside the container |
| STATUS | Current container status |
| NAMES | Container name |

Docker automatically assigns a random container ID and name if you do not specify one.

### List all containers

```bash
docker ps -a
```

This shows:

- Running containers.
- Stopped containers.
- Exited containers.

---

## 3. `docker stop`

Stops a running container.

```bash
docker stop <container-id-or-name>
```

Example:

```bash
docker stop silly_samet
```

You can use either the container ID or its name.

Verify:

```bash
docker ps
```

The stopped container will no longer appear in the running container list.

To see it again:

```bash
docker ps -a
```

---

## 4. `docker rm`

Removes a stopped or exited container permanently.

```bash
docker rm <container-id-or-name>
```

Example:

```bash
docker rm silly_samet
```

Verify:

```bash
docker ps -a
```

The container should no longer appear.

### Important

`docker rm` removes the container, not the image used to create it.

You generally need to stop a running container before removing it.

---

## 5. `docker images`

Lists images available on the Docker host.

```bash
docker images
```

Example:

```text
REPOSITORY   TAG       IMAGE ID       SIZE
nginx        latest    abc123         ...
redis        latest    def456         ...
ubuntu       latest    ghi789         ...
alpine       latest    jkl012         ...
```

Important fields:

- Repository — Image name.
- Tag — Image version or tag.
- Image ID — Unique image identifier.
- Size — Image size.

We will cover image tags in more detail later.

---

## 6. `docker rmi`

Removes a Docker image from the local host.

```bash
docker rmi <image-name-or-id>
```

Example:

```bash
docker rmi nginx
```

### Important

Before removing an image:

- Make sure no running containers depend on it.
- Remove stopped containers that still reference it, if required.

Example:

```bash
docker ps -a
docker rm <container-id>
docker rmi nginx
```

Removing an image does not remove containers that were already created from it.

---

## 7. `docker pull`

Downloads an image without running a container.

```bash
docker pull ubuntu
```

What happens:

```text
Docker Hub
    |
    v
Download Image
    |
    v
Store Image Locally
```

Example:

```bash
docker pull nginx
docker pull redis
docker pull ubuntu
```

Verify:

```bash
docker images
```

The image should now be available locally.

---

# 8. Why Does `docker run ubuntu` Exit Immediately?

Consider:

```bash
docker run ubuntu
```

You might expect the Ubuntu container to keep running.

But it exits immediately.

### Why?

A container lives as long as its main process is running.

```text
Container Starts
      |
      v
Main Process Runs
      |
      v
Process Exits
      |
      v
Container Stops
```

Ubuntu is commonly used as a base image for other applications. It does not start a long-running service by default.

So when the container starts, there is no long-running foreground process to keep it alive.

### Important Concept

> A container is not a virtual machine. It is designed to run a process or task.

Examples of tasks:

- Run a web server.
- Run an application server.
- Run a database.
- Perform a computation.
- Execute a script.

When the main process completes, the container exits.

---

## 9. Run a Command Inside a Container

You can provide a command when starting a container.

### Example: Sleep for 5 Seconds

```bash
docker run ubuntu sleep 5
```

What happens:

```text
Start Ubuntu Container
        |
        v
Run sleep 5
        |
        v
Wait 5 Seconds
        |
        v
Sleep Process Exits
        |
        v
Container Stops
```

Check:

```bash
docker ps -a
```

You will see the container in an exited state.

### Another Example

```bash
docker run ubuntu echo "Hello Docker"
```

The command prints the message and exits.

---

# 10. `docker exec`

The `docker exec` command is used to execute a command inside an already running container.

### Example

Suppose a container is running:

```bash
docker ps
```

Execute a command inside it:

```bash
docker exec <container-id-or-name> cat /etc/hosts
```

This prints the contents of `/etc/hosts` inside the container.

### Common Uses

`docker exec` is useful for:

- Debugging.
- Checking configuration.
- Reading logs or files.
- Running quick queries.
- Inspecting a running container.

### Example: Open a Shell

```bash
docker exec -it <container-id-or-name> /bin/bash
```

If Bash is not available, try:

```bash
docker exec -it <container-id-or-name> /bin/sh
```

`-i` keeps standard input open. (Intractive Mode)

`-t` allocates a terminal. (TTY)

---

# 11. Attached vs Detached Mode

Docker containers can run in two common modes.

## Attached Mode

By default, `docker run` runs in the foreground.

Example:

```bash
docker run nginx
```

Your terminal remains attached to the container's output.

```text
Terminal
    |
    v
Docker Container
    |
    v
Application Output
```

You can see the output on your screen.

Press:

```text
Ctrl + C
```

to stop the attached container in this example.

### Limitation

You cannot use the same terminal normally for other commands while it is attached to the foreground process.

## Detached Mode

Use the `-d` option to run a container in the background.

```bash
docker run -d nginx
```

Example output:

```text
a1b2c3d4e5f6...
```

Docker returns the container ID and gives you back the terminal prompt.

The container continues running in the background.

Verify:

```bash
docker ps
```

---

## 12. `docker attach`

Attaches your terminal to the main process of a running container.

```bash
docker attach <container-id-or-name>
```

Example:

```bash
docker attach a1b2c
```

You can use the container name instead:

```bash
docker attach silly_samet
```

### Short Container IDs

Docker allows you to use the first few characters of a container ID, provided they uniquely identify one container.

Example:

```bash
docker attach a1b2c
```

You do not need to type the entire container ID.

---

# 13. Container Lifecycle

```text
docker run
     |
     v
Container Created + Started
     |
     v
Main Process Running
     |
     +----> docker exec
     |
     +----> docker attach
     |
     v
Main Process Exits
     |
     v
Container Exited
     |
     v
docker rm
     |
     v
Container Removed
```

---

# Quick Revision

| Command | Purpose |
|---|---|
| `docker run` | Create and start a container |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker stop` | Stop a running container |
| `docker rm` | Remove a container |
| `docker images` | List local images |
| `docker rmi` | Remove an image |
| `docker pull` | Download an image |
| `docker exec` | Execute a command inside a running container |
| `docker attach` | Attach to a running container |

### Important Interview Points

1. `docker run` creates and starts a container.
2. If the image is missing locally, Docker pulls it from a registry.
3. `docker ps` shows running containers only.
4. `docker ps -a` shows running and stopped containers.
5. A container runs as long as its main process is alive.
6. `docker run ubuntu` exits because no long-running default process is running.
7. `docker exec` runs a command inside an existing running container.
8. `docker pull` downloads an image without starting a container.
9. `docker stop` stops a container, while `docker rm` removes it.
10. `-d` runs a container in detached mode.
11. `docker attach` connects to the main process of a running container.
12. Images and containers are different resources.
```
