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






