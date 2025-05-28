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
├── playbook.yml
├── group_vars/
│   └── local.yml.example
├── inventories/
│   └── local/
│       └── hosts
├── roles/
│   └── system_report/
│       ├── tasks/
│       │   ├── main.yml
│       │   ├── install_msmtp.yml
│       │   ├── create_msmtprc.yml
│       │   ├── upload_script.yml
│       │   └── schedule_cron.yml
│       └── templates/
│           ├── daily-upgrade.sh.j2
│           └── msmtprc.j2
└── README/
    └── system-report.md
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

Expected output: `ansible [core ...]`

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

To run only the system report role:

```bash
ansible-playbook -i inventories/local playbook.yml --ask-become-pass --tags system_report
```

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
