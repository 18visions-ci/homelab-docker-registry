# homelab-docker-registry

This repository contains an [Ansible](https://www.ansible.com/) playbook and roles to deploy a native [Docker Registry](https://github.com/distribution/distribution) backed by an NFS share, suitable for homelab or small-scale environments.

## Features

- Installs Docker Registry as a native binary (not in a container)
- Configures persistent storage using an NFS mount (e.g., from Unraid NAS)
- Sets up a systemd service for easy management
- Customizable registry version and NFS parameters

## Requirements

- Ansible 2.9+
- Target host(s) running a Debian-based Linux (uses `apt` for NFS client)
- NFS server accessible from the target host(s)

## Usage

1. **Clone this repository:**
   ```sh
   git clone https://github.com/yourusername/homelab-docker-registry.git
   cd homelab-docker-registry
   ```

2. **Inventory Setup:**
    Ensure your Ansible inventory defines a registry-node host group with the appropriate hosts.

3. **Set Required Variables:**
    You must provide the nas_ip variable (the IP or hostname of your NFS server). Optionally, set env to customize the NFS share path.

Example command line:
```
ansible-playbook -i inventory playbook.yaml -e "nas_ip=192.168.1.100 env=prod"
```

4. **Run the playbook:**
```
ansible-playbook -i inventory playbook.yaml -e "nas_ip=<NFS_SERVER_IP>"
```

## Playbook Structure

- playbook.yaml: Main playbook orchestrating the deployment.
- roles/
    - nfs_mount: Installs NFS client, mounts the NFS share, and verifies the mount.
    - registry_install: Downloads and installs the Docker Registry binary.
    - registry_config: Installs the registry configuration file.
    - registry_service: Installs and enables the systemd service for the registry.

## Customization
- Registry Version: Set registry_version in playbook.yaml or override via -e.
- NFS Mount Point/Share: Adjust nas_mount_point and nas_share as needed.

## Example Configuration
The default registry configuration (roles/registry_config/files/config.yaml) stores images at /mnt/registry-data and listens on port 5000.