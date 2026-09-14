# Docker Images, Containers, and Registries

A large number of applications are already available as container images.

Organizations can package their applications into container images and publish them to container registries.

## 1. Container Registries

A **container registry** is a place where container images are stored and distributed.

The most popular public registry is **Docker Hub**.

Other commonly used registries include:

- Docker Hub
- GitHub Container Registry (GHCR)
- Amazon Elastic Container Registry (Amazon ECR)
- Google Artifact Registry
- Azure Container Registry (ACR)

You can find images for many common:

- Operating systems
- Databases
- Programming languages
- Web servers
- Developer tools
- Monitoring tools
- Other applications and services

---

## 2. Running an Application from an Image

Once Docker is installed on a host and the required image is available, we can run an application using:

```bash
docker run <image-name>
```

For example:

```bash
docker run ansible
```

This creates and starts a container from the `ansible` image.

Similarly, we can run other applications:

```bash
docker run mongo
docker run redis
docker run node
```

The exact image name and tag should be checked before running an image.

---

## 3. Running Multiple Instances

If we need multiple instances of an application, we can create multiple containers from the same image.

For example:

```text
             Load Balancer
                  |
        +---------+---------+
        |         |         |
   Container  Container  Container
       1          2          3
        |         |         |
      App       App       App
```

If one container fails, it can be removed and another container can be started.

In real production environments, container orchestration platforms and other solutions can automate:

- Container deployment
- Health checks
- Restarting failed containers
- Scaling
- Load balancing

These topics can be covered later.

---

# 4. Docker Image vs Container

It is important to understand the difference between an **image** and a **container**.

### Docker Image

An image is a **package/template** used to create containers.

It contains the application and everything required to run it, such as:

- Application code
- Runtime
- Libraries
- Dependencies
- Configuration/files required by the application

An image is similar to a **VM template** in the virtualization world.

```text
Docker Image
     |
     +----> Container 1
     |
     +----> Container 2
     |
     +----> Container 3
```

The same image can be used to create multiple containers.

### Docker Container

A container is a **running instance of an image**.

It provides an isolated environment with its own processes, filesystem view, network configuration, etc.

```text
Image
  |
  +----> Running Container
  |
  +----> Running Container
  |
  +----> Running Container
```

### Simple Comparison

| Image | Container |
|---|---|
| Template/package | Running instance |
| Used to create containers | Created from an image |
| Stored in a registry/local Docker host | Runs on a Docker host |
| Read-only image layers | Uses image + writable container layer |

**Easy way to remember:**

> **Image = Template**  
> **Container = Running instance of the template**

---

# 5. Using Existing Images

Many applications are already available as Docker images.

For example:

```text
Docker Registry
      |
      +---- Nginx
      +---- Redis
      +---- MongoDB
      +---- Node.js
      +---- Ansible
      +---- Other applications
```

We can pull an image and use it to create containers.

```bash
docker pull redis
docker run redis
```

Docker can also automatically pull an image when required by `docker run`, if the image is not already available locally.

---

# 6. Creating Our Own Docker Image

If an application or tool is not available as an existing image, we can create our own Docker image.

The image can then be stored in a container registry.

```text
Application
     |
 Dockerfile
     |
     v
Docker Image
     |
     v
Container Registry
     |
     v
Other Docker Hosts
```

For example:

```text
Build Image
     |
     v
Push to Docker Hub / ECR / GHCR
     |
     v
Pull Image
     |
     v
Run Container
```

This makes it possible to package an application once and run it consistently across different environments.

## Quick Revision

- A **container registry** stores and distributes container images.
- **Docker Hub** is a popular public container registry.
- Other registries include **GHCR, Amazon ECR, Google Artifact Registry, and Azure Container Registry**.
- A **Docker image** is a template/package used to create containers.
- A **container** is a running instance of an image.
- One image can be used to create multiple containers.
- `docker run <image>` creates and starts a container from an image.
- Multiple containers can run the same application.
- If an application is not available as an existing image, we can build our own image and push it to a registry.
