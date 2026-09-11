# AI Future — DevOps CI/CD Project

A modern static website about **Artificial Intelligence and its impact on different industries**, deployed automatically to an **AWS EC2 instance using GitHub Actions, SSH, and Nginx**.

The project demonstrates a simple real-world DevOps CI/CD workflow where every push to the `main` branch automatically deploys the latest website code to an EC2 server.

---

## 🚀 Project Overview

This project contains a responsive static website built using:

* HTML5
* CSS3
* Nginx
* AWS EC2
* GitHub
* GitHub Actions
* SSH

The website explains how AI is transforming industries such as:

* Healthcare
* Finance
* Manufacturing
* Technology
* Retail
* Transportation

The main focus of this project is not only the website but also the **automated deployment process using GitHub Actions**.

---

## 🏗️ Architecture

```text
                    Developer
                        |
                        | git push
                        ↓
                GitHub Repository
                        |
                        | Push to main
                        ↓
                GitHub Actions
                        |
                        | SSH
                        ↓
                 AWS EC2 Instance
                        |
                        | git pull
                        ↓
          /home/ubuntu/github-actions-static-site
                        |
                        | Copy files
                        ↓
                  /var/www/html
                        |
                        ↓
                      Nginx
                        |
                        ↓
                  Web Browser
```

---

## 📁 Project Structure

```text
github-actions-static-site/
│
├── .github/
│   └── workflows/
│       └── deployment.yaml
│
├── index.html
├── style.css
└── README.md
```

### Files

| File              | Description                   |
| ----------------- | ----------------------------- |
| `index.html`      | Main website HTML             |
| `style.css`       | Website styling               |
| `deployment.yaml` | GitHub Actions CI/CD workflow |
| `README.md`       | Project documentation         |

---

## ⚙️ Technologies Used

### Frontend

* HTML5
* CSS3

### Cloud

* AWS EC2

### Web Server

* Nginx

### CI/CD

* GitHub Actions
* SSH

### Version Control

* Git
* GitHub

---

# 🔄 CI/CD Workflow

The deployment is triggered whenever code is pushed to the `main` branch.

```text
Developer pushes code
        ↓
GitHub receives push
        ↓
GitHub Actions workflow starts
        ↓
GitHub Actions connects to EC2 using SSH
        ↓
EC2 pulls latest code using git pull
        ↓
Website files are copied to Nginx
        ↓
Nginx is reloaded
        ↓
Latest website is available
```

---

# 🔐 GitHub Actions Secrets

The workflow uses GitHub Secrets to securely connect to the EC2 instance.

The following secrets are configured in:

**Repository → Settings → Secrets and variables → Actions**

```text
EC2_HOST
EC2_USER
EC2_SSH_KEY
```

### EC2_HOST

The public IP address or hostname of the EC2 instance.

### EC2_USER

The SSH user used to connect to the EC2 instance.

For this project:

```text
ubuntu
```

### EC2_SSH_KEY

The private SSH key used by GitHub Actions to authenticate with the EC2 instance.

> The private SSH key should never be committed to the GitHub repository.

---

# 📝 GitHub Actions Workflow

The deployment workflow is stored at:

```text
.github/workflows/deployment.yaml
```

The workflow is triggered when code is pushed to `main`.

```yaml
name: Deploy AI Website

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v6

      - name: Deploy Website to EC2
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}

          script: |
            cd /home/ubuntu/github-actions-static-site

            git pull origin main

            sudo cp index.html /var/www/html/index.html
            sudo cp style.css /var/www/html/style.css

            sudo systemctl reload nginx
```

---

# 🔍 Deployment Steps Explained

### 1. Trigger

```yaml
on:
  push:
    branches:
      - main
```

The workflow runs whenever code is pushed to the `main` branch.

---

### 2. GitHub Actions Runner

```yaml
runs-on: ubuntu-latest
```

GitHub provides an Ubuntu runner to execute the workflow.

---

### 3. Checkout Repository

```yaml
uses: actions/checkout@v6
```

This checks out the repository code into the GitHub Actions runner.

---

