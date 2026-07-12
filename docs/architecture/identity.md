# Identity Architecture

## Purpose

This document defines the identity architecture of the HomeLab, the design decisions that have been made, and the reasoning behind them.

Its purpose is to provide a long-term architectural reference for future changes and to ensure that the laboratory evolves consistently as new technologies such as Microsoft Intune, Windows Autopilot, Microsoft 365, and Microsoft Defender are introduced.

---

## Architecture Overview

```text
                           Internet
                               │
                               ▼
                         Cloudflare DNS
                               │
                               ▼
                    lab.home-hub.es (Verified)
                               │
                               ▼
                    Microsoft Entra ID Tenant
               richardrubiolab.onmicrosoft.com
                               ▲
                               │
                     Microsoft Entra Connect
                  Password Hash Synchronization
                               ▲
                               │
                     Active Directory Domain
                     lab.richardrubio.com
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
     Employees OU          Groups OU          Workstations OU
```

---

## Design Decisions

### 1. Active Directory Forest

#### Decision

The Active Directory forest permanently remains:

`lab.richardrubio.com`

#### Rationale

Renaming an Active Directory forest is a complex operation that provides no benefit for this HomeLab.

Microsoft Entra Connect does not require the internal forest name to match the users' sign-in domain.

#### Alternatives Considered

Rename the forest to:

`lab.home-hub.es`

This alternative was rejected because it would add unnecessary complexity without improving the architecture.

#### Impact

The internal Active Directory namespace remains stable while user identities use a separate verified domain.

#### Future Modification

The forest name must not be changed.

Future identity changes should be implemented through UPN suffixes, verified domains, or synchronization configuration rather than by renaming the forest.

---

### 2. User Principal Name

#### Decision

All synchronized Active Directory users sign in with:

`@lab.home-hub.es`

instead of the internal forest suffix:

`@lab.richardrubio.com`

#### Rationale

This separates the internal Active Directory namespace from the public identity used by users.

It reflects a common enterprise design in which the internal AD domain and the user sign-in domain are different.

#### Alternatives Considered

Use the forest suffix:

`@lab.richardrubio.com`

This alternative was rejected because the domain is not verified in Microsoft Entra ID and is intended to remain an internal infrastructure namespace.

#### Impact

Users authenticate with a verified and externally manageable domain while the Active Directory forest remains unchanged.

#### Future Modification

Additional UPN suffixes may be added if new verified domains are introduced.

Existing synchronized users should only be changed after reviewing the impact on Microsoft Entra ID, Microsoft 365, and device sign-in.

---

### 3. Public DNS

#### Decision

The public DNS zone for:

`lab.home-hub.es`

is managed through Cloudflare.

#### Rationale

Cloudflare centralizes the public DNS records required for Microsoft Entra ID, Microsoft 365, and future cloud services.

It also keeps public DNS management separate from the internal Active Directory DNS infrastructure.

#### Alternatives Considered

Manage the public domain through another DNS provider or attempt to manage it from the internal DNS server.

Managing the public domain from the internal Active Directory DNS infrastructure was rejected because internal and public DNS have different purposes and security boundaries.

#### Impact

Microsoft domain verification and future cloud service records depend on Cloudflare.

Internal Active Directory name resolution continues to be handled separately by the DNS service integrated with AD DS.

#### Future Modification

Any Microsoft 365 or cloud integration requiring public DNS records must be configured in Cloudflare.

Changes should be documented before implementation.

---

### 4. Microsoft Entra Connect

#### Decision

Microsoft Entra Connect is used as the synchronization engine between Active Directory Domain Services and Microsoft Entra ID.

Current configuration:

* Password Hash Synchronization
* Automatic Source Anchor management
* OU filtering
* 30-minute Delta synchronization cycle
* Staging Mode disabled

Microsoft Entra Connect is currently installed on:

`DC01`

#### Rationale

Password Hash Synchronization provides a simple and resilient hybrid identity model without requiring users to authenticate directly against the on-premises environment.

The selected configuration is appropriate for the HomeLab and aligns with the hybrid identity concepts covered by Microsoft Learn.

#### Alternatives Considered

* Pass-through Authentication
* Active Directory Federation Services
* Cloud-only identities for all users

Pass-through Authentication was rejected because it would introduce additional dependency on the on-premises environment during authentication.

Active Directory Federation Services was rejected because it would add significant infrastructure and operational complexity without providing value for the current objectives.

A completely cloud-only identity model was rejected because the HomeLab is specifically designed to study hybrid identity and Active Directory integration.

#### Impact

Active Directory is the source of authority for synchronized users.

