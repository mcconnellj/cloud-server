# Always Free Cloud Development Server with Secret and Finance Tracking

[![License](https://img.shields.io/badge/license-MIT-blue)](https://github.com/mcconnellj/cloud-server/blob/main/LICENSE) 
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/mcconnellj/cloud-server/pulls) 
[![Issues](https://img.shields.io/github/issues/mcconnellj/cloud-server)](https://github.com/mcconnellj/cloud-server/issues) 
[![Security Policy](https://img.shields.io/badge/security-policy-red)](https://github.com/mcconnellj/cloud-server/security/policy)  

---

## 📌 Table of Contents  

- [Always Free Cloud Development Server with Secret and Finance Tracking](#always-free-cloud-development-server-with-secret-and-finance-tracking)
  - [📌 Table of Contents](#-table-of-contents)
  - [🚀 What You Get](#-what-you-get)
    - [**Always Free Cloud Machine for App Deployment**](#always-free-cloud-machine-for-app-deployment)
    - [🛠️ Core Services (Running in Docker)](#️-core-services-running-in-docker)
    - [🖥️ Development Environment (VS Code with Host Access)](#️-development-environment-vs-code-with-host-access)
- [🛠️ Installation](#️-installation)
  - [Step 1: Create a Free-Tier e2 Micro Instance](#step-1-create-a-free-tier-e2-micro-instance)
      - [✅ Create Your Instance:](#-create-your-instance)
  - [Step 2: Configure DNS](#step-2-configure-dns)
  - [Step 3: Install Dependencies](#step-3-install-dependencies)
  - [Step 4: Clone the Repository](#step-4-clone-the-repository)
  - [Step 5: Configure Environment Files](#step-5-configure-environment-files)
  - [Step 6: Run Ansible Playbook](#step-6-run-ansible-playbook)
  - [📝 Issues \& Contributions](#-issues--contributions)
  - [🙏 Acknowledgments](#-acknowledgments)
  - [🎉 Happy Hosting!](#-happy-hosting)

---

## 🚀 What You Get  

### **Always Free Cloud Machine for App Deployment**  

This repo sets up a **Google Cloud free-tier e2-micro instance** with an **external IP** for secure app hosting and remote development.  

- The server is **managed using Docker Compose**.  
- Deployment is **automatically installed using Ansible**.  
- **VS Code Server** runs on the host, while other apps run in Docker containers.  

[![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/gettingstarted/)  
[![Ansible](https://img.shields.io/badge/Ansible-000000?style=for-the-badge&logo=ansible&logoColor=white)](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html)  

---

### 🛠️ Core Services (Running in Docker)  

- **`traefik.yourdomain.com`** – Reverse proxy handling traffic routing & SSL certificates.  
  - [Dashboard](https://doc.traefik.io/traefik/operations/dashboard/)  
  - [Quickstart via Docker](https://doc.traefik.io/traefik/getting-started/quick-start/)  

- **`firefly.yourdomain.com`** – Personal finance manager with bank data import support.  
  - [Demo](https://demo.firefly-iii.org/login)  
  - [Homepage](https://www.firefly-iii.org)  
  - [GitHub](https://github.com/firefly-iii/firefly-iii)  
  - [Bank imports](https://docs.firefly-iii.org/references/data-importer/third-party-tools/)  

- **`vaultwarden.yourdomain.com`** – Secure password & secret manager with browser extensions.  
  - [Homepage](https://www.vaultwarden.ca)  
  - [GitHub](https://github.com/dani-garcia/vaultwarden)  

---

### 🖥️ Development Environment (VS Code with Host Access)  

- **`code.yourdomain.com`** – Web-based VS Code with Git support for cloud development.  
  - [Code Server & Remote Extension](https://code.visualstudio.com/docs/remote/vscode-server)  
  - [GitHub](https://github.com/coder/code-server)  

---

# 🛠️ Installation  

## Step 1: Create a Free-Tier e2 Micro Instance  

Google Cloud provides **one free e2-micro instance per month** in specific US regions:  

- **Regions:** Oregon (`us-west1`), Iowa (`us-central1`), South Carolina (`us-east1`)  
- **Storage:** 30 GB standard persistent disk  

#### ✅ Create Your Instance:  

1. Go to **Google Cloud Console** → **Compute Engine** → **VM Instances**  
2. Click **Create Instance**  
3. Set **Machine type** to **e2-micro**  
4. Choose a **free-tier region** (`us-west1`, `us-central1`, or `us-east1`)  
5. Select **Ubuntu/Debian** as the OS  
6. Enable **Allow HTTP & HTTPS traffic**  
7. Click **Create**  

Once created, **use the web-based SSH** in Google Cloud Console to connect.  

[![Google Cloud](https://img.shields.io/badge/Google_Cloud-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white)](https://cloud.google.com/compute/docs/instances/creating-instance-with-custom-machine-type)  

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

Run the following commands on your server:  

\`\`\`bash
sudo apt-get update  
sudo apt install -y git pipx ca-certificates curl  
sudo pipx install --include-deps ansible  
sudo ansible-galaxy collection install community.docker  
sudo pipx ensurepath  
source ~/.bashrc  
\`\`\`  

---

## Step 4: Clone the Repository  

\`\`\`bash
git clone https://github.com/mcconnellj/cloud-server  
cd cloud-server  
\`\`\`  

---

## Step 5: Configure Environment Files  

\`\`\`bash
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
\`\`\`  

Rename the files:  

\`\`\`bash
mv .env-template .env  
mv .db.env-template .db.env  
\`\`\`  

---

## Step 6: Run Ansible Playbook  

\`\`\`bash
ansible-playbook ./playbooks/site.yml --connection=local  
\`\`\`  

---

## 📝 Issues & Contributions  

Contributions are welcome!  

- **Found a bug?** [Open an issue](https://github.com/mcconnellj/cloud-server/issues)  
- **Want to contribute?** Submit a **PR**  
- **Feature requests?** Start a discussion  

---

## 🙏 Acknowledgments  

Special thanks to:  
- **S Zarichney** for introducing me to CodePilot  
- **Bear** for providing a switch for my local server  
- **Google Cloud Free Tier** for free hosting  

---

## 🎉 Happy Hosting!  
