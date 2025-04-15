# Cloud Cost Showback & Chargeback Implementation Guide

**Date:** April 13, 2025

## Executive Summary

This guide provides a structured approach to implementing a comprehensive showback and chargeback model for your multi-cloud environment. It specifically addresses the challenge of shared service accounts (logging, security, perimeter, etc.) that benefit the entire organization, while aligning with your existing FinOps initiatives, including the FinOps Champions Framework and tagging strategy.

## Current State Assessment

Your organization currently has:

- **Multiple Account Types**: 
  - Project-specific accounts (directly attributable to business units)
  - Shared service accounts (logging, security, perimeter, etc.)
  - Organizational accounts (offering varying services to multiple teams)

- **Limited Governance**: 
  - No formal process to identify shared service accounts
  - Lack of consistent ownership for shared services
  - No clear funding model for organizational services

- **Dashboard Capabilities**:
  - Existing dashboards showing cost by account over time
  - Planned integration of Azure costs into QuickSight
  - Current showback capabilities without enforced chargeback

- **Implementation Challenges**:
  - Project team identification issues (being addressed via FinOps Champions)
  - Resistance from teams managing shared services to accept cost responsibility
  - CCOE department currently bearing the cost of many shared services
  - No standardized allocation methodology for shared costs

## Shared Services Classification Framework

### 1. Service Categories & Allocation Models

| Service Category | Description | Recommended Allocation Model | Funding Responsibility |
|------------------|-------------|------------------------------|------------------------|
| **Foundational Services** | Core infrastructure required for all cloud operations (e.g., DNS, identity) | No allocation - centralized cost | CCOE |
| **Security & Compliance** | Security tools, logging, and compliance systems | Allocation based on consumption metrics | Shared proportionally |
| **Platform Services** | Shared technical platforms (e.g., Kubernetes, databases) | Allocation based on usage metrics | Shared proportionally |
| **Development Tools** | CI/CD pipelines, code repositories, artifact storage | Allocation based on team size or usage | Shared proportionally |
| **Project-Specific** | Resources exclusively used by one project/team | Direct assignment | Business unit |

### 2. Shared Service Account Identification Process

To address the lack of governance in identifying shared service accounts, implement this standardized process:

1. **Initial Assessment**:
   - Inventory all 185 accounts across environments
   - Document current purpose and beneficiaries
   - Collect key stakeholder information

2. **Service Classification Workflow**:
   - Department heads to review and classify each account
   - Apply classification framework from above table
   - Document classification decisions and rationale

3. **Validation Process**:
   - FinOps team reviews classifications
   - Executive approval for funding assignments
   - Annual reassessment of classification decisions

4. **Documentation Requirements**:
   - Service description and beneficiaries
   - Funding model and allocation metrics
   - Accountable owner and decision authority

## Showback & Chargeback Implementation Plan

### Phase 1: Enhanced Showback (Months 1-3)

1. **Account Classification Setup**
   - Create account classification tagging scheme:
     ```
     AccountType: Project/Shared/Foundational
     ServiceCategory: Security/Platform/Development/Project
     FundingModel: CCOE/Allocated/Direct
     ```
   - Apply classification tags to all accounts
   - Integrate with existing tagging strategy

2. **Shared Service Identification**
   - Form working group with department representatives
   - Inventory and classify all accounts
   - Document service relationships and dependencies
   - Executive approval of classification decisions

3. **Enhanced Showback Dashboards**
   - Extend CUDOS/CID dashboards to show:
     - Costs by account classification
     - Costs before and after shared service allocation
     - Department-level aggregated views

### Phase 2: Allocation Model Development (Months 3-5)

1. **Metric Collection Framework**
   - Implement collection of usage metrics for shared services:
     - Identity service: Active user count
     - Logging: Log volume or instance count
     - Security: Protected resource count
     - Shared compute: vCPU/memory usage

2. **Allocation Algorithm Development**
   - Design allocation algorithms for each service type
   - Create Athena queries to calculate allocations
   - Implement test calculations with historical data
   - Validate results with stakeholders

3. **Allocation Dashboard Development**
   - Create "pre-allocation" and "post-allocation" views
   - Develop trends of allocated costs over time
   - Build what-if scenarios for allocation models

### Phase 3: Chargeback Implementation (Months 6-9)

1. **Financial Integration**
   - Develop cost transfer process with Finance
   - Create allocation validation workflow
   - Implement dispute resolution process

2. **Department Engagement**
   - Present allocation models to department heads
   - Conduct workshops on cost impact and optimization
   - Establish feedback mechanism for refinement

