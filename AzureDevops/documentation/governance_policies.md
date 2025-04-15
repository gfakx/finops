# Azure DevOps Migration - Cloud Cost Governance Policies

## Overview
This document establishes the comprehensive governance framework for managing cloud costs throughout the Azure DevOps migration project. It defines policies, controls, approval workflows, and monitoring mechanisms to ensure cost efficiency, compliance, and optimization from the initial migration through ongoing operations.

## Objectives
- Establish clear policies for resource provisioning and management
- Define approval workflows for cost-impacting changes
- Implement role-based access control for cost management
- Create monitoring and compliance verification mechanisms
- Document change management processes for cost governance

## Policy Areas

### 1. Resource Provisioning Policies

#### 1.1 Resource Creation Policy
- All resources must be created through approved deployment pipelines or templates
- Manual resource creation requires documented justification and approval
- Resources must include all mandatory tags as defined in the Tagging Strategy
- Resource naming must follow organizational naming conventions

#### 1.2 Resource Sizing Policy
- VM sizes must be justified based on workload requirements
- Over-provisioning requires explicit approval from the FinOps team
- Right-sizing recommendations must be reviewed monthly
- Auto-scaling must be implemented where applicable

#### 1.3 Resource Lifecycle Policy
- All temporary resources must have an expiration date
- Resources inactive for 30+ days will be flagged for review
- Dev/Test environments must implement auto-shutdown during non-working hours
- Decommissioning procedures must be documented for all resource types

### 2. Cost Control Policies

#### 2.1 Budget Management Policy
- All departments must have defined monthly cloud budgets
- Budget overruns require escalation and remediation plans
- Budget adjustments require finance department approval
- Budget forecasts must be updated quarterly

#### 2.2 Cost Optimization Policy
- Azure Advisor recommendations must be reviewed bi-weekly
- Resources must be rightsized based on utilization metrics
- Reserved Instances must be evaluated for all stable workloads
- Savings Plans must be implemented where cost-effective

#### 2.3 Anomaly Management Policy
- Cost anomalies must be investigated within 24 hours
- Root cause analysis required for significant variances
- Corrective actions must be documented and implemented
- Prevention measures must be established for recurrent issues

### 3. Licensing Governance

#### 3.1 License Management Policy
- License inventory must be maintained and reviewed monthly
- License assignments must be justified and documented
- Unused licenses must be reclaimed after 30 days of inactivity
- License procurement requires approved business justification

#### 3.2 License Optimization Policy
- License utilization must be reviewed quarterly
- License tier adjustments must be made based on actual usage
- Annual vs. monthly license cost comparison required before renewal
- License consolidation opportunities must be evaluated bi-annually

### 4. Azure DevOps Usage Governance

#### 4.1 Pipeline Usage Policy
- Parallel job usage must be monitored and optimized
- Pipeline retention policies must be enforced
- Build and release pipelines must be optimized for efficiency
- Microsoft-hosted vs. self-hosted agent decisions must be cost-justified

#### 4.2 Storage Usage Policy
- Artifact retention policies must be enforced
- Git repository cleanup must be performed quarterly
- Test attachment storage must be monitored and purged
- Package feed governance must be implemented

## Approval Workflows

### 1. High-Cost Resource Approval

#### Approval Thresholds
| Resource Category | Cost Threshold | Approver |
|-------------------|----------------|----------|
| Compute | >$1,000/month | Department Head + FinOps |
| Storage | >$500/month | Department Head + FinOps |
| Networking | >$300/month | Department Head + FinOps |
| Licenses | >$5,000/year | Department Head + Finance |

#### Approval Process
1. Requestor submits resource request with business justification
2. FinOps team reviews cost impact and alternatives
3. Department head approves business need
4. FinOps approves cost efficiency
5. Implementation team provisions approved resources

### 2. Budget Exception Approval

#### Exception Types
- Temporary budget increase
- Budget reallocation between departments
- Emergency spending authorization

