## End-to-End CI/CD Pipeline Using Jenkins Master-Agent, Docker & GitHub

This project demonstrates a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline using GitHub, Jenkins Master-Agent architecture, Docker, and Ubuntu Linux.

Whenever a developer pushes code to GitHub, Jenkins automatically triggers a build through GitHub Webhooks, creates a Docker image, deploys a containerized web application, and updates the application without manual intervention.

### Architecture

```text
Developer
    |
    | Git Push
    v
GitHub Repository
    |
    | Webhook Trigger
    v
Jenkins Master
    |
    | JNLP Connection
    v
Jenkins Agent (Slave)
    |
    | Docker Build & Deploy
    v
Docker Container
    |
    v
Web Application
```

## Technologies Used

- Git
- GitHub
- Jenkins
- Jenkins Master-Agent Architecture
- Docker
- Ubuntu Linux
- GitHub Webhooks
- JNLP Agent
- CI/CD

### Project Workflow

#### Step 1: Developer Pushes Code

```bash
git add .
git commit -m "Updated application"
git push origin master
```

#### Step 2: GitHub Webhook Triggers Jenkins

GitHub sends an event notification to Jenkins whenever code is pushed.

#### Step 3: Jenkins Pulls Latest Code

Jenkins fetches the latest source code from GitHub.

#### Step 4: Docker Image Build

```bash
docker build -t webserver-img .
```

#### Step 5: Remove Existing Container

```bash
docker rm -f cont1 || true
```

#### Step 6: Deploy New Container

```bash
docker run -itd --name cont1 -p 5000:80 webserver-img
```

#### Step 7: Application Goes Live

Access the application using:

```text
http://<SERVER-IP>:5000
```

### Jenkins Agent Configuration

#### Node Details

| Parameter | Value |
|------------|---------|
| Node Name | node1 |
| Label | prod |
| Remote Root Directory | /dir1 |
| Launch Method | JNLP |

The Jenkins Agent executes build jobs assigned by the Jenkins Master.

#### Dockerfile

```dockerfile
FROM ubuntu
RUN apt-get update -y
RUN apt-get install apache2 -y
COPY . /var/www/html
CMD ["/usr/sbin/apache2ctl","-D","FOREGROUND"]
```

#### Key Features
- Automated CI/CD Pipeline
- GitHub Webhook Integration
- Jenkins Master-Agent Architecture
- Dockerized Application Deployment
- Continuous Deployment
- Automated Container Replacement
- Linux Server Administration
- Infrastructure Automation Concepts

#### Jenkins Build Commands

```bash
sudo docker rm -f cont1 || true
sudo docker build -t webserver-img .
sudo docker run -itd --name cont1 -p 5000:80 webserver-img
```

#### Project Outcomes
- Automated application deployment process.
- Reduced manual deployment effort.
- Implemented CI/CD best practices.
- Improved deployment consistency using Docker containers.
- Learned Jenkins administration and agent management.
- Integrated GitHub with Jenkins using Webhooks.

#### Future Enhancements

- Convert Freestyle Jobs to Jenkins Pipelines.
- Deploy applications on Kubernetes.
- Integrate SonarQube for code quality analysis.
- Add automated testing stage.
- Store Docker images in Docker Hub or Amazon ECR.
- Deploy infrastructure using Terraform.

---

