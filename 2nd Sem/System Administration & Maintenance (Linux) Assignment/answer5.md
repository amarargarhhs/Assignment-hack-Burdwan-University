## 5. Steps of Sharing Printer, Files and Folders Over the Network

### 1. Prerequisites (Common to All Sharing)

1. **All PCs on the same network** — connected via Ethernet switch/router or Wi-Fi (the same subnet).
2. **Network profile set to Private** — Settings → Network & Internet → Ethernet (or Wi-Fi) → select the connection → set **Network profile type** to **Private**. (*Sharing is disabled on the Public profile by default for security.*)
3. **Enable Network Discovery and File and Printer Sharing** — Control Panel → Network and Sharing Center → Change advanced sharing settings → under the **Private** profile, turn ON **Network discovery** and **File and printer sharing**.
4. **Firewall** — Windows Firewall must allow **File and Printer Sharing** (verify under Control Panel → Windows Defender Firewall → Allow an app).
5. **Same Workgroup (optional but recommended)** — Control Panel → System → Change settings → set the same **Workgroup** name on all PCs (default: `WORKGROUP`).
6. **Test basic connectivity** — `ping` each PC by IP address to confirm the network is working.

### 2. Sharing Files and Folders

**Method A — Quick/Simple Sharing (recommended for the assignment):**
1. **Right-click** the folder → **Properties**.
2. Go to the **Sharing** tab → click **Share…**.
3. Select **Everyone** from the dropdown → click **Add**.
4. Set the **Permission level**: **Read** (view only) or **Read/Write** (view, edit, delete).
5. Click **Share**, then note the network path (e.g., `\\PC-1\SharedFolder`).
6. Click **Done**.

**Method B — Advanced Sharing (more control over permissions):**
1. Right-click the folder → **Properties** → **Sharing** tab → **Advanced Sharing…**.
2. Check **"Share this folder"**.
3. Give the share a **Share name** (e.g., `ProjectData`).
4. Click **Permissions** and set who can access it (default is Everyone → Read). Tick **Full Control / Change / Read** as required → **OK**.
5. Click **OK** twice to apply.

**Method C — Share a specific drive:** In File Explorer → This PC, right-click the drive → **Properties → Sharing → Advanced Sharing**, and follow the same steps as Method B. (*Note: sharing full drives is a security risk and usually unnecessary.*)

### 3. Sharing a Printer

*A printer physically connected to one PC (e.g., via USB) can be shared so all network PCs can print to it — this makes the host PC act as a **print server**.*

**Step 1: On the Host PC (the one with the printer attached):**
1. Open **Settings → Bluetooth & devices → Printers & scanners** (Windows 11) or **Control Panel → Devices and Printers** (Windows 10).
2. Select the printer → click **Printer properties**.
3. Open the **Sharing** tab.
4. Click **Change Sharing Options** (if prompted by UAC).
5. Check **"Share this printer"**.
6. Give it a **Share name** (e.g., `HP LaserJet`).
7. *(Optional)* Check **"Render print jobs on client computers"** to reduce load on the host PC.
8. Click **OK** / **Apply**.

**Step 2: Verify the Share Path** — The printer can now be reached at `\\<HostPC-name>\<Share-name>` e.g., `\\DESKTOP-ABC123\HP LaserJet`.

### 4. Accessing Shared Resources from Another PC (Client)

**A) Access a shared folder:**
1. Open **File Explorer**.
2. In the address bar, type the **UNC path**: `\\192.168.1.10` or `\\PC-1` and press **Enter**.
3. Double-click the shared folder.
4. If prompted, enter the host PC's **username and password** (if password-protected sharing is ON).
5. *(Optional)* **Map a network drive:** right-click **This PC** → **Map network drive** → choose a drive letter (e.g., `Z:`) → enter `\\192.168.1.10\SharedFolder` → tick **Reconnect at sign-in** → **Finish**.

**B) Add a shared printer:**
1. On the client PC, open **Settings → Bluetooth & devices → Printers & scanners** (or **Control Panel → Devices and Printers**).
2. Click **Add device** / **Add a printer**.
3. Select **"Add a printer manually"** (if the printer isn't auto-discovered).
4. Choose **"Select a shared printer by name"** and enter: `\\192.168.1.10\HP LaserJet` (use the host's IP if the computer name fails to resolve).
5. Click **Next** → Windows installs the driver → **Finish**.
6. *(Optional)* **Set as default printer** by right-clicking → **Set as default printer**.

**C) Access via Network window:** Open **File Explorer** → click **Network** in the left pane → turn on **Network discovery** if prompted → all PCs with sharing enabled appear; double-click a PC to see its shared folders and printers.

### 5. Controlling Access (Permissions Summary)

| Setting | Effect |
|---|---|
| **Everyone / Read** | Users can open and copy files but not modify them |
| **Everyone / Read & Write** | Users can open, edit, and delete files |
| **Password-protected sharing ON** | Only users with a user account + password on the host PC can connect |
| **Password-protected sharing OFF** | Anyone on the network can access shares (simpler for a lab, less secure) |
| **Share permissions** | Control access at the *share* level (Everyone, specific users) |
| **NTFS permissions** | Control access at the *file/folder* level; the stricter of the two applies |

> **Practical note for assignments/labs:** set **Password protected sharing to OFF** (Control Panel → Network and Sharing Center → Advanced sharing settings → All Networks) for the simplest, password-free access.

### 6. Troubleshooting Common Sharing Problems

| Problem | Likely Cause | Solution |
|---|---|---|
| PC not visible in Network | Network Discovery off / Public profile | Enable discovery; set profile to **Private** |
| "Access is denied" | Password-protected sharing ON, wrong credentials | Enter correct host credentials, or disable password-protected sharing |
| Can ping but can't access share | File sharing not enabled / firewall blocking | Enable File and Printer Sharing; allow it in Firewall |
| Printer not found on client | Share name wrong / host PC off | Check share name `\\IP\ShareName`; keep host PC powered on |
| "Windows cannot connect to printer (0x00000709)" | Incorrect share path / driver issue | Use the correct UNC path; reinstall/update the printer driver |
| Credentials prompt loop | Network profile mismatch | Set both PCs to Private; add host credentials in **Credential Manager** |

### Summary Checklist

1. Set both PCs to a **Private** network profile.
2. Enable **Network discovery** and **File and printer sharing**.
3. (Recommended) Same **workgroup** on all PCs.
4. **Share the folder:** right-click → Properties → Sharing → Share → add **Everyone** → set permissions.
5. **Share the printer:** Printers & scanners → Printer properties → Sharing tab → **Share this printer** → give a share name.
6. **From client PC:** open `\\<Host-IP>` in File Explorer for folders; **Add device** with path `\\<Host-IP>\<ShareName>` for the printer.
7. Map network drives for convenience; set default printer if required.

---
