 - Containers have existed for about 10 years now, and some of the different types of containers are LXC, LXD, LXCFS, and so on.

- Now, here's the thing that's actually changed since the early days. Docker started out using LXC under the hood,but Docker moved away from LXC back in version 0.9.0, and that was around 2014,and switched to its own library called libcontainer.libcontainer later became runc, which is the reference implementationof the OCI runtime specification.

- So OCI stands for Open Container Initiative, and it's the open standard that defines how container runtimes and image formats should work. So today, when Docker runs a container, it's actually going through containerd and runc, not LXC, LXD, or LXCFS, which are still around as separate Linux container technologies,but Docker itself doesn't use them anymore.

- Now, setting up these low-level container environments is hard, and that is where Docker offers a high-level tool with several powerful functionalities, making it really easy for end users like us. And this is how Docker became so popular.

-So we've been talking about images and containers. 

- Let's understand the difference between the two.

- An image is a package or a template, just like a VM template that you might have worked with in the virtualization world. It is used to create one or more containers.

- Containers are running instances of images that are isolated and have their own environments and set of processes.

- This Dockerfile is then used to create an image for their applications. This image can now run on any host with Docker installed on it, and it's guaranteed to run the same way everywhere.

- So the ops team can now simply use the image to deploy the application, since the image was already working when the developer built it, and the operations have not modified it, it continues to work the same way when deployed in production. And that's one example of how a tool like Docker contributes to the DevOps culture.





  https://docs.docker.com/desktop/setup/install/linux/

 ```
 cat /etc/*release*
 dpkg --print-architecture
  ```



- Let's start by looking at the docker run command. The docker run command is used to run a container from an image.


```
docker run  <image name>
docker run nginx 
```




- Running the docker run nginx command will run an instance of the nginx application on the docker host if it already exists. If the image is not present on the host, it will go out to docker hub and pull that image down.

- But this is only done the first time.For subsequent executions, the same image will be reused.

 - Now one heads up before we go further, docker hub limits unauthenticated pulls to 100 every six hours per IP address. If you log in with docker login, that goes up to 200 every six hours on the free personal tier and unlimited on the paid pro team or business plans.

- So if you start hitting too many requests, errors during this lesson, that's why log in or use a paid account for CI work.


