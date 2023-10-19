# 🐋
> *"The idea of using software containerization technology as a time-saving and cost-reduction solution is popular. One of the strengths of containerization is that you don't have to configure hardware and spend time installing operating systems and software to host a deployment. Containers are isolated from each other, and multiple containers can run on the same hardware. This configuration helps us use hardware more efficiently and can help improve our application's security."*

## There are several challenges that we'll need to consider in a usual scenario:

> ### Managing hosting environments
> The different environments all require both software and hardware management. We have to ensure that both the installed software and configured hardware in each is the same. We also need to configure aspects such as network access, data storage, and security per environment in a consistent and easily reproducible manner.

> ### Continuity in software delivery
> Deploying applications to our environments must happen consistently. Each deployment package must include all system packages, binaries, libraries, configuration files, and other items that ensure a fully functional application. We also need to make sure that all of these dependencies match software versions and architecture.

> ### Efficient hardware use
> Each deployed application must execute in such a way that it's isolated from other applications running on the same hardware. We aim to run more than one application per server to make the best use of resources without compromising each other.

> ### Application portability
> There are several reasons why application portability is essential. A hosting environment might fail, or we might need to scale out our application. In both instances, the potential result is redeploying our software to a new environment. We want to move software from one host to another even if the underlying infrastructure is different. Such a move needs to happen as fast as possible to reduce downtime for our customers.

## Introduction to Docker containers 

<center><img src="https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/media/2-docker-architecture.svg"></center>

### What is a container?
> A container is a loosely isolated environment that allows us to build and run software packages. These software packages include the code and all dependencies to run applications quickly and reliably on any computing environment. We call these packages container images.
>
> The container image becomes the unit we use to distribute our applications.

### What is software containerization?
> Software containerization is an OS-virtualization method used to deploy and run containers without using a virtual machine (VM). Containers can run on physical hardware, in the cloud, on VMs, and across multiple operating systems.

### What is Docker?
> Docker is a containerization platform used to develop, ship, and run containers. Docker doesn't use a hypervisor, and you can run Docker on your desktop or laptop if you're developing and testing applications. The desktop version of Docker supports Linux, Windows, and macOS. For production systems, Docker is available for server environments, including many variants of Linux and Microsoft Windows Server 2016 and above. Many clouds, including Azure, support Docker.

### Docker architecture
> The Docker platform consists of several components that we use to build, run, and manage our containerized applications.

### Docker Engine
> The Docker Engine consists of several components configured as a client-server implementation where the client and server run simultaneously on the same host. The client communicates with the server using a REST API, which enables the client to also communicate with a remote server instance.

### The Docker client
> There are two alternatives for Docker client: A command-line application named docker or a Graphical User Interface (GUI) based application called Docker Desktop. Both the CLI and Docker Desktop interact with a Docker server. The docker commands from the CLI or Docker Desktop use the Docker REST API to send instructions to either a local or remote server and function as the primary interface we use to manage our containers.

### The Docker server
> The Docker server is a daemon named dockerd. The dockerd daemon responds to requests from the client via the Docker REST API and can interact with other daemons. The Docker server is also responsible for tracking the lifecycle of our containers.

### Docker objects
> There are several objects that you'll create and configure to support your container deployments. These include networks, storage volumes, plugins, and other service objects. We won't cover all of these objects here, but it's good to keep in mind that these objects are items that we can create and deploy as needed.

### Docker Hub
> Docker Hub is a Software as a Service (SaaS) Docker container registry. Docker registries are repositories that we use to store and distribute the container images we create. Docker Hub is the default public registry Docker uses for image management.
>
> Keep in mind that you can create and use a private Docker registry or use one of the many cloud provider options available. For example, you can use Azure Container Registry to store container images to use in several Azure container-enabled services.

### How Docker images work
> Recall that we said the container image becomes the unit we use to distribute applications. We also mentioned that the container is in a standardized format both our developer and operation teams use.

