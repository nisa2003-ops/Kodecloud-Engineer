## Day 1

### Question 1
Create a user named **james** with a non-interactive shell on **App Server 2**.

---

### Solution & Commands

**Connect to App Server 2 via SSH:**
   ```bash
   ssh thor@stapp02
   sudo useradd -s /sbin/nologin james
   grep james /etc/passwd