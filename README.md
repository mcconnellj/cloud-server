# Cloud Server  

[![License](https://img.shields.io/badge/license-MIT-blue)](https://github.com/mcconnellj/cloud-server/blob/main/LICENSE)  
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/mcconnellj/cloud-server/pulls)  
[![Issues](https://img.shields.io/github/issues/mcconnellj/cloud-server)](https://github.com/mcconnellj/cloud-server/issues)  
[![Security Policy](https://img.shields.io/badge/security-policy-red)](https://github.com/mcconnellj/cloud-server/security/policy)  

---

## 🚀 Tech Stack  

[![Google Cloud](https://img.shields.io/badge/Google_Cloud-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white)](https://cloud.google.com/compute/docs/instances/creating-instance-with-custom-machine-type)  
[![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/engine/install/ubuntu/)  
[![Ansible](https://img.shields.io/badge/Ansible-000000?style=for-the-badge&logo=ansible&logoColor=white)](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#pipx-install)  


---

## Self-Hosted Cloud Server on Google Cloud  

### Please Contribute

We welcome **all contributions**—whether it's **bug fixes, new features, or documentation improvements**.  

- **Issues**: Found a bug? [Open an issue](https://github.com/mcconnellj/cloud-server/issues).  
- **Pull Requests**: Fixes or enhancements? Submit a **PR**—we’ll review and merge!  
- **Feature Requests**: Have an idea? Start a discussion in [GitHub Discussions](#).
- **Security & Networking**: This repo needs improvements in this space.

### Technology 

### What You Get

#### **Always Free Cloud Machine For App Deployment**

This repo aims to run a google cloud **free-tier e2-micro** instance with an **external IP** for secure app hosting and remote development. The apps allow you to manage: secrets, passwords, cards, financial data.

The server is **managed using Docker Compose** however you will not run docker commands because the server deployment is automaticaly **installed using Ansible**. VS Code Server runs on the host allowing it to run commands, while the reamining apps run inside docker containers.

## What You Get  

### Core Services (Running in Docker)  
- **`traefik.yourdomain.com`** – Reverse proxy handling traffic routing & SSL certificates.  
- **`firefly.yourdomain.com`** – Personal finance manager with bank data import support.  
- **`vaultwarden.yourdomain.com`** – Secure password & secret manager with browser extensions.  

### Development Environment (VS Code With Host Access)  
- **`code.yourdomain.com`** – Web-based VS Code with Git support for cloud development.    

## Common Use Cases  

### Remote Development & Cloud-Based Coding  
- Work from anywhere with a full-featured **VS Code Web IDE**.  
- Develop, test, and deploy applications directly from the cloud.  
- Push & pull from Git, ideal for **CI/CD pipelines** like **ArgoCD** or **GitHub Actions**.  

### Secure & Centralized Credential Management  
- Store and manage **passwords, API keys, and secrets** using **Vaultwarden**.  
- Access sensitive information securely from any device with browser extensions.  

### Personal & Business Finance Management  
- Track **expenses, budgets, and transactions** with **Firefly III**.  
- Import **bank data** to automate financial tracking and reporting.  

### Self-Hosted, Always Available Services  
- Run essential apps on an always-free cloud instance.  
- Automate SSL, domain routing, and security using **Traefik**.  
- Access your services from anywhere with a custom **`*.yourdomain.com`** setup.  

## Screenshots / Demo  

The following applications will be deployed:  

- **[Traefik](https://doc.traefik.io/traefik/)** (Reverse Proxy & Dashboard)  
- **[Visual Studio Code Web](https://vscode.dev/)** (Browser-based cloud IDE)  
- **[Firefly III](https://firefly-iii.org/)** (Personal Finance Manager)  
- **[Vaultwarden](https://github.com/dani-garcia/vaultwarden)** (Bitwarden-compatible password manager)  

Each service is accessible via **your custom subdomains**, secured with **Let's Encrypt SSL**.  

---

# 1. Project Overview

### Requirements  
- A **Google Cloud account** with Compute Engine enabled  
- A **registered domain name**  
- Basic knowledge of **command-line usage**  

### Notes  
- **Edit `.env` and `.db.env`** before deployment.  
- **Hash passwords** where necessary before adding them.  
- After deployment, **test your subdomains** to ensure everything is working.  

---

# 2. Installation & Usage  

## Step 1: Create a Free-Tier e2 Micro Instance  

Google Cloud provides **one free e2-micro instance per month** in specific US regions:  
- **Regions:** Oregon (`us-west1`), Iowa (`us-central1`), South Carolina (`us-east1`)  
- **Storage:** 30 GB standard persistent disk  

#### Create Your Instance:  
1. Go to **Google Cloud Console** → **Compute Engine** → **VM Instances**  
2. Click **Create Instance**  
3. Set the **Machine type** to **e2-micro**  
4. Choose a **free-tier region** (`us-west1`, `us-central1`, or `us-east1`)  
5. Select **Ubuntu/Debian** as the operating system  
6. Enable **Allow HTTP & HTTPS traffic**  
7. Click **Create**  

Once created, **use the web-based SSH** in the Google Cloud Console to connect to your instance.  

---

## Step 2: Configure DNS  

Set **A records** in your domain registrar to point to your server’s **public IP address**:  

| Name (Subdomain) | Type | TTL  | Value (Your Server's IP) |
|------------------|------|------|--------------------------|
| `@` (root domain) | A    | 300  | `xxx.xxx.xxx.xxx`        |
| `code`           | A    | 300  | `xxx.xxx.xxx.xxx`        |
| `firefly`        | A    | 300  | `xxx.xxx.xxx.xxx`        |
| `traefik`        | A    | 300  | `xxx.xxx.xxx.xxx`        |
| `vaultwarden`    | A    | 300  | `xxx.xxx.xxx.xxx`        |

**Note:** Replace `xxx.xxx.xxx.xxx` with your actual **public IP address**.  

---

## Step 3: Install Dependencies  

Run the following commands on your server to install required tools:  

```bash
sudo apt-get update  
sudo apt install -y git pipx ca-certificates curl  
sudo pipx install --include-deps ansible  
sudo ansible-galaxy collection install community.docker  
sudo pipx ensurepath  
source ~/.bashrc  
```

---

## Step 4: Clone the Repository  

```bash
git clone https://github.com/mcconnellj/cloud-server  
cd cloud-server  
```

---

## Step 5: Configure Environment Files  

Create and edit **`.env` and `.db.env`** before running Ansible:  

```bash
cat <<EOF > .env-template
DOMAIN=
CODE_SUBDOMAIN=code
FIREFLY_SUBDOMAIN=firefly
TRAEFIK_SUBDOMAIN=traefik
VAULTWARDEN_SUBDOMAIN=vaultwarden
TRAEFIK_USER=admin
TRAEFIK_PASSWORD=""
TRAEFIK_PASSWORD_HASH=""
EMAIL=
EOF
```

```bash
cat <<EOF > .db.env-template
DOMAIN=
EMAIL=
EOF
```

Rename the files after adding your details:  

```bash
mv .env-template .env  
mv .db.env-template .db.env  
```

---

## Step 6: Run Ansible Playbook  

Execute the playbook to configure your cloud server:  

```bash
ansible-playbook ./playbooks/site.yml --connection=local  
```

---

## Acknowledgments  

Special thanks to:  
- **S Zarichney** for making this possible
- **Bear** for making this possible  
- **Google Cloud Free Tier** for hosting

---

## Happy Hosting!  
