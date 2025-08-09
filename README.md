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
