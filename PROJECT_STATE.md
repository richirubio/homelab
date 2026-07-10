# Project State

## Current Version
v0.9

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
- The MD-102 course is temporarily paused to validate Microsoft Entra ID and hybrid identity in the HomeLab.
- Microsoft Entra Connect has been deployed and the initial synchronization has completed successfully.
- Remaining hybrid identity checks must be completed before returning to Microsoft Learn.

## Identity Architecture

Active Directory forest and domain:
- lab.richardrubio.com

User sign-in UPN:
- lab.home-hub.es

Microsoft Entra tenant:
- richardrubiolab.onmicrosoft.com

Microsoft 365 primary domain:
- lab.home-hub.es

DNS management:
- home-hub.es is managed through Cloudflare.
- Microsoft 365 DNS records for lab.home-hub.es are managed through Cloudflare.

Architecture decision:
- The existing AD DS forest remains lab.richardrubio.com.
- The forest is not renamed.
- AD DS users use @lab.home-hub.es as their sign-in UPN.
- AD DS remains the source of truth for employee identities.
- Microsoft Entra ID receives selected identities through Microsoft Entra Connect.

## Microsoft 365 Tenant

Tenant:
- richardrubiolab.onmicrosoft.com

Primary domain:
- lab.home-hub.es

Subscription:
- Microsoft 365 Business Premium trial

Cloud administrator:
- richard.admin@richardrubiolab.onmicrosoft.com
- Display name: Richard Berriel
- Role: Global Administrator
- Purpose: tenant administration / emergency account
- Source: cloud-only
- This account must not be synchronized from AD DS.

## Virtual Machines
- DC01
- LAB-ADMIN

## Current Checkpoints
- DC01: v0.7 - Microsoft Defender Real-time Protection
- LAB-ADMIN: v0.7 - Microsoft Defender Real-time Protection

## Active Directory Structure

Domain:
- lab.richardrubio.com

Alternative UPN suffix:
- lab.home-hub.es

OUs:
- Admin
- Employees
- Groups
- Workstations

### Admin OU

Users:
- richard.admin
  - UPN: richard.admin@lab.home-hub.es
  - Purpose: AD DS administration
  - Member of:
    - GG-Workstation-Admins
    - Domain Admins
    - Enterprise Admins
  - Excluded from Microsoft Entra synchronization

### Employees OU

Users:
- richard.rubio
  - UPN: richard.rubio@lab.home-hub.es
  - Purpose: daily user / systems administrator identity
  - Synchronized to Microsoft Entra ID

- richard.helpdesk
  - UPN: richard.helpdesk@lab.home-hub.es
  - Purpose: future helpdesk and delegated administration practice
  - Currently a standard AD DS user
  - Synchronized to Microsoft Entra ID

- ana.lopez
  - UPN: ana.lopez@lab.home-hub.es
  - Purpose: HR user
  - Synchronized to Microsoft Entra ID

- pedro.garcia
  - UPN: pedro.garcia@lab.home-hub.es
  - Purpose: management user
  - Synchronized to Microsoft Entra ID

### Groups OU

Known group:
- GG-Workstation-Admins
  - Purpose: grants local administrator rights on domain workstations
  - Member:
    - richard.admin

### Workstations OU

Known workstation:
- LAB-ADMIN
  - Joined to lab.richardrubio.com
  - Located in Workstations OU
  - Uses DC01 as primary DNS
  - RSAT installed
  - Remote administration verified

## Microsoft Entra Connect

Status:
- Installed on DC01
- Configured
- Initial synchronization completed successfully
- Microsoft Entra Connect status: Enabled

Sign-in method:
- Password Hash Synchronization

Single sign-on:
- Not enabled

Source Anchor:
- Managed automatically by Microsoft Entra Connect

Directory:
- lab.richardrubio.com

Tenant:
- richardrubiolab.onmicrosoft.com

UPN configuration:
- lab.home-hub.es is verified in Microsoft Entra ID.
- lab.richardrubio.com remains the AD DS forest name and is not added as a Microsoft Entra domain.
- No AD DS users currently use @lab.richardrubio.com as their UPN.

Synchronization scope:

Synchronized OUs:
- Employees
- Groups
- Workstations

Excluded OUs and containers:
- Admin
- Builtin
- Computers
- Domain Controllers
- ForeignSecurityPrincipals
- Infrastructure
- LostAndFound
- Managed Service Accounts
- Program Data
- System
- Users

Optional features:
- Password Hash Synchronization enabled
- Exchange hybrid deployment disabled
- Exchange Mail Public Folders disabled
- Password writeback disabled
- Group writeback disabled
- Device writeback disabled
- Directory extension attribute sync disabled
- Microsoft Entra ID app and attribute filtering disabled
- Staging mode disabled
- Accidental deletion threshold configured with the default value of 500

