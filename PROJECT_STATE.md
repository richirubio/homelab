# Project State

## Current Version
v0.7

## Current Module
Group Policy - Microsoft Defender

## Lab Domain
lab.richardrubio.com

## Virtual Machines
- DC01
- LAB-ADMIN

## Current Checkpoints
- DC01: v0.5 - Local Administrator GPO
- LAB-ADMIN: v0.5 - Local Administrator GPO

## Completed
- Windows Server 2025 installed on DC01.
- DC01 promoted to Domain Controller.
- Forest created: lab.richardrubio.com.
- Active Directory-integrated DNS configured.
- Initial OU structure created.
- Domain users created:
  - richard.rubio
  - richard.admin
- Domain group created:
  - GG-Workstation-Admins
- richard.admin added to GG-Workstation-Admins.
- LAB-ADMIN joined to the domain.
- LAB-ADMIN moved to the Workstations OU.
- LAB-ADMIN configured to use DC01 as primary DNS.
- RSAT installed on LAB-ADMIN.
- Remote administration verified from LAB-ADMIN.
- First GPO created:
  - GPO - Local Administrators - Workstations
- GPO linked to the Workstations OU.
- GG-Workstation-Admins configured as local Administrators on domain workstations.
- GPO successfully applied.
- Verified that richard.admin is local administrator on LAB-ADMIN.
- GPO created:
  - GPO - Lock Screen - Workstations
- Corporate resources created:
  - \\DC01\NETLOGON\Corporate\
  - \\DC01\NETLOGON\Corporate\LockScreen\
- Corporate lock screen configured through Group Policy.
- Users prevented from changing the corporate lock screen.
- Lock screen tips and suggestions disabled.
- Group Policy Search adopted as the standard method for locating Administrative Template policies.
- GPO created:
  - GPO - Microsoft Defender - Workstations
- GPO linked to the Workstations OU.
- Microsoft Defender Real-time Protection baseline configured.

## Next Goal
Continue configuring Microsoft Defender through Group Policy.

## Methodology
- One step at a time.
- Explain the why before the how.
- Use concise mental models.
- Avoid unnecessary detail.
- Do not repeat concepts already understood unless needed.
- Work like a professional Microsoft systems administrator.
- Design every solution with enterprise scalability in mind, even when implementing it in a small lab.
- Standard Group Policy workflow:
  1. Search the policy using Group Policy Search.
  2. Read the Microsoft documentation.
  3. Configure the policy.
  4. Validate with gpresult.
  5. Validate the functional behavior.
- One GPO = One functional purpose.