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

### Infrastructure Prerequisites

1. **Vagrant**: [Install Vagrant](https://www.vagrantup.com/downloads)
2. **VirtualBox**: [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)
3. **Ansible**: Required on the guest VM for provisioning.

---

### Getting Started

#### Stage 1. Clone the Repository
```bash
git clone https://github.com/Adhito/learning-devops-vagrant-ansible-env-docker
cd learning-devops-vagrant-ansible-env-docker
```

#### Stage 2. Customize Configuration (Optional)

You can modify VM settings by editing the following variables in the `Vagrantfile`:

| Variable                 | Description                                | Default Value         |
|--------------------------|--------------------------------------------|-----------------------|
| `VM_TOTAL_CPU`           | Number of CPUs allocated to the VM         | `8`                   |
| `VM_TOTAL_MEMORY`        | RAM allocated to the VM (in MB)            | `8192`                |
| `VM_IP_ADDRESS`          | Private network IP address                 | `192.168.1.10`        |
| `VM_NAME`                | Name of the VM in VirtualBox               | `DEVDOCKER-01-ENVLOCAL` |
| `VAGRANT_CLUSTER_NAME`   | Group name for the VM in VirtualBox         | `Development Container Engine Docker REST API 01` |

---

#### Stage 3. Start the VM

Run the following command to spin up the environment:

```bash
vagrant up
```

Run the following command to check the environment status:

```bash
vagrant status
```
