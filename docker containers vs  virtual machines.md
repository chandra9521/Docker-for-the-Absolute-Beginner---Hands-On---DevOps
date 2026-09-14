# Docker Containers vs Virtual Machines

Containers and virtual machines both provide isolation, but they work differently and have different resource requirements.

## 1. Docker Container Architecture

In Docker, the basic architecture looks like this:

```text
Underlying Hardware
        |
   Host Operating System
        |
    Docker Engine
        |
   +----+----+----+
   |    |    |    |
  C1   C2   C3   C4
   |    |    |    |
 App  App  App  App
```

Containers mainly contain the application and the libraries/dependencies required by that application.

They share the host OS kernel.

## 2. Virtual Machine Architecture

With virtual machines, a hypervisor such as VMware ESXi runs on the underlying hardware.

Each VM has its own complete operating system.

```text
Underlying Hardware
        |
     Hypervisor
        |
   +----+----+----+
   |    |    |    |
  VM1  VM2  VM3  VM4
   |    |    |    |
  OS   OS   OS   OS
   |    |    |    |
 Deps Deps Deps Deps
   |    |    |    |
 App  App  App  App
```

Each virtual machine has its own OS and kernel.

---

## 3. Resource Utilization

### Virtual Machines

VMs require more resources because every VM needs its own operating system and kernel.

```text
VM 1 → OS + Kernel + Dependencies + App
VM 2 → OS + Kernel + Dependencies + App
VM 3 → OS + Kernel + Dependencies + App
```

This creates additional overhead.

### Containers

Containers share the host kernel and only need the application and its required libraries/dependencies.

```text
Host OS + Kernel
       |
 Docker Engine
       |
Container → Dependencies + App
Container → Dependencies + App
Container → Dependencies + App
```

Because of this, containers generally use fewer resources than VMs.

---

## 4. Disk Space

Virtual machines are relatively heavy because each VM contains a complete operating system.

- VMs are commonly measured in **GBs**.
- Container images are often much smaller and can be measured in **MBs**, although the actual size varies significantly depending on the base image and application.

This makes containers more lightweight compared to traditional VMs.

---

## 5. Startup Time

Containers usually start much faster because they do not need to boot a complete operating system.

```text
Container
    ↓
Start application
    ↓
Usually seconds
```

A VM needs to boot its complete guest operating system first.

```text
VM
 ↓
Boot OS
 ↓
Initialize services
 ↓
Start application
 ↓
Usually takes longer
```

In practice, startup times depend on the workload and configuration, but containers are generally much faster to start.

---

## 6. Isolation

Virtual machines generally provide stronger isolation because each VM has its own operating system and kernel.

Containers provide process-level isolation while sharing the host kernel.

| Feature | Containers | Virtual Machines |
|---|---|---|
| Kernel | Shared | Separate kernel per VM |
| Isolation | Lower compared to VMs | Stronger isolation |
| Resource usage | Lower | Higher |
| Disk usage | Usually smaller | Usually larger |
| Startup | Usually seconds | Usually longer |
| OS flexibility | Host-kernel dependent | Can run different guest OSs |

> **Important:** "Less isolation" does not mean containers are insecure. Containers provide strong isolation mechanisms, but VMs provide a different and generally stronger isolation boundary.

---

## 7. Different Operating Systems

VMs do not depend on the host OS kernel in the same way containers do.

For example, a hypervisor can run:

```text
Physical Hardware
       |
   Hypervisor
       |
   +-----------+-----------+
   |           |           |
 Linux VM   Windows VM  Linux VM
```

This allows Linux and Windows workloads to run on the same physical infrastructure.

Containers, on the other hand, share the host kernel, so the container's OS environment needs to be compatible with that kernel.

---

# 8. Containers + Virtual Machines

It is not always a choice between containers **or** virtual machines.

In real-world environments, they are often used together.

A common architecture is:

```text
Physical Hardware
       |
    Hypervisor
       |
   +-----------+-----------+
   |           |           |
 Docker Host Docker Host Docker Host
   |           |           |
 Containers   Containers   Containers
   |           |           |
  Apps         Apps         Apps
```

Here, Docker hosts themselves can run as virtual machines.

This gives us the advantages of both technologies.

### Benefits of VMs

- Easy to provision and decommission Docker hosts.
- Stronger isolation at the VM level.
- Ability to run different guest operating systems.
- Better utilization of physical infrastructure.

### Benefits of Containers

- Lightweight application packaging.
- Faster startup.
- Easier application deployment.
- Easy scaling.
- Better application density on a host.

---

## 9. Traditional vs Containerized Approach

Earlier, it was common to provision a separate VM for each application.

```text
VM 1 → Application 1
VM 2 → Application 2
VM 3 → Application 3
VM 4 → Application 4
```

With containers, we can run many applications/containers on fewer Docker hosts.

```text
Docker VM 1
 ├── Container 1
 ├── Container 2
 ├── Container 3
 ├── Container 4
 └── Container 5

Docker VM 2
 ├── Container 6
 ├── Container 7
 ├── Container 8
 └── Container 9
```

So instead of creating a VM for every application, a VM can act as a Docker host running many containers.

---

## Quick Revision

- VMs include a complete guest OS and kernel.
- Containers share the host OS kernel.
- VMs generally consume more resources.
- Containers are generally more lightweight.
- VMs usually require more disk space.
- Containers usually start faster.
- VMs provide stronger isolation boundaries.
- VMs can run different guest operating systems on the same hypervisor.
- Containers depend on a compatible host kernel.
- In real environments, containers often run **inside VMs**.
- One Docker host can run many containers.
- The combination of VMs + containers provides benefits from both technologies.
