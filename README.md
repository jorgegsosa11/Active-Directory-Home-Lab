# 🛠️ Active-Directory-Home-Lab

## Essentials
- VirtualBox
- Windows 10 ISO
- Windows Server 2019 ISO

---

## Step 1: Download Tools
1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).  
2. Download [Windows Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019).  
3. Download [Windows 10 ISO](https://www.microsoft.com/software-download/windows10).  


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
5. Set Password Policy
6. Right-click user → Properties > Member Of → Add to Domain Admins.

---

# Active Directory Home Lab Setup (Steps 6–9)

## Step 6: Configure RAS / NAT

1. Open **Server Manager** → **Add Roles and Features**.
2. Select:
   - **Remote Access**
   - Under **Role Services** → **Routing**
3. After installation:
   - Go to **Tools** > **Routing and Remote Access**.
   - Right-click **DC (local)** → **Configure and Enable**.
   - Select **NAT**.
   - Choose **Internet-facing adapter** → **Finish**.

---

## Step 7: Configure DHCP Server

1. Open **Server Manager** → **Add Roles and Features**.
2. Select **DHCP Server** → Install.
3. Open **Tools** > **DHCP**.
4. Right-click **IPv4** → **New Scope**:
   - **Name**: `LabScope`  
   - **Range**: `172.16.0.100 - 172.16.0.200`  
   - **Subnet Mask**: `255.255.255.0`  
   - **Lease Duration**: `8 days`  
   - **Router**: `172.16.0.1`  
   - **DNS**: `mydomain.com`
5. Finish setup.
6. Right-click **DHCP server** → **Authorize** → **Refresh**.

---

## Step 8: Create Users with PowerShell

1. Disable **IE Enhanced Security Configuration** (for ease of download).
2. Download and extract PowerShell scripts [PowerShell scripts (e.g., AD_PS-master)](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbC00c3N6YTRvcVVGckdRZENmdEVJMHlpQ3BYd3xBQ3Jtc0tubC1IQlBzeVpnZUR0Nk9TdzA4VWdzN3JjejJXWDc4TVBXZ1hSZmE2ZkluSG5xZG81RzA1VjNLRFUycFBsaG42QkR6MzJBZWNvZ1pXV1AzVEhES0I1UldJRjZySjJObWlnU3ZmTF9nUFMwaE80UVpoaw&q=https%3A%2F%2Fgithub.com%2Fjoshmadakor1%2FAD_PS%2Farchive%2Frefs%2Fheads%2Fmaster.zip&v=MHsI8hJmggI).
3. Open **PowerShell ISE** as Administrator.
4. If blocked, unblock the script:
   ```powershell
   Unblock-File -Path "C:\Users\user\Desktop\AD_PS-master\1_CREATE_USERS.ps1"
5. Run the script and verify users in Active Directory.

---

## Step 9: Install Windows 10 Client (`client1`)

### 🖥️ Create VM in VirtualBox
- **Memory**: 2 GB RAM  
- **CPU**: 2 CPUs  
- **General > Advanced**:  
  - Enable **Bidirectional Clipboard**  
  - Enable **Drag & Drop**: Bidirectional  
- **Network > Adapter 1**:  
  - Set to **Internal Network**

---

### 🌐 Configure Networking
- Client should receive IP address via **DHCP**
- If DNS issues occur, manually set DNS to:  
  `172.16.0.1`

---

### 🔐 Join the Domain
1. Right-click **Start** → **System** → **Rename this PC**
2. Change name to: `client1`
3. Under **Member of**, select **Domain** and enter:  
   `mydomain.com`
4. Provide credentials of a domain user or admin
5. Restart when prompted
6. Log in using **domain credentials**

---

### ✅ Lab Summary
At this point, your lab setup includes:

| Component | Description |
|----------|-------------|
| **DC** | Windows Server 2019 running AD DS, DHCP, and NAT |
| **client1** | Windows 10 machine joined to the domain |
| **Users** | Created via PowerShell script |
| **Status** | Fully functional AD lab with internet access |

---

## 📌 Credits
This lab setup is inspired by [Joshua Madakor](https://www.youtube.com/watch?v=MHsI8hJmggI).  


