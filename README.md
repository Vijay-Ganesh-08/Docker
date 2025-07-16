### What is Docker?
    - It is a Virtualization Software
    - Makes developing and deploying applications much easier
    - Packaging application with all the necessary dependencies, configuration, system tools and runtime.
    - A standardized unit, that has everything the application needs to run. (Ex. Application Code, Its dependencies, run time and environment configurations)

### Development Process before Containers.
    - Each developer needs to install and configure all services directly on their OS on their local machine. (Database, Messaging etc)
    - Installation process different for each OS Environment.
    - Many steps, where something can go wrong.

### Development Process with Containers.
    - Own Isolated environment.
    - Postgres Packaged with all dependencies and configs.
    - Start service as a Docker container using a 1 Docker command.
    - Command same for all OS.
    - Command same for all services.
    - Easy to run different versions of same app without any conflicts.

### Deployment Process before Containers.
    - Development team will create the Artifact package (ex. JAR) and provide installation instructions to Operations Team.
    - Operations Team handles installing and configuring apps and its dependencies.
    - Problems:
        - Installations and configurations done directly on the server’s OS.
        - Dependency version conflicts etc.
        - Miscommunication b/w Dev and Ops team.
        - Human errors can happen.

### Deployment Process with Containers.
    - Docker artifact includes everything an app needs.
    - No Configurations needed on the server except Docker runtime.
    - Less room for errors.
    - Install Docker runtime on the server - OneTime effort

### Virtual Machine vs Docker
    - Docker
        - Contains the OS Application Layer
        - Services and apps installed on top of that layer
        - It uses the OS kernel of the host to connect to hardware and make the applications running.
        - Docker Images are much smaller.
        - Containers takes seconds to start.
        - Compatible only with Linux distribution system.
    - Virtual Machine
        - Contains the OS Application and OS Kernel layer on its own.
        - Virtual Machines are heavier in terms of disk space.
        - VMs take minutes to start.
        - VM is compatible with all OS
    - Docker Desktop
        - Uses a Hypervisor layer with a lightweight Linux distribution system makes possible to run Linux based systems in Mac/Windows.
        - Docker Engine
            - A Server with a long-running daemon process “dockerd”
            - Manages images & containers
        - Docker CLI-Client
            - Command Line Interface (”docker”) to interact with Docker Server.
            - Execute docker commands to start/stop/.. containers.
        - GUI Client
            - To manage your containers in a Graphical user interface.

### Docker Image
    - Can be easily shared and moved like a zip or tar file.
    - An Executable application artifact.
    - Included app source code, but also complete environment configurations like (ex. JRE, JVM for java; NPM, Node for JavaScript)
    - Add environment variables, create directories, files etc.
    - Immutable template that defines how a container will be realized.
    - Image Versions:
        - Each updates are new versions as Docker Images.
        - Different versions are identified by tags.
        - *latest* tag pulls the latest version of the image, if the version not specified, it pulls latest by default.

### Docker Container
    - Actually starts the application.
    - A running instance of an Docker Image.
    - That’s when the container environment is created.
    - You can run multiple containers from 1 image.

### Docker Registry
    - A storage and distribution system for Docker images.
    - Official images available from applications like Redis, Mongo, Postgres etc.
    - Official images are maintained by the software authors or in collaboration with the Docker community.
    - Docker hosts one of the biggest Docker Registry, called “Docker Hub” to find and share images.
    - A dedicated team responsible for reviewing and publishing all content in the Docker Official images repositories.
    - Works in collaboration with Software maintainers, security experts.
    - However anyone can participate as collaboration takes place openly on GitHub.

### Container Port vs Host Port
    - Application inside container runs in an isolated Docker Network.
    - We need to expose the container port to the host (the machine the container runs on)
    - We need to bind the port number of the container with the host port in-order to access the application.
    - Standard to use the same port on your host as container is using.

### Public Docker Registries :
    - Largest public registry
    - Anyone can search and download Docker Images.

### Private Docker Registries :
    - Images which is are created by an Organization for internal usages.
    - You need to authenticate before accessing the registry.
    - All big cloud provider offer private registries : Amazon ECR, Google Container Registry etc.
    - Nexus (Popular artifact repository manager)
    - Docker Hub.

