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

