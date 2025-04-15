# Azure DevOps Migration - Cost Allocation Model

## Overview
This document outlines the comprehensive cost allocation model for the Azure DevOps migration project. The model defines how cloud-related costs will be tracked, allocated, and charged back to business units, ensuring financial accountability and transparency throughout the migration process.

## Objectives
- Provide accurate distribution of cloud costs across organizational units
- Enable transparent showback/chargeback for temporary and permanent instances
- Establish clear ownership of costs for licensing, usage, and infrastructure
- Support data-driven decision making for optimization
- Create a sustainable model that persists beyond the migration

## Cost Categories

### 1. User Licensing Costs
- **Microsoft-provided licenses**: Monthly, quarterly, or annual Azure DevOps user licenses
- **Third-party tool licenses**: Any additional tooling required for the migration
- **License management overhead**: Administrative costs for license management

### 2. System Usage Costs
- **Azure Pipelines consumption**: Build and release pipeline minutes
- **Test execution costs**: Automated testing environments and execution time
- **Storage and data transfer**: Artifacts, code, and work item storage

### 3. System Hosting Costs
- **IaaS infrastructure**: Virtual machines, storage, and networking for ADO on-premises
- **PaaS components**: App Services, Azure SQL, and other platform services
- **Shared services**: Monitoring, security, and management tools

## Allocation Methods

### Direct Attribution
Costs that can be directly attributed to a specific team, project, or department based on resource tags:
- Individual VM instances with clear ownership
- Project-specific storage accounts
- Dedicated network resources
- User license assignments with clear departmental mapping

### Shared Services Attribution
For shared resources that benefit multiple teams or projects:
- **Usage-Based**: Allocated based on actual consumption metrics
- **Equal Division**: Divided equally among consuming teams/projects
- **Weighted Distribution**: Allocated based on predetermined ratios

### Temporary vs. Permanent Instance Attribution
- **Temporary Instance**: Costs allocated to migration project budget
- **Enterprise Instance**: Costs allocated based on departmental usage
- **Transition Period**: Dual allocation during parallel operations

## Allocation Rules by Cost Type

### User Licensing
| License Type | Allocation Method | Billing Cycle | Cost Center |
|--------------|-------------------|---------------|-------------|
| Basic Licenses | Direct - By User Department | Monthly | Department |
| Basic + Test Plans | Direct - By User Department | Monthly | Department |
| Stakeholder | Direct - By User Department | Monthly | Department |
| Extension Licenses | Direct - By Installing Department | Monthly | Department |

### System Usage
| Usage Type | Allocation Method | Calculation | Cost Center |
|------------|-------------------|-------------|-------------|
| Pipeline Minutes | Usage-Based | Minutes Used ÷ Total Minutes × Cost | Project/Team |
| Test Environments | Usage-Based | Hours Used ÷ Total Hours × Cost | Project/Team |
| Storage | Direct + Usage-Based | GB Used ÷ Total GB × Cost | Project/Team |
| Bandwidth | Usage-Based | GB Transferred ÷ Total GB × Cost | Project/Team |

### System Hosting
| Resource Type | Allocation Method | Calculation | Cost Center |
|---------------|-------------------|-------------|-------------|
| Dedicated VMs | Direct Attribution | 100% of Resource Cost | Owning Team |
| Shared VMs | Usage-Based | % of Resource Used × Cost | Multiple Teams |
| Storage | Direct + Usage-Based | GB Used ÷ Total GB × Cost | Multiple Teams |
| Networking | Weighted | Predefined Allocation % | All Teams |
| Management Tools | Equal Division | Total Cost ÷ Number of Teams | All Teams |

## Cross-Charging Implementation

### Temporary Instance Charging
1. **During Migration Phase**:
   - All temporary instance costs tagged with `TemporaryInstance: Yes`
   - Costs charged to central migration project budget
   - Monthly showback to departments based on resource usage

2. **Post-Migration**:
   - Transition to departmental chargeback model
   - Phased implementation based on migration completion

### Shared Resource Allocation
1. **Identification**:
   - Resources tagged with multiple owners or as shared services
   - Central services identified through application tags