3. **Transition to Chargeback**
   - Run parallel showback/chargeback period
   - Implement soft limits and notifications
   - Final transition to full chargeback model

## Recommended Shared Service Allocation Models

### 1. Foundational Services (CCOE-Funded)

**What to Include:**
- Core identity and access management
- Basic network infrastructure
- Security baseline systems
- Enterprise directory services

**Implementation Approach:**
- Tag identified accounts with `FundingModel: CCOE`
- Create separate dashboard views for CCOE-funded services
- Track cost trends to justify centralized budget

### 2. Usage-Based Allocation (Share Proportionally)

**What to Include:**
- Shared logging and monitoring
- Security scanning and compliance tools
- Shared compute platforms (e.g., Kubernetes)
- Data platforms and analytics

**Allocation Metrics Examples:**
- **Logging & Monitoring**: Proportion of log volume or monitored instances
- **Security Services**: Count of resources being protected
- **Shared Compute**: vCPU hours, memory consumption, or pod count
- **Data Platforms**: Storage utilization, query volume, or data transfer

**Implementation Approach:**
1. Collect usage metrics via CloudWatch/Azure Monitor
2. Calculate allocation percentages for each business unit
3. Apply allocation percentages to the total cost
4. Present allocated costs in dashboards

### 3. Equal Distribution (Share Equally)

**What to Include:**
- Services with uniform benefit regardless of usage
- Core development tools with enterprise licenses
- General-purpose infrastructure platforms

**Implementation Approach:**
1. Identify services providing equal benefit
2. Divide total cost by number of consuming departments
3. Apply equal allocation in the chargeback model

### 4. Weighted Distribution (Custom Formulas)

**What to Include:**
- Services where benefit varies by department size
- Resources with complex allocation requirements

**Implementation Approach:**
1. Define weighting factors (team size, revenue, etc.)
2. Create custom allocation formula
3. Apply weighted distribution in calculated fields

## Technical Implementation

### 1. Account Classification in AWS

```bash
# Add classification tags to accounts
aws organizations tag-resource \
  --resource-id 123456789012 \
  --tags Key=AccountType,Value=Shared Key=ServiceCategory,Value=Security Key=FundingModel,Value=Allocated
```

### 2. Amazon Athena Query for Cost Allocation

```sql
-- Calculate department allocations for a shared logging service
WITH department_usage AS (
  SELECT
    tags.business_unit AS department,
    SUM(line_item_usage_amount) AS log_volume,
    COUNT(DISTINCT resource_id) AS resource_count
  FROM
    summary_view
  WHERE
    line_item_usage_start_date >= DATE_ADD('month', -1, CURRENT_DATE)
    AND product_service_code = 'AmazonCloudWatch'
  GROUP BY
    tags.business_unit
),
total_usage AS (
  SELECT
    SUM(log_volume) AS total_log_volume,
    SUM(resource_count) AS total_resources
  FROM
    department_usage
),
shared_cost AS (
  SELECT
    SUM(line_item_unblended_cost) AS total_shared_cost
  FROM
    summary_view
  WHERE
    line_item_usage_start_date >= DATE_ADD('month', -1, CURRENT_DATE)
    AND line_item_usage_account_id = '123456789012' -- Shared logging account
)
SELECT
  du.department,
  du.log_volume,
  du.resource_count,
  du.log_volume / tu.total_log_volume AS volume_allocation_percentage,
  du.resource_count / tu.total_resources AS resource_allocation_percentage,
  (du.log_volume / tu.total_log_volume * 0.7 + du.resource_count / tu.total_resources * 0.3) AS weighted_allocation_percentage,
  (du.log_volume / tu.total_log_volume * 0.7 + du.resource_count / tu.total_resources * 0.3) * sc.total_shared_cost AS allocated_cost
FROM
  department_usage du
CROSS JOIN
  total_usage tu
CROSS JOIN
  shared_cost sc
ORDER BY
  allocated_cost DESC;
```

### 3. QuickSight Dashboard Development

1. **Create Account Classification Dataset**
   - Build dataset joining account information with classifications
   - Include allocation percentages by department
   - Refresh daily for accurate allocations

2. **Shared Service Allocation Dashboard**
   - Views by service category and allocation model
   - Department cost impact before and after allocation
   - Trend analysis of shared service costs

3. **Department-Specific Views**
   - Direct vs. allocated costs
   - Allocation breakdown by service
   - Optimization recommendations

## Governance Framework

### 1. Shared Service Governance Committee

**Structure:**
- FinOps team representatives
- Department finance stakeholders
- Shared service owners
- Executive sponsor

