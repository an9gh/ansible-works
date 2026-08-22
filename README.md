# Ansible Infrastructure Automation Lab

🚀 **Just wrapped up my first Ansible lab!**

I recently completed my **RHCSA**, and I have now started working toward my **RHCE**. This lab was my first real step into that territory — building an automation foundation completely from the ground up.

## Lab Architecture
* **Platform:** 3 KVM/virt-manager VMs running on Ubuntu.
* **Nodes:** 1 Control Node, 2 Managed Nodes.
* **Network:** Static IP addresses and dedicated hostnames configured for seamless management.

## Pre-requisite Fundamentals
Before Ansible could execute tasks, I ensured the foundational infrastructure was solid:
* 🔑 **SSH Authentication:** Key-based, fully passwordless authentication.
* 👤 **User Access:** Dedicated automation users on each managed node with proper `sudo` privileges.
* 🏷️ **Name Resolution:** Local hostname resolution configured via `/etc/hosts`.
* ⚡ **Connectivity:** Verified raw SSH connectivity from the control node to all managed nodes.

## Ansible Setup Flow
Once the foundation was ready, I configured the control node:
1. Installed Ansible and confirmed dependencies with `ansible --version`.
2. Created a dedicated project workspace directory.
3. Formatted the `inventory.ini` file and optimized settings in `ansible.cfg`.
4. Verified end-to-end orchestration using the ad-hoc command: `ansible all -m ping`.

> 💡 **The Core Takeaway:** Ansible doesn't replace sysadmin fundamentals — it depends on them completely. No amount of automation tooling fixes broken SSH authentication or missing privileges.



## Step-by-Step Installation & Lab Setup

Follow this sequence of steps to reproduce this exact lab infrastructure environment:

### 1. Network & Host Configuration (All Nodes)
Open the local hosts file across your instances to map your node hostnames directly:
```bash
sudo vi /etc/hosts
```
Add the corresponding local IP mapping layout for your specific KVM virtual environment:
```text
# Example infrastructure layout mappings
192.168.122.10   control
192.168.122.21   node1
192.168.122.22   node2
```

### 2. User & Sudo Privilege Escalation (Managed Nodes)
Log into both **node1** and **node2** to create your dedicated automation user account and provide passwordless execution privileges:
```bash
# Create the user
sudo useradd -m jon

# Set an initial account password
sudo passwd jon

# Configure passwordless sudo entry
echo "jon ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/jon
```

### 3. Passwordless SSH Architecture (Control Node)
Generate your secure, dedicated cryptographic identity file on the control workstation and distribute it to your managed endpoints:
```bash
# Generate a dedicated key pair named 'ansible'
ssh-keygen -t ed25519 -f ~/.ssh/ansible -N ""

# Copy the public identification file to each host using hostname targets
ssh-copy-id -i ~/.ssh/ansible.pub jon@node1
ssh-copy-id -i ~/.ssh/ansible.pub jon@node2
```

### 4. Engine Installation & Workspace Initialization (Control Node)
Install the core runtime engine onto the control workstation, set up your project tree, and initialize configuration variables:
```bash
# Update repository lists and install Ansible core packages
sudo apt update && sudo apt install ansible -y

# Verify execution engine path and installation version
ansible --version

# Build your production workspace directory tree
mkdir -p ~/ansible && cd ~/ansible

# Initialize your engine workspace variables
touch ansible.cfg inventory.ini
```

### 5. Component Configuration & Blueprint Architecture
Populate your key configuration and targeting files with your host infrastructure parameters:

**`ansible.cfg`**
```ini
[defaults]
inventory = inventory.ini
remote_user = jon
host_key_checking = False
```

**`inventory.ini`**
```ini
# Ungrouped verification node mapping
node1

# Dynamic collection-grouped targeting architecture
[webservers]
node2
```

### 6. Orchestration Execution Verification
Trigger your deployment code via the command-line interface using an ad-hoc ping module test to check connection mapping paths:
```bash
# Execute standard infrastructure ad-hoc validation test
ansible all -m ping
```


---

## 📘 Reference: Managing Multiple Identities (Session vs. Configuration)

When handling multiple SSH keys on a control node, there are two primary management methods. Using a dedicated configuration profile file is the standard best practice for multi-key management.

### Method A: Temporary Sessions (`ssh-agent` + `ssh-add`)
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/ansible
```
* **Pros:** Ideal for quick, one-off runtime tasks or working with transient infrastructure.
* **Cons:** Temporary. The memory process terminates when you close your terminal window or restart your system, forcing you to reload keys manually.

### Method B: Permanent Multi-Key Routing Matrix (`~/.ssh/config`)
```text
Host github.com
  IdentityFile ~/.ssh/ansible
  IdentitiesOnly yes
```
* **Pros:** Persistent, automated, and scales seamlessly across dozens of different infrastructure keys.
* **Cons:** Requires file creation and precise syntax parameters.

### 💡 Why `~/.ssh/config` is the Right Choice
Without a configuration routing profile, the SSH client defaults to "Key Guessing Mode"—sending every identity file listed inside `~/.ssh/` to the target host sequentially. This behavior triggers explicit security thresholds on remote hosts, leading to a `Too many authentication failures` rejection error. 

Enforcing `IdentitiesOnly yes` forces the SSH sub-system to isolate and map one explicit cryptographic identity to one matching host profile.


---

## 📖 Built-In Documentation (`ansible-doc`)

During engineering development or offline lab environments (such as the RHCE exam), use `ansible-doc` to browse parameter requirements, data types, and production examples.

### Core Documentation Shortcuts
* **View full documentation manual:**
  ```bash
  ansible-doc ansible.builtin.file
  ```
* **Extract a clean YAML code block snippet (ideal for playbooks):**
  ```bash
  ansible-doc -s ansible.builtin.user
  ```
* **List all active system modules:**
  ```bash
  ansible-doc -l
  ```