#### Approval Process
1. Department head submits exception request with justification
2. FinOps reviews impact on overall project budget
3. Finance validates funding availability
4. Executive sponsor approves significant exceptions
5. Finance implements approved budget changes

### 3. Optimization Deferral Approval

#### Deferral Criteria
- Business criticality of current configuration
- Technical limitations preventing optimization
- Time-limited deferral with expiration

#### Approval Process
1. Resource owner submits deferral request with justification
2. FinOps validates cost impact of deferral
3. Technical team reviews technical constraints
4. Department head approves business justification
5. FinOps implements deferral with expiration date

## Role-Based Access Control

### 1. Cost Management RBAC Model

#### Role Definitions
| Role | Permissions | Assigned To |
|------|-------------|-------------|
| Cost Viewer | View costs, reports, and recommendations | Team Leads, Project Managers |
| Cost Contributor | Create views, export data, recommend optimizations | FinOps Team, Department Heads |
| Cost Administrator | Create budgets, configure alerts, implement policies | FinOps Lead, Finance Representatives |
| Enterprise Administrator | Establish organizational policies, approve exceptions | Executive Sponsors, Finance Director |

#### Access Management
- Role assignments must be reviewed quarterly
- Principle of least privilege must be enforced
- Role changes require documented approval
- Service accounts require specific justification and enhanced monitoring

### 2. Resource Management RBAC Model

#### Role Definitions
| Role | Permissions | Assigned To |
|------|-------------|-------------|
| Resource Viewer | View resource configurations and metrics | All Project Team Members |
| Resource Contributor | Create, modify, and delete resources | DevOps Engineers, Infrastructure Team |
| Resource Administrator | Manage resource groups, apply policies | Infrastructure Leads |
| Policy Administrator | Create and assign Azure policies | Governance Team, FinOps Lead |

#### Environment Segregation
- Development: Relaxed controls with budget caps
- Testing: Moderate controls with scheduled provisioning
- Production: Strict controls with formal change management

## Monitoring and Compliance

### 1. Compliance Monitoring

#### Monitoring Areas
- Tag compliance: All resources properly tagged
- Budget compliance: Spending within approved budgets
- Policy compliance: Resources adhering to governance policies
- RBAC compliance: Access permissions appropriately assigned

#### Monitoring Mechanisms
- Automated weekly compliance reports
- Azure Policy audit results
- Azure Advisor recommendations
- Custom Power BI dashboards

### 2. Enforcement Mechanisms

#### Automated Enforcement
- Deny resource creation without required tags
- Enforce auto-shutdown on dev/test environments
- Automatically apply retention policies
- Implement budget alerts and spending limits

#### Manual Enforcement
- Monthly compliance review meetings
- Remediation action assignments
- Escalation for persistent non-compliance
- Exception documentation and tracking

### 3. Compliance Reporting

#### Report Types
- Executive compliance summary
- Department-level compliance scorecard
- Resource-level compliance details
- Trend analysis and improvement tracking

#### Reporting Schedule
- Weekly: Automated compliance summaries
- Monthly: Detailed compliance reports
- Quarterly: Trend analysis and recommendations
- Annual: Comprehensive compliance review

## Change Management

### 1. Cost-Impacting Changes

#### Change Categories
- Resource provisioning or decommissioning
- Licensing changes (additions, removals, tier changes)
- Service tier modifications
- Storage capacity adjustments

#### Change Process
1. Change requestor submits detailed request with cost impact analysis
2. FinOps team reviews and validates cost impact
3. Change Advisory Board reviews significant changes
4. Approved changes implemented with proper documentation
5. Post-implementation review verifies expected cost impact

### 2. Governance Policy Changes

#### Change Types
- New policy implementation
- Policy modification or exception
- Policy retirement
- Threshold adjustments

#### Change Process
1. Policy change proposal with justification and impact analysis
2. Stakeholder review and feedback period
3. Executive approval for significant policy changes
4. Implementation plan with timeline and responsibilities
5. Communication to affected teams and training if needed

