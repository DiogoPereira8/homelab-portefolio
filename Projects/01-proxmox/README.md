
# Project 01 — Proxmox VE Infrastructure

## 1. Project Overview

This project documents the deployment, configuration, and management of a virtualized IT infrastructure using Proxmox Virtual Environment.

The infrastructure is hosted on a dedicated desktop computer and provides an environment for running Windows and Linux virtual machines.

## 2. Project Objectives

- Document the physical server hardware.
- Identify and document storage resources.
- Configure and manage virtual machines.
- Explore Linux bridges and virtual networking.
- Validate virtual machine connectivity.
- Document technical configurations and troubleshooting.

## 3. Infrastructure Environment

| Component | Description |
|---|---|
| Hypervisor | Proxmox VE 9.2.2 |
| Node | homelab |
| Physical Hardware | Desktop Computer |
| Virtualization | KVM |
| Storage | local, local-lvm, localnetwork |
| Operating Systems | Windows 11, Windows Server, Ubuntu |

## 4. Virtual Machines Inventory

| VM ID | Virtual Machine | Operating System |
|---|---|---|
| 100 | Copy-of-VM-windows-11 | Windows 11 |
| 101 | windows-11 | Windows 11 |
| 102 | Ubuntu-desktop | Ubuntu Desktop |
| 103 | ubuntu-server | Ubuntu Server |
| 104 | Windows-server-graphic | Windows Server |

## 5. Implementation

### Step 1 — Proxmox Installation

Proxmox VE was installed on a dedicated desktop computer using a bootable USB installer.

The installation provided a virtualization platform for deploying and managing multiple operating systems.

### Step 2 — Virtual Machine Deployment

Virtual machines were created through the Proxmox VE web interface.

The environment includes Windows and Ubuntu virtual machines for practicing systems administration and networking.

### Step 3 — Resource Management

Virtual machines are managed through the Proxmox interface, including CPU, memory, storage, and virtual network settings.

Resource allocation details will be documented after completing the infrastructure inventory.

## 6. Validation and Testing

The following tests are planned:

- Verify virtual machine startup and shutdown.
- Check virtual network connectivity.
- Validate IP address configuration.
- Test communication between virtual machines.
- Review CPU, RAM, and storage utilization.

## 7. Troubleshooting

This section will document technical issues, diagnostic procedures, and implemented solutions.

## 8. Security Considerations

- Restrict access to the Proxmox management interface.
- Use strong authentication credentials.
- Keep the hypervisor and guest systems updated.
- Avoid publishing sensitive configuration details.
- Review virtual machine network isolation.

## 9. Lessons Learned

This project provides hands-on experience with:

- Hypervisor installation and administration
- Virtual machine deployment
- Resource management
- Virtual infrastructure documentation

Additional findings will be documented as the project progresses.

## 10. Project Status

**In Progress**

Current focus: Infrastructure inventory and documentation.

