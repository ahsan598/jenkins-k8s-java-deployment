### Process Flow:
![Project Diagram](https://github.com/ahsan598/java-k8s-deployment-pipeline-demo/blob/main/Deployment/processflow.png)

### I. Implementation steps:
#### 1. Setting Up the Environment

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