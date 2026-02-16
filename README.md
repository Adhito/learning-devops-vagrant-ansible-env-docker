# Learning Vagrant & Ansible By Provisioning VM + Docker

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Vagrant](https://img.shields.io/badge/Vagrant-2.4+-1868F2?logo=vagrant)](https://www.vagrantup.com/)
[![Ansible](https://img.shields.io/badge/Ansible-2.9+-EE0000?logo=ansible)](https://www.ansible.com/)
[![VirtualBox](https://img.shields.io/badge/VirtualBox-7.0+-183A61?logo=virtualbox)](https://www.virtualbox.org/)

This repository contains a Vagrant configuration to set up a development environment for Docker REST API. The virtual machine (VM) is provisioned using VirtualBox and is pre-configured with necessary specifications and networking settings.

---

## Table of Contents

- [Infrastructure Information](#infrastructure-information)
- [Architecture Overview](#architecture-overview)
- [File Structure](#file-structure)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Getting Started](#getting-started)
- [Ansible Playbook Details](#ansible-playbook-details)
- [Security Notes](#security-notes)
- [Networking](#networking)
- [Running Docker Compose Applications](#running-docker-compose-applications)
- [Troubleshooting](#troubleshooting)
- [Additional Notes](#additional-notes)
- [Contributing](#contributing)
- [References](#references)
- [License](#license)

---

## Infrastructure Information

- **Operating System**: Ubuntu Jammy 64-bit (`ubuntu/jammy64`)
- **CPU and Memory Allocation**: 8 CPUs and 8GB RAM
- **Networking**:
  - Private network with static IP: `192.168.1.10`
  - Forwarded ports for HTTP (80), HTTPS (443), custom services (8080, 10001)
  - SSH port forwarding: Host `2222` → Guest `22`
- **Shared Folder**: Synced folder between host and guest (`/vagrant`)
- **Provisioning**: Uses Ansible for provisioning tasks via `ansible-playbook.yml`
- **User Accounts**:
  - `vagrant`: Default user with key-based SSH authentication
  - `administrator-01`: Admin user with password authentication (`S4a*K!uLt&&Wof`)
  - Both users have `sudo` and `docker` group access
- **Custom VM Settings**:
  - VM Name: `DEVDOCKER-01-ENV01`
  - Cluster Name: `Development Container Engine Docker REST API 01`

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        Host Machine                         │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                    vagrant up                         │  │
│  └───────────────────────┬───────────────────────────────┘  │
│                          ▼                                  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │              VirtualBox VM (Ubuntu Jammy)             │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │              Ansible Local Provisioner          │  │  │
│  │  │  ┌───────────────────────────────────────────┐  │  │  │
│  │  │  │  Stage 1: Install Docker + Dependencies   │  │  │  │
│  │  │  │  Stage 2: Configure User Permissions      │  │  │  │
│  │  │  │  Stage 3: Validate with hello-world       │  │  │  │
│  │  │  └───────────────────────────────────────────┘  │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  │                          ▼                            │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │           Docker Engine (Ready to Use)          │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## File Structure

```
├── Vagrantfile              # VM configuration (VirtualBox provider)
├── ansible-playbook.yml     # Ansible playbook for Docker setup
├── ansible-requirements.yml # Ansible Galaxy dependencies
├── README.md                # Project documentation
├── CLAUDE.md                # Claude Code instructions
├── LICENSE                  # GPLv3 license
├── .gitignore               # Git ignore rules
├── src-application-01/      # Sample app: Traefik + Go backend
│   ├── docker-compose.yaml
│   ├── backend/
│   │   ├── Dockerfile
│   │   └── main.go
│   └── README.md
└── src-application-02/      # Sample app: React + Node.js + MongoDB
    ├── docker-compose.yaml
    ├── frontend/
    │   ├── Dockerfile
    │   ├── package.json
    │   └── src/
    ├── backend/
    │   ├── Dockerfile
    │   ├── package.json
    │   ├── server.js
    │   ├── config/
    │   ├── db/
    │   ├── models/
    │   └── routes/
    └── README.md
```

---

## Prerequisites

1. **Vagrant**: [Install Vagrant](https://www.vagrantup.com/downloads)
2. **VirtualBox**: [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)
3. **Ansible**: Required on the guest VM for provisioning (automatically installed by Vagrant).

---

## Quick Start

For experienced users, get up and running in 3 commands:

```bash
git clone https://github.com/Adhito/learning-devops-vagrant-ansible-env-docker
cd learning-devops-vagrant-ansible-env-docker
vagrant up
```

Then access the VM:
```bash
vagrant ssh devdocker-01
```

---

## Getting Started

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
| `VM_NAME`                | Name of the VM in VirtualBox               | `DEVDOCKER-01-ENV01` |
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

There are multiple ways to SSH into the VM:

##### Method 1: Using Vagrant SSH (Recommended for Quick Access)

```bash
vagrant ssh devdocker-01
```

##### Method 2: Using Traditional SSH with Private Key

```bash
ssh -i ".vagrant/machines/devdocker-01/virtualbox/private_key" -p 2222 vagrant@127.0.0.1
```

Or via the private network:

```bash
ssh vagrant@192.168.1.10
```

##### Method 3: Using SSH with Password Authentication (Administrator User)

The VM includes a pre-configured administrator user with password authentication enabled:

```bash
ssh -p 2222 administrator-01@127.0.0.1
```

Or via the private network:

```bash
ssh administrator-01@192.168.1.10
```

**Administrator Credentials:**
- **Username**: `administrator-01`
- **Password**: `S4a*K!uLt&&Wof`
- **Privileges**: Full sudo access and Docker group membership

##### SSH Connection Details

| Parameter | Value |
|-----------|-------|
| **Default User** | `vagrant` (key-based auth) |
| **Admin User** | `administrator-01` (password auth) |
| **Private Key Path** | `.vagrant/machines/devdocker-01/virtualbox/private_key` |
| **SSH Port (NAT)** | `2222` |
| **Private Network IP** | `192.168.1.10` |
| **SSH Port (Private Network)** | `22` (default) |

##### Adding SSH Config Entry (Optional)

You can add these entries to your `~/.ssh/config` file for easier access:

```
# Vagrant user with key-based authentication
Host devdocker-01-vagrant
  HostName 127.0.0.1
  User vagrant
  Port 2222
  IdentityFile /path/to/project/.vagrant/machines/devdocker-01/virtualbox/private_key
  StrictHostKeyChecking no

# Administrator user with password authentication
Host devdocker-01-admin
  HostName 127.0.0.1
  User administrator-01
  Port 2222
  PreferredAuthentications password
```

Then connect using:
```bash
ssh devdocker-01-vagrant  # For vagrant user
ssh devdocker-01-admin     # For administrator user
```

##### Example Session

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


#### Stage 5. Provisioning with Ansible

The VM uses Ansible for provisioning. Ensure the following files are present in the repository:

- **`ansible-playbook.yml`**: Main playbook for provisioning tasks.
- **`ansible-requirements.yml`**: Contains role requirements for Galaxy.

To setup the Docker Engine and other VM settings Ansible are choosen for configuring the environment. Vagrant supports two types of provisioners: the traditional [ansible], which requires a Linux-based host system, and [ansible_local], which is ideal when the host system is not Linux. When using the ansible_local provisioner, Vagrant will automatically install Ansible on the guest VM and execute the playbooks there. For more information, refer to the official documentation.

To use ansible_local, the minimum configuration requires specifying a [playbook.yml] file, which Vagrant will look for in the current project directory.

Provisioning runs automatically when the VM is created with `vagrant up`. To manually re-provision the VM, use:

```bash
vagrant provision
```

---

## Ansible Playbook Details

The `ansible-playbook.yml` executes four stages:

| Stage | Task | Description |
|-------|------|-------------|
| 1 | Install Docker & Dependencies | Installs aptitude, curl, Docker GPG key, Docker CE, and Python Docker module |
| 2 | Create Administrator User | Creates `administrator-01` user with password authentication and SSH access |
| 3 | Configure User Permissions | Adds vagrant user to docker group for non-root Docker access |
| 4 | Validation | Pulls and runs hello-world container to verify installation |

**Key Features:**
- Uses `become: true` for privilege escalation during package installation
- Creates administrator user with password: `S4a*K!uLt&&Wof` (SHA512 hashed)
- Enables SSH password authentication by removing cloud-init override
- Adds administrator user to `sudo` and `docker` groups for full privileges
- Includes validation check that fails if user is not properly added to docker group
- Automatically tests Docker with hello-world container
- Maintains both key-based (vagrant) and password-based (administrator-01) SSH authentication

---

## Security Notes

### SSH Authentication Methods

This VM supports two authentication methods:

1. **Key-based Authentication (vagrant user)**
   - More secure, recommended for production environments
   - Private key automatically generated by Vagrant
   - No password required

2. **Password Authentication (administrator-01 user)**
   - Enabled for testing and learning purposes
   - Default password: `S4a*K!uLt&&Wof`
   - **⚠️ IMPORTANT**: Change this password in production environments

### Changing the Administrator Password

To change the administrator password, update the `ansible-playbook.yml` file:

```yaml
- name: Create administrator-01 user with password
  ansible.builtin.user:
    name: administrator-01
    password: "{{ 'YOUR_NEW_PASSWORD' | password_hash('sha512') }}"
```

Then run:
```bash
vagrant provision
```

Alternatively, change the password directly on the VM:
```bash
vagrant ssh devdocker-01
sudo passwd administrator-01
```

### Disabling Password Authentication

For production environments, it's recommended to disable password authentication and use only key-based authentication:

1. Comment out or remove the "Create Administrator User" stage in `ansible-playbook.yml`
2. Remove the SSH configuration tasks that enable password authentication
3. Run `vagrant provision` to apply changes

---

## Networking

The VM is accessible via the following ports:

| Service            | Guest Port | Host Port | IP Address      |
|--------------------|------------|-----------|-----------------|
| HTTP               | 80         | 80        | 192.168.1.10    |
| HTTPS              | 443        | 443       | 192.168.1.10    |
| Custom Service 1   | 8080       | 8080      | 192.168.1.10    |
| Custom Service 2   | 10001      | 10001     | 192.168.1.10    |

---

## Running Docker Compose Applications

This repository includes two sample Docker Compose applications to help you learn container orchestration and multi-service deployments.

### Application Overview

#### Application 1: Traefik + Go Backend
- **Location**: `src-application-01/`
- **Stack**: Traefik 3.1 reverse proxy + Go HTTP server
- **Ports**: 80 (HTTP), 8080 (Traefik Dashboard)
- **Purpose**: Learn reverse proxy configuration and path-based routing

#### Application 2: React + Node.js + MongoDB
- **Location**: `src-application-02/`
- **Stack**: React frontend + Express backend + MongoDB database
- **Port**: 3000 (Frontend)
- **Purpose**: Learn full-stack application deployment with database persistence

---

### Running Application 1: Traefik + Go Backend

**Step 1: SSH into the VM**
```bash
vagrant ssh devdocker-01
```

**Step 2: Navigate to the application directory**
```bash
cd /vagrant/src-application-01
```

**Step 3: Start the application**
```bash
docker compose up -d
```

This will pull the Traefik 3.1 image, build the Go backend, and start both containers.

**Step 4: Verify containers are running**
```bash
docker compose ps
```

Expected output:
```
NAME                          IMAGE                          SERVICE     STATUS
src-application-01-backend-1  src-application-01-backend     backend     running
src-application-01-frontend-1 traefik:3.1                    frontend    running
```

**Step 5: Test the application**

From inside the VM:
```bash
curl http://localhost/v1/hello-world
```

Expected output:
```
          ##         .
    ## ## ##        ==
 ## ## ## ## ##    ===
/"""""""""""""""""\___/ ===
{                       /  ===-
\______ O           __/
 \    \         __/
  \____\_______/


Hello world from Go in Docker!
```

**Step 6: Access Traefik Dashboard**

From your **host machine** browser:
```
http://192.168.1.10:8080
```

**Step 7: Stop the application**
```bash
docker compose down
```

---

### Running Application 2: React + Node.js + MongoDB

**Step 1: SSH into the VM**
```bash
vagrant ssh devdocker-01
```

**Step 2: Navigate to the application directory**
```bash
cd /vagrant/src-application-02
```

**Step 3: Start the application**
```bash
docker compose up -d
```

This builds the React and Node.js images, pulls MongoDB 4.2.0, and starts all three containers.

**Step 4: Monitor startup (recommended for first run)**
```bash
docker compose logs -f
```

Wait for these messages:
- Backend: `MongoDB connected successfully`
- Frontend: `webpack compiled successfully` or `Compiled successfully!`
- Mongo: `waiting for connections on port 27017`

Press `Ctrl+C` to exit logs (containers keep running).

**Step 5: Verify containers are running**
```bash
docker compose ps
```

Expected output:
```
NAME                          IMAGE                          SERVICE     STATUS
src-application-02-backend-1  src-application-02-backend     backend     running
src-application-02-frontend-1 src-application-02-frontend    frontend    running
src-application-02-mongo-1    mongo:4.2.0                    mongo       running
```

**Step 6: Access the Todo application**

From your **host machine** browser:
```
http://192.168.1.10:3000
```

**Step 7: Test the application**
- Add a new todo item using the form
- The item should appear in the list below

**Step 8: Inspect the database (optional)**

From inside the VM:
```bash
docker exec -it src-application-02-mongo-1 mongosh TodoApp --eval "db.todos.find().pretty()"
```

**Step 9: Stop the application**
```bash
docker compose down
```

To also remove volumes (delete database data):
```bash
docker compose down -v
```

---

### Application Access Summary

| Application | Guest Port | Host Access | Purpose |
|-------------|------------|-------------|---------|
| Traefik HTTP | 80 | http://192.168.1.10:80 | Go backend endpoint (`/v1/hello-world`) |
| Traefik Dashboard | 8080 | http://192.168.1.10:8080 | Traefik admin UI |
| React Frontend | 3000 | http://192.168.1.10:3000 | Todo application |

---

### Common Docker Compose Commands

```bash
# Start services in detached mode
docker compose up -d

# View logs
docker compose logs -f

# View logs for specific service
docker compose logs -f backend

# Check service status
docker compose ps

# Stop services (keeps containers)
docker compose stop

# Stop and remove containers
docker compose down

# Stop, remove containers, and delete volumes
docker compose down -v

# Rebuild images
docker compose build --no-cache

# Restart specific service
docker compose restart backend
```

---

### Troubleshooting Docker Applications

#### Port Conflicts
```bash
# Check what's using a port inside VM
sudo netstat -tulpn | grep :<port>

# Check running containers
docker ps
```

#### Build Failures
```bash
# Rebuild with no cache
docker compose build --no-cache
docker compose up -d
```

#### Application Not Accessible from Host
```bash
# Inside VM, verify services are listening
sudo netstat -tulpn | grep -E ':(80|3000|8080)'

# From host, test connectivity
curl http://192.168.1.10:80
curl http://192.168.1.10:3000
```

#### MongoDB Connection Issues (Application 2)
```bash
# Check MongoDB logs
docker compose logs mongo

# Restart backend if it started before MongoDB was ready
docker compose restart backend
```

#### Complete Cleanup
```bash
# Stop all running containers
docker stop $(docker ps -aq)

# Remove all containers
docker rm $(docker ps -aq)

# Remove all images
docker rmi $(docker images -q)

# Remove all volumes
docker volume prune -f

# Remove all networks
docker network prune -f
```

---

## Troubleshooting

### Port Already in Use
If you get port conflict errors, modify the port mappings in `Vagrantfile` or stop the conflicting service.

### Provisioning Failed
Re-run provisioning:
```bash
vagrant provision
```

### VM Won't Start
Check VirtualBox is running and has enough resources:
```bash
vagrant status
VBoxManage list runningvms
```

### Docker Permission Denied
SSH into the VM and verify group membership:
```bash
vagrant ssh devdocker-01
groups vagrant
```

### Reset Everything
```bash
vagrant destroy -f
vagrant up
```

---

## Additional Notes

- **To stop the VM**:  
  Use the following command to gracefully stop the VM:  
  ```bash
  vagrant halt
  ```
- **To destroy the VM**:
  Use the following command to destroy the VM:
  ```bash
  vagrant destroy
  ```

---

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## References

- [Vagrant Documentation](https://developer.hashicorp.com/vagrant/docs)
- [Ansible Documentation](https://docs.ansible.com/)
- [Ansible Local Provisioner](https://developer.hashicorp.com/vagrant/docs/provisioning/ansible_local)
- [Docker Documentation](https://docs.docker.com/)
- [VirtualBox Documentation](https://www.virtualbox.org/manual/)

---

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.