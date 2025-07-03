# ansible-monitoring-stack

Automate the deployment of a modern monitoring stack (Prometheus, Grafana, Node Exporter, Blackbox Exporter) using Ansible.  
This project is structured for clarity, security, and easy demonstration as a portfolio.

---

## Features

- **Automated Provisioning:** One-command setup for Prometheus, Grafana, Node Exporter, and Blackbox Exporter.
- **Role-Based Structure:** Each component is modularized as an Ansible role.
- **Secure Variable Management:** Sensitive variables are managed with [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html).
- **Environment Grouping:** Inventory supports grouping hosts by environment (e.g., `dev`, `prod`).
- **Systemd Integration:** All exporters and services are managed as systemd units.

---

## Project Structure

```
ansible-monitoring-stack/s
├── ansible.cfg
├── inventory/
│   └── hosts.yml
├── playbook.yml
├── roles/
│   ├── blackbox-exporter/
│   ├── grafana/
│   ├── node-exporter/
│   └── prometheus/
├── ssh-secret.yaml
└── .gitignore
```

---

## Getting Started

### Prerequisites

- Ansible 2.9+ installed on your control machine
- SSH access to target hosts
- Python 3 on target hosts

---

### Inventory

Edit [`inventory/hosts.yml`](inventory/hosts.yml) to define your target hosts and groupings. Example:

```yaml
all:
  children:
    dev:
      hosts:
        vm1:
          ansible_host: 127.0.0.1
      vars:
        ansible_user: "{{ username }}"
        ansible_ssh_private_key_file: ../ssh-keys/
        ansible_ssh_port: 22
```

> **Note:** Replace `{{ username }}` and SSH key path as needed.

---

### Secure Variables

Sensitive variables (like passwords) should be stored in an encrypted file using Ansible Vault.

**Create a vault file:**
```bash
ansible-vault create ssh-secret.yaml
```

**Example content:**
```yaml
ansible_password: your_ssh_password
ansible_become_password: your_sudo_password
```

---

### Running the Playbook

Run the playbook with:

```bash
ansible-playbook playbook.yml --ask-vault-pass
```

Or, if you use a vault password file:

```bash
ansible-playbook playbook.yml --vault-password-file vault_pass.txt
```

---

### Customization

- Modify variables in each role under `roles/<role>/defaults/main.yml` as needed.
- Add or remove roles in `playbook.yml` to suit your stack.

---

## Roles Overview

- **Prometheus:** Installs and configures Prometheus server.
- **Grafana:** Installs and configures Grafana for visualization.
- **Node Exporter:** Deploys Node Exporter for system metrics.
- **Blackbox Exporter:** Deploys Blackbox Exporter for endpoint probing.

---

## Best Practices

- **Do not commit sensitive files** (like `ssh-secret.yaml` or real inventory) to version control. This project’s `.gitignore` is set up to help prevent this.
- Use variables and placeholders for credentials and secrets.
- Use Ansible Vault for all sensitive data.

---

## Author

Made with ❤️ by Danuptra
