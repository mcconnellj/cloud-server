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
touch .env
nano .env
```

Add the following content to the `.env` file. To save and exit `nano`, use the following key shortcuts:
- Save: `Ctrl + O`
- Exit: `Ctrl + X`

```env
DOMAIN=yourdomain.com
VAULTWARDEN_SUBDOMAIN=vaultwarden
FIREFLY_SUBDOMAIN=firefly
TRAEFIK_SUBDOMAIN=traefik
TRAEFIK_PASSWORD_HASH=your_hashed_password
```

### Step 7: Run the Ansible Playbook
```bash
ansible-playbook ./playbooks/site.yml --connection=local
```