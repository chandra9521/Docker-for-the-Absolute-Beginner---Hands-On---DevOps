 - Containers have existed for about 10 years now, and some of the different types of containers are LXC, LXD, LXCFS, and so on.

- Now, here's the thing that's actually changed since the early days. Docker started out using LXC under the hood,but Docker moved away from LXC back in version 0.9.0, and that was around 2014,and switched to its own library called libcontainer.libcontainer later became runc, which is the reference implementationof the OCI runtime specification.

- So OCI stands for Open Container Initiative, and it's the open standard that defines how container runtimes and image formats should work. So today, when Docker runs a container, it's actually going through containerd and runc, not LXC, LXD, or LXCFS, which are still around as separate Linux container technologies,but Docker itself doesn't use them anymore.

- Now, setting up these low-level container environments is hard, and that is where Docker offers a high-level tool with several powerful functionalities, making it really easy for end users like us. And this is how Docker became so popular.

-
