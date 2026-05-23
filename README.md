<div align="center">

# 🚀 Ansible Galaxy Network Automation Suite

## RIP Routing + DHCP Services on Cisco Multi-VLAN Infrastructure

[![Ansible](https://img.shields.io/badge/Ansible-2.9+-red?style=for-the-badge&logo=ansible)](https://www.ansible.com/)
[![Cisco](https://img.shields.io/badge/Cisco-IOS-blue?style=for-the-badge&logo=cisco)](https://www.cisco.com/)
[![Automation](https://img.shields.io/badge/Automation-100%25-brightgreen?style=for-the-badge)](https://github.com/yourusername)
[![Network](https://img.shields.io/badge/Network-RIP%20v2-orange?style=for-the-badge)](https://github.com/yourusername)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

### 🎯 Zero-Touch Network Deployment | Production Ready | Idempotent Design

</div>

---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Key Features](#-key-features)
- [Network Architecture](#-network-architecture)
- [Technical Stack](#-technical-stack)
- [Quick Start](#-quick-start)
- [Installation Guide](#-installation-guide)
- [Project Structure](#-project-structure)
- [Configuration Details](#-configuration-details)
- [Running the Automation](#-running-the-automation)
- [Verification Steps](#-verification-steps)
- [Troubleshooting](#-troubleshooting)
- [Performance Metrics](#-performance-metrics)
- [Future Roadmap](#-future-roadmap)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Project Overview

This **enterprise-grade network automation** project demonstrates the power of **Infrastructure as Code (IaC)** by automatically configuring Cisco routers and switches using **Ansible Galaxy**. The automation deploys a complete multi-VLAN network with **RIP version 2 dynamic routing** and **DHCP services** across 5 network segments.

### Business Impact
- **⏱️ 95% Time Reduction**: 2-3 hours of manual CLI work → 30 seconds automated
- **🔒 Zero Configuration Errors**: Eliminate human error from network deployments
- **🔄 Consistent Deployments**: Same configuration every time, every device
- **📈 Scalable**: Add new routers/switches by simply updating inventory

### What Gets Deployed?
- ✅ 2 Routers with 5 VLAN subinterfaces each (10 total)
- ✅ 10 DHCP pools with excluded addresses and DNS configuration
- ✅ RIP v2 routing between routers with network advertisements
- ✅ 2 Switches with 5 VLANs, trunk ports, and access ports
- ✅ 150+ individual configuration lines automated

---

## 🌟 Key Features

| Feature | Description | Benefit |
|---------|-------------|---------|
| **Idempotent Configuration** | Run playbook multiple times without breaking config | Safe, repeatable deployments |
| **Conditional Logic** | Same playbook handles different devices dynamically | Single source of truth |
| **VLSM Support** | /28 subnets with proper IP planning | Efficient IP utilization |
| **DHCP Exclusion Ranges** | Reserves .1-.4 for infrastructure | Prevents IP conflicts |
| **Trunk + Access Ports** | Complete switch configuration | End-to-end automation |
| **RIP Version 2** | Classless routing with no auto-summary | Modern routing protocol |
| **Rapid PVST+** | Advanced spanning-tree on switches | Loop prevention |

---

## 🏗️ Network Architecture

### Physical Topology

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ MANAGEMENT NETWORK (OOB) │
│ 192.168.60.0/24 │
│ ┌────────────────────────────┐ │
│ │ Ansible Control Node │ │
│ │ (Docker/Linux Host) │ │
│ └────────────┬───────────────┘ │
│ │ │
│ ┌──────────────────┼──────────────────┐ │
│ │ │ │ │
│ ▼ ▼ ▼ │
│ ┌────────┐ ┌────────┐ ┌────────┐ │
│ │ R1 │ │ R2 │ │ SW1 │ │
│ │.60.11 │ │.60.12 │ │.60.21 │ │
│ └────┬───┘ └────┬───┘ └────┬───┘ │
│ │ │ │ │
│ Trunk │802.1Q Trunk│802.1Q │ │
│ │ │ │ │
│ ┌────┴───┐ ┌────┴───┐ │ │
│ │ SW1 │ │ SW3 │ │ │
│ │.60.21 │ │.60.22 │ │ │
│ └────┬───┘ └────┬───┘ │ │
│ │ │ │ │
│ Access Ports Access Ports │ │
│ to VLANs 10-50 to VLANs 10-50 │ │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Logical Diagram (R1 Side)

```
R1 Router
│
┌───────────────┼───────────────┐
│ │ │
┌────┴────┐ ┌────┴────┐ ┌────┴────┐
│Gi0/0.10 │ │Gi0/0.20 │ │Gi0/0.30 │
│VLAN 10 │ │VLAN 20 │ │VLAN 30 │
│IT Dept │ │HR Dept │ │Finance │
└────┬────┘ └────┬────┘ └────┬────┘
│ │ │
┌────┴────┐ ┌────┴────┐ ┌────┴────┐
│DHCP Pool│ │DHCP Pool│ │DHCP Pool│
│.5-.14 │ │.21-.30 │ │.37-.46 │
└─────────┘ └─────────┘ └─────────┘
```


### IP Addressing Scheme

#### Router R1 - VLAN Subinterfaces
| VLAN | Department | Subinterface | Network (CIDR) | Gateway | DHCP Range | Usable IPs |
|------|-----------|--------------|----------------|---------|------------|------------|
| 10 | IT | Gi0/0.10 | 192.168.10.0/28 | 192.168.10.1 | .5 - .14 | 10 |
| 20 | HR | Gi0/0.20 | 192.168.10.16/28 | 192.168.10.17 | .21 - .30 | 10 |
| 30 | FINANCE | Gi0/0.30 | 192.168.10.32/28 | 192.168.10.33 | .37 - .46 | 10 |
| 40 | HUMAN_RESOURCE | Gi0/0.40 | 192.168.10.48/28 | 192.168.10.49 | .53 - .62 | 10 |
| 50 | WIFI | Gi0/0.50 | 192.168.10.64/28 | 192.168.10.65 | .69 - .78 | 10 |

#### Router R2 - VLAN Subinterfaces
| VLAN | Department | Subinterface | Network (CIDR) | Gateway | DHCP Range | Usable IPs |
|------|-----------|--------------|----------------|---------|------------|------------|
| 10 | IT | Gi0/0.10 | 192.168.11.0/28 | 192.168.11.1 | .5 - .14 | 10 |
| 20 | HR | Gi0/0.20 | 192.168.11.16/28 | 192.168.11.17 | .21 - .30 | 10 |
| 30 | FINANCE | Gi0/0.30 | 192.168.11.32/28 | 192.168.11.33 | .37 - .46 | 10 |
| 40 | HUMAN_RESOURCE | Gi0/0.40 | 192.168.11.48/28 | 192.168.11.49 | .53 - .62 | 10 |
| 50 | WIFI | Gi0/0.50 | 192.168.11.64/28 | 192.168.11.65 | .69 - .78 | 10 |

#### Router-to-Router Link
| Connection | Network | Subnet Mask | R1 IP | R2 IP |
|------------|---------|-------------|-------|-------|
| R1 ↔ R2 | 192.168.15.0/30 | 255.255.255.252 | 192.168.15.1 | 192.168.15.2 |

---

## 💻 Technical Stack

### Core Technologies


```
┌─────────────────────────────────────────────────────────────┐
│ AUTOMATION LAYER │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ Ansible │ │Ansible Core │ │ Galaxy CLI │ │
│ │ 2.9+ │ │ Collections│ │ │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ │
├─────────────────────────────────────────────────────────────┤
│ NETWORK LAYER │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ Cisco IOS │ │ network_cli│ │ SSH │ │
│ │ 15.x+ │ │ Connection │ │ Protocol │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ │
├─────────────────────────────────────────────────────────────┤
│ ROUTING PROTOCOLS │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ RIP v2 │ │ VLSM │ │ No Auto- │ │
│ │ │ │ │ │ Summary │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ │
├─────────────────────────────────────────────────────────────┤
│ NETWORK SERVICES │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ DHCP │ │ DNS │ │ VLAN │ │
│ │ Server │ │ Forwarding │ │ 802.1Q │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ │
└─────────────────────────────────────────────────────────────┘
```


### Dependencies
```
# requirements.yml
collections:
  - name: cisco.ios
    version: ">=2.5.0"
  - name: ansible.netcommon
    version: ">=2.5.0"

python_packages:
  - ansible>=2.9
  - paramiko
  - netmiko
  - pyyaml

```

## 📁 Project Structure

```
ansible-galaxy-rip-automation/
│
├── 📄 host.ini                      # Inventory with device credentials
├── 📄 site.yml                      # Master playbook entry point
├── 📄 requirements.yml              # Galaxy collection dependencies
├── 📄 README.md                     # This comprehensive guide
├── 📄 .gitignore                    # Git ignore rules
│
├── 📁 roles/
│   │
│   ├── 📁 router_config/            # Router automation role
│   │   ├── 📁 tasks/
│   │   │   └── 📄 main.yml          # Router configuration tasks
│   │   ├── 📁 vars/
│   │   │   └── 📄 main.yml          # Router-specific variables
│   │   ├── 📁 defaults/
│   │   │   └── 📄 main.yml          # Default values
│   │   ├── 📁 templates/
│   │   │   └── 📄 rip_config.j2     # RIP template (optional)
│   │   ├── 📁 handlers/
│   │   │   └── 📄 main.yml          # Event handlers
│   │   ├── 📁 meta/
│   │   │   └── 📄 main.yml          # Role metadata
│   │   └── 📄 README.md             # Role documentation
│   │
│   └── 📁 switch_config/            # Switch automation role
│       ├── 📁 tasks/
│       │   └── 📄 main.yml          # Switch configuration tasks
│       ├── 📁 vars/
│       │   └── 📄 main.yml          # Switch-specific variables
│       ├── 📁 defaults/
│       │   └── 📄 main.yml          # Default values
│       ├── 📁 handlers/
│       │   └── 📄 main.yml          # Event handlers
│       ├── 📁 meta/
│       │   └── 📄 main.yml          # Role metadata
│       └── 📄 README.md             # Role documentation
│
└── 📁 tests/                        # Validation tests
    ├── 📄 test_inventory.yml
    └── 📄 test_playbook.yml
```


# Configure SSH for Old Cisco Devices

Some old Cisco routers and switches use legacy SSH algorithms that modern Linux systems block by default.

Create or edit SSH config file:

```bash
nano ~/.ssh/config
```

Paste this configuration:

```ssh
Host 192.168.*.*
    User admin
    KexAlgorithms diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1
    HostKeyAlgorithms ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
    Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,aes192-cbc,aes256-cbc,3des-cbc
    MACs hmac-sha1,hmac-sha1-96
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null


```
---
#  Fix SSH Permissions

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
```

---


# 6. Create Ansible Inventory File

Create inventory file:

```bash
nano host.ini
```

Paste:

```ini
[cisco]
SW1 ansible_host=<HOST_IP>

[cisco:vars]
ansible_connection=ansible.netcommon.network_cli
ansible_network_os=cisco.ios.ios
ansible_user=username
ansible_password=password
ansible_become=yes
ansible_become_method=enable
ansible_become_password=enable_password
```

---

# Example Inventory File

```ini
[cisco]
SW1 ansible_host=192.168.10.11
SW2 ansible_host=192.168.10.12

[cisco:vars]
ansible_connection=ansible.netcommon.network_cli
ansible_network_os=cisco.ios.ios
ansible_user=admin
ansible_password=cisco123
ansible_become=yes
ansible_become_method=enable
ansible_become_password=cisco123
```

---

# . Run Ansible Command

Run Cisco command using Ansible:

```bash
ansible -i host.ini cisco -m ios_command -a "commands='show version'"
```

---

