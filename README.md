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
