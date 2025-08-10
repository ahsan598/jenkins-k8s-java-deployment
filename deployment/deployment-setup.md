# <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/jenkins/jenkins-original.svg" alt="Jenkins" width="40"/>  CI/CD Pipeline Deployment with Jenkins, SonarQube, Nexus, and Kubernetes

This guide explains how to set up a CI/CD pipeline using Jenkins, SonarQube, Trivy, Nexus, and Docker, and then deploy the app to Kubernetes.

---

### 1. 🛠 Environment Setup on AWS EC2
- Launch EC2 Instance:
  - Use Amazon Linux 2 or Ubuntu
  - Open these ports in the Security Group:
    - `22` (SSH)
    - `80` (HTTP)
    - `8080` (Jenkins)
    - `8081` (Nexus) 
    - `9000` (Sonarqube)

- SSH into the instance using **Git Bash**:
```sh
ssh -i /path/to/key.pem ubuntu@<ec2-public-ip>
```

**If you want to install Jenkins, Docker, SonarQube, and other required tools on your instance, please follow this deployment setup guide.**

### 2. ⚙️ Install Required Tools

**1. Jenkins**
```sh
sudo yum update -y
sudo yum install java-1.8.0-openjdk-devel -y
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
> Access via: http://<ec2-public-ip>:8080


**2. Docker**
```sh
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

**3. SonarQube**
- Running as a container
```sh
docker pull sonarqube
docker run -d --name sonarqube -p 9000:9000 sonarqube
```
> Access via: http://<ec2-public-ip>:9000

**4. Trivy**
```sh
docker pull aquasec/trivy:latest
```

**5. Nexus**
```sh
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -xzvf latest-unix.tar.gz
cd nexus-<version>
sudo useradd -r -s /bin/false nexus
sudo chown -R nexus:nexus ./nexus-<version>
sudo -u nexus ./nexus-<version>/bin/nexus start
```
> Access via: http://<ec2-public-ip>:8081

> **Note**: Make sure that ports `8080`, `8081`, and `9000` are open for inbound traffic in your EC2 instance’s security group. Otherwise, you won’t be able to access these services remotely via http://<ec2-public-ip>:<port>.


### 3. 🌐 GitHub Configuration
- Create a GitHub repository and clone it:
```sh
git clone https://github.com/your-username/your-repo.git
cd your-repo
```

### 📁 Files to Include in Repo
- **Dockerfile**
- **Jenkinsfile**
- **deployment.yaml**
- **service.yaml**


### 4. 🔄 Integrate Jenkins with Tools

**1. Jenkins Plugin Setup:**
- Install the following Jenkins plugins:
  - GitHub Integration Plugin
  - Docker Pipeline
  - SonarQube Scanner
  - Nexus Artifact Uploader

**2. GitHub Webhook:**
- Add a webhook in GitHub repo:
  - Payload URL: http://<$ your-jenkins-ip>:8080/github-webhook/


### 5. 🧪 Jenkins Pipeline
- Create a Pipeline Job in Jenkins
- Link it to your GitHub repository
- Use the included **Jenkinsfile** (from your repo)


### 6. 🐳 Dockerize the App
- Ensure **Dockerfile** is present in your repo.
- Build Docker Image:
```sh
docker build -t <your-docker-image-name> .
```

> Push to DockerHub or Nexus if configured.


### 7. ✅ SonarQube Quality Gate
- Access SonarQube UI: http://<$ ec2-instance-ip>:9000
- Create a project and generate a token
- Add token & project key to **Jenkinsfile**


### 8. 🚢 Kubernetes Deployment

- Deploy the Application
```sh
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

- Verify Deployment
```sh
kubectl get pods
kubectl get services
```

---

### 📦 Summary of Required Ports

| Service   | Port |
| --------- | ---- |
| SSH       | 22   |
| Jenkins   | 8080 |
| Nexus     | 8081 |
| SonarQube | 9000 |
| HTTP/App  | 80   |


### 🧾 Optional: Clean-Up
```sh
docker stop sonarqube
sudo systemctl stop jenkins
```