### Docker Registry :
    - A service providing storage.
    - Collection of repositories.
    - Docker Hub is a registry and you can host public or private repositories for your application.

### Docker Repository :
    - Collection of related images with same name but different versions.

###  Custom Images:
    - Companies create custom docker images for their applications.
    - UseCase : We want to deploy our app as a Docker Container.
    - We need to create a “definition” of how to build an image from our application.
    - Dockerfile is a text document that contains commands to assemble an image
    - Docker can then build an image by reading those instructions.
    - Dockerfile start from a parent image or “base image”
    - It’s a Docker image that your image is based on.
    - You choose the base image, depending on which tools you need to have available.
    - Basically we are drafting the commands/process which we follow locally to the container by Dockerfile.
    - `FROM <baseImage>` - Builds the custom image from specified image
        - Ex: FROM node:19-alpine
    - `COPY <src> <dest>` - Copies files and directories from <src> and adds them to the file system of the container path <dest>
        - Ex: `COPY src /app/`
        - While RUN is executed in the container and COPY is executed on the host.
    - `WORKDIR <setWorkDirectory>` - sets the working directory for all the following commands, like changing directory in terminal “cd” command.
        - Ex: `WORKDIR /app`
    - `RUN <Command>` - will execute any command in a shell inside the container environment.
        - Ex: `RUN npm install`
    - `CMD ["Command", "Params"]` - the instruction that is to be executed when a Docker container starts, there can be only 1 CMD instruction in a Dockerfile.
        - Ex:` CMD [”node”, ”server.js”]` - command and parameter is passed inside [].

### Build Image:
    - Once the Dockerfile is ready, we need to build as an image in-order to use it in a container.
    - `docker build -t <imageName>:<Version> <DockerFile directory>` command is used to build the image.
    - Each instruction given in the docker file is considered as layer/steps to build an image.
    - Once the Image is built, you can start the container and access the application with the port provided.

### Commands
    - `docker images` - Lists all the images available in the system
    - `docker ps` - Lists all the containers which are running in the system
    - `docker ps -a` - Lists all the container which are available in the system
    - `docker pull <imageName>:<version>` - to pull the image from docker hub.
    - `docker pull <imageName>` - to pull the latest image from docker hub.
    - `docker run <imageName>:<version>` - to run the docker image which blocks the terminal/command prompt for more command executions. if the given image is not available, it will pull from docker hub and start the container and the container name is auto generated by docker internally.
    - `docker run -d <imageName>:<version>` - to run the docker image in detached mode which will not block the terminal/command prompt for more command executions. if the given image is not available, it will pull from docker hub and start the container and the container name is auto generated by docker internally.
    - `docker logs <containerID>` - to print the logs from the container.
    - `docker start <containerID>` - To start the existing container available in the system, since docker run will create a new container everytime.
    - `docker stop <containerID>` - to stop the container.
    - `docker run -d -p <hostPort>:<containerPort> <imageName>:<version>` - to start the container with Port Binding.
        - Ex : `docker run -d -p 8080:80 nginx:1.29` - to start the container of ngnix version 1.29 which has the container port of 80 with the host port 8080.
        - Only 1 service can run on a specific port, so only 1 service can run on 8080.
    - `docker run —name <containerName> -d -p <hostPort>:<containerPort> <imageName>:<version>` - to start the container with Port Binding and the defined name.
        - Ex : `docker run --name web-app -d -p 8080:80 nginx:1.29` - to start the container of ngnix with the name web-app and version 1.29 which has the container port of 80 with the host port 8080.
    - `docker build -t <imageName>:<Version> <DockerFile directory>`
        - Ex: `docker build web-app:1.0 .`
            - web-app is the image name.
            - 1.0 is the version of this image.
            - . is the current directory.

### How Docker helps in Big Picture of SDLC
    - For Example:
    - Developer writes an application in JavaScript which connects to MongoDB.
    - Instead of installing MongoDB, developer downloads a docker container from docker hub to connect with MongoDB from JavaScript application in local machine.
    - Developer then commits the code in Git Repository.
    - CI/CD gets triggered (Ex. Jenkins) and it provides artifacts from the application. it first build the application and then builds an image out of the artifacts.
    - Docker image is pushed to a Private Repository.
    - At the time of deployment, the server pulls the image which is pushed to private repository and also the docker image which used to connect with MongoDB.