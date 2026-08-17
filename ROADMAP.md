# HomeLab: Microsoft Systems Administration - Roadmap

## Mission

Become job-ready for Microsoft Systems Administrator roles as quickly as possible while building a strong professional foundation for long-term growth.

## Success Criteria

- Obtain a Microsoft Systems Administrator, Endpoint Administrator or Modern Workplace role.
- Build a professional and well-documented HomeLab.
- Develop practical skills that can be demonstrated during technical interviews.
- Follow industry standards and Microsoft best practices.
- Continue improving the HomeLab after getting a job.

## Project Principles

- Microsoft Learn determines the learning order.
- HomeLab practice must support employment, certification or real administration skills.
- Explain and understand the reason for a configuration before implementing it.
- Apply one functional change at a time.
- Validate configurations technically and functionally.
- Keep policies modular and avoid overlapping management authorities.
- Document permanent state and architecture decisions, not every learning step.
- Use PowerShell when it improves administration, automation or repeatability.

## Current Status

- Base AD DS infrastructure is operational.
- LAB-ADMIN is domain joined and remotely administered through RSAT.
- Core workstation GPOs are implemented.
- Microsoft 365 Business Premium tenant is operational.
- Hybrid identity with Microsoft Entra Connect and Password Hash Synchronization is operational.
- Selected users, groups and workstations are included in the synchronization scope.
- Microsoft Learn and MD-102 training are in progress.
- The current learning path is Protección de dispositivos mediante Microsoft Intune.

The exact technical and learning state is maintained in `PROJECT_STATE.md`.

## Current Sprint

Complete the Microsoft Learn module:

- Implementar directivas de seguridad y cifrado de dispositivos mediante Microsoft Intune.

Then perform the planned Defender and Intune practical block:

- Connect Intune with Defender.
- Onboard LAB-ADMIN through an EDR policy.
- Validate telemetry and incident visibility.
- Analyse the existing Defender GPO with Group Policy Analytics.
- Migrate supported settings to modular Intune endpoint security policies.
- Validate before removing overlapping GPO settings.

## Learning Roadmap

1. Complete the official MD-102 learning paths in Microsoft Learn.
2. Convert selected concepts into controlled HomeLab implementations.
3. Consolidate endpoint management with Intune and Microsoft Defender.
4. Implement and validate Conditional Access and device compliance.
5. Strengthen PowerShell administration and automation.
6. Build interview-ready technical scenarios and documentation.
7. Review exam objectives and complete targeted MD-102 preparation.

## Certification Plan

Current priority:

- Microsoft Certified: Endpoint Administrator Associate.
- Exam: MD-102.

Future certification decisions will be made after completing MD-102 and assessing job-market relevance.

## Completed Milestones

- AD DS forest and DNS foundation.
- Domain-joined Windows administration workstation.
- OU, user and administrative group structure.
- Workstation local administrator delegation through Group Policy.
- Corporate lock screen policy.
- Microsoft Defender workstation GPO.
- Microsoft 365 tenant and custom domain.
- Hybrid identity foundation with Microsoft Entra Connect.
- Password Hash Synchronization and OU filtering.
- Controlled Microsoft Entra device-join authorization.
- Core Microsoft Intune enrollment, configuration, maintenance and troubleshooting learning.
- First Microsoft Defender and Intune security module completed.

## Decision Log

- Keep the AD DS forest as `lab.richardrubio.com`.
- Use `@lab.home-hub.es` as the user sign-in UPN.
- Keep AD DS as the source of authority for synchronized employee identities.
- Keep privileged AD DS accounts outside Microsoft Entra synchronization.
- Use Password Hash Synchronization for hybrid authentication.
- Keep Intune MDM User Scope set to `Some` for controlled enrollment.
- Use one GPO or Intune profile per functional purpose where practical.
- Migrate from GPO to Intune only after technical and functional validation.