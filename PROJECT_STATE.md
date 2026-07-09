# Project State

## Current Version
v0.8

## Current Module
Microsoft Entra Foundation

## Project Goal
Build a Microsoft administrator HomeLab aligned with Microsoft Learn and MD-102.

Main priorities:
1. Get ready for a Microsoft Systems Administrator role.
2. Prepare official Microsoft certifications.
3. Build practical experience with AD DS, Microsoft Entra ID, Intune, Microsoft 365, PowerShell and hybrid identity.

## Current Learning Path
MD-102 - Explore endpoint management

Current status:
- Microsoft Learn remains the main guide.
- The MD-102 course is temporarily paused to make Microsoft Entra concepts visible in the real portal.
- This pause is limited to Microsoft Entra ID, users, licenses and hybrid identity preparation.
- After validating the hybrid identity foundation, return to Microsoft Learn.

## Lab Domain
lab.richardrubio.com

## Microsoft 365 Tenant
Tenant:
- richardrubiolab.onmicrosoft.com

Subscription:
- Microsoft 365 Business Premium trial

Cloud administrator:
- richard.admin@richardrubiolab.onmicrosoft.com
- Purpose: tenant administration / break-glass account
- This account is cloud-only and must not be synchronized from AD DS.

## Virtual Machines
- DC01
- LAB-ADMIN

## Current Checkpoints
- DC01: v0.7 - Microsoft Defender Real-time Protection
- LAB-ADMIN: v0.7 - Microsoft Defender Real-time Protection

## Active Directory Structure

Domain:
- lab.richardrubio.com

Known OUs:
- Admin
- Employees
- Groups
- Workstations

Known AD users:
- richard.rubio
  - Purpose: daily user / future daily admin account
- richard.admin
  - Purpose: domain administration account
  - Current known location: Employees OU
  - Important: do not synchronize this account to Microsoft Entra ID

Known AD group:
- GG-Workstation-Admins
  - Purpose: grants local administrator rights on domain workstations
  - Member: richard.admin

Known workstation:
- LAB-ADMIN
  - Joined to lab.richardrubio.com
  - Located in Workstations OU
  - Uses DC01 as primary DNS
  - RSAT installed

## Microsoft Entra Current State

Cloud-only users:
- richard.admin@richardrubiolab.onmicrosoft.com
  - Global Administrator
  - Tenant / emergency account

Deleted test users:
- ana.lopez@richardrubiolab.onmicrosoft.com
  - Created temporarily to validate cloud user creation, licensing, onboarding and Exchange Online provisioning
  - Deleted to keep the future hybrid identity model clean

## Hybrid Identity Decision

Target architecture:
- AD DS is the source of truth for employee identities.
- Microsoft Entra ID receives employee identities through Microsoft Entra Connect.
- richard.admin cloud-only remains the tenant break-glass administrator.
- richard.admin from AD DS must not be synchronized.
- Employee and operational users should be created in AD DS first, then synchronized to Microsoft Entra ID.

Target AD users before Microsoft Entra Connect:
- richard.rubio
  - Daily administrator / systems administrator
- helpdesk
  - Support user
- ana.lopez
  - HR user
- pedro.garcia
  - Management user

Target synchronization result:
- richard.rubio -> synchronized to Entra
- helpdesk -> synchronized to Entra
- ana.lopez -> synchronized to Entra
- pedro.garcia -> synchronized to Entra
- richard.admin from AD DS -> excluded from synchronization

## Completed

### Base Infrastructure
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

### Group Policy
- GPO created:
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
- Microsoft Defender Antivirus configured:
  - Remediation
    - Scheduled Full Scan
    - Brute-Force Protection
    - Remote Encryption Protection

### Microsoft Entra Foundation
- Microsoft 365 Business Premium trial tenant created.
- Microsoft Entra Admin Center accessed.
- Microsoft 365 Admin Center accessed.
- Tenant language/interface changed toward English.
- Microsoft 365 Business Premium licensing explored.
- Cloud onboarding workflow validated with temporary user ana.lopez.
- Exchange Online provisioning validated.
- Hybrid identity approach defined.

## Next Goal
Prepare AD DS users and deploy Microsoft Entra Connect.

Immediate next steps:
1. Verify current OU/user structure in AD DS.
2. Create missing AD users:
   - helpdesk
   - ana.lopez
   - pedro.garcia
3. Prepare synchronization scope.
4. Install Microsoft Entra Connect.
5. Synchronize selected AD DS users to Microsoft Entra ID.
6. Verify that synchronized users show On-premises sync = Yes.
7. Assign licenses and roles after synchronization.

## Methodology
- Microsoft Learn is the main guide.
- The HomeLab exists to make Microsoft Learn practical.
- Pause the course only when a concept needs to be seen in the real environment.
- Return to Microsoft Learn after the practical validation.
- One step at a time.
- Explain the why before the how.
- Use concise mental models.
- Avoid unnecessary detail.
- Do not repeat concepts already understood unless needed.
- Work like a professional Microsoft systems administrator.
- Design only what is useful for work, certification or real administration decisions.

## Documentation Rules
- PROJECT_STATE.md is the source of truth for the current HomeLab state.
- CHANGELOG.md records what changed between versions.
- Documentation must help future troubleshooting and decision-making.
- Do not document obvious facts unless they help avoid confusion later.
- Comments and notes should explain decisions, not repeat what a setting does.

## Standard Group Policy Workflow
1. Search the policy using Group Policy Search.
2. Read the Microsoft documentation.
3. Configure the policy.
4. Validate with gpresult.
5. Validate the functional behavior.

## Design Rules
- One GPO = One functional purpose.
- AD DS remains the source of employee identities in the hybrid model.
- Cloud-only tenant admin account must remain available for emergency access.
- Do not synchronize privileged AD DS admin accounts unless there is a specific reason.