# Azure Hybrid Cybersecurity Lab

## 1. Overview
This project is a hands-on **Hybrid Cloud Cybersecurity Lab** built on Microsoft Azure to simulate enterprise-grade identity, network, and security operations.  
The lab will serve as a platform to practice:
- Active Directory setup and management
- Network segmentation and firewall rule configuration
- SIEM integration with Azure Sentinel & Splunk
- Identity Governance with SailPoint
- Vulnerability scanning and security monitoring

---

## 2. Lab Architecture (as of 08-Aug-2025)

**Region**: West US 2  
**Resource Group**: `rg-csc-lab-westus2`  

### Components Created Today:
| Resource Name | Type | Location | Notes |
|---------------|------|----------|-------|
| vnet-csc | Virtual Network | West US 2 | CIDR: 10.0.0.0/16 |
| subnet-dc | Subnet | West US 2 | CIDR: 10.0.1.0/24 |
| nsg-dc | Network Security Group | West US 2 | Inbound RDP (3389) allowed |
| DC01-ip | Public IP Address | West US 2 | Static |
| dc01635 | Network Interface | West US 2 | Linked to DC01 |
| DC01 | Virtual Machine (Windows Server) | West US 2 | Planned Domain Controller |

---

## 3. Network Diagram
*(Diagram Placeholder — to be added later)*

Example layout:

Internet
|
Public IP (DC01-ip)
|
NSG (nsg-dc)
|
Subnet (subnet-dc) — 10.0.1.0/24
|
Virtual Machine (DC01)


---

## 4. Step-by-Step Progress (08-Aug-2025)

1. Created **Resource Group** `rg-csc-lab` in East US *(later recreated in West US 2 for regional consistency)*.
2. Created **Virtual Network** `vnet-csc` with address space `10.0.0.0/16`.
3. Added **Subnet** `subnet-dc` with `10.0.1.0/24` CIDR.
4. Configured **Network Security Group** `nsg-dc`:
   - Allowed inbound RDP (TCP 3389) from my IP.
5. Created **Public IP** `DC01-ip` (Static).
6. Created **NIC** `dc01635` linked to `subnet-dc` & `nsg-dc`.
7. Deployed **Windows Server VM** `DC01` in West US 2:
   - Size optimized for Azure Education credits.
   - Monitoring features disabled to reduce cost.
8. Deleted unused **East US** resources after migration.

### **Update – 10 Aug 2025**

1. Created **Organizational Units (OUs)** for better AD structure:
  - `_Servers`
  - `_Workstations`
  - `_Users`
    - `Admins`
    - `StandardUsers`
2. Moved **CLIENT01** into `_Workstations` OU.
3. Created test domain user **Harry Potter** in `StandardUsers`.
4. Attempted **GPO deployment** for desktop wallpaper *(requires shared UNC path for completion)*.
5. Noted RDP restriction for new users — pending group membership fix.

---

## 5. Configuration Notes
- **Security Type**: Standard
- **Public IP**: Static for consistent RDP connection
- **Monitoring**: All optional monitoring disabled to preserve credits
- **Cost Optimization**: Selected lowest viable VM size for DC role

---

## 6. Next Steps
- Promote `DC01` to Domain Controller and configure Active Directory.
- Add a client VM to join the domain.
- Set up Azure Sentinel for SIEM.
- Integrate Splunk for log management.
- Deploy SailPoint for Identity Governance testing.
- Implement vulnerability scanning tools (OpenVAS, Nessus, etc.).

---
## 7. 📂 Case Studies
- [GPO Desktop Wallpaper Deployment – Case Study]()
---

## 8. Screenshots

1. **Search - Resource Group**  
    ![Search Resource Group](images/Search%20for%20Resource%20Group%20and%20create.png)
   
2. **Create Resource Group - East US**  
   ![Create Resource Group - East US](images/01-create-resource-group-eastus.png)

3. **Search - Virtual Network**  
    ![Search Virtual Network](images/search%20for%20Virtual%20network%20and%20create.png)
    
4. **Create Virtual Network - West US 2**  
   ![Create Virtual Network - West US 2](images/02-create-virtual-network-westus2.png)

5. **Search - Network Security Group**  
    ![Search NSG](images/search%20for%20Network%20Security%20Group%20and%20create.png)
    
6. **Create Network Security Group - West US 2**  
   ![Create NSG - West US 2](images/03-create-network-security-group-westus2.png)

7.**Add Inbound Rules to NSG**  
   ![NSG Inbound Rules](images/03-a-nsg-adding-inbound-rules.png)
   
8.**Search - Virtual Machine**  
    ![Search Virtual Machine](images/search%20for%20Virtual%20Machine%20and%20create.png)
    
9.  **VM Basics Tab (Part 1)**  
   ![VM Basics Tab](images/04-vm-basics-tab.png)

10. **VM Basics Tab (Part 2)**  
   ![VM Basics Tab 2](images/04-vm-basics-tab(2).png)

11. **VM Networking Tab**  
   ![VM Networking Tab](images/05-vm-networkingtab.png)

12. **VM Management Tab**  
   ![VM Management Tab](images/06-vm-management-tab.png)

13. **VM Monitoring Tab**  
   ![VM Monitoring Tab](images/07-vm-monitoring-tab.png)

14. **VM Advanced Tab**  
    ![VM Advanced Tab](images/08-vm-advanced-tab.png)

15. **Resource Group - Move Options**  
    ![Resource Group Move Options](images/10-resource-group-move-options.png)

16. **Moving Resources to New Resource Group**  
    ![Moving Resources](images/12-moving-resources-to-new-rg.png)

17. **Delete Old Resource Group - East US**  
    ![Delete Old Resource Group](images/13-delete-old-resource-group-eastus.png)

18. **GitHub Repo Creation**  
    ![GitHub Repo Creation](images/14-github-repo-create.png)

19. **GitHub Images Folder**  
    ![GitHub Folder Images](images/15-github-folder-images.png)

---

## 8. Credits
Built by **Venkat Maddala** as part of a personal cybersecurity skills development lab.  
This project is continuously evolving to replicate **real-world enterprise hybrid cloud environments**.

