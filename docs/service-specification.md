# Active Directory Domain Join Service

## Summary

This service will provide an opt-in service to enable Active Directory domain-join as a self-service.

## Requirements specification

### Functional requirements

- [R01] event-driven automation (using Azure Functions with PowerShell)
- [R02] Opt-in service to enable Active Directory domain-join as a self-service
- [R03] The service is configured to use `contoso.com`, an on-premises Active Directory as a target
- [R04] Windows Server virtual machines and Windows client OSes are in scope.
- [R05] prevent racing condition (several events coming from the same VM, triggering the automation multiple times)
- [R06] automated dependency management (of PowerShell modules)
- [R07] dynamic configuration (including references to Key Vault secrets)
- [R08] Validation of `Computer` object name uniqueness in Active Directory (requires connectivity to AD), ensuring the VM that is tagged for being domain-joined, has a unique name (it does not exist already in the domain)
- [R09] lifecycle management (ALM) and deployment using CI/CD pipeline and staging environment(s), including IaC/CaC for all domainJoin service components
- [R10] test/verify/document re-join/re-deploy/delete & create scenarios
- [R11] automated test (inside the pipeline) and ad-hoc test (for quick troubleshooting)
- [R12] integration of alerts with Microsoft Teams
- [R19] use 'hostname' property of the Virtual Machine, rather than 'resource name'

### Security requirements

- [R13] utilize Managed Identity for authentication and authorization to Azure APIs (ARM, Key Vault, etc.). Remove dependency on `RunAs` account in Azure Automation and problems related with certificate renewal
- [R14] store secrets in Azure Key Vault (grant access permissions to the Managed Identity)

### Operational requirements

- [R15] logging to Azure Monitor workspace with alert rules (failed domain join)
- [R16] feedback for Omnia Operations in case of a failure
- [R17] message to the customer upon successful completion
- [R18] reporting - Azure Dashboard with e.g. Logic App runs, Function runs
