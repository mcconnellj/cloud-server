# 🌩️ Cloud Server  
This repository automates the setup of a cloud server using Ansible.  

## 📖 Overview  
This guide will walk you through setting up a cloud server, configuring DNS, installing dependencies, and deploying services using Ansible.  

### 🛠️ Requirements  
- A **Google Cloud account** with Compute Engine enabled  
- A **registered domain name**  
- Basic knowledge of **command-line usage**  

### ✅ Notes  
- **Edit `.env` and `.db.env`** before running Ansible.  
- **Hash passwords** where necessary before adding them.  
- After deployment, **test your subdomains** to ensure everything is working.  

---

# 🚀 Quick Start  

## 🖥️ Step 1: Create a Free-Tier e2 Micro Instance  

Google Cloud provides **one free e2-micro instance per month** in specific US regions: **Oregon (`us-west1`), Iowa (`us-central1`), or South Carolina (`us-east1`)**. The free tier also includes **30 GB of standard persistent disk storage**.  

#### **Create Your Instance:**  
1. Go to **Google Cloud Console** → **Compute Engine** → **VM Instances**  
2. Click **Create Instance**  
3. Set the **Machine type** to **e2-micro**  
4. Choose a **free-tier region** (`us-west1`, `us-central1`, or `us-east1`)  
5. Select **Ubuntu/Debian** as the operating system  
6. Enable **Allow HTTP & HTTPS traffic**  
7. Click **Create**  

Once created, **use the web-based SSH** in the Google Cloud Console to connect to your instance.  

---

## 🌍 Step 2: Configure DNS  

Set **A records** in your domain registrar to point to your server’s **public IP address**:  

| Name (Subdomain) | Type | TTL  | Value (Your Server's IP) |
|------------------|------|------|--------------------------|
| `@` (root domain) | A    | 300  | `xxx.xxx.xxx.xxx`        |
| `code`           | A    | 300  | `xxx.xxx.xxx.xxx`        |
| `firefly`        | A    | 300  | `xxx.xxx.xxx.xxx`        |
| `traefik`        | A    | 300  | `xxx.xxx.xxx.xxx`        |
| `vaultwarden`    | A    | 300  | `xxx.xxx.xxx.xxx`        |

🚨 **Note:** Replace `xxx.xxx.xxx.xxx` with your actual **public IP address**.  

---

## 📦 Step 3: Install Dependencies  

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

## 🔗 Step 4: Clone the Repository  

```bash
git clone https://github.com/mcconnellj/cloud-server  
cd cloud-server  
```

---

## 🛠️ Step 5: Configure Environment Files  

Create and edit **`.env` and `.db.env`** before running Ansible:  

```bash
cat <<EOF > .env-template
DOMAIN=example.com
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

## 🚀 Step 6: Run Ansible Playbook  

Execute the playbook to configure your cloud server:  

```bash
ansible-playbook ./playbooks/site.yml --connection=local  
```

---