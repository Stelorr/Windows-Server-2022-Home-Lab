# Windows-Server-2022-Home-Lab
A virtualized corporate network environment featuring Active Directory Domain Services, DNS configuration, and Windows 11 client integration. Used for personal learning.

# Windows Server 2022 Active Directory Home Lab
**Enterprise Infrastructure Simulation & User Lifecycle Management**

![Server Manager](./images/server-manager1.png)
![Server Manager](./images/server-manager2.png)


## 🌐 Project Overview
This project demonstrates the deployment and configuration of a localized Windows Server 2022 environment. The goal was to simulate a corporate infrastructure that manages centralized user authentication, automated resource provisioning, and enforceable security policies.

### 🛠️ Core Technologies
* **Hypervisor:** VMware Fusion (MacOS)
* **Server OS:** Windows Server 2022 (Domain Controller)
* **Client OS:** Windows 11 Enterprise
* **Directory Services:** Active Directory Domain Services (AD DS)
* **Protocol/Policy:** Group Policy Objects (GPO), DNS, SMB/NTFS Permissions

## 🚀 Key Features Implemented
* **Active Directory Architecture:** Established the `lorio.local` domain with custom Organizational Units (OUs) for streamlined user management.

![Active Directory](./images/ad-users-computers.png)

* **Automated Provisioning:** Implemented GPO-driven network drive mapping (S: Drive) for department-wide resource sharing.

![Company Sharing](./images/company-share.png)

* **Security Enforcement:** Configured Interactive Logon Banners via Group Policy to meet corporate legal compliance standards.

![Banner Pop Up](./images/banner-popup.png)

* **Resource Security:** Managed tiered NTFS and Share permissions to ensure data integrity and the principle of least privilege.

## 🛠️ Technical Challenges & Troubleshooting
Building this environment required resolving several real-world configuration hurdles. Below are the key issues encountered and the logic used to resolve them.

### 1. The "Invisible" Local Drive
* **Issue:** During the file sharing phase, the Local Disk (C:) was not appearing in the standard File Explorer view on the Domain Controller.
* **Diagnostic:** Utilized **Disk Management (`diskmgmt.msc`)** to verify the partition status and health. 
* **Resolution:** Identified that the drive was hidden due to VM view settings; forced access via the Disk Management console to initialize the `CompanyShare` directory.

### 2. GPO Drive Mapping Failure (The S: Drive)
* **Issue:** The Windows 11 client successfully joined the domain, but the mapped network drive did not appear in "This PC."
* **Diagnostic:** Used `net share` on the DC to discover the directory lacked an active SMB share pointer, and `gpupdate /force` on the client showed successful policy application but no visual result.
* **Resolution:** * Enabled **Advanced Sharing** and configured **SMB/NTFS permissions** for "Domain Users."
    * Updated the GPO Action from **"Create"** to **"Replace"** to force a refresh of the network drive icon during the user logon sequence.
    * Adjusted **Windows Defender Firewall** rules to permit File and Printer Sharing (SMB-In) traffic.

### 3. Password Synchronization & Reset
* **Issue:** User lockout/credential loss during the initial testing phase.
* **Resolution:** Demonstrated administrative recovery by performing a password reset in **Active Directory Users and Computers (ADUC)** while enforcing the "User must change password at next logon" security standard.

## 📖 How to Replicate
1.  Deploy a Windows Server 2022 VM and promote to Domain Controller.
2.  Configure a static IP and DNS for the `lorio.local` forest.
3.  Deploy a Windows 11 VM and point DNS to the DC's IP.
4.  Join the Client to the domain and verify GPO propagation.