## Microsoft Entra Current State

Cloud-only users:
- richard.admin@richardrubiolab.onmicrosoft.com
  - Display name: Richard Berriel
  - Global Administrator
  - Tenant administration / emergency account
  - On-premises sync: No

Synchronized users:
- richard.rubio@lab.home-hub.es
  - On-premises sync: Yes

- richard.helpdesk@lab.home-hub.es
  - On-premises sync: Yes

- ana.lopez@lab.home-hub.es
  - On-premises sync: Yes

- pedro.garcia@lab.home-hub.es
  - On-premises sync: Yes

Deleted test users:
- ana.lopez@richardrubiolab.onmicrosoft.com
  - Created temporarily to validate cloud user creation, licensing, onboarding and Exchange Online provisioning
  - Deleted before hybrid synchronization to avoid duplicate identities

## Completed

### Base Infrastructure
- Windows Server 2025 installed on DC01.
- DC01 promoted to Domain Controller.
- Forest created: lab.richardrubio.com.
- Active Directory-integrated DNS configured.
- Initial OU structure created.
- LAB-ADMIN joined to the domain.
- LAB-ADMIN moved to the Workstations OU.
- LAB-ADMIN configured to use DC01 as primary DNS.
- RSAT installed on LAB-ADMIN.
- Remote administration verified from LAB-ADMIN.

### Active Directory Identity
- AD DS users created:
  - richard.rubio
  - richard.admin
  - richard.helpdesk
  - ana.lopez
  - pedro.garcia
- richard.admin moved to the Admin OU.
- Employee and operational users located in the Employees OU.
- Alternative UPN suffix lab.home-hub.es added to the forest.
- All current AD DS users migrated to the @lab.home-hub.es UPN.
- richard.admin added to Domain Admins.
- richard.admin added to Enterprise Admins.
- GG-Workstation-Admins created.
- richard.admin added to GG-Workstation-Admins.

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

- GPO created:
  - GPO - Microsoft Defender - Workstations
- GPO linked to the Workstations OU.
- Microsoft Defender Real-time Protection baseline configured.
- Microsoft Defender Antivirus configured:
  - Remediation
  - Scheduled Full Scan
  - Brute-Force Protection
  - Remote Encryption Protection

- Group Policy Search adopted as the standard method for locating Administrative Template policies.

### Microsoft Entra Foundation
- Microsoft 365 Business Premium trial tenant created.
- Microsoft Entra Admin Center accessed.
- Microsoft 365 Admin Center accessed.
- Tenant language/interface changed toward English.
- Microsoft 365 Business Premium licensing explored.
- Cloud onboarding workflow validated with a temporary user.
- Exchange Online provisioning validated.
- Hybrid identity architecture defined.
- lab.home-hub.es added and verified as a Microsoft 365 custom domain.
- lab.home-hub.es configured as the tenant default domain.
- Microsoft 365 DNS records configured through Cloudflare.
- Microsoft Entra Connect installed and configured.
- Password Hash Synchronization enabled.
- OU filtering configured.
- Initial synchronization completed.
- Expected synchronized users verified in Microsoft Entra ID.
- AD DS administrative account successfully excluded from synchronization.

## Next Goal
Validate the completed hybrid identity foundation and return to Microsoft Learn.

Immediate next steps:
1. Inspect the AD DS synchronization account created by Microsoft Entra Connect.
2. Verify the synchronization service and scheduler.
3. Test sign-in with a synchronized user.
4. Confirm Password Hash Synchronization behavior.
5. Assign licenses according to each test user's purpose.
6. Review whether Enterprise Admins membership for richard.admin should remain permanent.
7. Update CHANGELOG.md.
8. Document the hybrid identity architecture and account purposes.
9. Create updated Hyper-V checkpoints.
10. Return immediately to the MD-102 Microsoft Learn path.

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
- Do not document temporary web interface locations or other fast-changing portal details.
- Do not document obvious facts unless they help avoid future confusion.
- Comments and notes should explain decisions, not repeat what a setting does.
- Important architecture decisions must record what was chosen, why it was chosen and where it would be changed later.

## Standard Group Policy Workflow
1. Search the policy using Group Policy Search.
2. Read the Microsoft documentation.
3. Configure the policy.
4. Validate with gpresult.
5. Validate the functional behavior.

## Design Rules
- One GPO = One functional purpose.
- AD DS remains the source of employee identities in the hybrid model.
- The AD DS forest remains lab.richardrubio.com.
- The standard user UPN is @lab.home-hub.es.
- Cloudflare manages public DNS for home-hub.es.
- Cloud-only tenant administrator account must remain available for emergency access.
- Privileged AD DS accounts must not be synchronized unless there is a specific reason.
- Synchronization scope is controlled through OU filtering.
- New AD DS users must be created with the @lab.home-hub.es UPN.