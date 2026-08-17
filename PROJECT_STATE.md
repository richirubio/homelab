# Project State

## Current Version
v1.0

## Current Module

MD-102 - Implementar directivas de seguridad y cifrado de dispositivos mediante Microsoft Intune

## Project Goal

Build a Microsoft administrator HomeLab aligned with Microsoft Learn and MD-102.

Main priorities:

1. Get ready for a Microsoft Systems Administrator role.
2. Prepare official Microsoft certifications.
3. Build practical experience with AD DS, Microsoft Entra ID, Intune, Microsoft 365, PowerShell and hybrid identity.

## Current Learning Path

MD-102 - Protección de dispositivos mediante Microsoft Intune

Current status:

- Microsoft Learn remains the main guide.
- The learning path Administración y mantenimiento de dispositivos mediante Microsoft Intune is complete.
- The module Implementación de la seguridad del punto de conexión con Microsoft Defender y Microsoft Intune is complete.
- Current module: Implementar directivas de seguridad y cifrado de dispositivos mediante Microsoft Intune.
- Exact continuation point: first content unit, Comprender la importancia del cifrado de dispositivos para el cumplimiento y la seguridad, not yet started.

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
- ms-DS-ConsistencyGuid
- Selected and managed automatically by Microsoft Entra Connect.

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

Synchronization Engine

- ADSync service verified.
- Synchronization Service architecture inspected.
- Connectors validated.
- Metaverse inspected.
- Scheduler configuration verified.
- MSOL synchronization account verified.

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

## Microsoft Entra Device Configuration

### Device Join Authorization Group

Applied and verified:
- Security group created: GG-Entra-Device-Join
- Membership type: Assigned
- Microsoft Entra roles can be assigned: No
- Description: Users authorized to join Windows devices to Microsoft Entra ID
- Owner: richard.admin@richardrubiolab.onmicrosoft.com
- Member: richard.rubio@lab.home-hub.es

Design decision:
- GG-Entra-Device-Join uses assigned membership because it grants a sensitive authorization.
- Membership must be managed explicitly and must not depend on dynamic attributes.

### Device Settings

Applied and verified:
- Users may join devices to Microsoft Entra ID is configured as Selected.
- GG-Entra-Device-Join is configured as the authorized group.
- Require Multifactor Authentication to register or join devices with Microsoft Entra is configured as Require.

Future design:
- Replace the legacy MFA device setting with a Conditional Access policy for the Register or join devices user action.
- After Conditional Access is configured and validated, change the legacy MFA device setting to Do not require.

### Intune Automatic Enrollment

Design decision:
- Keep MDM User Scope configured as Some to represent a controlled enterprise deployment.
- Before troubleshooting automatic enrollment, verify which group is assigned to the MDM User Scope and whether the test user belongs to it.

Important distinction:
- Microsoft Entra registration or join establishes device identity.
- Intune enrollment establishes device management.
- MDM User Scope controls automatic Intune enrollment; it does not authorize Microsoft Entra registration or join.

### Pending Controlled Test

A future controlled test must determine whether a standard user can:
- Register a personal device in Microsoft Entra ID.
- Join a corporate Windows device to Microsoft Entra ID.
- Enroll a device in Intune.
- Accidentally register or enroll a domain-joined Windows device.
- Obtain local administrator rights during a Microsoft Entra join.

The test must identify which layer permits or blocks each action:
- Windows
- Active Directory
- Microsoft Entra ID
- Intune
- User permissions
- Device settings
- MDM User Scope
- Conditional Access
- GPO or MDM policy

LAB-ADMIN is currently joined to lab.richardrubio.com. Its join state must not be changed without planning the test and checking the available Hyper-V checkpoint first.

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

## MD-102 Learning Progress

### Completed Areas

Completed learning includes:

- Microsoft Entra registered, joined and hybrid joined devices.
- Device identity and authentication with Microsoft Entra ID.
- Windows Hello for Business, MFA, SSPR and device authentication methods.
- Planning and implementing device enrollment with Microsoft Intune.
- Windows, iOS/iPadOS and Android enrollment.
- Windows Autopilot deployment.
- Device configuration profiles, assignments, groups and Intune filters.
- Device monitoring and maintenance.
- Windows update rings, feature updates, Hotpatch and Windows Autopatch.
- Device lifecycle actions: Retire, Wipe, Fresh Start and Autopilot Reset.
- Troubleshooting enrollment, compliance and configuration profile conflicts.
- Intune diagnostics, client-side logs and Remediations.
- The learning path Administración y mantenimiento de dispositivos mediante Microsoft Intune.

### Current Learning Path

Learning path:
- Protección de dispositivos mediante Microsoft Intune

Completed module:
- Implementación de la seguridad del punto de conexión con Microsoft Defender y Microsoft Intune

Topics completed:

- Difference between Microsoft Defender Antivirus and Microsoft Defender for Endpoint.
- Endpoint telemetry, cloud analysis, EDR and automated investigation and remediation.
- Integration between Microsoft Defender, Microsoft Intune and Microsoft Entra Conditional Access.
- Device onboarding to Defender through an Intune EDR policy.
- Security baselines and separate endpoint security policies for antivirus, firewall, ASR and EDR.
- ASR staged deployment using audit, evaluation and block modes.
- Investigation, triage and response to incidents in the Microsoft Defender portal.
- Defender for Business, Defender for Endpoint Plan 1 and Plan 2 licensing concepts.

Important mental model:

- Intune enrolls, configures and manages the device.
- An Intune EDR onboarding policy connects the device sensor to the organization’s Defender tenant.
- Defender receives and analyses telemetry and provides incident investigation and response.
- Intune can consume Defender device-risk signals for compliance.
- Microsoft Entra Conditional Access can restrict access based on the resulting compliance state.

### Current Module

Module:
- Implementar directivas de seguridad y cifrado de dispositivos mediante Microsoft Intune

Status:
- Not started.

Exact continuation point:
- Begin the first content unit:
  - Comprender la importancia del cifrado de dispositivos para el cumplimiento y la seguridad.

## Next Goal

Immediate next step:

1. Begin the first content unit of:
   - Implementar directivas de seguridad y cifrado de dispositivos mediante Microsoft Intune.
   - Unit: Comprender la importancia del cifrado de dispositivos para el cumplimiento y la seguridad.

Planned Defender and Intune practice:

1. Verify the available Microsoft 365 Business Premium and Defender for Business licensing.
2. Connect Microsoft Intune with Microsoft Defender.
3. Create a pilot device group.
4. Onboard LAB-ADMIN to Defender through an Intune EDR policy.
5. Validate policy application in Intune.
6. Validate device presence and telemetry in the Microsoft Defender portal.
7. Export and analyse GPO - Microsoft Defender - Workstations with Group Policy Analytics.
8. Compare the existing GPO settings with the Microsoft Defender security baseline and endpoint security policies in Intune.
9. Migrate supported settings to modular Intune policies.
10. Validate the new configuration technically and functionally before retiring overlapping GPO settings.
11. Generate a safe Defender simulation and follow the resulting alert and incident investigation.

Other pending tasks:

- Review the group assigned to MDM User Scope = Some.
- Create and validate Conditional Access for Register or join devices.
- Replace the legacy device MFA setting after Conditional Access is operational.
- Plan the controlled LAB-ADMIN device identity and enrollment test.
- Create dynamic device groups only when real Microsoft Entra devices and a useful targeting requirement exist.

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
- Architecture documents should record decisions and rationale, not detailed learning material.

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