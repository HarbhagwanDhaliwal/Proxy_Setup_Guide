# 🌐 Proxy Setup Guide

This guide provides step-by-step instructions for setting up your own proxy server using Squid with basic authentication.

## ⚙️ Prerequisites
- 🖥️ A Linux server with root or sudo access.
- 📚 Basic understanding of terminal commands.

---

## 🛠️ Installation Steps

### 1️⃣ Update System Packages
📦 Update the package list to ensure you get the latest versions of the required software:
```bash
sudo apt update
```

### 2️⃣ Install Squid and Apache Utilities
🐙 Install Squid (the proxy server) and `htpasswd` (for creating user credentials):
```bash
sudo apt install squid apache2-utils
```

---

## 🛡️ Configuration Steps

### 3️⃣ Prepare Configuration Directory
📁 Create and set proper permissions for the Squid configuration directory:
```bash
sudo mkdir -p /etc/squid
sudo chown root:root /etc/squid
sudo chmod 755 /etc/squid
```

### 4️⃣ Create a User for Proxy Authentication
🔒 Create a user for proxy access using the `htpasswd` command (replace `proxyuser` with your desired username):
```bash
sudo htpasswd -c /etc/squid/passwd proxyuser
```
You will be prompted to set a password for the user.

### 5️⃣ Secure the Password File
🔐 Set appropriate permissions for the password file:
```bash
sudo chmod 640 /etc/squid/passwd
sudo chown proxy:proxy /etc/squid/passwd
```

---

## 🚀 Enable and Start Squid Service
Ensure Squid starts on boot and run the service immediately:
```bash
sudo systemctl enable squid
sudo systemctl start squid
```

---

## 📝 Configure Squid

### 6️⃣ Edit the Squid Configuration File
🛠️ Open the Squid configuration file for editing:
```bash
sudo nano /etc/squid/squid.conf
```

### 7️⃣ Add Authentication Settings
✍️ Add the following content to the configuration file to enable basic authentication:
```text
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Squid Proxy Server
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive off
acl authenticated proxy_auth REQUIRED
```

💾 Save the file and exit the editor (`Ctrl+O`, `Enter`, `Ctrl+X`).

---

## ✅ Final Steps
🔄 Restart Squid to apply the changes:
```bash
sudo systemctl restart squid
```

🎉 Your proxy server is now ready and configured to use basic authentication with the credentials you created.

---

## 🐛 Troubleshooting
- **🔍 Verify Squid Status:**
  ```bash
  sudo systemctl status squid
  ```
  Ensure the service is active and running.

- **📜 Check Logs:**
  Squid logs can be found at:
  - `/var/log/squid/access.log`
  - `/var/log/squid/cache.log` for debugging.

---

## 📌 Notes
- 🛡️ For additional security, consider restricting access to specific IP ranges or adding firewall rules.
- ➕ If you wish to add more users, use:
  ```bash
  sudo htpasswd /etc/squid/passwd <username>
  ```

