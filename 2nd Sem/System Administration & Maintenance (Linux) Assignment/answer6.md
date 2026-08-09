## 6. Steps of Creating Users in Windows Environment - Administrator and Regular

### 1. Understanding the Two Account Types

| Feature | Standard (Regular) User | Administrator User |
|---|---|---|
| Install/uninstall software | Not allowed (needs admin approval) | Allowed |
| Change system-wide settings | Not allowed | Allowed |
| Manage other user accounts | Not allowed | Allowed |
| Access other users' files | Restricted | Full access |
| Security level | Lower risk — safe for daily use | Higher risk — use only when needed |
| Group membership | `Users` group | `Administrators` group |

**Key fact:** When you create a new local account in Windows, it is created as a **Standard** account by default. To make it an **Administrator**, you must *promote* it afterward (or add it to the `Administrators` group).

> **Note:** `lusrmgr.msc` (Local Users and Groups) is available on **Windows Pro / Enterprise / Education**, but **not on Windows Home**. On Home editions, use Settings, Command Prompt, or PowerShell instead.

### 2. Method 1 — Create Users via Windows Settings (GUI)

**Step 1:** Press **Windows + I** → **Settings** → **Accounts** → **Family & other users** (Win 11) or **Family & other people** (Win 10).

**Step 2:** Under **Other users**, click **Add account** → **"I don't have this person's sign-in information"** → **"Add a user without a Microsoft account"**.

**Step 3:** Enter the **username** (e.g., `rahul`), then set a **password** and **password hint**. Set up **security questions** and click **Next**. The new **Standard** account is now created.

**Step 4 (promote to Administrator if required):** In **Settings → Accounts → Other users**, click the new account → **Change account type** → select **Administrator** → **OK**.

### 3. Method 2 — Create Users via Computer Management (lusrmgr.msc)

*Recommended for a server/lab environment and available on Windows Pro/Enterprise.*

**Step 1:** Press **Windows + X** → **Computer Management** → expand **System Tools → Local Users and Groups → Users**.

**Step 2:** **Right-click** the **Users** folder → **New User…** → fill in: **User name**, **Full name / Description**, **Password**, and password options:
- **User must change password at next logon** (recommended for new users)
- **User cannot change password**
- **Password never expires** (often ticked in labs)
- **Account is disabled** (leave unticked)

Click **Create**, then **Close**. The account is created as a **Standard** user.

**Step 3 (make it an Administrator):** Right-click the new user → **Properties** → **Member Of** tab → **Add…** → type `Administrators` → **Check Names** → **OK** → **Apply** → **OK**.

### 4. Method 3 — Create Users via the User Accounts Dialog (netplwiz)

**Step 1:** Press **Windows + R**, type `netplwiz`, press **Enter**.

**Step 2:** Click **Add…** → choose **"Sign in without a Microsoft account"** → **Local account** → enter **user name**, **password**, and **password hint** → **Next** → **Finish**.

**Step 3:** Select the new user → **Properties** → **Group Membership** tab → choose **Administrator** (or **Standard user**) → **Apply** → **OK**.

### 5. Method 4 — Create Users via Command Prompt (net user)

*Fastest method for labs; requires an **elevated** Command Prompt (Run as administrator).*

**Create a Regular (Standard) User:**
```
net user rahul password123 /add
```

**Create an Administrator User:**
```
net user adminuser admin@123 /add
net localgroup administrators adminuser /add
```

**Verify the Account:**
```
net user rahul
```

**Other useful `net user` options:**
| Command | Function |
|---|---|
| `net user` | List all local users |
| `net user username /delete` | Delete a user |
| `net user username /active:yes` | Enable/disable an account |
| `net user username password` | Change a user's password |

### 6. Method 5 — Create Users via PowerShell

*Recommended for automation; requires an **elevated** PowerShell session.*

**Create a Regular (Standard) User:**
```powershell
$Password = ConvertTo-SecureString "password123" -AsPlainText -Force
New-LocalUser -Name "rahul" -Password $Password -FullName "Rahul Sharma" -Description "Regular user"
```

**Create an Administrator User:**
```powershell
$Password = ConvertTo-SecureString "admin@123" -AsPlainText -Force
New-LocalUser -Name "adminuser" -Password $Password -FullName "Admin User" -Description "Administrator"
Add-LocalGroupMember -Group "Administrators" -Member "adminuser"
```

**Verify:**
```powershell
Get-LocalUser
Get-LocalGroupMember -Group "Administrators"
```

### 7. Summary — Creating Admin vs Regular Users

| Task | GUI (Settings) | Command (cmd) | PowerShell |
|---|---|---|---|
| Create regular user | Settings → Accounts → Other users → Add | `net user name pass /add` | `New-LocalUser -Name name -Password $pw` |
| Create admin user | Settings → Other users → Change account type → Administrator | `net localgroup administrators name /add` | `Add-LocalGroupMember -Group "Administrators" -Member name` |
| Default type on creation | **Standard** | **Standard** | **Standard** |

### 8. Security Best Practices

1. **Use Standard accounts for daily work** — Principle of Least Privilege.
2. **Use strong passwords** — mixed case, numbers, symbols; enforce complexity and expiry via Local Security Policy (`secpol.msc`).
3. **Require password change at first logon**.
4. **Limit the number of admin accounts**.
5. **Disable the built-in Administrator** account when not needed — `net user administrator /active:no`.
6. **Audit account activity** — Event Viewer → Windows Logs → Security (Event IDs 4720/4722/4724).

### 9. Troubleshooting Common Issues

| Problem | Likely Cause | Solution |
|---|---|---|
| `lusrmgr.msc` won't open | Windows Home edition | Use Settings, `net user`, or PowerShell instead |
| "Access is denied" | Command not run as administrator | Reopen Command Prompt/PowerShell as Administrator |
| "The account already exists" | Duplicate username | Choose a different name or delete the old account |
| Password complexity error | Password doesn't meet policy | Use a stronger password (≥8 chars, mixed case + numbers/symbols) |
| New user can't log in | Account disabled or password expired | `net user name /active:yes`; check password policy |
| Can't install software on new account | Account is Standard, not Admin | Promote via Change account type / add to Administrators group |

---
