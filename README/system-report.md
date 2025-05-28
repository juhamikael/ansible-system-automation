# 🧾 Automated System Report Setup with Ansible

This Ansible setup automates the deployment of a system health report script. The script generates a daily HTML-formatted report including:

- ✅ Hostname, LAN IP, uptime  
- 📦 Security & manual package upgrades  
- 📍 Disk usage  
- 📈 CPU load average  
- 🔁 Reboot status  
- 📩 Report is emailed using `msmtp`

---

## 📁 Project Structure

```text
├── playbook.yml                  # Main playbook
├── group_vars/
│   └── local.yml.example         # Template for credentials
├── templates/
│   ├── daily-upgrade.sh.j2       # Email report script
│   └── msmtprc.j2                # Mail config template
```

---

## 🔐 Configure Gmail with an App Password

To use Gmail with `msmtp`:

1. Enable **2-Step Verification** in your Google Account  
2. Visit: [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)  
3. Create a new password (e.g. name it *Linux Server*)  
4. Copy and store it securely

---

## 🔧 Prerequisites

- Ubuntu or Debian-based system  
- Gmail account (uses app-specific password)  
- Python 3 (comes with modern Ubuntu versions)  
- `sudo` privileges

> ⚠️ You do NOT need to install Python manually. It is typically pre-installed and used automatically by Ansible.

---

## 🛠️ Installing Ansible

```bash
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```

Verify installation:

```bash
ansible --version
```

Expected: output like `ansible [core ...]`

---

## 📦 Variables in `group_vars/local.yml`

First, copy the example file:

```bash
cp group_vars/local.yml.example group_vars/local.yml
```

Edit values:

```yaml
email: "your-email@gmail.com"
app_password: "your-gmail-app-password"
```

> ⚠️ Store secrets securely in production:
> `ansible-vault encrypt group_vars/local.yml`

---

## 🚀 How to Run

If your user requires sudo password:

```bash
ansible-playbook playbook.yml --ask-become-pass
```

If your user has passwordless sudo:

```bash
ansible-playbook playbook.yml
```

This will:

- Install `msmtp`
- Create `.msmtprc` for both `root` and the current user
- Upload and schedule the system report cron job (daily at 04:00)

---

## 📬 Manual Test

Test email directly:

```bash
echo "test message" | msmtp your-email@gmail.com
```

Manually run the report:

```bash
sudo /usr/local/bin/daily-upgrade.sh
```

---

✅ After a successful run, the system will email you a daily HTML system report with uptime, disk usage, update info, and more.
