## Task Manager – 3-Tier Application

A simple **3-Tier Task Manager Application** deployed on AWS EC2, with a frontend, backend API, and database. This project demonstrates application deployment and DevOps practices using Git, GitHub, Ubuntu, Node.js, Angular, MongoDB, Docker, Docker Compose, and Jenkins.

---

## 📌 Table of Contents

1. [Project Overview](#-project-overview)
2. [3-Tier Architecture](#-3-tier-architecture)
3. [Technology Stack](#-technology-stack)
4. [Prerequisites](#-prerequisites)
5. [Project Structure](#-project-structure)
6. [Step 1: Push Code to GitHub](#-step-1-push-code-to-github)
7. [Step 2: Launch AWS EC2 Instance](#-step-2-launch-aws-ec2-instance)
8. [Step 3: Install Dependencies](#-step-3-install-dependencies-on-ubuntu)
9. [Step 4: Clone the Repository](#-step-4-clone-the-repository)
10. [Step 5: Configure and Run the Backend](#-step-5-configure-and-run-the-backend)
11. [Step 6: Configure and Run the Frontend](#-step-6-configure-and-run-the-frontend)
12. [Step 7: Access the Application](#-step-7-access-the-application)
13. [Docker Installation](#-docker-installation)
14. [Docker Deployment](#-docker-deployment)
15. [Disk Space Troubleshooting](#-disk-space-troubleshooting)
16. [Jenkins Installation and Setup](#-jenkins-installation-and-setup)
17. [Restart and Redeployment](#-restart-and-redeployment)
18. [Troubleshooting](#-troubleshooting)
19. [Security Best Practices](#-security-best-practices)
20. [Future Enhancements](#-future-enhancements)

---

## 🚀 Project Overview

The Task Manager application follows a three-tier architecture to separate the user interface, application logic, and data storage.

The application is designed to demonstrate:

- Frontend development and deployment using Angular.
- Backend API development using Node.js and Express.
- Database integration using MongoDB.
- Version control and source code management using Git and GitHub.
- Application deployment on AWS EC2.
- Containerization using Docker.
- Multi-container orchestration using Docker Compose.
- CI/CD automation using Jenkins.

**GitHub Repository:**  
https://github.com/SanketParbat1211/task-manager-3tier

**Application URL:**

```text
http://<EC2-PUBLIC-IP>:4200
```

Replace `<EC2-PUBLIC-IP>` with the public IPv4 address of your EC2 instance.

## 🏗️ 3-Tier Architecture

The application consists of three main tiers.

### Architecture Diagram

```text
                  USERS
                    |
                    v
          +--------------------+
          |   Angular Frontend |
          |      Port 4200     |
          +--------------------+
                    |
                    | HTTP REST API
                    v
          +--------------------+
          | Node.js / Express  |
          |    Backend API     |
          |      Port 5000     |
          +--------------------+
                    |
                    | MongoDB Driver
                    v
          +--------------------+
          |      MongoDB       |
          |      Port 27017    |
          +--------------------+
```

### Tier 1: Presentation Layer

**Technology:** Angular

Responsibilities:

- Displays the Task Manager user interface.
- Accepts user input.
- Sends HTTP requests to the backend API.
- Displays application data and API responses.

Development port: `4200`

### Tier 2: Application Layer

**Technology:** Node.js and Express

Responsibilities:

- Processes frontend API requests.
- Implements application and business logic.
- Validates incoming requests.
- Communicates with MongoDB.
- Returns responses to the frontend.

Development port: `5000`

### Tier 3: Data Layer

**Technology:** MongoDB

Responsibilities:

- Stores task-related data.
- Handles database queries.
- Persists application records.

Default MongoDB port: `27017`

**Important:** The frontend should communicate with the backend API, not directly with MongoDB. MongoDB should not be publicly exposed to the internet.

---

## 🛠️ Technology Stack

| Category | Technology |
|---|---|
| Frontend | Angular |
| Backend | Node.js, Express |
| Database | MongoDB |
| Version Control | Git |
| Source Code Management | GitHub |
| Cloud Platform | AWS |
| Compute | Amazon EC2 |
| Operating System | Ubuntu Server 22.04 LTS |
| Containerization | Docker |
| Container Orchestration | Docker Compose |
| CI/CD | Jenkins |
| Runtime | Node.js and npm |
| Network Protocol | HTTP/REST API |

---

## 📋 Prerequisites

Before starting, ensure that you have:

- An AWS account.
- A GitHub account.
- Git installed on your local machine.
- Visual Studio Code.
- An AWS EC2 SSH private key (`.pem` file).
- Basic knowledge of Linux commands.
- Basic knowledge of Git and GitHub.
- Basic knowledge of Docker and Docker Compose.

---

## 📁 Project Structure

The repository is expected to contain a frontend and backend directory.

```text
task-manager-3tier/
│
├── frontend/
│   ├── src/
│   │   └── environments/
│   │       └── environment.ts
│   ├── package.json
│   └── angular.json
│
├── backend/
│   ├── package.json
│   ├── .env.example
│   └── ...
│
├── Dockerfile                 # If included in the repository
├── docker-compose.yml         # If included in the repository
├── .gitignore
└── README.md
```

The exact structure may differ depending on the files present in the repository. Check the actual repository before running Docker commands.

---

## 📤 Step 1: Push Code to GitHub

### 1. Create a GitHub repository

Create an empty repository named:

`task-manager-3tier`

Repository URL:

https://github.com/SanketParbat1211/task-manager-3tier

### 2. Open the local project

Open Visual Studio Code.

Select:

`File → Open Folder → 3-tier-app`

Open the VS Code terminal.

### 3. Initialize Git and push the code

Run the following commands one by one:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/SanketParbat1211/task-manager-3tier.git
git push -u origin main
```

### 4. Verify the repository

Open the GitHub repository and confirm that your project files are visible.

### GitHub Authentication

If Git asks for credentials over HTTPS:

- **Username:** Your GitHub username.
- **Password:** Use a GitHub Personal Access Token (PAT), not your GitHub account password.

Create a token from:

`GitHub → Settings → Developer settings → Personal access tokens`

For a classic token, select the necessary `repo` scope for a private repository. Use the minimum permissions required.

Never commit your PAT or other credentials to the repository.

---

## ☁️ Step 2: Launch AWS EC2 Instance

### 1. Create the instance

Open:

`AWS Console → EC2 → Instances → Launch instances`

Configure the instance as follows:

| Setting | Value |
|---|---|
| Name | `task-manager-server` |
| AMI | Ubuntu Server 22.04 LTS |
| Instance type | `t2.micro` or `t2.medium` |
| Key pair | Select or create a `.pem` key |
| Storage | Start with suitable EBS storage; increase if required |

A micro instance may have insufficient memory for the application, database, Docker builds, and Jenkins running together.

### 2. Configure the security group

For initial testing, configure the following inbound rules:

| Type | Port | Source |
|---|---:|---|
| SSH | 22 | My IP |
| Custom TCP | 4200 | Your IP, preferably |
| Custom TCP | 5000 | Your IP, preferably |
| Custom TCP | 8080 | Your IP, preferably, for Jenkins |

Restrict access to trusted IP addresses whenever possible. Do not expose MongoDB port `27017` publicly.

### 3. Connect to the EC2 instance

From Windows PowerShell, run:

```powershell
ssh -i "C:\path\to\your-key.pem" ubuntu@<EC2-PUBLIC-IP>
```

Example format:

```powershell
ssh -i "C:\Users\YOUR_USER\OneDrive\aws-keys\awskey.pem" ubuntu@<EC2-PUBLIC-IP>
```

Replace the path and public IP with your actual values.

---

## 🐧 Step 3: Install Dependencies on Ubuntu

Update the system package index:

```bash
sudo apt update
```

Install Git, Node.js, npm, and supporting packages:

```bash
sudo apt install -y nodejs npm git curl wget ca-certificates
```

Verify the installations:

```bash
node -v
npm -v
git --version
```

**Version note:** Ubuntu's default Node.js package may not meet the application's required version. Check the `engines` field in `package.json` and install a compatible Node.js LTS release if needed.

### Install MongoDB

Follow the official MongoDB Community Edition installation instructions for Ubuntu 22.04:

https://www.mongodb.com/docs/v7.0/tutorial/install-mongodb-on-ubuntu/

After configuring the official MongoDB repository and installing the packages, start the database service:

```bash
sudo systemctl enable --now mongod
```

Check the service status:

```bash
sudo systemctl status mongod
```

Check the installed version:

```bash
mongod --version
```

### Install Angular CLI

```bash
sudo npm install -g @angular/cli
```

Verify the installation:

```bash
ng version
```

---

## 📥 Step 4: Clone the Repository

Clone the project from GitHub:

```bash
git clone https://github.com/SanketParbat1211/task-manager-3tier.git
```

Navigate to the repository:

```bash
cd ~/task-manager-3tier
```

List the project files:

```bash
ls -la
```

Check the frontend and backend directories:

```bash
ls frontend
ls backend
```

---

## ⚙️ Step 5: Configure and Run the Backend

Open the first SSH terminal.

Navigate to the backend:

```bash
cd ~/task-manager-3tier/backend
```

Install the Node.js dependencies:

```bash
npm install
```

Create the environment configuration file:

```bash
cp .env.example .env
nano .env
```

Configure the variables required by your backend. For example:

```dotenv
PORT=5000
MONGODB_URI=mongodb://127.0.0.1:27017/taskmanager
```

Use these example values only if they match the application's configuration. Keep any existing required environment variables and follow the actual backend source code.

Save the file and exit the editor.

Start the backend in development mode:

```bash
npm run dev
```

Expected output may resemble:

```text
MongoDB Connected: ...
Server running on port 5000
```

The exact messages depend on the application's implementation.

### Verify the backend

From another terminal, test the API server:

```bash
curl http://localhost:5000/
```

If the application exposes a health endpoint, test that endpoint instead.

A response of `Cannot GET /` does not necessarily mean the server is down; the root route may not be defined.

---

## 🖥️ Step 6: Configure and Run the Frontend

Open a second terminal and connect to the EC2 instance if necessary.

Navigate to the frontend:

```bash
cd ~/task-manager-3tier/frontend
```

Install dependencies:

```bash
npm install
```

### Configure the Backend API URL

Open the Angular environment file:

```bash
nano src/environments/environment.ts
```

The development configuration may look like:

```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:5000/api'
};
```

When Angular runs in a user's browser, `localhost` refers to the user's own computer, not the EC2 server. Therefore, the frontend cannot reach the EC2 backend using this URL.

For initial testing, change it to:

```typescript
export const environment = {
  production: false,
  apiUrl: 'http://<EC2-PUBLIC-IP>:5000/api'
};
```

Replace `<EC2-PUBLIC-IP>` with the actual public IPv4 address of your instance.

**Important:** Use the correct API base path from your backend routes. If the API uses a different path than `/api`, configure that path instead.

Save and exit Nano:

- `Ctrl + O` to save.
- Press `Enter` to confirm.
- `Ctrl + X` to exit.

### Start the frontend

```bash
npm start -- --host 0.0.0.0
```

If the start script does not support this command, use the appropriate Angular CLI command defined by the project.

For example:

```bash
ng serve --host 0.0.0.0 --port 4200
```

Keep both the frontend and backend running during this development test.

---

## 🌐 Step 7: Access the Application

Open a browser and navigate to:

```text
http://<EC2-PUBLIC-IP>:4200
```

Verify that:

- The Angular frontend loads successfully.
- API requests reach the backend.
- MongoDB connects successfully.
- Task operations work as implemented in the application.

If the frontend loads but API calls fail, inspect the browser's developer console and Network tab for connection errors, CORS errors, and incorrect API URLs.

**Note:** If the EC2 instance is stopped and started again, its public IPv4 address may change unless you use an Elastic IP. Update any affected configuration and rebuild or restart the application as required.

---

# 🐳 Docker Installation

Docker allows the frontend and backend to run in containers, making deployment more consistent.

## Step 1: Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

## Step 2: Install required packages

```bash
sudo apt install -y ca-certificates curl
```

## Step 3: Create the Docker keyring directory

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

## Step 4: Download the Docker signing key

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc
```

## Step 5: Add the Docker repository

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Step 6: Update the package index

```bash
sudo apt update
```

## Step 7: Install Docker Engine and Compose

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Step 8: Verify Docker installation

```bash
docker --version
docker compose version
```

## Step 9: Start and enable Docker

```bash
sudo systemctl enable --now docker
```

Check the service:

```bash
sudo systemctl status docker
```

Press `q` to exit the status screen.

## Step 10: Configure Docker permissions

To run Docker commands without `sudo`:

```bash
sudo usermod -aG docker "$USER"
newgrp docker
```

Membership in the Docker group grants powerful privileges equivalent to root access. Use it only for trusted users.

## Step 11: Test Docker

```bash
docker run hello-world
```

If the test succeeds, Docker can pull and run a test container.

---

# 🚀 Docker Deployment

## Step 1: Check the project configuration

Navigate to the project directory:

```bash
cd ~/task-manager-3tier
```

List the files:

```bash
ls -la
```

Check whether a Docker Compose file exists:

```bash
ls -l docker-compose.yml compose.yaml
```

If no Compose file exists, create one that matches the actual frontend and backend Dockerfiles and networking configuration before running the deployment commands.

## Step 2: Configure application settings

Ensure that:

- The backend listens on an appropriate interface inside its container.
- The backend connects to MongoDB using a valid connection string.
- The frontend uses an API URL reachable from the user's browser.
- The backend allows requests from the intended frontend origin.
- Required environment variables are configured without hardcoding secrets.

**Important networking distinction:** Inside a Docker container, `localhost` refers to that container. A backend container should generally reach a MongoDB container through its Compose service name. However, an Angular application running in a browser needs a browser-accessible API URL or a reverse-proxy path.

If you modify Angular environment files that are compiled into the frontend bundle, rebuild the frontend image after changing the API URL.

## Step 3: Build and start containers

If the repository contains a valid Compose configuration:

```bash
docker compose up --build -d
```

Verify running containers:

```bash
docker ps
```

View all containers, including stopped ones:

```bash
docker ps -a
```

Check application logs:

```bash
docker compose logs --tail=100
```

Follow logs continuously:

```bash
docker compose logs -f
```

Press `Ctrl + C` to stop following the logs.

## Step 4: Verify the application

Open the browser:

```text
http://<EC2-PUBLIC-IP>:4200
```

Confirm that the frontend, backend, and database are communicating correctly.

---

# 💾 Disk Space Troubleshooting

Docker builds can consume considerable disk space, especially on smaller EC2 instances.

## Step 1: Check available disk space

```bash
df -h
```

Check memory:

```bash
free -h
```

Check Docker disk usage:

```bash
docker system df
```

## Step 2: Identify large directories

Check the project directory:

```bash
du -sh ~/task-manager-3tier/* 2>/dev/null
```

Check Docker storage:

```bash
sudo du -sh /var/lib/docker 2>/dev/null
```

Check containerd storage:

```bash
sudo du -sh /var/lib/containerd 2>/dev/null
```

Find the largest directories on the system:

```bash
sudo du -xhd1 / 2>/dev/null | sort -rh | head -15
```

## Step 3: Clean up unnecessary files

If the project is no longer using local Node.js dependencies:

```bash
rm -rf ~/task-manager-3tier/backend/node_modules
rm -rf ~/task-manager-3tier/frontend/node_modules
```

These directories can be recreated with `npm install` if needed.

Clean the package cache:

```bash
sudo apt clean
sudo apt autoremove -y
```

Review Docker resources before deleting them.

To remove unused Docker objects:

```bash
docker system prune
```

To remove all unused images as well:

```bash
docker system prune -a
```

**Warning:** Docker pruning can remove images and build cache needed for future deployments. Do not use volume deletion commands unless you understand their impact on persistent application data.

## Step 4: Increase the EC2 EBS volume

If storage remains insufficient:

1. Open the AWS EC2 console.
2. Navigate to **Elastic Block Store → Volumes**.
3. Select the EBS volume attached to the instance.
4. Choose **Modify volume**.
5. Increase the size, for example from 8 GB to 20 GB.
6. Wait until the modification is available or sufficiently progressed.
7. Identify the actual disk and partition on Ubuntu.

Inspect the storage layout:

```bash
lsblk
df -hT
```

For an Ubuntu EC2 instance whose root partition is `/dev/xvda1`, a typical resize sequence is:

```bash
sudo growpart /dev/xvda 1
sudo resize2fs /dev/xvda1
df -h
```

These commands are specific to an ext4 filesystem and the assumed partition layout. Confirm your actual device and filesystem before executing them. NVMe-based EC2 devices and other filesystems may require different commands.

## Step 5: Rebuild the application

Once sufficient disk space is available:

```bash
cd ~/task-manager-3tier
docker compose up --build -d
docker ps
```

Review logs if the application fails to start.

---

# 🔧 Jenkins Installation and Setup

Jenkins can be used to automate the build, test, and deployment process.

## Step 1: Install Java

```bash
sudo apt update
sudo apt install -y fontconfig openjdk-17-jre
```

Verify Java:

```bash
java -version
```

## Step 2: Add the official Jenkins repository

Follow the official installation instructions for the current Jenkins release:

https://www.jenkins.io/doc/book/installing/linux/

The following commands illustrate the repository setup for Ubuntu:

```bash
sudo mkdir -p /etc/apt/keyrings
```

Download the signing key:

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```

Add the repository:

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" \
  | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

If the key URL or repository changes, use the commands published in the official Jenkins documentation.

## Step 3: Install Jenkins

```bash
sudo apt update
sudo apt install -y jenkins
```

Enable and start the service:

```bash
sudo systemctl enable --now jenkins
```

Check its status:

```bash
sudo systemctl status jenkins
```

## Step 4: Retrieve the initial administrator password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Use the displayed password for the initial Jenkins setup wizard.

Do not store this password in GitHub or in the README.

## Step 5: Open the Jenkins dashboard

Navigate to:

```text
http://<EC2-PUBLIC-IP>:8080
```

Complete the setup wizard:

1. Enter the initial administrator password.
2. Install the recommended plugins.
3. Create an administrator account.
4. Configure the Jenkins URL.
5. Sign in to the dashboard.

Restrict port `8080` to your trusted IP address.

## Step 6: Basic Jenkins pipeline workflow

A typical CI/CD workflow for this application is:

```text
Developer
    |
    v
GitHub Repository
    |
    v
Jenkins Pipeline
    |
    v
Checkout Source Code
    |
    v
Install Dependencies
    |
    v
Run Tests
    |
    v
Build Docker Images
    |
    v
Deploy with Docker Compose
    |
    v
Verify Application Health
```

The actual pipeline requires a Jenkinsfile, suitable plugins, repository access, and the correct build and deployment configuration. These steps describe the workflow; they do not imply that a Jenkins pipeline is already configured in the repository.

---

# 🔄 Restart and Redeployment

## Step 1: Check the current public IP

After restarting an EC2 instance, check the current public IPv4 address in the AWS console.

Without an Elastic IP, the public IPv4 address may change after a stop/start cycle.

## Step 2: Connect to the instance

From Windows PowerShell:

```powershell
ssh -i "C:\path\to\your-key.pem" ubuntu@<EC2-PUBLIC-IP>
```

## Step 3: Navigate to the repository

```bash
cd ~/task-manager-3tier
```

## Step 4: Update the API URL if necessary

For the manual Angular development setup:

```bash
nano frontend/src/environments/environment.ts
```

Update the API URL to the correct public IP and backend API path.

If the frontend is served through Docker, changing a source file on disk does not necessarily update an already-built frontend container. Rebuild the frontend image and recreate the relevant container if the configuration is baked into the build.

For production, prefer a stable domain name and reverse proxy instead of hardcoding a changing EC2 public IP.

## Step 5: Start Docker Compose

```bash
docker compose up --build -d
```

Check running containers:

```bash
docker ps
```

Check logs:

```bash
docker compose logs --tail=100
```

## Step 6: Verify Jenkins

```bash
sudo systemctl status jenkins
```

If Jenkins is stopped:

```bash
sudo systemctl start jenkins
```

Enable it to start automatically after a reboot:

```bash
sudo systemctl enable jenkins
```

## Step 7: Open the application

```text
http://<EC2-PUBLIC-IP>:4200
```

Verify the frontend and backend API before considering the deployment successful.

---

# 🐛 Troubleshooting

| Issue | Possible Cause | Recommended Action |
|---|---|---|
| Git push fails | Authentication or remote URL issue | Verify the remote URL and use a supported authentication method |
| SSH connection fails | Incorrect key, permissions, IP, or security group | Check the `.pem` path, SSH rule, and instance IP |
| `node` or `npm` not found | Missing or incompatible installation | Install a compatible Node.js version and verify `node -v` |
| MongoDB connection refused | Service stopped or incorrect URI | Check `sudo systemctl status mongod` and the connection string |
| Backend does not start | Missing environment variables or dependencies | Review `.env`, `npm install`, and application logs |
| Frontend cannot call API | Incorrect API URL, CORS, or port | Check browser Network tools and backend logs |
| Website does not load | Container stopped or port blocked | Check `docker ps`, logs, and security group rules |
| Docker build fails | Insufficient disk or memory | Check `df -h`, `free -h`, and Docker disk usage |
| Docker Compose file not found | Missing or incorrectly named Compose file | Check the repository and create a matching Compose configuration |
| Jenkins page unavailable | Service stopped or port blocked | Check Jenkins status and inbound rules |
| Website fails after restart | Changed public IP or stopped containers | Verify the current IP, API configuration, and container status |

### Useful diagnostic commands

```bash
# Check disk space
df -h

# Check RAM usage
free -h

# Check running processes
ps aux

# Check listening ports
sudo ss -lntp

# Check Docker containers
docker ps -a

# Check Docker resource usage
docker system df

# Check Compose logs
docker compose logs --tail=100

# Check MongoDB
sudo systemctl status mongod

# Check Jenkins
sudo systemctl status jenkins
```

---

# 🔐 Security Best Practices

- Never commit `.env` files, GitHub PATs, SSH private keys, or Jenkins passwords.
- Add `.env` and other secret files to `.gitignore`.
- Keep MongoDB private and avoid opening port `27017` to the public internet.
- Restrict SSH, Jenkins, and development ports to trusted IP addresses.
- Use IAM roles and least-privilege permissions where applicable.
- Use HTTPS and a reverse proxy for production deployments.
- Avoid using Angular development mode as a production web server.
- Store persistent database data in properly managed storage or Docker volumes.
- Back up important data before performing cleanup or infrastructure changes.
- Regularly update Ubuntu, Docker, Jenkins, and application dependencies.
- Use an Elastic IP or domain name when a stable application endpoint is required.

---

# 🚀 Future Enhancements

The project can be extended with the following DevOps improvements:

- Create a Jenkinsfile for automated CI/CD.
- Push Docker images to Amazon Elastic Container Registry (ECR).
- Deploy the application to Amazon EKS.
- Provision AWS infrastructure using Terraform.
- Add automated testing and code quality checks using SonarQube.
- Integrate vulnerability scanning using Trivy.
- Configure Prometheus and Grafana for monitoring.
- Add HTTPS with a domain name and a TLS certificate.
- Configure automated backups and disaster recovery.
- Add centralized logging and application health checks.

---

## 👨‍💻 Author and Repository

**Project:** Task Manager – 3-Tier Application

**GitHub Repository:**  
https://github.com/SanketParbat1211/task-manager-3tier

**Primary Objective:** Learn and demonstrate a complete application deployment workflow using AWS, Linux, GitHub, Docker, Docker Compose, and Jenkins.

---

## 📄 License

Add a `LICENSE` file to the repository if you intend to distribute the project under an open-source license. Until a license is specified, do not assume that others have permission to reuse or redistribute the code.
