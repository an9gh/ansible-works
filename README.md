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