## Implementation and Enforcement

### 1. Azure Policy Implementation

#### Policy Definitions
- Resource tagging enforcement
- Allowed resource types and sizes
- Budget policy enforcement
- Lifecycle management policy enforcement

#### Assignment Scope
- Management Group: Organization-wide policies
- Subscription: Migration project-specific policies
- Resource Group: Environment-specific policies

### 2. Microsoft Cost Management Configuration

#### Configuration Areas
- Budget setup and alert configuration
- Cost view creation and sharing
- Anomaly detection settings
- Export configuration for reporting

#### Access Configuration
- Department-specific view access
- Budget owner assignments
- Alert recipient configuration
- API access for custom reporting

### 3. Azure DevOps Integration

#### Integration Points
- Pipeline templates with governance checks
- Custom dashboards for cost visibility
- Work item integration for optimization tasks
- Approval workflows for resource provisioning

## Appendix A: Policy Implementation Examples

### Example: Resource Tagging Policy (Azure Policy JSON)
```json
{
  "properties": {
    "displayName": "Require specified tags on resources",
    "description": "Enforces required tags for all resources",
    "mode": "Indexed",
    "parameters": {
      "tagNames": {
        "type": "Array",
        "metadata": {
          "displayName": "Required Tags",
          "description": "The list of tag names that must be provided"
        },
        "defaultValue": [
          "Project",
          "Environment",
          "Owner",
          "CostCenter",
          "TemporaryInstance"
        ]
      }
    },
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "[concat('tags[', parameters('tagNames')[0], ']')]",
            "exists": "false"
          },
          {
            "field": "[concat('tags[', parameters('tagNames')[1], ']')]",
            "exists": "false"
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### Example: Auto-Shutdown Policy
```json
{
  "properties": {
    "displayName": "Auto-shutdown of VMs in Dev environments",
    "description": "Ensures Dev VMs are shut down outside business hours",
    "mode": "Indexed",
    "parameters": {},
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "tags.Environment",
            "equals": "Dev"
          }
        ]
      },
      "then": {
        "effect": "deployIfNotExists",
        "details": {
          "type": "Microsoft.DevTestLab/schedules",
          "evaluationDelay": "AfterProvisioning",
          "deployment": {
            "properties": {
              "mode": "incremental",
              "template": {
                "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                  "vmName": {
                    "type": "string"
                  }
                },
                "resources": [
                  {
                    "name": "[concat('shutdown-computevm-', parameters('vmName'))]",
                    "type": "Microsoft.DevTestLab/schedules",
                    "apiVersion": "2018-09-15",
                    "properties": {
                      "status": "Enabled",
                      "taskType": "ComputeVmShutdownTask",
                      "dailyRecurrence": {
                        "time": "1900"
                      },
                      "timeZoneId": "UTC",
                      "targetResourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
                    }
                  }
                ]
              },
              "parameters": {
                "vmName": {
                  "value": "[field('name')]"
                }
              }
            }
          }
        }
      }
    }
  }
}
```

## Appendix B: Compliance Scorecard Example

```
+------------------------------------------------------+
| Azure DevOps Migration - Governance Compliance Score |
| April 2025                                           |
+------------------------------------------------------+
| Overall Compliance Score: 87%                        |
+------------------------------------------------------+
| Category            | Score | Trend | Actions Needed |
|---------------------|-------|-------|----------------|
| Tag Compliance      | 92%   | ↑ 4%  | 12 resources   |
| Budget Compliance   | 85%   | ↓ 2%  | 3 departments  |
| Policy Compliance   | 88%   | ↑ 7%  | 8 resources    |
| RBAC Compliance     | 94%   | ↑ 1%  | 2 roles        |
| Lifecycle Compliance| 78%   | ↑ 12% | 15 resources   |
+------------------------------------------------------+
```

## Revision History
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-04-15 | FinOps Team | Initial document |
