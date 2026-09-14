
# What are Containers?

Containers are isolated environments used to run applications and their dependencies.

A container can have its own:
- Processes or services
- Network interfaces
- Mounts and filesystem view

The main difference between containers and virtual machines is that **containers share the host OS kernel**, while virtual machines have their own guest OS kernel.

## Containers vs Virtual Machines

| Feature | Containers | Virtual Machines |
|---|---|---|
| Isolation | Isolated environment | Isolated virtual machine |
| OS Kernel | Share the host OS kernel | Have their own guest OS kernel |
| Startup | Generally faster | Generally slower |
| Overhead | Lower | Higher |

## Containers existed before Docker

Containers are not a new technology introduced by Docker. Linux container technologies existed before Docker became popular.

Some examples:
- LXC (Linux Containers)
- LXD
- LXCFS

### Docker container runtime history

Docker originally used LXC under the hood.

Later, Docker moved away from LXC in version 0.9.0, around 2014, and switched to its own library called **libcontainer**.

The evolution was:

```text
LXC
  ↓
libcontainer
  ↓
runc
```

Today, Docker uses **containerd and runc** to run containers. It does not use LXC, LXD, or LXCFS as its container runtime.

## What is OCI?

OCI stands for **Open Container Initiative**.

It is an open standard that defines how container runtimes and image formats should work.

OCI mainly covers:

1. **Runtime Specification** – Defines how a container runtime should run containers.
2. **Image Specification** – Defines how container images should be structured.

This helps different container tools follow common standards.

## Docker Architecture

At a high level, Docker uses the following components:

```text
Docker CLI / Docker Engine
          ↓
       containerd
          ↓
          runc
          ↓
      Linux Kernel
```

### containerd

containerd is responsible for managing the container lifecycle, such as creating, starting, and stopping containers.

### runc

runc is a low-level container runtime that follows OCI runtime specifications. It is responsible for creating and running containers.

### Linux Kernel

Containers share the host Linux kernel. The kernel provides the underlying process isolation and other features required to run containers.

## Why Docker became popular

Setting up low-level container environments manually can be difficult.

Docker provides a high-level tool that makes it easier to build, run, and manage containers.

For example:

```bash
docker run nginx
```

This command runs an Nginx container using Docker.

## Quick Revision

- Containers are isolated environments.
- Containers share the host OS kernel.
- Containers existed before Docker.
- Docker originally used LXC.
- Docker moved to libcontainer around version 0.9.0 in 2014.
- libcontainer later became runc.
- OCI defines standards for container runtimes and image formats.
- Docker uses containerd and runc to run containers.
- Docker makes container management easier for developers.
