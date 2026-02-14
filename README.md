# Ansible Role: Firecracker


[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-%23FE5196?logo=conventionalcommits&logoColor=white)](https://conventionalcommits.org)

## Description
This Ansible role automates the installation and configuration of the
[Firecracker](https://firecracker-microvm.github.io/) microVM hypervisor and
manages it as a systemd service. The role performs end-to-end setup using
Ansible: it installs the Firecracker binary, configures the systemd service
and socket, and applies the chosen service/user settings.

When requested, the role also downloads and configures a default kernel and a
default root filesystem (rootfs) following the recommendations and links in the
official Firecracker documentation. These default artifacts are stored under
`/opt/firecracker` by default and can be customized via the role variables.


## Requirements

- Ubuntu 24+ (tested only in this version)
- KVM kernel module loaded
- Root/sudo access

## Role Variables

Available variables with default values (see `defaults/main.yml`):

```yaml
# Firecracker latest binary path. Original binary always stored in /opt/firecracker/{version}/firecracker
firecracker_binary_path: /usr/local/bin/firecracker

# Default kernel installation
firecracker_use_default_kernel: true
firecracker_kernel_dir: /opt/firecracker/kernels

# Default rootfs installation
firecracker_use_default_rootfs: true
firecracker_rootfs_dir: /opt/firecracker/rootfs
firecracker_rootfs_size: 1G

# Firecracker API socket location
firecracker_socket_path: /tmp/firecracker.socket

# Enable PCI bus support
firecracker_enable_pci: true

# Systemd service configuration
firecracker_install_service: true
firecracker_service_user: root
firecracker_restart_policy: on-failure
firecracker_restart_sec: 5s

# Enable and start service automatically
firecracker_service_enabled: true
firecracker_service_state: started
```

## Example Playbook

```yaml
---
- hosts: workers
  become: true
  roles:
    - role: firecracker
      firecracker_socket_path: /var/run/firecracker.socket
      firecracker_use_default_kernel: true
      firecracker_use_default_rootfs: true
      firecracker_rootfs_size: 2G
```

## Features

- Idempotent installation (won't re-download if already installed)
- Automatic latest version detection from GitHub releases
- Default kernel download and configuration
- Default Ubuntu rootfs download and ext4 conversion
- Systemd service management
- Configurable socket path and service settings
- Automatic restart on failure

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- **Firecracker**: [GitHub Repository](https://github.com/firecracker-microvm/firecracker/). Followed the [Getting Started.ms](https://github.com/firecracker-microvm/firecracker/blob/main/docs/getting-started.md) to make the tasks.

- **Project Context**: Developed as part of my Computer Science final degree project (TFG) at Universitat Pompeu Fabra

## Author Information

Created by Xavier Canadas.
