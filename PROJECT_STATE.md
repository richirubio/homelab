# Project State

## Current Version
v0.4

## Current Module
Active Directory

## Lab Domain
lab.richardrubio.com

## Virtual Machines
- DC01
- LAB-ADMIN

## Current Checkpoints
- DC01: v0.2 - Active Directory Baseline
- LAB-ADMIN: v0.4 - Remote Administration

## Completed
- Windows Server 2025 installed on DC01.
- DC01 promoted to Domain Controller.
- Forest created: lab.richardrubio.com.
- Active Directory-integrated DNS configured.
- Initial OU structure created.
- Domain users created:
  - richard.rubio
  - richard.admin
- LAB-ADMIN renamed and joined to the domain.
- LAB-ADMIN configured to use DC01 as primary DNS.
- RSAT installed on LAB-ADMIN.
- Remote administration verified from LAB-ADMIN.
- Group created:
  - GG-Workstation-Admins
- richard.admin added to GG-Workstation-Admins.

## Next Goal
Create the first GPO to make GG-Workstation-Admins local administrators on domain workstations.

## Methodology
- One step at a time.
- Explain the why before the how.
- Use concise mental models.
- Avoid unnecessary detail.
- Do not repeat concepts already understood unless needed.
- Work like a professional Microsoft systems administrator.