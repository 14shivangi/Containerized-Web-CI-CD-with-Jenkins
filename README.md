## End-to-End CI/CD Pipeline Using Jenkins Master-Agent, Docker & GitHub

#### Project Overview
This project demonstrates an automated CI/CD pipeline using:
- GitHub as Source Code Management (SCM)
- Jenkins Master Server for orchestration
- Jenkins Slave (Agent) for build execution
- Docker for containerization
- GitHub Webhooks for automatic triggering

Whenever a developer pushes code to GitHub, Jenkins automatically triggers a build through GitHub Webhooks, creates a Docker image, deploys a containerized web application, and updates the application without manual intervention.

### Architecture

```text
Developer Server
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

#### Technologies Used

| Technology | Purpose |
|------------|----------|
| Ubuntu 24.04 | Operating System |
| Git | Version Control |
| GitHub | Source Code Repository |
| Jenkins | CI/CD Automation |
| Docker | Containerization |
| GitHub Webhooks | Automated Build Trigger |
| Apache2 | Web Server |

### Infrastructure Setup
#### Server 1: Developer Server
Purpose:
- Application development
- Git operations
- Push code to GitHub

#### Server 2: Jenkins Master
Purpose:
- Manage CI/CD jobs
- Receive GitHub webhook events
- Control Jenkins agents

#### Server 3: Jenkins Slave (Agent)
Purpose:
- Execute build jobs
- Build Docker images
- Deploy containers

---

### Project Workflow

#### Step 1: Developer Creates application 
Create application files:
```bash
mkdir project
cd project
```
##### Inside this create a Dockerfile
```
vim Dockerfile
```

```
FROM ubuntu
RUN apt-get update -y
RUN apt-get install -y apache2
COPY . /var/www/html
CMD ["/usr/sbin/apache2ctl","-D","FOREGROUND"]
```
#### Step 2: Push Code to GitHub 
- Initialize repository:
  
```bash
git init
git add .
git commit -m "Initial Comme=it"
```
- Connect GitHub repository
  
```bash
git remote add origin <repository-url>
git push origin master
```

### Step 3: Install Jenkins on Master Server
Update server:

```bash
sudo apt update
```

Install Java:

```bash
sudo apt install fontconfig openjdk-21-jre -y
```

Install Jenkins:

```bash
sudo apt install jenkins -y
```
Start Jenkins:

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Verify:

```bash
sudo systemctl status jenkins
```

---

### Step 4: Configure Jenkins

Open:

```text
http://<Jenkins-Master-IP>:8080
```

Retrieve admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Install suggested plugins and create admin user.

---

### Step 5: Install Docker on Jenkins Master

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

---

### Step 6: Configure GitHub Webhook

GitHub Repository:

```text
Settings → Webhooks → Add Webhook
```

Payload URL:

```text
http://<Jenkins-IP>:8080/github-webhook/
```

Content Type:

```text
application/json
```

Save webhook.

---

### Step 7: Create Jenkins Job

Create:

```text
New Item → Freestyle Project
```

Configure:

#### Source Code Management

```text
Git
```

Provide repository URL.

#### Build Trigger

Enable:

```text
GitHub hook trigger for GITScm polling
```

---

### Step 8: Build Docker Image

Build Step → Execute Shell

```bash
sudo docker rm -f cont1 || true
sudo docker build -t webserver-img .
sudo docker run -itd --name cont1 -p 5000:80 webserver-img
```

Save and Build.

---

## Jenkins Slave Configuration

### Install Dependencies

```bash
sudo apt update -y
sudo apt install fontconfig openjdk-21-jre -y
sudo apt install docker.io -y
```

Start Docker:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

### Create Jenkins Node

Jenkins Dashboard:

```text
Manage Jenkins
    ↓
Nodes
    ↓
New Node
```

Configuration:

```text
Node Name: node1
Remote Root Directory: /dir1
Labels: prod
Launch Method:
Launch agent by connecting it to the controller
```

Save.

---

### Connect Agent

Copy the generated agent command from Jenkins and run it on the slave server.

Example:

```bash
curl -O <agent-file>
java -jar agent.jar url http://<master-ip>:8080 \
-secret <secret> 
-name node1
```

Agent status should become:

```text
Online
```

---

### Restrict Job to Slave Node

Job Configuration:

```text
Restrict where this project can be run
```

Enter:

```text
node1
```

Save configuration.

---

## Testing the Pipeline

Modify:

```html
index.html
```

Commit changes:

```bash
git add index.html
git commit -m "Updated website
git push origin master
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

