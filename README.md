# Jenkins CI/CD Pipeline Assignment

## Author
**Name:** Aswath Parves  
**Email:** aswathparves@gmail.com  

---

## Overview
This assignment demonstrates a complete **Jenkins CI/CD Pipeline** for a Node.js application, including:

- Cloning the Git repository  
- Building and testing inside a Docker container  
- Creating and running a Docker image  
- Sending **email notifications** for build success or failure  

The pipeline is designed to be **reproducible, automated, and modular**.

---

## Prerequisites
- Jenkins installed on host machine  
- Docker installed and running on Jenkins host  
- Git installed  
- Node.js app repository (GitHub: `https://github.com/aswathparves/1-CICD-Jenkins`)  
- Email account for notifications (Gmail used in this assignment)  
- Required Jenkins plugins:
  - Pipeline
  - Email Extension Plugin (for `mail` step)

---

## Pipeline Features

### **Environment Variables**
- `IMAGE_NAME` – Name of the Docker image (`my-node-app`)  
- `CONTAINER_NAME` – Name of the running container (`my-node-app`)  
- `APP_DIR` – Workspace path inside Jenkins container (`/var/jenkins_home/workspace/assessment-1`)  

### **Stages**
1. **Prepare Workspace**
   - Clones the Git repository into the workspace  
   - Skips cloning if repo already exists  
2. **Build & Test in Docker**
   - Uses Node.js Docker image (`node:18`)  
   - Mounts Jenkins workspace and Docker socket  
   - Installs dependencies with `npm install`  
   - Runs tests with `npm test`  
   - Builds Docker image  
3. **Run Docker Container**
   - Stops any existing container with the same name  
   - Runs the newly built Docker container on port `3000`  

### **Post-Build Notifications**
- Sends email to `aswathparves@gmail.com`:
  - On **success**: Build succeeded notification  
  - On **failure**: Build failed notification  

---

## Jenkins Pipeline Code

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-node-app"
        CONTAINER_NAME = "my-node-app"
        APP_DIR = "/var/jenkins_home/workspace/assessment-1"
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                echo "Preparing workspace..."
                sh "git clone https://github.com/aswathparves/1-CICD-Jenkins ${APP_DIR} || echo 'Repo already exists'"
            }
       }
//You can check the full pipeline in Jenkinsfile!
```
## Configure Jenkins SMTP Email

- **SMTP server:** `smtp.gmail.com`  
- **Port:** `465`  
- **SSL:** ✔  
- **Username:** `aswathparves@gmail.com`  
- **Password:** Gmail App Password  
---
## Create Pipeline Job

1. Go to **Jenkins → New Item → Pipeline**  
2. Paste the pipeline code above  
---
## Test Pipeline

1. Run the job  
2. Ensure Docker builds and container runs  
3. Check email for notifications  

**Notes:**  
- `${currentBuild.fullDisplayName}` and `${currentBuild.currentResult}` are built-in Jenkins variables used in email notifications  
- Docker socket (`/var/run/docker.sock`) is mounted to allow Jenkins container to build and run Docker containers on host  
- Workspace cloning skips if repository already exists  
---
## Outcome

- Git repo cloned  
- Node.js dependencies installed and tests run inside Docker  
- Docker image built and container deployed  
- Email notifications sent on success or failure  

---

## References

- [Jenkins Pipeline Documentation](https://www.jenkins.io/doc/book/pipeline/)  
- [Jenkins Email Extension Plugin](https://plugins.jenkins.io/email-ext/)
- [WSL](https://learn.microsoft.com/en-us/windows/wsl/install)
- [Installing Docker on WSL2](https://docs.docker.com/desktop/features/wsl/)
