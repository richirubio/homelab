# Changelog

## v0.2 - Active Directory Baseline

### Added
- Installed Windows Server 2025 on DC01.
- Promoted DC01 to Domain Controller.
- Created the forest lab.richardrubio.com.
- Created the initial OU structure.
- Created the first domain user.

### Hyper-V Checkpoints
- DC01: v0.2 - Active Directory Baseline

## v0.3 - Domain Joined

### Added
- Renamed workstation to LAB-ADMIN.
- Joined LAB-ADMIN to the lab.richardrubio.com domain.
- Verified domain authentication with LAB\richard.rubio.
- Configured LAB-ADMIN to use DC01 as its DNS server.
- Configured Microsoft Defender Antivirus Remediation.
- Configured Scheduled Full Scan baseline.
- Configured Brute-Force Protection baseline.
- Configured Remote Encryption Protection baseline.

### Hyper-V Checkpoints
- LAB-ADMIN: v0.3 - Domain Joined

## v0.4 - Remote Administration

### Added
- Installed RSAT administration tools.
- Installed Active Directory administration tools.
- Installed DNS management tools.
- Installed Group Policy Management.
- Verified remote administration from LAB-ADMIN.

### Hyper-V Checkpoints
- LAB-ADMIN: v0.4 - Remote Administration
  
## v0.5 - Local Administrator Baseline

### Added
- Created the Local Administrators workstation GPO.
- Linked the GPO to the Workstations OU.
- Configured GG-Workstation-Admins as local Administrators on domain workstations.
- Verified successful Group Policy deployment.

### Hyper-V Checkpoints
- DC01: v0.5 - Local Administrator GPO
- LAB-ADMIN: v0.5 - Local Administrator GPO

## v0.6 - Corporate Lock Screen

### Added
- Created the corporate Lock Screen GPO.
- Created the corporate NETLOGON resource structure.
- Configured the corporate lock screen.
- Prevented users from changing the corporate lock screen.
- Disabled Windows lock screen tips.
- Adopted Group Policy Search as the standard policy discovery workflow.

### Hyper-V Checkpoints
- DC01: v0.6 - Corporate Lock Screen
- LAB-ADMIN: v0.6 - Corporate Lock Screen

## v0.7 - Microsoft Defender Real-time Protection

### Added
- Created the Microsoft Defender workstation GPO.
- Configured the Microsoft Defender Real-time Protection baseline.
- Standardized real-time protection settings.
- Standardized local override behavior.

### Hyper-V Checkpoints
- DC01: v0.7 - Microsoft Defender Real-time Protection
- LAB-ADMIN: v0.7 - Microsoft Defender Real-time Protection


## v0.8 - Microsoft Entra Foundation

### Added
- Created the Microsoft 365 Business Premium trial tenant.
- Accessed the Microsoft 365 Admin Center.
- Accessed the Microsoft Entra Admin Center.
- Validated Microsoft 365 Business Premium licensing.
- Created a temporary cloud user to validate onboarding.
- Assigned a Microsoft 365 Business Premium license to the temporary user.
- Validated first sign-in and Exchange Online mailbox provisioning.
- Deleted the temporary cloud user to keep the future hybrid identity model clean.
- Defined the initial hybrid identity approach:
  - AD DS will be the source of employee identities.
  - Microsoft Entra ID will receive users through Microsoft Entra Connect.
  - The cloud richard.admin account will remain tenant-only.
  - The AD DS richard.admin account will not be synchronized.

### Next
- Verify current AD DS OU and user structure.
- Create missing AD DS users.
- Prepare Microsoft Entra Connect.
- Synchronize selected AD DS users to Microsoft Entra ID.