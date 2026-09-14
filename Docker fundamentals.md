
# Docker Fundamentals

## 1. Operating System Basics

To understand how Docker works, we first need to understand the basic structure of an operating system.

An operating system mainly consists of two parts:

1. OS Kernel
2. Software

### OS Kernel

The kernel is the core part of the operating system. It interacts with the underlying hardware and manages system resources.

For Linux-based operating systems, the kernel is Linux.

### Software

The software runs above the kernel and makes one operating system different from another.

It may include:

- User interface
- Drivers
- Compilers
- File managers
- Developer tools
- System utilities

### Linux OS Architecture

```text
+----------------------------------+
|          Applications            |
|  Compilers | File Managers       |
|  Developer Tools | Utilities     |
+----------------------------------+
|          Linux Kernel            |
+----------------------------------+
|            Hardware              |
+----------------------------------+
```

## 2. Different Linux Distributions

Ubuntu, Fedora, OpenSUSE, and AlmaLinux are different Linux distributions.

They share the Linux kernel, but the software and tools installed above the kernel can be different.

```text
             Linux Kernel
                  |
    +-------------+-------------+
    |             |             |
  Ubuntu        Fedora       OpenSUSE
    |             |             |
 Different      Different    Different
  Software      Software     Software
```

The kernel provides the core functionality, while the software gives each distribution its own features and user experience.

## 3. How Docker Containers Share the Kernel

Containers share the kernel of the Docker host.

For example, suppose Docker is installed on an Ubuntu machine.

Docker can run containers based on other Linux distributions, such as:

- Debian
- Fedora
- OpenSUSE
- AlmaLinux

The container includes the additional software needed by the application and its environment.

It uses the Linux kernel provided by the Docker host.

### Example

```text
Docker Host
Ubuntu Linux
    |
    +-----------------------------+
    |         Linux Kernel        |
    +-----------------------------+
       |            |           |
       v            v           v
   Container     Container    Container
    Debian        Fedora       OpenSUSE
```

Each container has its own isolated environment, but the containers share the underlying Linux kernel.

**Key point:** The container does not need to include a complete operating system kernel.

## 4. Can Linux Docker Hosts Run Windows Containers?

A Linux Docker host cannot directly run Windows containers that require the Windows kernel.

Why?

Because Linux and Windows use different operating system kernels.

```text
Linux Docker Host
       |
   Linux Kernel
       |
   Linux Containers
```

```text
Windows Docker Host
       |
   Windows Kernel
       |
  Windows Containers
```

The container and host kernel must be compatible.

## 5. Docker on Windows

You may have installed Docker Desktop on Windows and successfully run a Linux container.

For example:

```bash
docker run nginx
```

This works because Docker Desktop can run Linux containers inside a Linux virtual machine or Linux environment provided by its backend.

The simplified architecture is:

```text
Windows Machine
       |
   Windows OS
       |
   Linux Virtual Machine
       |
    Linux Kernel
       |
   Linux Container
       |
      Nginx
```

So the Linux container is actually running on a Linux environment, not directly on the Windows kernel.

> Note: Docker Desktop uses virtualization or other supported backend technologies to run Linux containers on Windows. The exact architecture depends on the configuration.

## 6. Is Sharing the Kernel a Disadvantage?

Not really.

Docker is not primarily designed to virtualize different operating systems and kernels on the same hardware.

That is the main purpose of hypervisors and virtual machines.

### Hypervisors

A hypervisor allows different virtual machines to run different operating systems on the same physical hardware.

Example:

```text
Physical Hardware
       |
    Hypervisor
       |
   +-----------+-----------+
   |           |           |
 Ubuntu VM  Windows VM  Fedora VM
```

Each VM has its own guest OS kernel.

### Docker

Docker focuses on packaging and running applications in containers.

```text
Docker Host
    |
 Docker Engine
    |
 +----------+----------+
 |          |          |
 App 1     App 2      App 3
Container  Container  Container
```

The containers share the host kernel.

## 7. Main Purpose of Docker

The main purpose of Docker is to:

- Package applications and their dependencies.
- Containerize applications.
- Ship applications consistently.
- Run applications in different environments.
- Start and manage applications easily.
- Run applications as many times as needed.

### Example

A developer can package an application into a Docker image and run it in different environments:

```text
Developer Machine
       |
   Docker Image
       |
       +----------> Development
       |
       +----------> Testing
       |
       +----------> Production
```

This helps reduce the common problem:

> "It works on my machine, but it doesn't work in production."

## Quick Revision

1. An operating system consists of a kernel and software.
2. The kernel interacts with the underlying hardware.
3. Ubuntu, Fedora, OpenSUSE, and AlmaLinux use the Linux kernel.
4. Different Linux distributions mainly differ in their software and tools.
5. Docker containers share the host OS kernel.
6. A Linux Docker host can run Linux-based containers from different distributions.
7. Linux and Windows use different kernels.
8. Windows Docker Desktop can run Linux containers using a Linux environment.
9. Hypervisors are designed to run different operating systems and kernels.
10. Docker focuses on packaging, shipping, and running applications in containers.