| What is the host OS? | What is the container OS? |
| -------------------- | ------------------------- |
| The host OS is the OS on which the Docker engine runs. Docker containers running on Linux share the host OS kernel, and don't require a container OS as long as the binary can access the OS kernel directly. However, Windows containers need a container OS. The container depends on the OS kernel to manage services such as the file system, network management, process scheduling, and memory management. <br> <img src="https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/media/3-container-scratch-host-os.svg" > | The container OS is the OS that's part of the packaged image. We have the flexibility to include different versions of Linux or Windows operating systems in a container. This flexibility allows us to access specific OS features or install additional software our applications might use. The container OS is isolated from the host OS, and it's the environment in which we deploy and run our application. Combined with the image's immutability, this isolation means the environment for our application running in development is the same as in production. In our example, we're using Ubuntu Linux as the container OS, and this OS doesn't change from development or production. The image we use is always the same. <br> <img src="https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/media/3-container-ubuntu-host-os.svg"> | 

### Software packaged into a container
> The software packaged into a container isn't limited to the applications that our developers build. When we talk about software, we refer to application code, system packages, binaries, libraries, configuration files, and the operating system running in the container.
>
> For example, assume we're developing an order-tracking portal that our company's various outlets will use. We need to look at the complete stack of software that will run our web application. The application we're building is a .NET Core MVC app, and we plan to deploy the application using nginx as a reverse proxy server on Ubuntu Linux. All of these software components form part of the container image.

### What is a container image?
> A container image is a portable package that contains software. It's this image that, when run, becomes our container. The container is the in-memory instance of an image.
>
> A container image is immutable. Once you've built an image, you can't change it. The only way to change an image is to create a new image. This feature is our guarantee that the image we use in production is the same image used in development and QA.

### What is the Stackable Unification File System (Unionfs)?
| | | 
| --- | ---- | 
| We use Unionfs to create Docker images. Unionfs is a filesystem that allows you to stack several directories—called branches—in such a way that it appears as if the content is merged. However, the content is physically kept separate. Unionfs allows you to add and remove branches as you build out your file system. <br> <br> For example, assume we're building an image for our web application from earlier. We'll layer the Ubuntu distribution as a base image on top of the boot file system. Next, we'll install nginx and our web app. We're effectively layering nginx and the web app on top of the original Ubuntu image. <br> <br> A final writeable layer is created once the container is run from the image. However, this layer doesn't persist when the container is destroyed. | <img src="https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/media/3-unionfs-diagram.svg"> |

# Play with Docker Classroom
## For Developers
### Stage 1: The Basics
- Use the following command to clone the lab’s repo from GitHub. This will make a copy of the lab’s repo in a new sub-directory called `linux_tweet_app`. 
  
  `git clone https://github.com/dockersamples/linux_tweet_app` 

- In this step we’re going to start a new container and tell it to run the hostname command. The container will start, execute the hostname command, then exit. 
  
  `docker container run alpine hostname`
  
  The output below shows that the alpine:latest image could not be found locally. When this happens, Docker automatically pulls it from Docker Hub.
  
  ```
  Unable to find image 'alpine:latest' locally
  latest: Pulling from library/alpine
  88286f41530e: Pull complete
  Digest: sha256:f006ecbb824d87947d0b51ab8488634bf69fe4094959d935c0c103f4820a417d
  Status: Downloaded newer image for alpine:latest
  888e89a3b36b
  ```
- Docker keeps a container running as long as the process it started inside the container is still running. In this case the hostname process exits as soon as the output is written. This means the container stops. However, Docker doesn’t delete resources by default, so the container still exists in the Exited state.
  
  List all containers. `docker container ls --all`
  
  > Containers which do one task and then exit can be very useful. You could build a Docker image that executes a script to configure something. Anyone can execute that task just by running the container - they don’t need the actual scripts or configuration information.
  
- In the next example, we are going to run an Ubuntu Linux container on top of an Alpine Linux Docker host (Play With Docker uses Alpine Linux for its nodes). Run a Docker container and access its shell. 

  `docker container run --interactive --tty --rm ubuntu bash`
  
  parmeters:
  - `interactive` says you want an interactive session.
  - `tty` allocates a pseudo-tty.
  - `rm` tells Docker to go ahead and remove the container when it’s done executing.


### Sources:
- https://docker-curriculum.com
- https://training.play-with-docker.com
- https://docs.docker.com/get-started/resources/
- https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/



