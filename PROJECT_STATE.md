# Project State

## Current Version
v0.5

## Current Module
Active Directory

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

## Next Goal
Learn Group Policy processing and create additional administrative GPOs for domain workstations.

## Methodology
- One step at a time.
- Explain the why before the how.
- Use concise mental models.
- Avoid unnecessary detail.
- Do not repeat concepts already understood unless needed.
- Work like a professional Microsoft systems administrator.