2. **Cost Split Implementation**:
   - Primary method: Usage-based metrics from Azure Monitor
   - Secondary method: Predefined allocation percentages
   - Recalculation frequency: Monthly

3. **Dispute Resolution**:
   - Department review period: 5 business days after monthly reports
   - Escalation path: FinOps team → Finance department → Executive sponsor

## Cost Distribution Models

### Showback Model
- **Purpose**: Informational reporting only, no actual transfers
- **Audience**: Department heads, project managers, team leads
- **Frequency**: Monthly with trend analysis
- **Format**: Power BI dashboard + detailed Excel exports

### Chargeback Model
- **Purpose**: Actual internal cost transfer between departments
- **Audience**: Finance, department heads, executive leadership
- **Frequency**: Quarterly actual transfers with monthly accruals
- **Format**: Official financial system entries with documentation

## Implementation Timeline

### Phase 1: Initial Reporting (Months 1-2)
- Implement tagging strategy
- Set up basic showback reports
- Validate cost data accuracy

### Phase 2: Department-Level Attribution (Months 3-4)
- Implement department-level showback
- Refine allocation rules
- Establish monthly review process

### Phase 3: Full Chargeback (Months 5-6)
- Transition to full chargeback model
- Integrate with financial systems
- Implement dispute resolution process

## Roles and Responsibilities

### FinOps Team
- Maintain allocation models and rules
- Generate and distribute cost reports
- Facilitate cost review meetings
- Recommend optimization opportunities

### Department Leaders
- Review allocated costs
- Approve chargeback transfers
- Identify optimization opportunities
- Ensure correct resource tagging

### Finance Department
- Process internal transfers
- Validate allocation accuracy
- Provide accounting guidance
- Support budgeting processes

### Project Teams
- Maintain proper resource tagging
- Review team-level cost reports
- Implement optimization recommendations
- Forecast future resource needs

## Report Types and Distribution

### 1. Executive Summary
- **Content**: High-level cost overview by department and service type
- **Audience**: Executive leadership
- **Frequency**: Monthly
- **Format**: PowerPoint deck with key metrics

### 2. Department Cost Reports
- **Content**: Detailed breakdown of department costs by service, project, and resource
- **Audience**: Department heads, finance business partners
- **Frequency**: Monthly
- **Format**: Power BI dashboard + Excel exports

### 3. Project-Level Reports
- **Content**: Project-specific costs with usage metrics and trends
- **Audience**: Project managers, team leads
- **Frequency**: Weekly
- **Format**: Power BI dashboard

### 4. Resource Optimization Reports
- **Content**: Underutilized resources, cost-saving opportunities
- **Audience**: Department heads, project managers
- **Frequency**: Monthly
- **Format**: Excel report with specific recommendations

## Appendix A: Cost Allocation Calculation Examples

### Example 1: Shared VM Allocation
A shared VM hosting build agents used by three teams with the following usage:
- Team A: 45% of build minutes
- Team B: 30% of build minutes
- Team C: 25% of build minutes

Monthly cost of VM: $1,000

**Allocation**:
- Team A: $1,000 × 0.45 = $450
- Team B: $1,000 × 0.30 = $300
- Team C: $1,000 × 0.25 = $250

### Example 2: License Allocation
Enterprise has 100 Basic licenses at $6/month/user
- Department X: 40 users
- Department Y: 35 users
- Department Z: 25 users

**Allocation**:
- Department X: 40 × $6 = $240
- Department Y: 35 × $6 = $210
- Department Z: 25 × $6 = $150

## Appendix B: Report Examples

### Executive Summary Dashboard
```
+-------------------------------------+
| Azure DevOps Migration             |
| Cost Summary - April 2025          |
+-------------------------------------+
| Total Cost:              $125,000  |
| ● User Licensing:         $25,000  |
| ● System Usage:           $45,000  |
| ● System Hosting:         $55,000  |
+-------------------------------------+
| Top Departments                     |
| 1. Engineering:           $60,000  |
| 2. Product Development:   $35,000  |
| 3. QA:                    $30,000  |
+-------------------------------------+
| Cost by Instance Type               |
| ● Temporary:              $75,000  |
| ● Enterprise:             $50,000  |
+-------------------------------------+
```

## Revision History
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-04-15 | FinOps Team | Initial document |
