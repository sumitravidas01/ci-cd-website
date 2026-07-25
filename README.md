# 🚀 Automated CI/CD Pipeline for a Static Website

A beginner-friendly DevOps project that demonstrates how to automate the deployment of a static website using **GitHub**, **Jenkins**, and **Docker**.

Every time code is pushed to GitHub, Jenkins automatically pulls the latest changes, builds a new Docker image, stops the old container, removes it, and deploys the updated application 

![Alt text](https://github.com/sumitravidas01/ci-cd-website/blob/main/images/Screenshot%202026-07-25%20161034.png?raw=true)
---

## 📌 Project Objective

Build a Continuous Integration and Continuous Deployment (CI/CD) pipeline that automatically deploys the latest version of a static website after every GitHub push — turning a manual, error-prone deploy process into a reliable, one-command-free workflow.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| VS Code | Source Code Editor |
| Git | Version Control |
| GitHub | Source Code Repository |
| Jenkins | CI/CD Automation Server |
| Docker | Containerization |

---

## 📁 Project Structure

```
cicd-website/
│
├── app/
│   ├── index.html
│   └── style.css
│
├── Docker/
│   └── Dockerfile
│
├── Jenkin/
│   └── Jenkinsfile
│
├── images/
│   ├── screenshort.png
```

---

## 🔄 CI/CD Workflow

```
Developer → VS Code → Git Commit → GitHub Repository
        │
        ▼
  GitHub Webhook
        │
        ▼
  Jenkins Pipeline
        │
        ├── Checkout Source Code
        ├── Build Docker Image
        ├── Stop Existing Container
        ├── Remove Existing Container
        └── Run New Docker Container
        │
        ▼
  Application Updated
```

---

## ⚙️ Project Workflow

### Step 1 – Create the Website

Create a basic HTML and CSS website in Visual Studio Code (`index.html`, `style.css`).

### Step 2 – Initialize Git Repository

```bash
git init
git add .
git commit -m "Initial commit"
```

### Step 3 – Push Code to GitHub

```bash
git remote add origin <https://github.com/sumitravidas01/ci-cd-website.git>
git branch -M main
git push -u origin main
```

### Step 4 – Install Jenkins

Install Jenkins locally or on a server, then verify at:

```
http://localhost:8081
```

Complete the setup wizard and install the recommended plugins.

### Step 5 – Connect Jenkins with GitHub

Create a new **Pipeline** job and configure:

- GitHub repository URL
- Branch to build (`main`)
- Path to `Jenkinsfile`

### Step 6 – Create the Jenkins Pipeline

See the actual `Jenkinsfile` below — it checks out the code, builds the image, and safely replaces the running container.

### Step 7 – Configure GitHub Webhook

In your GitHub repo: **Settings → ngrok → Webhooks → Add webhook**

```
Payload URL: https://gray-swimwear-waking.ngrok-free.dev/github-webhook/
Content type: application/json
Trigger: Just the push event
```

Every push now automatically triggers the pipeline.

---

## 📄 Dockerfile

```dockerfile
FROM nginx:latest

# Copy our static site into nginx's serving directory
COPY . /usr/share/nginx/html

EXPOSE 80

```

---

## 📄 Jenkinsfile

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "ci-cd-image"
        CONTAINER_NAME = "ci-cd-container"
        PORT = "8081"
    }

    stages {

        stage('Stop Old Container') {
            steps {
                bat '''
                docker stop %CONTAINER_NAME% || exit /b 0
                '''
            }
        }

        stage('Remove Old Container') {
            steps {
                bat '''
                docker rm %CONTAINER_NAME% || exit /b 0
                '''
            }
        }

        stage('Remove Old Image') {
            steps {
                bat '''
                docker rmi %IMAGE_NAME% || exit /b 0
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t %IMAGE_NAME% -f Docker\\Dockerfile .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                docker run -d --name %CONTAINER_NAME% -p %PORT%:80 %IMAGE_NAME%
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '''
                docker ps
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed!'
        }
    }
}

```

---

## 🐳 Docker Workflow

```
Source Code → Docker Build → Docker Image → Docker Container → Static Website
```

---

## 📂 Jenkins Pipeline Stages

- Checkout Source Code
- Stop Existing Container
- Remove Existing Container
- Remove Old Image
- Build Docker Image
- Run Docker Container
- Verify Deployment
- Deployment Successful!

---

## ▶️ How to Run Locally (Manual, Without Jenkins)

```bash
# Clone the repository
git clone <https://github.com/sumitravidas01/ci-cd-website.git>
cd ci-cd-static-website

# Build the Docker image
docker build -t ci-cd-image .

# Run the container
docker run -d -p 8081:80 --name ci-cd-container ci-cd-image
```

Then open:

```
http://localhost:8081
```

---

## 🔁 Full Deployment Process

1. Modify the website locally.
2. Commit the changes.
3. Push to GitHub.
4. GitHub sends a webhook event to Jenkins.
5. Jenkins checks out the latest code.
6. The old container is stopped and removed.
7. Docker image is rebuilt.
8. A new container is started from the fresh image.
9. The updated website is live — automatically.

---



## 📚 Skills Demonstrated

- Git Version Control
- GitHub Repository Management
- GitHub Webhooks
- Jenkins Pipeline (Groovy, Declarative Syntax)
- Docker Image Creation & Container Management
- Continuous Integration (CI)
- Continuous Deployment (CD)

---

## 📖 Learning Outcomes

After completing this project, you will understand:

- How Git integrates with Jenkins
- How GitHub Webhooks trigger automation
- How Jenkins Pipelines are structured and executed
- How Docker images are built and containers deployed
- How CI/CD removes manual steps from the release process

---

## 🚀 Future Improvements

- [ ] Add SonarQube for code quality analysis
- [ ] Add Trivy for Docker image security scanning
- [ ] Push Docker images to Docker Hub / a private registry
- [ ] Deploy the application to Kubernetes
- [ ] Add Prometheus and Grafana for monitoring
- [ ] Provision infrastructure using Terraform
- [ ] Configure servers with Ansible

---

## 👨‍💻 Author

**Sumit Ravidas**
  DevOps Engineer Intern 

[GitHub](https://github.com/sumitravidas01.git)



