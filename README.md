# Deployed Java application on Kubernetes using Jenkins: 

Integrated a CI/CD pipeline with Jenkins for the automated deployment of a Java application on Kubernetes. Utilized Nexus for artifact storage and Maven for build management, ensuring efficient deployment workflows. SonarQube was integrated into the pipeline for code quality analysis, enhancing the overall reliability and performance of the application.


### I. Resources used in this project:

**Kubernetes** (often abbreviated as K8s) is an open-source container orchestration platform designed to automate deploying, scaling, and managing containerized applications. It helps manage complex microservices architectures, allowing developers to deploy applications in a consistent and efficient manner. Kubernetes abstracts the underlying infrastructure and provides features such as load balancing, service discovery, storage orchestration, and self-healing capabilities (auto-restarting, auto-replacing, and auto-scaling).

**Jenkins** is a popular open-source automation server that helps automate various tasks in the software development process, including building, testing, and deploying software.

**Github** is a web-based platform for version control and collaboration, allowing developers to store, manage, and track changes in their code repositories using Git. It facilitates collaboration among teams and provides features for code review, issue tracking, and project management.

**Sonarqube** is an open-source platform for continuous inspection of code quality. It performs automatic reviews of code to detect bugs, code smells, and security vulnerabilities in various programming languages.

**Docker**  is a platform for developing, shipping, and running applications in containers. Containers are lightweight, portable, and provide an isolated environment for applications, making them ideal for microservices architecture.

**Maven** is a build automation and project management tool primarily used for Java projects. It simplifies the process of building, managing, and deploying Java applications by providing a standard way to organize project structure, manage dependencies, and automate tasks. Maven uses an XML file called pom.xml (Project Object Model) to configure the project settings, dependencies, and build process.

**Nexus**  is a repository manager that allows you to store and manage software components, binaries, and artifacts. It supports various formats, including Maven, Docker, npm, and more, providing a central location for all your dependencies.

**Trivy** is a vulnerability scanner for containers and other artifacts. It detects vulnerabilities in your images, file systems, and Git repositories.


### II. Process Flow:

![Project Diagram](https://github.com/ahsan598/deploy_javaapp_on_K8s/blob/main/processflow.png)


### III. Implementation steps:

##### 1. Setting Up the Environment

1.1. Jenkins Installation on EC2
- Launch EC2 Instance:
- Use an Amazon Linux 2 or Ubuntu instance.
- Configure security groups to allow HTTP (port 80), Nexus (port 8081), Jenkins (port 8080), SonarQube (9000).

1.2. Jenkins Installation
- Install Java:
```sh
sudo yum update -y
sudo yum install java-1.8.0-openjdk-devel -y
```

- Install Jenkins
```sh
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

1.3. Docker Installation
- Install Docker:
```sh
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

1.4. SonarQube Installation
- Pull SonarQube Docker Image:
```sh
docker pull sonarqube
```

- Run SonarQube Container:
```sh
docker run -d --name sonarqube -p 9000:9000 sonarqube
```

1.5. Trivy Installation
- Install via Docker
```sh
docker pull aquasec/trivy:latest
```
- Verify trivy
```sh
trivy --version
```

1.6. Nexus Installation:
- Install Nexus Repository
```sh
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -xzvf latest-unix.tar.gz
cd nexus-<version>
sudo useradd -r -s /bin/false nexus
sudo chown -R nexus:nexus /path/to/nexus-<version>
sudo -u nexus /path/to/nexus-<version>/bin/nexus start
```

- Access Nexus
```sh
http://localhost:8081
```


##### 2. Configuring GitHub Repository

2.1. Create a Repository:
- Create a new repository on GitHub for the web application.
- Clone Repository:

```sh
git clone https://github.com/your-username/your-repository.git
cd your-repository
```


##### 3. Integrating Jenkins with GitHub, Docker, SonarQube, Nexus & Maven

3.1. Install GitHub Plugin:
- Go to Jenkins Dashboard > Manage Jenkins > Manage Plugins > Available.
- Search for "GitHub Integration Plugin" and install it.

3.2.  Configure GitHub Webhook:
- Go to your GitHub repository settings.
- Navigate to Webhooks and add a new webhook.
- Set the payload URL to http://<your-jenkins-server-ip>:8080/github-webhook/.


Integrate the remaining tools by installing the necessary plugins and configuring them under the tools configuration settings.


##### 4. Jenkins Pipeline Configuration

4.1. Create Jenkins Pipeline Job:
- Create a new pipeline job in Jenkins.
- Configure the pipeline script to pull the code from GitHub and build it.
- Pipeline Script (Jenkinsfile)  is added in repository

#####  5. Dockerizing the Application

5.1.  Dockerfile:
- Dockerfile  is added in repository
- Build Docker Image:
```sh
docker build -t your-docker-image-name .
```

##### 6. SonarQube Quality Gate

6.1. SonarQube Configuration:
- Access SonarQube at http://<your-ec2-ip>:9000.
- Set up a new project and generate a token.
- Add the token and project key in Jenkinsfile.

##### 7. Deployment of application to Kubernetes

7.1. Apply the Deployment and Service:
- Use the kubectl command to deploy the application.
```sh
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

7.2. Verify the Deployment:
- Check Pods Status:
```sh
kubectl get pods
```

- Check Services:
```sh
kubectl get services
```