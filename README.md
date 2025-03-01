# Cloud Server
This repository automates the setup of a cloud server using Ansible.

## How to Use
- Setup an e2 micro running in google compute engine.
- SSH onto that machine.

### Step 1: Setup an e2 Micro Instance

Create an e2 micro instance in Google Compute Engine and ensure it is running.

### Step 2: Reroute Your Domain

Reroute your domain using A records to point to the IP address of your e2 micro instance. Create the following subdomains:
- `traefik.yourdomain.com`
- `vaultwarden.yourdomain.com`
- `firefly.yourdomain.com`

### Step 3: Install Git and Ansible
```bash
sudo apt install -y git ansible
```

### Step 4: Clone the Repository
```bash
git clone https://github.com/mcconnellj/cloud-server
cd cloud-server
```

### Step 5: Generate a Hashed Password

The `TRAEFIK_PASSWORD_HASH` must be a hashed password. You can generate it using the following command:

```bash
openssl passwd -apr1
```

### Step 6: Create a .env File
Create a `.env` file in the root of the repository with the following content:

```bash
nano .env
```

```env
# Main Domain
DOMAIN=

# Subdomains (without the main domain)
VAULTWARDEN_SUBDOMAIN=vaultwarden
TRAEFIK_SUBDOMAIN=traefik
ARGOCD_SUBDOMAIN=argocd
CODE_SERVER_SUBDOMAIN=code

# Traefik Dashboard Credentials
TRAEFIK_USER=admin
TRAEFIK_PASSWORD=""
TRAEFIK_PASSWORD_HASH=""

# Vaultwarden Credentials
VAULTWARDEN_USER=admin
VAULTWARDEN_PASSWORD=""
VAULTWARDEN_PASSWORD_HASH=""

# ArgoCD Credentials
ARGOCD_USER=admin
ARGOCD_PASSWORD=""
ARGOCD_PASSWORD_HASH=""

# Code Server
CODE_SERVER_PASSWORD=  
CODE_SERVER_SUDO_PASSWORD=  

# Email for Let's Encrypt SSL
EMAIL=
```

### Step 7: Run the Ansible Playbook
```bash
ansible-playbook ./playbooks/site.yml --connection=local
```