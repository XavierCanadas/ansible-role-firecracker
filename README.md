# Ansible Role: Firecracker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Install and configure [Firecracker](https://firecracker-microvm.github.io/) microVM hypervisor as a systemd service.

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

## Dependencies

None.

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

- ✅ Idempotent installation (won't re-download if already installed)
- ✅ Automatic latest version detection from GitHub releases
- ✅ Default kernel download and configuration
- ✅ Default Ubuntu rootfs download and ext4 conversion
- ✅ Systemd service management
- ✅ Configurable socket path and service settings
- ✅ Automatic restart on failure

## License

MIT

## Author Information

Created by Xavier Canadas.
