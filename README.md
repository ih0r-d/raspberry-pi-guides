# 🐧 Raspberry Pi Guides

A collection of practical guides for setting up and configuring Raspberry Pi 5 with SSD, headless Ubuntu, and common
enhancements.

---

## 📚 Available Guides

| File                                                | Description                                                                    |
|-----------------------------------------------------|--------------------------------------------------------------------------------|
| [`rpi-ssd-setup.md`](docs/rpi-ssd-setup.md)         | Full setup from flashing Ubuntu Server to auto-mounting SSD and enabling Wi-Fi |
| [`rpi-ssd-workloads.md`](docs/rpi-ssd-workloads.md) | Steps to move logs, Docker, swap, and user data from SD to SSD                 |
| [`ssh-key-setup`](docs/ssh-key-setup.md)            | SSH Key Authentication for Raspberry Pi                                        |

---

## ⚙️ Ansible Automation

This repo includes a ready-to-use [Ansible](https://www.ansible.com/) setup for automating Raspberry Pi configuration.

### 📂 Structure

```bash
ansible/
├── inventory/           # Hosts list
├── group_vars/          # Group-level variables (with .env support)
├── host_vars/           # Host-specific variables
├── roles/               # Modular roles (base, ssd, docker, swap, etc.)
├── site.yml             # Main playbook
├── ansible.cfg          # Config with sane defaults
```

### 🚀 Usage (via [Task](https://taskfile.dev))

1. Edit environment variables in `Taskfile.yml` or export manually:

```bash
export ANSIBLE_HOST=192.168.1.1     # need to replace with valid value
export ANSIBLE_USER=ubuntu          # need to replace with valid value
export HOSTNAME=rpi-dev             # need to replace with valid value
export SSD_MOUNT_POINT=/mnt/ssd     # need to replace with valid value
```

2. Run setup:

```bash
task setup
```

3. Other commands:

```bash
task ping    # test connectivity
task ssh     # open SSH session
task logs    # live system logs
```

Supports flexible overrides via environment variables.  
All paths, credentials, and flags are managed centrally.

---

## 📌 Notes

- All guides assume Raspberry Pi 5 with Ubuntu Server 25.04
- Focus is on minimizing SD wear and maximizing SSD usage
- Designed for performance, automation, and long-term use

---

## 🔧 Author & Contributions

Maintained by [ih0r-d].  
PRs, corrections, and additions welcome!