Changes made to synchronized identities should normally be performed in Active Directory and then synchronized to Microsoft Entra ID.

Installing Microsoft Entra Connect on `DC01` is acceptable for this HomeLab, but a dedicated synchronization server would normally be preferred in a production environment.

#### Future Modification

If the laboratory grows, Microsoft Entra Connect may be moved to a dedicated synchronization server.

A second server may also be introduced in Staging Mode for resilience.

Any migration must preserve the existing Source Anchor values and synchronization scope.

---

### 5. Source Anchor

#### Decision

The Source Anchor is:

`ms-DS-ConsistencyGuid`

Microsoft Entra Connect manages it automatically.

#### Rationale

The Source Anchor provides a stable identifier for a synchronized identity.

It allows Microsoft Entra Connect to recognize the same object even if mutable attributes change, including:

* Display name
* UPN
* Email address
* Organizational Unit location
* Given name
* Surname

#### Alternatives Considered

Use another attribute or manage the Source Anchor manually.

Manual management was rejected because it increases the risk of mismatches, duplicate cloud objects, and synchronization failures.

#### Impact

The Source Anchor must not be modified manually during normal administration.

Two users with the same name or CN remain separate identities because each Active Directory object has its own unique identifiers.

#### Future Modification

The Source Anchor should only be changed as part of a documented migration procedure.

Before any change, the relationship between `ms-DS-ConsistencyGuid` and the Microsoft Entra immutable identifier must be verified.

---

### 6. OU Filtering

#### Decision

Only the following Organizational Units are synchronized:

* Employees
* Groups
* Workstations

The following Organizational Units and containers are excluded:

* Admin
* Domain Controllers
* Builtin
* Users
* Remaining system containers

#### Rationale

OU filtering limits synchronization to the objects that have a valid reason to exist in Microsoft Entra ID.

Administrative identities and system objects remain isolated from the cloud synchronization scope.

#### Alternatives Considered

Synchronize the complete domain.

This alternative was rejected because it would unnecessarily expose administrative, service, and system objects to the synchronization engine.

#### Impact

Objects located outside the selected OUs are not imported into the Active Directory Connector Space and do not reach the Metaverse.

Moving an object into or out of a synchronized OU can change whether it is represented in Microsoft Entra ID.

#### Future Modification

Before adding another OU to the synchronization scope, review:

* The object types contained in the OU
* Whether all objects should be synchronized
* Administrative or service accounts that may be unintentionally included
* The impact on Microsoft Entra ID and licensing

---

### 7. Cloud-Only Global Administrator

#### Decision

The account:

`richard.admin@richardrubiolab.onmicrosoft.com`

remains Cloud Only.

It has no synchronized counterpart in Active Directory.

#### Rationale

The account provides independent administrative access to the Microsoft Entra tenant.

It remains available if Active Directory, Microsoft Entra Connect, or the synchronization process becomes unavailable.

#### Alternatives Considered

Synchronize the administrative account from the Active Directory `Admin` OU.

This alternative was rejected because it would make tenant administration dependent on the on-premises environment and the synchronization platform.

#### Impact

The account must be managed directly in Microsoft Entra ID.

Its credentials and authentication methods are independent from Active Directory.

#### Future Modification

The account must remain outside the synchronization scope.

If additional emergency administrative accounts are created, they should also be Cloud Only and protected with strong authentication controls.

---

### 8. Microsoft Entra Connect AD DS Account

#### Decision

Microsoft Entra Connect created and uses the following Active Directory account:

`MSOL_cc749501f8c2`

The account is enabled and is not a member of any explicit Active Directory security group.

#### Rationale

Microsoft Entra Connect uses a dedicated account with delegated directory permissions instead of using a Domain Administrator account.

This follows the Principle of Least Privilege.

#### Alternatives Considered

Use a Domain Administrator account as the permanent synchronization credential.

This alternative was rejected because it would grant the synchronization platform far more privileges than required.

#### Impact

The account has delegated permissions required for directory replication and selected attribute write operations.

It must not be used for interactive administration.

It must not be deleted, renamed, disabled, or have its password modified manually without following a supported Microsoft procedure.

#### Future Modification

If Microsoft Entra Connect is reinstalled or migrated, the synchronization account and delegated permissions must be reviewed as part of the migration plan.

---

## Current Synchronization Scope

### Included Organizational Units

* Employees
* Groups
* Workstations

### Excluded Organizational Units and Containers

* Admin
* Domain Controllers
* Builtin
* Users
* Remaining system containers

### Synchronized Users

* Richard Rubio
* Richard Helpdesk
* Ana López
* Pedro García

### Excluded Active Directory Administrator

* `richard.admin`

