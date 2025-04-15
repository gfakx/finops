# Azure DevOps Migration - Resource Tagging Strategy

## Overview
This document outlines the comprehensive tagging strategy for the Azure DevOps migration project. Proper resource tagging is essential for accurate cost allocation, governance enforcement, and optimization planning throughout the migration lifecycle.

## Objectives
- Enable accurate cost allocation across teams, projects, and environments
- Distinguish temporary from permanent resources
- Support automated governance through Azure Policy
- Provide metadata for cost analysis and optimization
- Enable chargeback/showback models

## Mandatory Tags

| Tag Name | Description | Format | Examples | Required |
|----------|-------------|--------|----------|----------|
| `Project` | Migration project identifier | String | `ADOMigration` | Yes |
| `Phase` | Migration phase identifier | String | `Phase1`, `Phase2` | Yes |
| `Environment` | Deployment environment | String | `Dev`, `Test`, `Prod`, `Staging` | Yes |
| `Owner` | Team or individual responsible | String | `TeamName`, `user@example.com` | Yes |
| `CostCenter` | Department or cost center | String | `IT-12345`, `DevOps-54321` | Yes |
| `TemporaryInstance` | Flag for temporary resources | Boolean | `Yes`, `No` | Yes |
| `Application` | Application or workload name | String | `BuildAgents`, `CoreServices` | Yes |
| `CreationDate` | Resource creation date | ISO 8601 | `2025-04-15` | Yes |
| `ExpiryDate` | Planned decommission date | ISO 8601 | `2025-12-31` | For temporary resources |

## Optional Tags

| Tag Name | Description | Format | Examples | When to Use |
|----------|-------------|--------|----------|-------------|
| `Criticality` | Business criticality | String | `High`, `Medium`, `Low` | For service prioritization |
| `SecurityLevel` | Security classification | String | `Public`, `Confidential`, `Restricted` | For sensitive resources |
| `DataClassification` | Data sensitivity level | String | `Public`, `Confidential`, `Restricted` | For data-containing resources |
| `MigrationWave` | Migration wave number | String | `Wave1`, `Wave2` | For phased migrations |
| `OptimizationStatus` | Cost optimization status | String | `Reviewed`, `Optimized`, `PendingReview` | For optimization tracking |

## Tag Value Standards
- All tag values should use PascalCase format (e.g., `DevEnvironment`, not `dev environment`)
- Boolean values should be represented as `Yes` or `No`
- Date formats should follow ISO 8601 standard (YYYY-MM-DD)
- Cost centers should follow the established organizational format (Department-Number)
- Email addresses should be organizational addresses, not personal emails

## Implementation Approach

### 1. Resource Group Level Tagging
- All resource groups must have the complete set of mandatory tags
- Resources inherit tags from resource groups by default
- Override at resource level only when necessary

### 2. Automated Enforcement
- Azure Policy will be configured to enforce mandatory tagging
- Non-compliant resources will be flagged for remediation
- Automated remediation will be applied where possible

### 3. Tag Inheritance
- Configure tag inheritance in Cost Management
- Resource group tags will be inherited by all child resources
- This ensures consistent cost allocation even when resource-level tags are missing

### 4. Tagging During Deployment
- All ARM templates and Terraform scripts must include mandatory tags
- CI/CD pipelines will validate tag compliance during deployment
- Deployment will fail if mandatory tags are missing

## Tagging Governance

### Compliance Monitoring
- Weekly tag compliance reports will be generated
- Compliance score will be tracked and reported to stakeholders
- Non-compliant resources will be flagged for remediation

### Update Process
- Tag schema changes require Change Advisory Board approval
- Tag value updates can be performed by resource owners
- Bulk tag updates should be performed through Azure Policy or scripts

### Responsibilities
- **FinOps Team**: Tag schema maintenance, compliance reporting
- **Project Teams**: Correct tagging during resource deployment
- **Operations Team**: Remediation of non-compliant resources
- **Governance Team**: Policy enforcement and exceptions

## Temporary Instance Tagging

Temporary resources created for the migration require special attention:

1. **Identification**: Must have `TemporaryInstance: Yes`
2. **Expiry Tracking**: Must include `ExpiryDate` tag
3. **Cost Attribution**: Must link to permanent instance through `RelatedPermanentResource` tag
4. **Monitoring**: Will be included in specific temporary resource reports

## Cost Management Integration

The tagging strategy enables the following cost management capabilities:

1. **Cost Allocation**: Filter and group costs by any tag dimension
2. **Budget Management**: Create budgets for specific tag values
3. **Anomaly Detection**: Configure alerts based on tagged resource groups
4. **Chargeback**: Generate department-specific cost reports based on CostCenter tags
5. **Lifecycle Management**: Track temporary resource lifecycle based on CreationDate and ExpiryDate

## Tag Usage Examples

### Example 1: Development Environment Build Agent
```json
{
  "tags": {
    "Project": "ADOMigration",
    "Phase": "Phase1",
    "Environment": "Dev",
    "Owner": "DevOpsTeam",
    "CostCenter": "IT-12345",
    "TemporaryInstance": "No",
    "Application": "BuildAgents",
    "CreationDate": "2025-04-15"
  }
}
```

### Example 2: Temporary Testing Instance
```json
{
  "tags": {
    "Project": "ADOMigration",
    "Phase": "Phase2",
    "Environment": "Test",
    "Owner": "QATeam",
    "CostCenter": "QA-54321",
    "TemporaryInstance": "Yes",
    "Application": "ADOTempInstance",
    "CreationDate": "2025-04-15",
    "ExpiryDate": "2025-07-15",
    "RelatedPermanentResource": "EnterpriseADO"
  }
}
```

## Appendix: Tagging Best Practices

1. **Consistency**: Use consistent naming and capitalization
2. **Automation**: Automate tagging wherever possible
3. **Validation**: Validate tags during deployment
4. **Review**: Review and update tags regularly
5. **Documentation**: Keep tagging schema documentation updated
