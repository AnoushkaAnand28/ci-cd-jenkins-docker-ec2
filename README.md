# ci-cd-jenkins-docker-ec2

# CI/CD Pipeline Project with Jenkins, Docker, and AWS EC2

## 🛠 Stack
- Jenkins
- GitHub
- Docker & DockerHub
- AWS EC2
- Shell Scripting

## 🔁 Pipeline Flow

1. Code pushed to GitHub
2. Jenkins triggers on change
3. Jenkins builds Docker image
4. Jenkins pushes image to DockerHub
5. Jenkins SSHs into EC2 and deploys latest image

## 📂 Folder Structure
. ├── Jenkinsfile ├── Dockerfile ├── app/ (your app code) └── README.md

markdown
Copy
Edit

## 📦 Docker Image
- Hosted on DockerHub: `anoushkaanand28/ci-cd-demo`

## 🧠 Author
- Anoushka Anand