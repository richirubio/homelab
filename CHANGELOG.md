
## v0.9 - Hybrid Identity Foundation

### Added
- Added the alternative UPN suffix:
  - lab.home-hub.es
- Migrated all AD DS user UPNs to @lab.home-hub.es.
- Verified lab.home-hub.es as the Microsoft 365 custom domain.
- Configured lab.home-hub.es as the tenant default domain.
- Configured Microsoft 365 DNS records through Cloudflare.
- Installed Microsoft Entra Connect on DC01.
- Configured Microsoft Entra Connect using Password Hash Synchronization.
- Configured automatic Source Anchor management.
- Configured OU filtering:
  - Employees
  - Groups
  - Workstations
- Excluded privileged administrative accounts from synchronization.
- Completed the initial synchronization to Microsoft Entra ID.
- Verified synchronized users:
  - richard.rubio
  - richard.helpdesk
  - ana.lopez
  - pedro.garcia
- Verified that the cloud-only Global Administrator remains independent from AD DS synchronization.

### Changed
- Adopted a hybrid identity architecture.
- AD DS forest remains:
  - lab.richardrubio.com
- Standard user sign-in UPN changed to:
  - @lab.home-hub.es
- Microsoft Entra ID now uses lab.home-hub.es as the primary domain.
- Public Microsoft 365 DNS management moved to Cloudflare.

### Architecture Decisions
- The AD DS forest will not be renamed.
- AD DS remains the source of truth for employee identities.
- User synchronization is controlled through OU filtering.
- Privileged AD DS administrative accounts remain excluded from synchronization.
- Password Hash Synchronization is the selected sign-in method.

### Next
- Inspect the synchronization service account created by Microsoft Entra Connect.
- Validate the synchronization scheduler.
- Test authentication using synchronized users.
- Assign Microsoft 365 licenses according to each user's role.
- Document the hybrid identity architecture.
- Return to the Microsoft Learn MD-102 learning path.