**Responsibilities:**
- Approve account classifications
- Review and adjust allocation models
- Resolve disputes in cost allocation
- Approve new shared services

**Meeting Cadence:**
- Monthly review of allocation models
- Quarterly review of funding decisions
- Annual comprehensive assessment

### 2. Decision-Making Framework for Shared Services

To determine whether a service should be CCOE-funded or allocated:

1. **Strategic Importance Assessment**
   - Is the service fundamental to cloud operations?
   - Is it required for security or compliance?
   - Does it provide enterprise-wide benefit?

2. **Usage Pattern Analysis**
   - Is usage uniform across departments?
   - Can usage be measured and allocated fairly?
   - Does consumption vary significantly by department?

3. **Financial Impact Consideration**
   - What is the total cost of the service?
   - How would allocation impact department budgets?
   - Is there an ROI in tracking detailed allocations?

4. **Decision Tree**:
   ```
   Is service fundamental to operations? → Yes → CCOE Funded
   ↓ No
   Can usage be measured accurately? → No → Equal Distribution
   ↓ Yes
   Is tracking allocation worth the effort? → No → CCOE Funded
   ↓ Yes
   Are usage patterns variable? → Yes → Usage-Based Allocation
   ↓ No
   Equal Distribution
   ```

## Integration with Existing FinOps Initiatives

### 1. FinOps Champions Framework
- Assign specific FinOps Champions to shared service accounts
- Include shared service allocation in Champions training
- Leverage Champions to validate allocation models for their departments

### 2. Tagging Strategy
- Extend tagging strategy to include allocation metadata
- Ensure consistent tagging across shared services
- Use tags to automate allocation calculations

### 3. Cloud Governance Framework
- Add shared service governance to existing framework
- Integrate allocation reviews into governance meetings
- Leverage existing escalation paths for disputes

### 4. Dashboard Customization
- Build allocation views into CUDOS and CID dashboards
- Create specialized allocation analysis dashboards
- Integrate allocation data into forecasting models

## Recommended Executive Decision Points

The following strategic decisions require executive input:

1. **Foundational Service Funding**:
   - Option A: CCOE fully funds all foundational services
   - Option B: CCOE funds a baseline with usage-based allocation for excess
   - Option C: Full allocation of all services regardless of type
   - **Recommendation**: Option A for truly foundational services, with clear criteria for qualification

2. **Allocation Methodology**:
   - Option A: Consistent methodology across all shared services
   - Option B: Service-specific methodologies based on service type
   - Option C: Department-negotiated custom allocations
   - **Recommendation**: Option B to ensure fair and appropriate allocations

3. **Implementation Timeline**:
   - Option A: Immediate transition to chargeback
   - Option B: Phased approach with showback preceding chargeback
   - Option C: Indefinite showback with no formal chargeback
   - **Recommendation**: Option B with a 6-month showback period

## Conclusion

Implementing a structured showback and chargeback model with clear handling of shared services will enhance cost transparency, accountability, and optimization across your organization. This approach aligns with your FinOps Champions Framework and provides a path to mature cloud financial management.

By clearly identifying and classifying shared services, developing appropriate allocation methodologies, and establishing proper governance, you can overcome the current challenges in project team ownership and financial responsibility for shared resources.

## Implementation Roadmap

### Immediate Actions (Next 30 Days)
1. Form the Shared Service Governance Committee
2. Begin account classification inventory
3. Develop initial classification tagging scheme
4. Seek executive decisions on funding models

### Short-Term (1-3 Months)
1. Complete account classification
2. Develop enhanced showback dashboards
3. Begin metric collection for allocations
4. Train FinOps Champions on allocation models

### Medium-Term (3-6 Months)
1. Implement allocation algorithms
2. Develop allocation dashboards
3. Run parallel allocation calculations
4. Begin department education on allocations

### Long-Term (6-12 Months)
1. Transition from showback to chargeback
2. Integrate with financial systems
3. Establish regular allocation reviews
4. Continuously optimize allocation models

## Appendix: Case Studies & Decision Examples

### Case Study 1: Shared Logging Infrastructure
- **Service**: Centralized logging account collecting logs from all environments
- **Classification**: Security & Compliance
- **Allocation Model**: Usage-based (log volume and instance count)
- **Decision Rationale**: Variable consumption patterns make usage-based allocation fair

### Case Study 2: Core Identity Services
- **Service**: AWS SSO and directory services
- **Classification**: Foundational Services
- **Allocation Model**: CCOE-funded (no allocation)
- **Decision Rationale**: Core service required for all operations with uniform benefit
