# Enterprise Linux Server Administration Lab (Ubuntu Server)

A hands-on lab documenting how I built, configured, secured, and administered an
Ubuntu Server environment from scratch — covering the core skills used in
real-world Linux system administration roles.

## 🎯 Project Goal

Starting from a blank VM, this project walks through setting up an Ubuntu
Server the way it would be done in a production/enterprise environment:
hardened access, proper user/permission management, a working web service,
monitoring, and documented troubleshooting.

## 🧰 Lab Environment

| Component        | Details                          |
|-------------------|-----------------------------------|
| Hypervisor        | VirtualBox                        |
| OS                | Ubuntu Server 26.04 LTS            |
| Host machine      | Windows 11                        |
| Networking        | NAT (internet) + Host-Only (SSH access) |
| Access method     | SSH (Windows Terminal)            |

## 📚 Modules

Each module below has its own write-up in `docs/`, including the commands
run, config snippets, screenshots, and what I learned/broke/fixed along the way.

| # | Module | Status |
|---|--------|--------|
| 1 | [Base Setup & Initial Hardening](docs/01-base-setup.md) | ✅ Done |
| 2 | [User & Group Management](docs/02-user-group-management.md) | ✅ Done |
| 3 | [SSH Hardening](docs/03-ssh-hardening.md) | ⬜ Not started |
| 4 | [Firewall Configuration (UFW)](docs/04-firewall-configuration.md) | ⬜ Not started |
| 5 | [Package & systemd Service Management](docs/05-package-service-management.md) | ⬜ Not started |
| 6 | [Web Server Deployment (Nginx)](docs/06-web-server-deployment.md) | ⬜ Not started |
| 7 | [Logging & Monitoring](docs/07-logging-monitoring.md) | ⬜ Not started |
| 8 | [Backups & Troubleshooting](docs/08-backup-troubleshooting.md) | ⬜ Not started |
| 9 | [Containerizing a Service with Docker](docs/09-docker.md) | ⬜ Not started |
| 10 | [Infrastructure as Code with Terraform](docs/10-terraform.md) | ⬜ Not started |

_Update the status column as you complete each module: ⬜ Not started → 🟨 In progress → ✅ Done_

## 🗺️ Architecture

```
                +--------------------------+
                |     Windows Host (PC)    |
                |  VirtualBox + Terminal   |
                +------------+-------------+
                             |
                 Host-Only Network (SSH)
                             |
                +------------v-------------+
                |     ubuntu-server (VM)   |
                |  Ubuntu Server 26.04 LTS |
                |--------------------------|
                | - SSH (hardened)         |
                | - UFW Firewall           |
                | - Nginx web server       |
                | - systemd services       |
                | - Logging / monitoring   |
                | - Group-based sudo access|
                +--------------------------+
```

## 🛠️ Skills Demonstrated

- Linux command line & filesystem navigation
- User, group, and sudo policy management
- SSH key-based authentication & hardening
- Firewall configuration with UFW
- systemd service and package management
- Web server (Nginx) installation and configuration
- Log analysis with `journalctl` and log rotation
- Backup scripting and cron scheduling
- Containerization fundamentals with Docker
- Infrastructure as Code with Terraform
- Practical troubleshooting and root-cause documentation

## 📁 Repo Structure

```
.
├── README.md
└── docs/
    ├── 01-base-setup.md
    ├── 02-user-group-management.md
    ├── 03-ssh-hardening.md
    ├── 04-firewall-configuration.md
    ├── 05-package-service-management.md
    ├── 06-web-server-deployment.md
    ├── 07-logging-monitoring.md
    ├── 08-backup-troubleshooting.md
    ├── 09-docker.md
    ├── 10-terraform.md
    └── screenshots/
```

## 🚀 How to Reproduce

1. Install VirtualBox and download Ubuntu Server 26.04 LTS (or the latest LTS).
2. Create a VM (4GB RAM, 25GB disk, NAT + Host-Only networking).
3. Follow the modules in `docs/` in order — each is self-contained with the
   exact commands used, plus real issues hit along the way and how they were fixed.

## 📌 Status

This lab is a work in progress, built and documented step by step.