### Cloud-Only Global Administrator

* `richard.admin@richardrubiolab.onmicrosoft.com`

---

## Microsoft Entra Connect Components

The following services were verified on `DC01`:

* Microsoft Azure AD Sync
* Microsoft Azure AD Connect Agent Updater
* Microsoft Entra Connect Health Agent

The synchronization engine service is:

`ADSync`

The Microsoft Entra Connect Health Agent provides monitoring and health information.

The Agent Updater manages updates for Microsoft Entra Connect components.

---

## Connectors

The Synchronization Service contains two connectors:

| Connector                               | Type                             |
| --------------------------------------- | -------------------------------- |
| `lab.richardrubio.com`                  | Active Directory Domain Services |
| `richardrubiolab.onmicrosoft.com - AAD` | Windows Azure Active Directory   |

The Active Directory connector represents the on-premises forest.

The Microsoft Entra connector represents the tenant using its original `onmicrosoft.com` identity, independently of the verified custom domain used by synchronized users.

---

## Identity Synchronization Flow

Microsoft Entra Connect does not copy objects directly from Active Directory to Microsoft Entra ID.

It processes objects through internal connector spaces and the Metaverse.

```text
Active Directory
        │
        ▼
AD DS Connector
        │
        ▼
AD Connector Space
        │
        ▼
Metaverse
        │
        ▼
Entra Connector Space
        │
        ▼
Microsoft Entra Connector
        │
        ▼
Microsoft Entra ID
```

The Metaverse represents identities using abstract object types such as:

* Person
* Group
* Device
* PublicFolder

This abstraction allows the synchronization engine to represent an identity independently from the naming model of a specific connected directory.

---

## Synchronization Cycle

Each synchronization cycle contains three principal phases:

```text
Import
   │
   ▼
Synchronization
   │
   ▼
Export
```

### Import

Reads objects and changes from a connected directory and stores them in its Connector Space.

### Synchronization

Applies synchronization rules, joins related objects, updates the Metaverse, and prepares pending exports.

This phase primarily processes information inside the synchronization engine.

### Export

Writes pending changes from the Connector Space to the connected directory.

---

## Scheduler Configuration

The Microsoft Entra Connect Scheduler currently has the following configuration:

| Property                           | Value          |
| ---------------------------------- | -------------- |
| Allowed synchronization interval   | 30 minutes     |
| Effective synchronization interval | 30 minutes     |
| Customized interval                | Not configured |
| Next synchronization policy        | Delta          |
| Synchronization enabled            | Yes            |
| Maintenance enabled                | Yes            |
| Staging Mode                       | Disabled       |
| Scheduler suspended                | No             |
| Synchronization in progress        | No             |
| Run history retention              | 7 days         |

The standard operational cycle is a Delta synchronization.

Full synchronization cycles are reserved for initial processing or configuration changes that require the complete synchronization scope to be recalculated.

---

## Verified Metaverse Objects

The `Person` object type currently contains:

* Richard Rubio
* Richard Helpdesk
* Ana López
* Pedro García

The Active Directory account `richard.admin` does not appear because the `Admin` OU is excluded from synchronization.

This confirms that OU filtering is applied before excluded objects reach the Metaverse.

---

## Architectural Principles

The identity architecture follows these principles:

* Keep the Active Directory forest stable.
* Separate the internal AD namespace from the users' sign-in domain.
* Use Microsoft-supported synchronization configurations.
* Apply the Principle of Least Privilege.
* Synchronize only the required Organizational Units.
* Keep an independent Cloud-Only Global Administrator.
* Allow Microsoft Entra Connect to manage the Source Anchor.
* Avoid manual modification of synchronization identities and service accounts.
* Document important architectural decisions before making significant changes.
* Prefer simplicity unless additional complexity solves a documented requirement.

---

## Deferred Improvements

The following recommendations were identified but are intentionally deferred:

* Enable Active Directory Recycle Bin.
* Review TPM-related recommendations.
* Evaluate whether Microsoft Entra Connect should move from `DC01` to a dedicated synchronization server.
* Evaluate a second Microsoft Entra Connect server in Staging Mode if resilience becomes a laboratory objective.

These items are not current installation failures and do not need to be resolved before returning to Microsoft Learn.

---

## Future Evolution

The current architecture provides a foundation for future work involving:

* Microsoft Intune
* Windows Autopilot
* Microsoft Entra Join
* Hybrid Microsoft Entra Join
* Microsoft Defender for Endpoint
* Microsoft 365
* Conditional Access
* Device compliance
* Additional verified domains
* Additional synchronized Organizational Units

Each future integration must be evaluated against the architectural principles documented here.
