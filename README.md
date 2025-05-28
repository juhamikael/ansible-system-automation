# 🧠 Ansible System Automation

This repository contains a collection of Ansible playbooks and templates for automating Linux system administration tasks.

## 📂 Structure

- `playbooks/` — All Ansible playbooks
- `roles/` — Role-based task organization
- `templates/` — Jinja2 template files (now mostly inside roles)
- `group_vars/` — Group-specific configuration
- `inventories/` — Host inventories
- `README/` — Documentation for specific playbooks

## 🚀 How to Run

To run all automation roles (default behavior):

```bash
ansible-playbook -i inventories/local playbook.yml --ask-become-pass
```

To run a specific tagged role (e.g. system_report):

```bash
ansible-playbook -i inventories/local playbook.yml --ask-become-pass --tags system_report
```

## 📖 Playbook Docs

- [📧 System Report Generator](README/system-report.md)
