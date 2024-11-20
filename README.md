# Learning Vagrant &amp; Ansible By Provisioning VM + Docker
Learning Vagrant &amp; Ansible By Provisioning VM + Docker


This repository contains a Vagrant configuration to set up a development environment for Docker REST API. The virtual machine (VM) is provisioned using VirtualBox and is pre-configured with necessary specifications and networking settings.

### Infrastructure Informations

- **Operating System**: Ubuntu Jammy 64-bit (`ubuntu/jammy64`)
- **CPU and Memory Allocation**: 8 CPUs and 8GB RAM
- **Networking**:
  - Private network with static IP: `192.168.1.10`
  - Forwarded ports for HTTP (80), HTTPS (443), custom services (8080, 10001)
- **Shared Folder**: Synced folder between host and guest (`/vagrant`)
- **Provisioning**: Uses Ansible for provisioning tasks via `ansible-playbook.yml`
- **Custom VM Settings**:
  - VM Name: `DEVDOCKER-01-ENV01`
  - Cluster Name: `Development Container Engine Docker REST API 01`

---