### 4. SSH Connection

```yaml
uses: appleboy/ssh-action@v1
```

The workflow connects to the EC2 instance using SSH.

The connection uses:

```text
EC2_HOST
EC2_USER
EC2_SSH_KEY
```

stored as GitHub Secrets.

---

### 5. Navigate to Application Directory

```bash
cd /home/ubuntu/github-actions-static-site
```

The EC2 instance contains a clone of the GitHub repository.

---

### 6. Pull Latest Code

```bash
git pull origin main
```

This downloads the latest changes from the GitHub `main` branch.

---

### 7. Deploy Website Files

```bash
sudo cp index.html /var/www/html/index.html
sudo cp style.css /var/www/html/style.css
```

The latest website files are copied into the Nginx web root.

Nginx serves files from:

```text
/var/www/html
```

---

### 8. Reload Nginx

```bash
sudo systemctl reload nginx
```

Nginx is reloaded so that the updated website is served.

---

# 🌐 Nginx

Nginx is used as the web server for this project.

The website files are deployed to:

```text
/var/www/html/
```

Expected files:

```text
/var/www/html/
├── index.html
└── style.css
```

You can verify the files using:

```bash
ls -la /var/www/html
```

---

# ☁️ AWS EC2 Setup

The application runs on an Ubuntu-based AWS EC2 instance.

Basic setup includes:

### Update packages

```bash
sudo apt update
```

### Install Nginx

```bash
sudo apt install nginx -y
```

### Start Nginx

```bash
sudo systemctl start nginx
```

### Enable Nginx at boot

```bash
sudo systemctl enable nginx
```

### Check Nginx status

```bash
sudo systemctl status nginx
```

---

# 🔥 Security Group

The EC2 Security Group should allow HTTP traffic.

Example:

```text
Type: HTTP
Protocol: TCP
Port: 80
Source: 0.0.0.0/0
```

SSH access is also required for administration and GitHub Actions deployment.

```text
Type: SSH
Protocol: TCP
Port: 22
```

For production environments, SSH access should be restricted rather than unnecessarily exposed to the entire internet.

---

# 🧪 Testing Deployment

After making changes to the website:

```bash
git add .
git commit -m "Update website"
git push origin main
```

GitHub Actions automatically starts the deployment workflow.

You can monitor the deployment from:

```text
GitHub → Actions → Deploy AI Website
```

After a successful deployment, verify the website through the EC2 public IP or configured domain.

---

# 🔎 Useful Troubleshooting Commands

### Check current user

```bash
whoami
```

### Check repository

```bash
cd /home/ubuntu/github-actions-static-site
ls -la
```

### Check Git status

```bash
git status
```

### Pull latest changes manually

```bash
git pull origin main
```

### Check Nginx files

```bash
ls -la /var/www/html
```

### Check Nginx status

```bash
sudo systemctl status nginx
```

### Check Nginx configuration

```bash
sudo nginx -t
```

### Reload Nginx

```bash
sudo systemctl reload nginx
```

### Check Nginx logs

```bash
sudo tail -f /var/log/nginx/access.log
```

```bash
sudo tail -f /var/log/nginx/error.log
```

---

# 🎯 Project Objective

The objective of this project is to demonstrate how a DevOps engineer can automate deployment of a static website using:

```text
Git
 ↓
GitHub
 ↓
GitHub Actions
 ↓
SSH
 ↓
AWS EC2
 ↓
Nginx
 ↓
Website
```

This removes the need to manually copy website files to the server after every change.

---

# 💡 DevOps Concepts Demonstrated

* Git version control
* GitHub repository management
* GitHub Actions
* CI/CD automation
* SSH-based deployment
* AWS EC2
* Linux administration
* Nginx web server
* Git pull based deployment
* Linux file permissions
* Service management using `systemctl`
* Basic deployment troubleshooting
* Secure handling of credentials using GitHub Secrets

---

# 👩‍💻 Author

**Swati**

DevOps / Cloud Engineer

This project was created as a practical DevOps CI/CD project to demonstrate automated deployment using GitHub Actions and AWS EC2.
