## Docker


### 命令
要从一个 Dockerfile 创建一个 Docker 容器并运行起来，你需要遵循一系列步骤。这些步骤包括构建 Docker 镜像、运行容器以及可选的操作如查看容器日志和容器管理等。以下是这些步骤的详细说明：

#### 1. 编写 Dockerfile

首先，你需要创建一个 Dockerfile，这是一个文本文件，包含了构建 Docker 镜像所需的所有命令。这个文件通常命名为 `Dockerfile`。例如，一个简单的 Dockerfile 可能看起来像这样：

```dockerfile
# 使用官方的 Python 运行时作为父镜像
FROM python:3.8-slim

# 设置工作目录
WORKDIR /app

# 将当前目录内容复制到位于 /app 的容器中
COPY . /app

# 安装 requirements.txt 中指定的任何所需程序包
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# 使端口 80 可供此容器外的环境使用
EXPOSE 80

# 定义环境变量
ENV NAME World

# 在容器启动时运行 app.py
CMD ["python", "app.py"]
```

#### 2. 构建 Docker 镜像

使用 Dockerfile 构建 Docker 镜像的命令如下：

```bash
docker build -t my-python-app .
```

这里，`-t my-python-app` 是给新创建的镜像设置标签的命令，而 `.` 指定 Dockerfile 所在的目录（当前目录）。

#### 3. 查看镜像

构建完镜像后，你可以使用下列命令来查看所有可用的 Docker 镜像：

```bash
docker images
```

#### 4. 运行 Docker 容器

一旦镜像构建完成，你可以使用 `docker run` 命令来启动一个容器实例：

```bash
docker run -p 4000:80 my-python-app
```

这里，`-p 4000:80` 指的是将容器的 80 端口映射到你的主机的 4000 端口。`my-python-app` 是你想要启动的镜像的名称。

#### 5. 查看运行中的容器

要查看当前运行中的所有 Docker 容器，可以使用：

```bash
docker ps
```

#### 6. 查看容器日志

如果你想查看容器的输出，可以使用 `docker logs` 命令：

```bash
docker logs <container_id>
```

其中 `<container_id>` 是通过 `docker ps` 得到的正在运行的容器的 ID。

#### 7. 停止运行的容器

当你想停止容器时，可以使用：

```bash
docker stop <container_id>
```

#### 8. 移除容器和镜像

如果需要清理不再使用的容器和镜像，可以使用以下命令：

- 移除容器：
  
  ```bash
  docker rm <container_id>
  ```

- 移除镜像：
  
  ```bash
  docker rmi <image_id>
  ```

以上就是从 Dockerfile 到运行 Docker 容器的完整流程。这些命令提供了基本的 Docker 操作方式，足以处理大多数基础的 Docker 使用场景。

Reference: https://www.interviewbit.com/docker-interview-questions/#docker-basic-questions
### Docker container
docker containers consist of applications and all their dependencies. They get rid of the infrastructure dependency while deploying and running applications. share the kernel and system resources with other containers and run as isolated systems
### Docker Image
executable packages(bundled with application code & dependencies, software packages, etc.) for the purpose of creating containers.

### Docker File
It is a text file that has all commands which need to be run for building a given image.
### Docker Compose
It is a YAML file. docker-compose is used for **creating multiple containers**, host them and **establish communication between them**. For the purpose of communication amongst the containers, ports are exposed by each and every container.
### volume in docker run
it is used to **create a persistent data storage** area that can be **shared between the host machine and the Docker container**. It provide a way for data to persist even if the container is stopped, restarted, or deleted. It can sharing data between containers ,  between the host and the container:


