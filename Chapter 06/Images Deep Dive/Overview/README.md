# Docker Images - The Deep Dive

## Images and Containers
A `Docker image` is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software, including the code, runtime, libraries, environment variables, and configuration files. Docker images are built in layers, where each layer represents a filesystem change (e.g., adding a file or installing a package). Images are immutable once created, meaning they

![](./images/image.png)

do not change. On the other hand, A `Docker container` is a runnable instance of a Docker image. Containers are isolated environments that run on a single operating system kernel, sharing the OS but maintaining separate user spaces. They encapsulate the application and its dependencies, running as lightweight, portable, and consistent environments across various platforms.

## Image Sizes
Docker images are designed to be small and contain only the essential components required to run a specific application or service. For instance, the official Alpine Linux image is just 7MB. Windows-based images, however, tend to be significantly larger due to the nature of the Windows OS.

## Pulling Images
A freshly installed Docker host has no images in its local repository. We can check for existing images and pull new ones using the following commands:

```sh
$ docker images
$ docker pull redis:latest
$ docker pull alpine:latest
```

### Example Outputs
**Linux Example:**

```sh
$ docker pull redis:latest
latest: Pulling from library/redis
Digest: sha256:ea30bef6a1424d032295b90db20a869fc8db76331091543b7a80175cede7d887
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest

$ docker pull alpine:latest
latest: Pulling from library/alpine
Digest: sha256:02bb6f428431fbc2809c5d1b41eab5a68350194fb508869a33cb1af4444c9b11
Status: Downloaded newer image for alpine:latest
docker.io/library/alpine:latest

$ docker images
REPOSITORY   TAG     IMAGE ID        CREATED       SIZE
alpine       latest  44dd6f223004    9 days ago    7.73MB 
redis        latest  2334573cc576    2 weeks ago   111MB
```

**Windows Example:**

```sh
> docker pull mcr.microsoft.com/powershell:latest
latest: Pulling from powershell
Digest: sha256:fbc9555...123f3bd7
Status: Downloaded newer image for mcr.microsoft.com/powershell:latest
mcr.microsoft.com/powershell:latest

> docker images
REPOSITORY                      TAG      IMAGE ID       CREATED      SIZE
mcr.microsoft.com/powershell    latest   73175ce91dff   2 days ago   495MB
mcr.microsoft.com/.../iis       latest   6e5c6561c432   3 days ago   5.05GB
```

## Image Naming conventions
When pulling an image, specify its name and tag. Images are stored in registries, with Docker Hub being the most common. Images from official repositories are vetted and secure, while unofficial repositories require careful scrutiny.

### Examples:
**Official Repositories:**

```sh
$ docker pull mongo:4.2.24
$ docker pull busybox:glibc
$ docker pull alpine
```

**Unofficial Repositories:**

```sh
$ docker pull nigelpoulton/tu-demo:v2
```

**Third-Party Registries:**

```sh
$ docker pull gcr.io/google-containers/git-sync:v3.1.5
```

## Image Registries
Registries store images, with Docker Hub being the default. They can offer advanced services like image scanning. Images from official repositories are easily identified by their "Docker official image" badge.

## Image Naming and Tagging
Tags are arbitrary alpha-numeric values stored as metadata. An image can have multiple tags, allowing flexible versioning.

### Example:

```sh
$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED       SIZE
nigelpoulton/tu-demo     latest    c610c6a38555   22 months ago 58.1MB
nigelpoulton/tu-demo     v1        c610c6a38555   22 months ago 58.1MB
nigelpoulton/tu-demo     v2        6ba12825d092   16 months ago 58.6MB
```

## Conclusion
This guide provides an in-depth look at Docker images, including their creation, management, and best practices for usage. By understanding these concepts, you can effectively manage Docker images and ensure a consistent deployment process.