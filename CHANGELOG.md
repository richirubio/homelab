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