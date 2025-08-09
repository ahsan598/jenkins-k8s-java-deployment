# 🚀 End-to-End CI/CD Pipeline for Deploying a Java Application to Kubernetes

This project demonstrates how to build and deploy a **Java application** using a **CI/CD pipeline** integrated with **Jenkins**, and deploy it on a **Kubernetes cluster**. The pipeline ensures that every code commit goes through **code quality checks**, **build**, **artifact storage**, and **automated deployment** — all done seamlessly using industry-standard DevOps tools.


### 🧰 Tools & Technologies Used

| Tool         | Purpose                                                                 |
|--------------|-------------------------------------------------------------------------|
| Jenkins      | CI/CD automation tool                                                   |
| SonarQube    | Static code analysis and code quality check                             |
| Nexus        | Artifact repository for storing built packages (e.g., `.jar` files)     |
| Maven        | Build and dependency management for Java applications                   |
| Kubernetes   | Container orchestration for scalable application deployment             |
| Docker       | Containerization of the Java application                                |
| Trivy        | Vulnerability Scanner for Container Images                              |
| Git          | Source code version control                                             |

---

### 📌 Project Objectives

- Automate the build and deployment process using **Jenkins**.
- Perform code quality analysis using **SonarQube**.
- Store build artifacts (e.g., `JAR` files) in **Nexus** Repository Manager.
- Containerize the Java application using **Docker**.
- Deploy the Dockerized application on a **Kubernetes** cluster.
- Ensure end-to-end DevOps flow from **code to production**.

---

### 📁 Project Structure

```sh
.
├── Dockerfile
├── Jenkinsfile
├── manifests/
│ ├── deployment.yaml
│ ├── service.yaml
├── src/
│ ├── main/
│ ├── test/
└── pom.xml
```


### ⚙️ What This Project Does

- **Jenkinsfile**: Contains the steps Jenkins uses for building, testing, and deploying the application.
- **Dockerfile**: Used to create a container image of the application.
- **kubernetes/**: Holds deployment manifests for running the app on a Kubernetes cluster.
  - `deployment.yaml`: Specifies how the application pods should be deployed and managed.
  - `service.yaml`: Exposes the application inside or outside the Kubernetes cluster.
- **src/**: Contains the Java source code of the application.
- **pom.xml**: Maven configuration file for managing dependencies, plugins, and build lifecycle.

---

### 🔄 CI/CD Pipeline Workflow

![Project Diagram](https://github.com/ahsan598/java-k8s-deployment-pipeline-demo/blob/main/deployment/processflow.png)

1. **Developer pushes code to Git**
2. **Jenkins** gets triggered by a webhook (or polling)
3. **SonarQube** scans the code for quality and vulnerabilities
4. **Maven** builds the application and generates a `.jar` file
5. Built artifact is **uploaded to Nexus Repository**
6. **Docker** builds an image using the JAR file.  
7. **Trivy** performs a security scan on the built image to detect vulnerabilities.  
8. If the scan passes, the image is pushed to **DockerHub** or a private registry. 
9. **Kubernetes** pulls the Docker image and deploys it using Deployment yamls.

---

### ⚙️ How to Run the Project Locally (For Practice)

> Note: These steps assume basic tools like Docker, Minikube (or any K8s cluster), and Jenkins are installed.

1. **Clone the repository:**

```bash
git clone https://github.com/yourusername/java-k8s-ci-cd.git
cd java-k8s-ci-cd
```

2. **Build with Maven:**

```bash
mvn clean install
```

3. **Run SonarQube locally (if testing locally):**

```bash
docker run -d -p 9000:9000 sonarqube
```

4. **Build Docker image:**

```bash
docker build -t yourusername/java-app:v1 .
```

5. **Deploy to Kubernetes:**

```bash
kubectl apply -f kubernetes/
```

---

### ✅ Benefits of This Project

 - Shows **end-to-end automation** of software delivery
 - Emphasizes **DevOps best practices**
 - Demonstrates **real-world toolchain integration**


### 📚 Learning Resources

- [Jenkins Official Docs](https://www.jenkins.io/doc/)
- [SonarQube Quickstart](https://docs.sonarsource.com/)
- [Nexus Repository Guide](https://help.sonatype.com/repomanager3)
- [Maven in 5 Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)
- [Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [Docker Docs](https://docs.docker.com/)

<!-- 
---

🛠️ **This is a personal project aimed at learning purposes**

**📚 Based on the tutorial by [@DevOps Shack](https://github.com/jaiswaladi246)** -->
