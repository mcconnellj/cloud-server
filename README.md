Cloud Server
This repository automates the setup of a cloud server using Ansible.

## How to Use
- Setup an e2 micro running in google compute engine.
- SSh on to that machine.

### Step 1: Install Git and Ansible
- sudo apt install -y git ansible

### Step 2: Clone the Repository
- git clone https://github.com/mcconnellj/cloud-server
- cd cloud-server

### Step 3: Run the Ansible Playbook
- ansible-playbook ./playbooks/site.yml --connection=local
