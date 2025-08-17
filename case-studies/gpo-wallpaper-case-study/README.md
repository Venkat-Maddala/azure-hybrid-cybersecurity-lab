# Group Policy Desktop Wallpaper Deployment – Case Study

## Overview
Deploy a corporate desktop wallpaper (`corp.bmp`) to domain-joined machines in the **Azure Hybrid Cybersecurity Lab**, and document the troubleshooting path from “not visible” to “working” — including the final root cause.

---

## Environment
- **Domain Controller:** DC01 (Windows Server 2022)
- **Client:** CLIENT01 (Windows 10)
- **Domain:** `csc-lab.local`
- **OU Structure:** `_Workstations`, `_Users` (`Admins`, `StandardUsers`)

---

## Objective
Apply a `.bmp` corporate wallpaper to users logging into workstations in the `_Workstations` OU via **Group Policy**.

---

## Symptoms
1. Desktop Wallpaper policy exists under **User Configuration** (not **Computer Configuration**).
2. `gpresult` showed the GPO as **Applied**, but the desktop remained **black**.
3. Clients initially failed to read the wallpaper when referenced over `\\domain\SYSVOL`.
4. During RDP sessions, wallpaper never displayed.

---

## Root Cause
The GPO was applying correctly, but **RDP client experience settings** were configured to **suppress desktop backgrounds** to save bandwidth.  
Once **Desktop background** was enabled in the RDP client, the wallpaper appeared immediately.

> The other fixes (SYSVOL path, permissions, loopback) were necessary for a correct setup, but the **decisive fix** was enabling the **RDP client** to render the background.

---

## Implementation & Fix

### 1) Place wallpaper in SYSVOL (domain-readable)
Use SYSVOL so all domain clients can read the file without custom shares:
```
\\dc01.csc-lab.local\SYSVOL\csc-lab.local\scripts\wallpapers\corp.bmp
```

**Permissions**
- **Share:** Domain Users → Read, Domain Computers → Read
- **NTFS:** Authenticated Users → Read & Execute

---

### 2) Configure the GPO
- **GPO Name:** `Workstation-Desktop-Wallpaper`
- **Link:** `_Workstations` OU
- **Loopback Processing:** Enabled (Merge) so user setting follows the workstation OU during testing.

**Policy path**
```
User Configuration →
Administrative Templates →
Desktop →
Desktop →
Desktop Wallpaper
```

**Values**
- **Wallpaper name:** `\\dc01.csc-lab.local\SYSVOL\csc-lab.local\scripts\wallpapers\corp.bmp`
- **Wallpaper style:** Fill

---

### 3) Allow wallpaper in RDP (both server-side and client-side)

**Server-side policy (optional hardening)**
```
Computer Configuration →
Administrative Templates →
Windows Components →
Remote Desktop Services →
Remote Desktop Session Host →
Remote Session Environment
```
- **Enforce removal of background images** → **Disabled**

**RDP client setting (decisive fix)**
- Open **Remote Desktop Connection (mstsc.exe)** → **Show Options** → **Experience** → check **Desktop background**.

---

### 4) Apply & verify
On DC01 and CLIENT01:
```powershell
gpupdate /force
```

Sign out and sign back in on CLIENT01.

Verify application:
```powershell
gpresult /h C:\gp.html
start C:\gp.html
```
Confirm the **Desktop Wallpaper** user policy is listed under **Applied GPOs** and that the wallpaper displays in both local and RDP sessions.

---

## Resolution Checklist
- [x] Wallpaper stored in SYSVOL with correct path
- [x] Share + NTFS permissions grant read access (Users, Computers/Authenticated Users)
- [x] GPO linked to `_Workstations`; scope and filtering correct
- [x] Loopback Processing (Merge) enabled for testing scenario
- [x] Server-side RDP policy **does not** enforce background removal
- [x] **RDP client** Experience → **Desktop background** enabled
- [x] `gpupdate /force` run; logoff/logon; verified with `gpresult`

---

## Key Learnings
- **GPO success ≠ visible UI**: client rendering (RDP Experience) can hide results.
- **SYSVOL** is the standard distribution point for GPO assets; keep sensitive data out.
- **Loopback** (Merge/Replace) lets user policies follow the computer’s OU — useful in labs.
- Effective troubleshooting means validating **scope**, **ACLs**, **policy processing**, **event logs**, and **client display settings**.

---

## Folder Structure
```
/Labs/GPO-Wallpaper-CaseStudy/
  README.md
  /screenshots
```
---

## MindMap - Troubleshooting

```mermaid
flowchart TD
    A[Start: Wallpaper GPO Issue] --> B[Check GPO Applied (gpresult)]
    B --> C[Verify UNC Path Accessible]
    C --> D[Check File Format (BMP/JPG)]
    D --> E[Check Permissions (Share & NTFS)]
    E --> F[Verify Loopback Processing (Merge)]
    F --> G[Check SYSVOL Path Exists]
    G --> H[Check Event Logs (Userenv/GP)]
    H --> I[Registry Verification (HKCU > Policies > System)]
    I --> J{RDP Supports Background?}
    J -- No --> K[Enable Desktop Background in RDP (Experience Tab)]
    J -- Yes --> L[Resolution: Wallpaper Applied]
    K --> L
```

## Screenshots

**GPMC console**  
![GPMC](./Screenshots/GPMC.png)

**GPMC Editor**  
![GPMC Editor](./Screenshots/GPMCeditor.png)

**Applied GPOs report**  
![GPO Report – Applied GPOs](./Screenshots/GPO-Report-AppliedGPOs.png)

**RDP Experience tab – Desktop background enabled**  
![RDP Experience Tab](./Screenshots/RDP-exp-tab.png)

**RDP session display settings**  
![VM via RDP – Display Settings](./Screenshots/VMviaRDP-Display-Settings.png)

**Event Viewer (DC01)**  
![Event Viewer – DC01](./Screenshots/eventviewer-DC01.png)


