## 3. Creating a Network Connection Between Two PCs in Windows (Manual/Static IP Configuration)

### 1. Introduction / Concept

Two computers can be networked directly to exchange files, share resources, or play LAN games without an internet connection. This requires three things:
1. A **physical connection** between the two machines.
2. **IP configuration** so each PC has a unique, compatible address (done **manually/static** so the addresses never change).
3. **Name/Workgroup consistency and firewall permissions** so the machines can discover and access each other.

Because there is **no router/DHCP server** in a direct connection, both PCs must be configured with static IP addresses in the **same subnet** — this is the key reason the assignment specifies "(Set IP manually)."

### 2. Physical Connection Setup

| Method | Cable / Device | Notes |
|---|---|---|
| **Direct cable connection** | Ethernet cable connecting the two PCs | Older PCs required a **crossover cable**; modern NICs (Gigabit) have Auto-MDI/X, so a **straight-through** cable works on modern machines |
| **Via a switch** | Two Ethernet cables + a network switch | The most reliable and common method |
| **Via a router** | Two Ethernet cables + a router/AP | Router acts as a switch; works even without internet |
| **Wi-Fi** | Wireless adapter + ad-hoc/hotspot | Less reliable; not recommended for assignments |

**Hardware check:** Both PCs need a functioning **Network Interface Card (NIC)**. Confirm the link by checking the Ethernet port lights are on after plugging in the cable.

### 3. Configuring a Static IP Address on Windows

*Perform these steps on **BOTH** computers, using two different IP addresses. The method below uses the classic Control Panel (works on Windows 7/8/10/11).*

**Step 1: Open Network Adapter Settings** — Press **Windows + R**, type `ncpa.cpl`, and press **Enter** (opens Network Connections directly).

**Step 2: Open the Adapter's Properties** — Right-click the active adapter (**Ethernet**, or **Local Area Connection**) → **Properties**. Click **Continue/Yes** on the UAC prompt (administrator rights required).

**Step 3: Open IPv4 Properties** — Select **Internet Protocol Version 4 (TCP/IPv4)** → click **Properties**.

**Step 4: Enter the Static IP Settings** — Select **"Use the following IP address:"** and enter:

PC 1 (e.g., PC-A):
```
IP address:       192.168.1.10
Subnet mask:      255.255.255.0
Default gateway:  (leave blank, or 192.168.1.1 if a router is present)
```

PC 2 (e.g., PC-B):
```
IP address:       192.168.1.20
Subnet mask:      255.255.255.0
Default gateway:  (leave blank, or 192.168.1.1 if a router is present)
```

**Important rules:**
- Both IPs must be in the **same subnet** (same first three octets `192.168.1.` here) so they can communicate directly.
- The **Subnet mask** must be identical on both (255.255.255.0 = /24).
- The two IP addresses must be **different** — identical addresses cause an **IP conflict**.
- Leave **DNS servers** as "Obtain automatically" or leave blank for a simple two-PC LAN.

**Step 5: Save the Settings** — Click **OK**, then **Close**. The adapter will re-initialize with the new static IP.

**Step 6: Verify the Configuration (on both PCs)** — In Command Prompt run:
```
ipconfig
```
Check that each PC now shows the static IP you assigned, with the correct subnet mask.

### 4. Verifying Connectivity (Ping Test)

From PC 1:
```
ping 192.168.1.20
```
From PC 2:
```
ping 192.168.1.10
```
- **Success:** `Reply from 192.168.1.20: bytes=32 time<1ms TTL=128`.
- **Failure:** `Request timed out` — check the Troubleshooting section.

> **Note:** ICMP (ping) is blocked by default on Windows Firewall's *public* profile — set the network profile to **Private** or allow ping.

### 5. Making Both PCs Discoverable (Workgroup + Network Profile)

1. **Set the same Workgroup on both PCs** — Settings → System → About → Rename this PC (Advanced) → Change → set the **Workgroup** name identically (e.g., `WORKGROUP` or `LABNET`) → restart if prompted.
2. **Set the Network Profile to "Private"** — Settings → Network & Internet → Ethernet → select the connected network → change profile from **Public** to **Private**.
3. **Enable Network Discovery and File and Printer Sharing** — Control Panel → Network and Sharing Center → Change advanced sharing settings → under **Private** profile, turn ON **Network discovery** and **File and printer sharing**.
4. **Allow File and Printer Sharing through Windows Firewall** — Control Panel → Windows Defender Firewall → Allow an app → tick **File and Printer Sharing** for **Private**.

### 6. Sharing Files Between the Two PCs

1. **Create a share on PC 1:** Right-click the folder → **Properties** → **Sharing** tab → **Share…** → select **Everyone** → set **Read/Write** permission → **Share** → note the path `\\PC-1\SharedFolder`.
2. **Access the share from PC 2:** Press **Windows + R** and type `\\192.168.1.10` (or `\\192.168.1.10\SharedFolder`).
3. Sign in with the PC 1 username/password if password-protected sharing is on.
4. **Map a network drive** (right-click This PC → Map network drive) to assign a drive letter (e.g., `Z:`) for easier access.

### 7. Alternative Methods to Set a Static IP

**Via Windows Settings (Windows 11):** Settings → Network & Internet → Ethernet → connection → under IP assignment click **Edit** → dropdown **Manual** → toggle **IPv4** ON → enter IP and **Subnet prefix length** (255.255.255.0 → prefix length **24**) → Save.

**Via PowerShell (administrator):**
```powershell
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.10 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 8.8.8.8
```

**Via Command Prompt (administrator):**
```
netsh interface ip set address name="Ethernet" static 192.168.1.10 255.255.255.0
```

### 8. Troubleshooting Common Issues

| Problem | Cause | Solution |
|---|---|---|
| Ping "Request timed out" | Firewall blocking ICMP, or IPs in different subnets | Allow ICMP / set Private profile; re-check both IPs and masks |
| "General failure" on ping | No default gateway / invalid IP | Verify IP and subnet mask match on both |
| IP conflict warning | Both PCs set to the same IP | Use different IPs (e.g., .10 and .20) |
| PC not visible in Network | Network Discovery off / different workgroups | Enable discovery; set same workgroup; set Private profile |
| Can ping but can't open share | File sharing not enabled / share permissions | Enable File and Printer Sharing; re-share folder; check permissions |
| Access denied | Password-protected sharing with wrong credentials | Use correct account/password, or disable password-protected sharing |

### Summary Checklist

1. Physically connect the two PCs (cable or switch).
2. On **PC 1**: set static IP `192.168.1.10`, mask `255.255.255.0`.
3. On **PC 2**: set static IP `192.168.1.20`, mask `255.255.255.0`.
4. Verify with `ipconfig` on both.
5. Test with `ping` in both directions.
6. Set the same **workgroup** and **Private** network profile on both.
7. Enable **Network Discovery** and **File and Printer Sharing**.
8. Share a folder on one PC and access it from the other via `\\192.168.1.10`.

---
