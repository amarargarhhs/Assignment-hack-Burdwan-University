## 7. How to Create Remote Desktop

### 1. What is Remote Desktop?

**Remote Desktop** is a Windows feature that allows one computer (the **client**) to connect to and control another computer (the **host/server**) from anywhere, as if sitting directly at it. It uses the **Remote Desktop Protocol (RDP)**, which runs on **TCP port 3389**.

**Uses:** remote administration, tech support, accessing work files from home, running programs on another machine, and system maintenance without physical access.

**Important edition requirement (from Microsoft):**
- **Host (the PC you connect TO):** must run Windows **Professional, Enterprise, or Education** — or Windows Server.
- **Client (the PC you connect FROM):** any edition works, including **Windows Home**.

### 2. Prerequisites

1. The host PC is **powered on**, connected to the network, and **not asleep**.
2. You have **administrator rights** on the host PC.
3. The user account you will connect with has a **password** (accounts with blank passwords cannot connect via RDP).
4. The **Remote Desktop service (TermService)** is running.
5. Both PCs can reach each other on the network (test with `ping`).

### 3. Enable Remote Desktop on the Host PC

**Method A — Via Settings (Windows 10/11) — Recommended:**
1. Press **Windows + I** → **System → Remote Desktop**.
2. Toggle **"Enable Remote Desktop"** to **On** → **Confirm**.
3. *(Optional)* Leave **"Make my PC discoverable on private networks"** checked.

**Method B — Via Control Panel:**
1. Press **Windows + R**, type `sysdm.cpl`, press **Enter** (System Properties).
2. Click the **Remote** tab.
3. Under **Remote Desktop**, select **"Allow remote connections to this computer"**.
4. Keep **"Allow connections only from computers running Remote Desktop with Network Level Authentication (recommended)"** ticked.
5. Click **Apply → OK**.

**Method C — Via Command Prompt (elevated):**
```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```
(Value `0` = enabled; `1` = disabled.)

**Method D — Via PowerShell (elevated):**
```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0
```

### 4. Add Users Who Are Allowed to Connect

By default, only members of the **Administrators** group can connect. To allow a standard user:

**Via Settings:** In **Settings → System → Remote Desktop**, click **Remote Desktop Users** → **Add** → enter the username → **Check Names** → **OK**.

**Via Command Prompt (elevated):**
```
net localgroup "Remote Desktop Users" rahul /add
```

### 5. Configure the Windows Firewall

1. Open **Control Panel → Windows Defender Firewall → Allow an app or feature through Windows Defender Firewall**.
2. Click **Change settings**.
3. Ensure **Remote Desktop** is checked for the **Private** network (and Domain if needed).
4. Click **OK**.

**Verify port 3389 is listening** (on the host, in an elevated Command Prompt):
```
netstat -an | findstr ":3389"
```
You should see `LISTENING` on `0.0.0.0:3389` or `[::]:3389`.

### 6. Find the Host PC's IP Address

On the host PC, run `ipconfig` in Command Prompt. Note the **IPv4 Address** (e.g., `192.168.1.10`). Also note the **computer name** (Settings → System → About) — you can connect using either.

### 7. Connect from the Client PC

**Method A — Remote Desktop Connection app (mstsc):**
1. Press **Windows + R**, type `mstsc`, press **Enter**.
2. In the **Computer** field, enter the host's **IP address or computer name** (e.g., `192.168.1.10`).
3. Click **Connect**.
4. Enter the **username and password** of the account on the host PC (use `.\username` or `COMPUTERNAME\username` for local accounts).
5. Click **Yes** to accept the certificate prompt.
6. You now have a full remote desktop session. Use the top bar to disconnect/fullscreen.

**Method B — Microsoft Remote Desktop app:** Add **PC** → enter the host IP/name → Save → double-click to connect.

**Method C — From another OS/mobile:** install the **Microsoft Remote Desktop** app (Windows, macOS, iOS, Android) and enter the same IP/credentials.

### 8. Accessing from Outside the Local Network (Internet)

| Method | How it works | Security |
|---|---|---|
| **VPN** (recommended) | Connect to the office network via VPN first, then use RDP normally | High |
| **Remote Desktop Gateway** | Publish RDP through an encrypted gateway server | High |
| **Port Forwarding** (last resort) | Forward port 3389 on the router to the host PC's IP | Risky — exposes RDP to the internet |

**Port forwarding steps (if needed):**
1. Log into the router (e.g., `192.168.1.1`).
2. Find **Port Forwarding**.
3. Forward **TCP 3389** to the host PC's local IP (e.g., `192.168.1.10`).
4. Use your **public IP address** (or a DDNS hostname) to connect.

> **Security warning:** Microsoft strongly recommends a VPN or Remote Desktop Gateway instead of directly exposing port 3389, since internet-facing RDP is a frequent target of brute-force attacks.

### 9. Security Best Practices

1. **Use strong passwords** on all accounts allowed to connect.
2. **Limit allowed users** — only add the accounts that genuinely need remote access.
3. **Keep NLA (Network Level Authentication) enabled**.
4. **Never expose port 3389 directly to the internet** without a VPN/Gateway.
5. **Set the host PC network profile to Private**, not Public.
6. **Change the default RDP port (3389)** in the registry if extra security is required (advanced users only).
7. **Keep Windows updated** — RDP vulnerabilities are patched regularly.

### 10. Troubleshooting Common Problems

| Problem | Likely Cause | Solution |
|---|---|---|
| "Remote Desktop can't connect" | Host PC asleep / wrong IP | Wake the PC; re-check the IP address |
| Connection timed out | Firewall blocking port 3389 | Allow Remote Desktop in Firewall; check with `Test-NetConnection IP -Port 3389` |
| "Credentials did not work" | Wrong username/password format | Use `.\username` or `COMPUTERNAME\username`; ensure account has a password |
| NLA error | Account or network issue | Re-enable/disable NLA temporarily; verify account permissions |
| Only admins can connect | User not added | Add the user to **Remote Desktop Users** group |
| Home edition can't host RDP | Edition limitation | Use Windows Pro, or a third-party tool |
| Multiple users kicked out | One RDP session only | Windows client allows only one session; use Windows Server/RDS for many |

### 11. Free Alternatives to Windows Remote Desktop

| Tool | Type | Notes |
|---|---|---|
| **RustDesk** | Open-source self-hosted | Free, works on Home edition |
| **TeamViewer / AnyDesk** | Third-party cloud | Easy setup, free for personal use |
| **Chrome Remote Desktop** | Google, browser-based | Free and simple |
| **VNC (TightVNC/RealVNC)** | Traditional remote control | Good for LAN use |

### Summary Checklist

1. Verify the host runs Windows **Pro/Enterprise/Server** (Home can't host).
2. **Enable Remote Desktop** — Settings → System → Remote Desktop → On.
3. Add the connecting user to **Remote Desktop Users** (if not an admin).
4. Confirm the **Firewall** allows Remote Desktop (port 3389).
5. Note the host's **IP address** (`ipconfig`).
6. From the client: run **`mstsc`** → enter IP → sign in with host credentials.
7. For internet access, use a **VPN or Remote Desktop Gateway** (never bare port forwarding).
