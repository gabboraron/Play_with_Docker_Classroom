# 🐋
***Contents***
- [Intro to docker containers](https://github.com/gabboraron/Play_with_Docker_Classroom/edit/main/README.md#there-are-several-challenges-that-well-need-to-consider-in-a-usual-scenario) based on [4]
- [Play with docker classroom](https://github.com/gabboraron/Play_with_Docker_Classroom/edit/main/README.md#there-are-several-challenges-that-well-need-to-consider-in-a-usual-scenario) based on [2]

  ----------

> *"The idea of using software containerization technology as a time-saving and cost-reduction solution is popular. One of the strengths of containerization is that you don't have to configure hardware and spend time installing operating systems and software to host a deployment. Containers are isolated from each other, and multiple containers can run on the same hardware. This configuration helps us use hardware more efficiently and can help improve our application's security."*
>
> *more: [Daniel Adetunji - How Docker Containers Work – Explained for Beginners](https://www.freecodecamp.org/news/how-docker-containers-work/)* 

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

> ### What is a container?
> A container is a loosely isolated environment that allows us to build and run software packages. These software packages include the code and all dependencies to run applications quickly and reliably on any computing environment. We call these packages container images.
>
> The container image becomes the unit we use to distribute our applications.

> ### What is software containerization?
> Software containerization is an OS-virtualization method used to deploy and run containers without using a virtual machine (VM). Containers can run on physical hardware, in the cloud, on VMs, and across multiple operating systems.

### What is Docker?
> Docker is a containerization platform used to develop, ship, and run containers. Docker doesn't use a hypervisor, and you can run Docker on your desktop or laptop if you're developing and testing applications. The desktop version of Docker supports Linux, Windows, and macOS. For production systems, Docker is available for server environments, including many variants of Linux and Microsoft Windows Server 2016 and above. Many clouds, including Azure, support Docker.

> ### Docker architecture
> The Docker platform consists of several components that we use to build, run, and manage our containerized applications.

> ### Docker Engine
> The Docker Engine consists of several components configured as a client-server implementation where the client and server run simultaneously on the same host. The client communicates with the server using a REST API, which enables the client to also communicate with a remote server instance.

> ### The Docker client
> There are two alternatives for Docker client: A command-line application named docker or a Graphical User Interface (GUI) based application called Docker Desktop. Both the CLI and Docker Desktop interact with a Docker server. The docker commands from the CLI or Docker Desktop use the Docker REST API to send instructions to either a local or remote server and function as the primary interface we use to manage our containers.

> ### The Docker server
> The Docker server is a daemon named dockerd. The dockerd daemon responds to requests from the client via the Docker REST API and can interact with other daemons. The Docker server is also responsible for tracking the lifecycle of our containers.

> ### Docker objects
> There are several objects that you'll create and configure to support your container deployments. These include networks, storage volumes, plugins, and other service objects. We won't cover all of these objects here, but it's good to keep in mind that these objects are items that we can create and deploy as needed.

> ### Docker Hub
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
| We use Unionfs to create Docker images. Unionfs is a filesystem that allows you to stack several directories—called branches—in such a way that it appears as if the content is merged. However, the content is physically kept separate. Unionfs allows you to add and remove branches as you build out your file system. <br> <br> For example, assume we're building an image for our web application from earlier. We'll layer the Ubuntu distribution as a base image on top of the boot file system. Next, we'll install nginx and our web app. We're effectively layering nginx and the web app on top of the original Ubuntu image. <br> <br> A final writeable layer is created once the container is run from the image. However, this layer doesn't persist when the container is destroyed. | <img src="https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/media/3-unionfs-diagram.svg"> |
| --- | ---- | 

> ### What is a base image?
> A base image is an image that uses the Docker scratch image. The scratch image is an empty container image that doesn't create a filesystem layer. This image assumes that the application you're going to run can directly use the host OS kernel.
>
> ### What is a parent image?
> A parent image is a container image from which you create your images.
>
> For example, instead of creating an image from scratch and then installing Ubuntu, we'll use an image already based on Ubuntu. We can even use an image that already has nginx installed. A parent image usually includes a container OS.
> 
> What is the main difference between base and parent images?
> Both image types allow us to create a reusable image. However, base images allow us more control over the final image contents. Recall from earlier that an image is immutable; you can only add to an image and not subtract.
>
> On Windows, you can only create container images that are based on Windows base container images. Microsoft provides and services these Windows base container images.

### What is a Dockerfile?
> A Dockerfile is a text file that contains the instructions we use to build and run a Docker image. The following aspects of the image are defined:
>
> - The base or parent image we use to create the new image
> - Commands to update the base OS and install additional software
> - Build artifacts to include, such as a developed application
> - Services to expose, such as storage and network configuration
> - Command to run when the container is launched
> 
> Let's map these aspects to an example Dockerfile. Suppose we're creating a Docker image for our ASP.NET Core website. The Dockerfile might look like the following example:
````Bash
# Step 1: Specify the parent image for the new image
FROM ubuntu:18.04

# Step 2: Update OS packages and install additional software
RUN apt -y update &&  apt install -y wget nginx software-properties-common apt-transport-https \
	&& wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
	&& dpkg -i packages-microsoft-prod.deb \
	&& add-apt-repository universe \
	&& apt -y update \
	&& apt install -y dotnet-sdk-3.0

# Step 3: Configure Nginx environment
CMD service nginx start

# Step 4: Configure Nginx environment
COPY ./default /etc/nginx/sites-available/default

# STEP 5: Configure work directory
WORKDIR /app

# STEP 6: Copy website code to container
COPY ./website/. .

# STEP 7: Configure network requirements
EXPOSE 80:8080

# STEP 8: Define the entry point of the process that runs in the container
ENTRYPOINT ["dotnet", "website.dll"]
````  
> There are several commands in this file that allow us to manipulate the image structure. For example, the `COPY` command copies the content from a specific folder on the local machine to the container image we're building.
>
> Recall that earlier, we mentioned that Docker images make use of `unionfs`. Each of these steps creates a cached container image as we build the final container image. These temporary images are layered on top of the previous image and presented as single image once all steps complete.
>
> Finally, notice the last step, *step 8.* The `ENTRYPOINT` in the file indicates which process will execute once we run a container from an image. If there's no `ENTRYPOINT` or another process to be executed, Docker will interpret that as there's nothing for the container to do, and the container will exit.
>
> more about docker files: [Anshita Bhasin - A step-by-step guide to create Dockerfile](https://medium.com/@anshita.bhasin/a-step-by-step-guide-to-create-dockerfile-9e3744d38d11)

### How to manage Docker images
> Docker images are large files that are initially stored on your PC, and we need tools to manage these files.
>
> The Docker CLI and Docker Desktop allow us to manage images by building, listing, removing, and running them. We manage Docker images by using the `docker` client. The client doesn't execute the commands directly, and sends all queries to the `dockerd daemon`.
>
> The most used command `docker build` to build Docker images. Let's assume we use the Dockerfile definition from earlier to build an image. Here's an example that shows the build command: `docker build -t temp-ubuntu .`. The output should end like this, if everythin works well: `Successfully tagged temp-ubuntu:latest`. When each step executes, a new layer gets added to the image we're building, based on the Dockerfile, showed earlier. Also, notice that we execute a number of commands to install software and manage configuration. Once the command runs, the intermediate container is removed. The underlying cached image is kept on the build host and not automatically deleted. This optimization ensures that later builds reuse these images to speed up build times.


### What is an image tag?
> An image tag is a text string that's used to version an image.
>
> When building an image, we name and optionally tag the image using the `-t` command flag. In our example, we named the image using `-t temp-ubuntu`, while the resulting image name was tagged `temp-ubuntu: latest`. An image is labeled with the `latest` tag if you don't specify a tag.
>
> A single image can have multiple tags assigned to it. By convention, the most recent version of an image is assigned the latest tag and a tag that describes the image version number. When you release a new version of an image, you can reassign the latest tag to reference the new image.
>
> For Windows, Microsoft doesn't provide base container images with the latest tag. For Windows base container images, you have to specify a tag that you want to use. For example, the Windows base container image for Server Core is `mcr.microsoft.com/windows/servercore`. Among its tags are `ltsc2016`, `ltsc2019`, and `ltsc2022`.

### How to list images
> The Docker software automatically configures a local image registry on your machine. You can view the images in this registry with the `docker images` command.
>
> The output here can be something like this:
> ````Text
> REPOSITORY          TAG                     IMAGE ID            CREATED                     SIZE
> tmp-ubuntu          latest             f89469694960        14 minutes ago         1.69GB
> tmp-ubuntu          version-1.0        f89469694960        14 minutes ago         1.69GB
> ubuntu              18.04                   a2a15febcdf3        5 weeks ago            64.2MB
> ````
>
> Notice how the image is listed with its Name, Tag, and an Image ID. Recall that we can apply multiple labels to an image.
>
> The image ID is a useful way to identify and manage images where the name or tag of an image might be ambiguous.

### How to remove an image
> You can remove an image from the local docker registry with the docker rmi command. This is useful if you need to save space on the container host disk, because container image layers add up to the total space available.
> 
> Specify the name or ID of the image to remove. This example removes the image for the sample web app using the image name:
> ````Bash
> docker rmi temp-ubuntu:version-1.0
> ````
> You can't remove an image if a container is still using the image. The docker rmi command returns an error message, which lists the container that relies on the image.


## How Docker containers work
> Earlier, you discovered the container becomes the unit you'll use to distribute your apps. You also learned the container is in a standardized format both your developer and operation teams use.
>
> In your example, you're developing an order-tracking portal that your company's various outlets will use. With the Docker image built, your operations team is now responsible for the deploying, rolling out updates, and managing your order-tracking portal.
>
> In the previous unit, you looked at how a Docker image is built. Here, you'll look a bit at a Docker container's lifecycle and how to manage containers. You'll also look at how to think about configuring data storage and the network options for your containers.

### How to manage Docker containers
> A Docker container has a lifecycle that you can use to manage and track the state of the container.
>
> To place a container in the run state, use the `run` command. You can also restart a container that's already running. When restarting a container, the container receives a termination signal to enable any running processes to shut down gracefully before the container's kernel terminates. **A container is considered in a running state until it's either paused, stopped, or killed.** A container can self-exit when the running process completes, or if the process goes into a fault state.
>
> - To pause a running container, use the `pause` command. **This command suspends all processes in the container.**
> - To stop a running container, use the `stop` command. The stop command enables the working process to shut down gracefully by sending it a termination signal. **The container's kernel terminates after the process shuts down.**
> - If you need to terminate the container, use the `kill` command to send a kill signal. The container's kernel captures the kill signal, but the running process doesn't. **This command forcefully terminates the working process in the container.**
> - To remove containers that are in a stopped state, use the `remove` command. **After removing a container, all data stored in the container gets destroyed.**
>

<center>
	<img alt="From the created state with the Docker run will become Running state where you can use Docker pause then will became Paused state from the with docker unpause can return to Running state, which can be stopped with Docker stop. The stopped container will be removed with docker remove." src="https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/media/4-docker-container-lifecycle-2.png">
</center>


### How to view available containers
> To list running containers, run the `docker ps` command. To see all containers in all states, pass the `-a` argument.
>
> *output:*
>
>
> ````Bash
> CONTAINER ID    IMAGE               COMMAND                CREATED          STATUS              PORTS          NAMES
> d93d40cc1ce9    tmp-ubuntu:latest  "dotnet website.dll …"  6 seconds ago    Up 5 seconds        8080/tcp      happy_wilbur
> 33a6cf71f7c1    tmp-ubuntu:latest  "dotnet website.dll …"  2 hours ago     Exited (0) 9 seconds ago            adoring_borg
> ````
>
> Here you can see:
> - **The image name** listed in the `IMAGE` column; in this example, `tmp-ubuntu: latest`. Notice how you're allowed to create more than one container from the same image. This is a powerful management feature you can use to enable scaling in your solutions.
> - **The container status** listed in the `STATUS` column. In this example, you have one container that's running and one container that has exited. The container's status is usually your first indicator of the container's health.
> - **The container name** listed in the `NAMES` column. Apart from the container ID in the first column, containers also receive a name. In this example, you didn't explicitly provide a name for each container, and as a result, Docker gave the container a random name. **To give a container an explicit name using the `--name` flag, use the `run` command.**
>

#### Why are containers given a name?
> This feature enables you to run multiple container instances of the same image. Container names are unique, which means if you specify a name, you can't reuse that name to create a new container.
>

#### run a container
> To start a container, use the `docker run` command. You only need to specify the image to run with its name or ID to launch the container from the image. A container launched in this manner provides an interactive experience.
>
> Here, to run the container with our website in the background, add the `-d` flag.
> ```Bash
> docker run -d tmp-ubuntu
> ```
> The command, in this case, only returns the ID of the new container.
>

#### pause a container
> To pause a container, run the docker pause command.
>
> Pausing a container suspends all processes. This command enables the container to continue processes at a later stage. The `docker unpause` command unsuspends all processes in the specified containers.
> 
> ```Bash
> docker pause happy_wilbur
> ```

#### restart a container
> To restart containers, run the `docker restart` command. The container receives a stop command followed by a start command. If the container doesn't respond to the stop command, then a kill signal is sent.
>
> ```Bash
> docker restart happy_wilbur
> ```

#### stop a container
> To stop a running container, run the `docker stop` command.
>
> The stop command sends a termination signal to the container and the processes running in the container.
> 
> ```Bash
> docker stop happy_wilbur
> ```

#### remove a container
> To remove a container, run the `docker rm` command.
>
> After you remove the container, all data in the container is destroyed. It's essential to always consider containers as temporary when thinking about storing data.
>
> ```Bash
> docker rm happy_wilbur
> ```

### container storage configuration
> ***As we described earlier, always consider containers as temporary when the app in a container needs to store data.***
>
> *Let's assume your tracking portal creates a log file in a subfolder to the root of the app; that is, directly to the container file system. When your app writes data to the log file, the system writes the data to the writable container layer.*
>
> - **Container storage is temporary** Your log file won't persist between container instances. For example, let's assume that you stop and remove the container. When you launch a new container instance, the new instance bases itself on the specified image, and all your previous data will be missing. Remember, all data in a container is destroyed with the container when you remove a container.
> - **Container storage is coupled to the underlying host machine** Accessing or moving the log file from the container is difficult, because the container is coupled to the underlying host machine. You have to connect to the container instance to access the file.
> - **Container storage drives are less performant** Containers implement a storage driver to allow your apps to write data. This driver introduces an extra abstraction to communicate with the host OS kernel, and is less performant than writing directly to a host filesystem.
>
> ***Containers can make use of two options to persist data. The first option is to make use of volumes, and the second is bind mounts.***
>
> #### volume?
> A volume is stored on the host filesystem at a specific folder location. Choose a folder where you know the data won't be modified by non-Docker processes.
>
> Docker creates and manages the new volume by running the `docker volume create` command. This command can form part of our Dockerfile definition, which means that you can create volumes as part of the container-creation process.
>
> Volumes are stored within directories on the host filesystem. Docker will mount and manage the volumes in the container. After mounting, these volumes are isolated from the host machine.
>
> In this example, you can create a directory on our container host and mount this volume into the container when you create the tracking portal container. When your tracking portal logs data, you can access this information via the container host's filesystem. You'll have access to this log file even if your container is removed.
>
> Docker also provides a way for third-party companies to build add-ons to be used as volumes. For example, Azure Storage provides a plugin to mount Azure Storage as volumes on Docker containers.
>
> #### bind mount?
> A bind mount is conceptually the same as a volume; however, instead of using a specific folder, you can mount any file or folder on the host. You're also expecting that the host can change the contents of these mounts. *Just like volumes, the bind mount is created if you mount it and it doesn't yet exist on the host.*
>
> **Bind mounts have limited functionality compared to volumes, and even though they're more performant, they depend on the host having a specific folder structure in place. Bind is the better choice if you want to update a file time to time.**
>
> Volumes are considered the preferred data-storage strategy to use with containers.
>
> For Windows containers, another option is available: You can mount an SMB path as a volume and present it to containers. This allows containers on different hosts to use the same persistent storage.

### Docker container network configuration
> The default Docker network configuration allows for isolating containers on the Docker host. This feature enables you to build and configure apps that can communicate securely with each other.
>
> You can choose which of these network configurations to apply to your container depending on its network requirements.
>
> | For Linux, there are six preconfigured network options | For Windows, there are six preconfigured network options |
> | ------------------------------------------------------ | -------------------------------------------------------- |
> | Bridge | NAT (Network Address Translation) |  
> | Host | Transparent | 
> | Overlay | Overlay |
> | IPvLan | L2Bridge |
> | MACvLan | L2Tunnel |
> | None | None |

### When to use Docker containers
> As we've learned, Docker has several features for us to use. Here, we'll look at the benefits that Docker provides to our development and operations teams. We'll also look at a few scenarios where Docker may not be the best choice.
>
> Recall from earlier that there are a number of challenges our team faces as they develop and publish our order-tracking portal. They're looking for a solution to: 
> - Manage our hosting environments with ease.
> - Guarantee continuity in how we deliver our software.
> - Ensure we make efficient use of server hardware.
> - Allow for the portability of our applications.
>
> Docker is a solution to these challenges.
>
> #### Efficient hardware use
> Containers run without using a virtual machine (VM). As we learned, the container relies on the host kernel for functions such as file system, network management, process scheduling, and memory management.
>
> Compared to a VM, we can see that a VM requires an OS installed to provide kernel functions to the running applications inside the VM. Keep in mind that the VM OS also requires disk space, memory, and CPU time. By removing the VM and the additional OS requirement, we can free resources on the host and use it for running other containers.
>
> #### Container isolation & Application portability
> Docker containers provide security features to run multiple containers simultaneously on the same host without affecting each other. As we saw, we can configure both data storage and network configuration to isolate our containers or share data and connectivity between specific containers.
>
> Because containers are lightweight, they don't suffer from slow startup or shutdown times like VMs. This aspect makes redeployment and other deploy scenarios—such as scaling up or down—smooth and fast.

### When not to use Docker containers
> #### Security and virtualization
> Containers provide a level of isolation. However, containers share a single host OS kernel, which can be a single point of attack.
>
> We also need to take into account aspects such as storage and networks to make sure that we consider all security aspects. For example, all containers will use the bridge network by default and can access each other via IP address.
>
> #### Service monitoring
> Managing the applications and containers is more complicated than traditional VM deployments. There are logging features that tell us about the state of the running containers, but more detailed information about services inside the container is harder to monitor.
>
> For example, Docker provides us with the `docker stats` command. This command returns information for the container such as percentage CPU usage, percentage memory usage, I/O written to disk, network data sent and received, and process IDs assigned. This information is useful as an immediate data stream; however, no aggregation is done because the data isn't stored. We'd have to install third-party software for meaningful data capture over a period of time.





























------------

# Play with Docker Classroom
> The Play with Docker classroom brings you labs and tutorials that help you get hands-on experience using Docker. In this classroom you will find a mix of labs and tutorials that will help Docker users, including SysAdmins, IT Pros, and Developers. There is a mix of hands-on tutorials right in the browser, instructions on setting up and using Docker.

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
1 https://docker-curriculum.com
2 https://training.play-with-docker.com
3 https://docs.docker.com/get-started/resources/
4 https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/



