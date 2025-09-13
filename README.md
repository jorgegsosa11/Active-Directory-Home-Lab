# 🛠️ Active-Directory-Home-Lab

## Essentials
- VirtualBox
- Windows 10 ISO
- Windows Server 2019 ISO

---

## Step 1: Download Tools
1. Download and install VirtualBox.
2. Download Windows Server 2019 ISO.
3. Download Windows 10 ISO.

---

## Step 2: Install Windows Server 2019
1. Open VirtualBox → Create New VM.
   - At least **2 GB RAM**.
   - Allocate processor cores depending on your host machine.
2. VM Settings:
   - **General > Advanced** → Clipboard & drag/drop → *Bidirectional*.
   - **System > Processor** → Add cores.
   - **Network > Adapter 1** → NAT.
   - **Network > Adapter 2** → Internal network.
3. Mount the Server 2019 ISO and start installation.

---

## Step 3: Configure Server 2019
1. Choose **Desktop Experience** during install.
2. Set a simple admin password (for lab use).
3. Install **VirtualBox Guest Additions**:
   - `Devices > Insert Guest Additions CD`.
   - Open Explorer → This PC → Run the `amd64` installer.
   - Reboot later, then shut down and restart VM.
4. Configure Networking:
   - **Adapter 1 (NAT)** → Automatic from home router.
   - **Adapter 2 (Internal)** → Manually set:
     ```text
     IP Address: 172.16.0.1
     Subnet Mask: 255.255.255.0
     DNS: 127.0.0.1
     ```
   - Rename NICs for clarity (e.g., *Internet* / *Internal*).
5. Rename PC to **DC** and restart.

---

## Step 4: Install Active Directory Domain Services
1. Open **Server Manager** → Add Roles and Features.
2. Select:
   - **Active Directory Domain Services**
   - **File and Storage Services**
3. After installation, click the flag notification → *Promote this server to a domain controller*.
4. Deployment Configuration:
   - Add a new forest → `mydomain.com`.
   - Set DSRM password (can reuse admin password).
5. Restart when prompted.

---

## Step 5: Create AD Admin User
1. Open **Active Directory Users and Computers**.
2. Navigate to `mydomain.com`.
3. Create a new OU → `Admins`.
4. Inside, create a new user:
   ```text
   Full Name: Jorge Garcia
   Username:  j-garcia
