# Kaniko demo

>docker build -t devops-toolkit .

## 1. Steps to Test building a docker image inside a docker container.
>minikube start
- Create a docker container: >kubectl apply -f docker.yaml
- command to wait untill pod is ready: >kubectl wait --for condition=containersready pod docker
- Open a shell on the docker container:
    >apk add -U git
    >git clone https://github.com/ezzat223/kaniko-demo.git (Don't use SSH)
    >cd kaniko-demo/
    >docker build -t devops-toolkit .
        - It will generate this error `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is  the docker daemon running?`
    - Finally delete the container.

## 2. Build docker image through Docker Socket (This is what DinD does):
- The Docker socket `(/var/run/docker.sock)` is a Unix socket file that the ***Docker daemon*** listens on. 
  It allows processes to communicate with the `Docker daemon` to manage containers, images, networks, etc.

### Docker Daemon (`dockerd`):
- is the core service behind Docker. 
- It’s a server that runs in the background on the host machine, managing Docker containers, images, networks, and volumes. 
- It listens to Docker API requests (either from docker CLI commands or external programs) and handles the lifecycle of containers.

#### How Docker Daemon Works:

- The Docker Daemon uses a `client-server architecture`.
1. `Client`: This is the `docker command-line interface (CLI)` or a program that sends requests to the daemon.
2. `Server`: This is the `dockerd` process, which listens to these API requests and processes them.

### Docker Runtime (`runc`):
- Once the container is `ready to start`, the Docker Daemon hands off control to the Docker Runtime.
- The runtime interacts directly with the kernel to launch and run the container using cgroups (for resource limits) and namespaces (for isolation).
- The runtime then ensures the container continues running as expected and interacts with the kernel-level container features.

>kubectl apply -f docker-socket.yml
- Open a shell on the docker container:
    >apk add -U git
    >git clone https://github.com/ezzat223/kaniko-demo.git (Don't use SSH)
    >cd kaniko-demo/
    >docker build -t devops-toolkit .
    - It worked!
- Finally delete the container.

## 3. Build using Kaniko
- Make changes to kaniko-git.yaml by adding your GitHub and Dockerhub users instead.
>git add .
>git commit -m "Changed registry info in kaniko-git.yaml"
>git push origin master