### get logs of a container
docker logs command:   `docker logs [OPTIONS] CONTAINER_ID_OR_NAME`
```shell
-f or --follow: This option allows you to continuously stream the logs of the container, similar to using tail -f on a log file.
--tail <number>: This option specifies the number of lines from the end of the logs to show.
--timestamps: This option adds timestamps to each log line.

```
### commands
list all of the docker commands you know, and briefly explain it.
```shell
docker run: Run a command in a new container 
docker run -d -p 8080:80 nginx

docker start: Start one or more stopped containers
docker start my-container
	
docker stop: Stop one or more running containers
docker stop my-container
	
docker build: Build an image from a Dockerfile
docker build -t my-app
	
docker pull: Pull an image or a repository from a registry
docker pull ubuntu:latest
	
docker push: Push an image or a repository to a registry
docker ps: lists all currently running containers.
docker images ists all locally available Docker images.
	
docker exec: Run a command in a running container, executes an interactive shell (bash) inside a running container named my-container
docker exec -it my-container bash 

	
docker search: Search the Docker Hub for images
docker attach: Attach to a running container
docker logs: Fetch the logs of a container
docker export: Export a containers filesystem as a tar archive
docker import: Import the contents from a tarball to create a filesystem image
docker history: Show the history of an image
docker tag: Tag an image into a repository
```


## Kubernetes
What is Kubernetes?
What is pod, what is service, and what is deployment?
	a. reading: https://www.educative.io/module/665EM3i03GVvJNjwJ/10370001/6231939383558144
	i. only need to know theories.
	b. reading: https://www.yuque.com/fairy-era/yg511q/eu30ue
Remember the below Kubectl commands
	a. 查看所有命名空间：
	`kubectl get namespaces`
	b. 查看所有Pod：
	`kubectl get pods`
	c. 查看指定命名空间的所有Pod：
	`kubectl get pods -n <namespace>`
	d. 查看所有运⾏中的Pod：
	`kubectl get pods --field-selector=status.phase=Running`
	e. 查看指定Pod的详细信息：
	`kubectl describe pod <pod-name>`
	f. 查看Pod的⽇志：
	`kubectl logs <pod-name>`
	g. 进⼊⼀个正在运⾏的容器：
	`kubectl exec -it <pod-name> -- /bin/bash`
	h. 删除⼀个Pod：
	`kubectl delete pod <pod-name>`
	i. 扩展⼀个部署：
	`kubectl scale deployment <deployment-name> --replicas=<num￾replicas>`
	a. 滚动更新⼀个部署：

## Agile

### Jira sprint scrum?
   Scrum is an agile framework that is designed to deliver complex products iteratively and incrementally. 
   
   The basic unit of development in Scrum is the sprint. It is a period of time during which specific work has to be completed and prepared for review in Scrum methodology.
   
   Jira is a project/task management tool. Jira provides issue tracking, project tracking, and agile project management functions. Users can create tasks, assign work, set due dates, and track progress right in the application. 

### Sprint length & daily routine.

   2 weeks.
   
   **Morning:** start my day by checking my emails and reviewing any updates or issues raised in our Jira dashboard.
   **Later:** daily standup meeting where each team member updates the rest of the team on what they worked on the previous day, what they plan to work on today, and any blockers they're facing
   **Daytime** spend my day coding, debugging, and testing.also spend time reviewing code from my peers.
   **Afternoon:** sometimes have grooming sessions where we review and estimate the complexity of future tasks in our backlog.
   **Till end of the day**:  learning and exploring new technologies, improving my understanding of our existing codebase.  updating the status of my tasks in Jira and preparing for the next day's work.
 
### Agile
   Agile is a project management and product development approach that is centered around iterative progress, team collaboration, and customer feedback.
   **Iterative and Incremental Development**: Rather than planning and executing the entire project at once, work is broken down into small parts and delivered incrementally over several iterations. Each iteration typically lasts 1-4 weeks.
   **User Stories and Backlog:** The requirements of the project are gathered in the form of user stories which are stored in a backlog. The backlog is continually updated as new requirements come in, existing ones change, or old ones are completed.
   **Retrospectives:** After each sprint, the team reflects on what went well, what didn't, and what they can improve in the next sprint.




