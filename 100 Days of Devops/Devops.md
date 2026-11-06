### Task 1
Create a user named **james** with a non-interactive shell on **App Server 2**.

---

### Solution & Commands

**Connect to App Server 2 via SSH:**
   ```bash
   ssh thor@stapp02
   sudo useradd -s /sbin/nologin james
   grep james /etc/passwd
   ```
### Task 2
As part of the temporary assignment to the Nautilus project, a developer named siva requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:


Create a user named siva on App Server 1 in Stratos Datacenter. Set the expiry date to 2027-01-28, ensuring the user is created in lowercase as per standard protocol.

---

### Solution & Commands

**Connect to App Server 2 via SSH:**
   ```bash
   ssh tony@stapp01
   sudo useradd -e 2027-01-28 siva
   sudo chage -l siva
```

### Task 3
Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.
Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter
---

### Solution & Commands

**Connect to App Server 2 via SSH:**
   ```bash
   ssh tony@stapp01
   sudo useradd -e 2027-01-28 siva
   sudo chage -l siva
```

### Task 3
Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.
Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter
---

### Solution & Commands

**Login into each app server**

```bash
   sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config
   sudo systemctl restart sshd
```

### Task 4
In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named xfusioncorp.sh. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 1 within the Stratos Datacenter.

Your task is to grant executable permissions to the /tmp/xfusioncorp.sh script on App Server 1. Additionally, ensure that all users have the capability to execute it (any user must be able to run it directly, for example /tmp/xfusioncorp.sh).

---

### Solution & Commands

```bash
   ssh tony@stapp01
   sudo chmod +x /tmp/xfusioncorp.sh
   sudo chmod a+x /tmp/xfusioncorp.sh
   sudo chmod 755 /tmp/xfusioncorp.sh
```

