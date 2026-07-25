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
http://localhost:8080
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

In your GitHub repo: **Settings → Webhooks → Add webhook**

```
Payload URL: http://<jenkins-server>:8080/github-webhook/
Content type: application/json
Trigger: Just the push event
```

Every push now automatically triggers the pipeline.

---

## 📄 Dockerfile

```dockerfile
FROM nginx:alpine

# Remove default nginx static assets
RUN rm -rf /usr/share/nginx/html/*

# Copy our static site into nginx's serving directory
COPY . /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

## 📄 Jenkinsfile

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "static-website"
        CONTAINER_NAME = "static-website"
        PORT = "8080"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh 'docker stop $CONTAINER_NAME || true'
            }
        }

        stage('Remove Existing Container') {
            steps {
                sh 'docker rm $CONTAINER_NAME || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p $PORT:80 --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Pipeline failed — check logs above.'
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
- Build Docker Image
- Stop Existing Container
- Remove Existing Container
- Run New Container

---

## ▶️ How to Run Locally (Manual, Without Jenkins)

```bash
# Clone the repository
git clone <repository-url>
cd ci-cd-static-website

# Build the Docker image
docker build -t static-website .

# Run the container
docker run -d -p 8080:80 --name static-website static-website
```

Then open:

```
http://localhost:8080
```

---

## 🔁 Full Deployment Process

1. Modify the website locally.
2. Commit the changes.
3. Push to GitHub.
4. GitHub sends a webhook event to Jenkins.
5. Jenkins checks out the latest code.
6. Docker image is rebuilt.
7. The old container is stopped and removed.
8. A new container is started from the fresh image.
9. The updated website is live — automatically.

---

## 🧯 Troubleshooting

| Issue | Likely Cause | Fix |
|---|---|---|
| Webhook not triggering builds | Jenkins server not reachable from GitHub | Expose Jenkins via a public URL/tunnel (e.g. ngrok) or check firewall rules |
| `docker: permission denied` in Jenkins logs | Jenkins user lacks Docker access | Add the `jenkins` user to the `docker` group: `sudo usermod -aG docker jenkins` |
| Port 8080 already in use | Jenkins and the container both default to 8080 | Run the container on a different host port, e.g. `-p 8081:80` |
| Old container still running after deploy | `docker stop`/`rm` stage failed silently | Check `|| true` isn't masking a real error; inspect with `docker ps -a` |

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

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 👨‍💻 Author

**Sumit Ravidas**
Aspiring DevOps Engineer

[![GitHub](https://img.shields.io/badge/GitHub-<your--username>-181717?logo=github)](https://github.com/<your-username>)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?logo=linkedin)](https://linkedin.com/in/<your-linkedin>)

---

⭐ **If you found this project helpful, consider giving it a star on GitHub — it helps a lot!**
