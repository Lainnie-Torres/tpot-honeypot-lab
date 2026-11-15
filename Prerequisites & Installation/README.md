# T-Pot Honeypot Lab – Prerequisites & Installation

## 🧰 PREREQUISITES

The following hardware and software were used to deploy the lab environment.

### Hardware & Software

- **Host Operating System:** Windows 11
- **Virtualization Platform:** Oracle VirtualBox
- **Guest Operating System:** Ubuntu Server
- **Honeypot Platform:** T-Pot Community Edition
- **Internet Connection:** Required for installation, updates, and deployment

### Virtual Machine Configuration

The T-Pot environment was deployed inside a virtual machine with the following resource allocation:

| Configuration | Specification |
|---|---|
| Virtual Machine Name | `T-Pot_Lab` |
| Operating System | Ubuntu Server (64-bit) |
| CPU Allocation | 4 CPUs |
| Memory Allocation | 8 GB RAM |
| Virtual Disk | 128 GB |
| Network Adapter | NAT |
| Host Port | 8443 |
| Guest Port | 64297 |



  <img src="screenshots/virtualbox-details.png" width="550" alt="VirtualBox VM configuration"><br>
  <sub>VirtualBox virtual machine configuration showing the allocated system resources.</sub>



  <img src="screenshots/virtualbox-storage.png" width="550" alt="VirtualBox Storage configuration"><br>
  <sub>VirtualBox storage configuration showing the virtual disk allocation.</sub>


### Network Configuration

The virtual machine uses **NAT networking** in VirtualBox. A port forwarding rule was configured to allow access to the T-Pot web interface from the host machine.

- **Host Port:** 8443
- **Guest Port:** 64297
- **Access Method:** Web browser through the host machine

## ⚙️ INSTALLATION & DEPLOYMENT

### 1. Virtual Machine Preparation

Created a virtual machine in Oracle VirtualBox to host the T-Pot environment. Allocated the necessary CPU, memory, and disk resources to support the Ubuntu Server operating system and T-Pot deployment.

### 2. Ubuntu Server Environment

Installed and configured Ubuntu Server as the guest operating system. The VM was configured with NAT networking and port forwarding to support access to the T-Pot web interface.

### 3. T-Pot Deployment

Deployed **T-Pot Community Edition** within the Ubuntu Server environment. T-Pot uses Docker to manage its honeypot services and supporting components.

The installed T-Pot version was verified from the local Git repository:

- **Release Reference:** `24.04.1`
- **Git Description:** `24.04.1-180-g5d95017d`

### 4. Web Interface Access
  <img src="screenshots/tpot web.png" width="550" alt="T-Pot Web Interface">

Configured VirtualBox port forwarding to access the T-Pot web interface from the Windows host.

The web interface was accessed through the browser using the forwarded host port.

## ✅ INSTALLATION VERIFICATION

The following checks were completed during deployment and verification:

- Confirmed the Ubuntu Server virtual machine boots and accepts login credentials.
- Accessed the Ubuntu command-line environment.
