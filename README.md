# Windows Server RDS Learning Path 06 — Create the RDS Lab Forest

**Level:** Beginner · **Module:** 6/70

## Goal
Promote `DC01` as the first domain controller and DNS server in a new `corp.lab` forest.

## Prerequisites
- AD DS role installed
- Static IP `10.30.30.10`
- Unique DSRM password
- Console access

## Setup
1. Run forest-installation prerequisite checks.
2. Prompt securely for the DSRM password.
3. Create forest `corp.lab` with NetBIOS name `CORP` and DNS.
4. Restart and sign in with the domain Administrator account.
5. Change DC01 DNS client settings to `10.30.30.10`.

```powershell
$DSRM=Read-Host 'DSRM password' -AsSecureString
Install-ADDSForest -DomainName corp.lab -DomainNetbiosName CORP `
  -InstallDns -SafeModeAdministratorPassword $DSRM -Force
```

## Validation
```powershell
Get-ADForest
Get-ADDomain
Get-ADDomainController -Identity DC01
Get-SmbShare -Name SYSVOL,NETLOGON
Resolve-DnsName -Type SRV _ldap._tcp.dc._msdcs.corp.lab
```

## Evidence
Store prerequisite output, forest/domain properties, FSMO ownership, SYSVOL/NETLOGON shares and SRV records under `evidence/`. Record actual warnings, errors and remediation.

## Troubleshooting
Resolve static IP, DNS, reboot and naming errors before promotion. Missing SYSVOL/NETLOGON requires DFSR investigation; do not create the shares manually.

## Security
Use a unique DSRM credential and do not browse the Internet interactively from the domain controller.

## Rollback
Forest removal is destructive. Demote only in a disposable lab after preserving required evidence.

## Next
`Windows-Server-RDS-Learning-Path-07-Validate-DC01-DNS-SYSVOL-and-Netlogon`
