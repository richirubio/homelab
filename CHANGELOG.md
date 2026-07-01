# Changelog

## v0.2 - Active Directory Baseline

### Added
- Installed Windows Server 2025 on DC01.
- Promoted DC01 to Domain Controller.
- Created the forest lab.richardrubio.com.
- Created the initial OU structure.
- Created the first domain user.

### Hyper-V Checkpoints
- DC01: v0.2 - Active Directory Baseline

## v0.3 - Domain Joined

### Added
- Renamed workstation to LAB-ADMIN.
- Joined LAB-ADMIN to the lab.richardrubio.com domain.
- Verified domain authentication with LAB\richard.rubio.
- Configured LAB-ADMIN to use DC01 as its DNS server.

### Hyper-V Checkpoints
- LAB-ADMIN: v0.3 - Domain Joined

## v0.4 - Remote Administration

### Added
- Installed RSAT administration tools.
- Installed Active Directory administration tools.
- Installed DNS management tools.
- Installed Group Policy Management.
- Verified remote administration from LAB-ADMIN.

### Hyper-V Checkpoints
- LAB-ADMIN: v0.4 - Remote Administration