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

#### Stage 4. Access the VM

To SSH into the VM, use the following command:

```bash
vagrant ssh <vm-name>
```

```powershell
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-118-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Mon Nov 18 02:53:16 UTC 2024

  System load:  1.33               Processes:               172
  Usage of /:   30.1% of 38.70GB   Users logged in:         0
  Memory usage: 3%                 IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge


vagrant@ubuntu-jammy:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           7.8Gi       531Mi       6.5Gi       1.0Mi       781Mi       7.0Gi
Swap:             0B          0B          0B
vagrant@ubuntu-jammy:~$ docker ps
```

