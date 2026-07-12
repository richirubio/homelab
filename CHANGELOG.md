# CHANGELOG

## v1.0 - Hybrid Identity Foundation

### Added

* Added the alternative UPN suffix:

  * `lab.home-hub.es`
* Migrated all AD DS user UPNs to `@lab.home-hub.es`.
* Verified `lab.home-hub.es` as the Microsoft 365 custom domain.
* Configured `lab.home-hub.es` as the tenant default domain.
* Configured Microsoft 365 DNS records through Cloudflare.
* Installed Microsoft Entra Connect on `DC01`.
* Configured Password Hash Synchronization (PHS).
* Configured automatic Source Anchor management using `ms-DS-ConsistencyGuid`.
* Configured OU filtering:

  * Employees
  * Groups
  * Workstations
* Completed the initial synchronization to Microsoft Entra ID.
* Verified synchronized users:

  * richard.rubio
  * richard.helpdesk
  * ana.lopez
  * pedro.garcia
* Verified the Microsoft Entra Connect Scheduler.
* Inspected the Synchronization Service architecture.
* Validated Connectors, Operations and Metaverse.
* Verified the automatically created MSOL synchronization account.
* Created the initial identity architecture documentation.

### Architecture Decisions

* The AD DS forest remains `lab.richardrubio.com`.
* User sign-in UPN is `@lab.home-hub.es`.
* AD DS remains the source of authority for synchronized identities.
* Privileged AD DS accounts remain outside the synchronization scope.
* Password Hash Synchronization is the selected authentication method.
* The Cloud-Only Global Administrator remains independent from Active Directory.

### Next

* Create updated Hyper-V checkpoints.
* Return to the Microsoft Learn MD-102 learning path.
* Continue validating Microsoft Entra and Intune concepts in the HomeLab as required by the course.
