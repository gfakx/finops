# Azure DevOps Migration - Monthly Cost Tracking Guide

## Overview
This document provides detailed procedures for implementing and maintaining the monthly cost tracking system for the Azure DevOps migration project. It focuses specifically on the three key cost areas: user licensing, system usage (Azure Pipelines), and system hosting infrastructure (ADO on-premises).

## Objectives
- Establish standardized procedures for monthly cost tracking
- Define data collection methodologies for each cost category
- Create reporting templates and visualization standards
- Document responsibilities and timeline for monthly tracking activities
- Define optimization review processes

## Monthly Cost Tracking Calendar

| Day | Activities | Responsible | Deliverables |
|-----|------------|-------------|--------------|
| 1-3 | Data collection and verification | FinOps Specialist | Raw cost data from all sources |
| 4-6 | Data processing and analysis | FinOps Specialist | Processed datasets, initial insights |
| 7-8 | Report generation | FinOps Specialist | Draft cost reports for all categories |
| 9-10 | Report review and validation | Department Leads | Validated cost reports |
| 11-12 | Final report distribution | FinOps Specialist | Distributed reports to stakeholders |
| 15 | Cost review meeting | FinOps Lead + Stakeholders | Meeting notes, action items |
| 16-20 | Optimization implementation | Technical Teams | Implementation documentation |
| 25 | Optimization validation | FinOps Specialist | Validation report |
| Last day | Month-end reconciliation | Finance + FinOps | Reconciliation report |

## 1. User Licensing Cost Tracking

### 1.1 Data Sources
- Azure DevOps Admin Portal (license assignments)
- Microsoft 365 Admin Center (for integrated licenses)
- Azure EA Portal or Microsoft Customer Agreement billing data
- Procurement records for annual license purchases

### 1.2 Data Collection Process
1. **Export License Inventory**
   - Access Azure DevOps organization settings
   - Navigate to Users section
   - Export complete user list with access levels
   - Document timestamp of export for consistency

2. **Gather Billing Data**
   - Access Cost Management & Billing in Azure Portal
   - Filter for Azure DevOps and related services
   - Export detailed usage and cost data
   - Include any EA adjustments or credits

3. **Reconcile License Counts**
   - Compare assigned licenses vs. billed licenses
   - Identify discrepancies for investigation
   - Document findings in the reconciliation template

### 1.3 Analysis Methodologies
1. **License Utilization Analysis**
   - Calculate utilization percentage (active users / total licenses)
   - Identify underutilized license types
   - Track utilization trends month-over-month

2. **Cost Per User Analysis**
   - Calculate average cost per user by department
   - Compare against budgeted cost per user
   - Identify outliers and anomalies

3. **License Optimization Analysis**
   - Identify users with inappropriate license levels
   - Calculate savings from potential downgrades
   - Identify users with multiple or redundant licenses

### 1.4 Reporting Templates
1. **Executive License Summary**
   ```
   +---------------------------------------------------+
   | User Licensing Summary - April 2025               |
   +---------------------------------------------------+
   | License Type    | Quantity | Cost    | Utilization |
   +-----------------+----------+---------+-------------+
   | Basic           | 250      | $12,500 | 92%         |
   | Basic + Test    | 100      | $10,000 | 88%         |
   | Stakeholder     | 300      | $0      | 76%         |
   | Visual Studio   | 50       | $7,500  | 94%         |
   +-----------------+----------+---------+-------------+
   | Total           | 700      | $30,000 | 85%         |
   +---------------------------------------------------+
   | Optimization Opportunities:                       |
   | • 12 inactive Basic licenses ($600 potential)     |
   | • 8 Basic+Test licenses for non-testers ($400)    |
   | • 72 inactive Stakeholder accounts (clean-up)     |
   +---------------------------------------------------+
   ```

2. **Department License Report**
   ```
   +---------------------------------------------------+
   | Department: Engineering - April 2025              |
   +---------------------------------------------------+
   | License Type    | Quantity | Cost    | Utilization |
   +-----------------+----------+---------+-------------+
   | Basic           | 100      | $5,000  | 95%         |
   | Basic + Test    | 50       | $5,000  | 90%         |
   | Stakeholder     | 75       | $0      | 80%         |
   | Visual Studio   | 30       | $4,500  | 97%         |
   +-----------------+----------+---------+-------------+
   | Total           | 255      | $14,500 | 91%         |
   +---------------------------------------------------+
   | Month-over-Month Change: +$500 (+3.6%)            |
   | Forecast Next Month: $15,000                      |
   +---------------------------------------------------+
   ```

### 1.5 Optimization Actions
1. **Regular Cleanup**
   - Remove licenses from users inactive for 30+ days
   - Downgrade underutilized licenses to appropriate levels
   - Consolidate duplicate accounts

2. **License Pool Management**
   - Implement shared license pool for occasional users
   - Configure automated license assignment/revocation
   - Track temporary license needs

3. **Renewal Planning**
   - Flag licenses approaching renewal 60 days in advance
   - Conduct right-sizing analysis before renewals
   - Consider annual vs. monthly cost comparison

## 2. System Usage Cost Tracking (Azure Pipelines)

### 2.1 Data Sources
- Azure DevOps Analytics (pipeline usage metrics)
- Azure Cost Management (billed pipeline minutes)
- Azure Monitor (if configured for pipeline monitoring)
- Azure DevOps organization settings (parallel job configuration)

### 2.2 Data Collection Process
1. **Export Pipeline Usage Data**
   - Access Azure DevOps Analytics views
   - Export pipeline run history with durations
   - Gather parallel job utilization metrics
   - Document job concurrency patterns

2. **Gather Billing Data**
   - Access Cost Management & Billing in Azure Portal
   - Filter for Azure Pipelines services
   - Export detailed usage and cost data
   - Separate Microsoft-hosted vs. self-hosted costs

3. **Collect Resource Utilization Data**
   - For self-hosted agents, gather VM utilization metrics
   - Document storage usage for artifacts and sources
   - Track networking costs for data transfers

### 2.3 Analysis Methodologies
1. **Pipeline Efficiency Analysis**
   - Calculate average pipeline duration by project
   - Identify longest-running and most frequent pipelines
   - Analyze cost per pipeline run

2. **Parallel Job Utilization Analysis**
   - Calculate peak and average job utilization
   - Identify periods of queue waittime
   - Determine optimal parallel job count

3. **Build Agent Analysis**
   - Compare costs: Microsoft-hosted vs. self-hosted
   - Calculate agent utilization percentages
   - Identify idle agent capacity

### 2.4 Reporting Templates
1. **Executive Pipeline Summary**
   ```
   +---------------------------------------------------+
   | Azure Pipelines Usage Summary - April 2025        |
   +---------------------------------------------------+
   | Category           | Usage      | Cost    | Trend  |
   +--------------------+------------+---------+--------+
   | MS-Hosted Minutes  | 15,000     | $7,500  | ↑ 5%   |
   | Self-Hosted Agents | 10 agents  | $3,000  | ↔ 0%   |
   | Storage (Artifacts)| 500 GB     | $750    | ↑ 12%  |
   | Bandwidth          | 2 TB       | $150    | ↑ 8%   |
   +--------------------+------------+---------+--------+
   | Total              |            | $11,400 | ↑ 7%   |
   +---------------------------------------------------+
   | Optimization Opportunities:                       |
   | • Pipeline consolidation ($500/mo potential)      |
   | • Artifact cleanup policies ($200/mo potential)   |
   | • Off-hours builds for non-critical paths ($300)  |
   +---------------------------------------------------+
   ```

2. **Project Pipeline Report**
   ```
   +---------------------------------------------------+
   | Project: Core Services - April 2025               |
   +---------------------------------------------------+
   | Pipeline Type      | Runs   | Avg Duration | Cost  |
   +--------------------+--------+--------------+-------+
   | CI Pipelines       | 450    | 12.5 min     | $2,250|
   | Release Pipelines  | 120    | 18.2 min     | $910  |
   | Scheduled Builds   | 90     | 25.3 min     | $950  |
   +--------------------+--------+--------------+-------+
   | Total              | 660    |              | $4,110|
   +---------------------------------------------------+
   | Top 3 Most Expensive Pipelines:                   |
   | 1. Full Product Build: $725                       |
   | 2. Integration Tests: $550                        |
   | 3. Deployment Pipeline: $390                      |
   +---------------------------------------------------+
   ```

### 2.5 Optimization Actions
1. **Pipeline Optimization**
   - Implement pipeline caching strategies
   - Configure parallel execution where beneficial
   - Optimize build container images

2. **Job Allocation Optimization**
   - Adjust parallel job allocation based on usage patterns
   - Schedule non-critical builds during off-peak hours
   - Implement job prioritization

3. **Resource Cleanup**
   - Implement artifact retention policies
   - Configure automated cleanup for temporary resources
   - Optimize repository size and build history

## 3. System Hosting Cost Tracking (ADO On-Premises)

### 3.1 Data Sources
- Azure IaaS billing data (for VMs, storage, networking)
- On-premises infrastructure monitoring tools
- Hardware asset management system
- CMDB or infrastructure inventory database

### 3.2 Data Collection Process
1. **Infrastructure Inventory Update**
   - Validate current inventory of servers, storage, and networking
   - Document any changes from previous month
   - Update configuration details and dependencies

2. **Gather Azure IaaS Costs**
   - Access Cost Management & Billing in Azure Portal
   - Filter for resources tagged for ADO on-premises
   - Export detailed usage and cost data
   - Categorize by resource type (compute, storage, network)

3. **Collect Utilization Metrics**
   - Gather CPU, memory, and storage utilization
   - Document peak and average usage patterns
   - Track performance metrics against established baselines

### 3.3 Analysis Methodologies
1. **Infrastructure Efficiency Analysis**
   - Calculate resource utilization percentages
   - Identify underutilized and overutilized resources
   - Compare actual vs. expected utilization

2. **Cost Allocation Analysis**
   - Distribute costs based on established allocation model
   - Track cost per ADO service component
   - Calculate cost per user/project for on-prem resources

3. **Migration Impact Analysis**
   - Compare on-premises costs with cloud equivalents
   - Track cost changes during migration phases
   - Document cost avoidance through decommissioning

### 3.4 Reporting Templates
1. **Infrastructure Cost Summary**
   ```
   +---------------------------------------------------+
   | ADO On-Premises Infrastructure - April 2025       |
   +---------------------------------------------------+
   | Resource Type     | Quantity | Cost    | Util %   |
   +-------------------+----------+---------+----------+
   | Application VMs   | 12       | $7,200  | 65%      |
   | Database Servers  | 4        | $4,800  | 72%      |
   | Storage           | 10 TB    | $1,000  | 83%      |
   | Networking        | -        | $500    | -        |
   | Support Costs     | -        | $2,500  | -        |
   +-------------------+----------+---------+----------+
   | Total             |          | $16,000 |          |
   +---------------------------------------------------+
   | Migration Status:                                 |
   | • 4 VMs migrated to Azure this month              |
   | • $3,200 in decommissioned infrastructure         |
   | • 2 TB of data transferred to cloud storage       |
   +---------------------------------------------------+
   ```

2. **Resource Utilization Report**
   ```
   +---------------------------------------------------+
   | Resource Utilization - April 2025                 |
   +---------------------------------------------------+
   | Server ID   | Role      | CPU % | RAM % | Disk %  |
   +-------------+-----------+-------+-------+---------+
   | ADO-APP-01  | Web Tier  | 75%   | 65%   | 58%     |
   | ADO-APP-02  | Web Tier  | 72%   | 68%   | 62%     |
   | ADO-APP-03  | App Tier  | 82%   | 75%   | 70%     |
   | ADO-APP-04  | App Tier  | 78%   | 72%   | 68%     |
   | ADO-DB-01   | Database  | 85%   | 80%   | 88%     |
   | ADO-DB-02   | Database  | 82%   | 78%   | 85%     |
   +-------------+-----------+-------+-------+---------+
   | Performance Notes:                                |
   | • ADO-APP-03 approaching CPU threshold            |
   | • ADO-DB-01 storage approaching capacity          |
   | • Consider resource allocation adjustments        |
   +---------------------------------------------------+
   ```

### 3.5 Optimization Actions
1. **Resource Right-sizing**
   - Adjust VM sizes based on utilization metrics
   - Implement auto-scaling where appropriate
   - Consolidate underutilized servers

2. **Storage Optimization**
   - Implement tiered storage strategies
   - Archive older data to lower-cost storage
   - Clean up temporary and redundant data

3. **Migration Acceleration**
   - Identify on-prem resources ready for migration
   - Calculate ROI for accelerated migration
   - Document decommissioning plan and schedule

## 4. Consolidated Monthly Reporting

### 4.1 Executive Dashboard
The monthly executive dashboard provides a high-level overview of all three cost categories:

```
+-----------------------------------------------------------+
| Azure DevOps Migration - Monthly Cost Dashboard           |
| April 2025                                                |
+-----------------------------------------------------------+
| Cost Category       | Actual  | Budget  | Variance | Trend |
|---------------------|---------|---------|----------|-------|
| User Licensing      | $30,000 | $32,000 | -$2,000  | ↓ 5%  |
| System Usage        | $11,400 | $12,000 | -$600    | ↑ 7%  |
| System Hosting      | $16,000 | $15,000 | +$1,000  | ↓ 12% |
|---------------------|---------|---------|----------|-------|
| Total               | $57,400 | $59,000 | -$1,600  | ↓ 3%  |
+-----------------------------------------------------------+
| Key Metrics                                               |
|-----------------------------------------------------------|
| • License Utilization: 85% (+2% from last month)          |
| • Pipeline Efficiency: 95% (+5% from last month)          |
| • Infrastructure Utilization: 72% (+8% from last month)   |
| • Migration Progress: 36% complete (+4% from last month)  |
+-----------------------------------------------------------+
| Top Optimization Opportunities                            |
|-----------------------------------------------------------|
| 1. License optimization potential: $1,500/month           |
| 2. Pipeline consolidation potential: $500/month           |
| 3. Infrastructure right-sizing potential: $2,000/month    |
+-----------------------------------------------------------+
| Cost Projection                                           |
|-----------------------------------------------------------|
| Next Month Forecast: $58,500 (+1.9%)                      |
| Quarterly Projection: $175,000                            |
+-----------------------------------------------------------+
```

### 4.2 Cost Trend Analysis
Monthly cost trend analysis should include:
- 6-month historical trend for each cost category
- Year-to-date spend against annual budget
- Variance analysis with root causes
- Seasonality and pattern identification
- Projection based on current trends

### 4.3 Cost Driver Analysis
For each significant cost change, document:
- Specific resources or services driving the change
- Quantitative impact on overall costs
- Root cause of the change (e.g., increased usage, price change)
- Whether the change was anticipated or unexpected
- Recommended actions to address unfavorable changes

## 5. Monthly Cost Review Meeting

### 5.1 Meeting Structure
| Agenda Item | Duration | Lead | Content |
|-------------|----------|------|---------|
| Cost Summary | 10 min | FinOps Lead | Overview of monthly costs across all categories |
| Variance Analysis | 15 min | FinOps Specialist | Detailed explanation of significant variances |
| Optimization Status | 15 min | Technical Leads | Status of ongoing optimization initiatives |
| New Optimization Opportunities | 15 min | All | Discussion of newly identified opportunities |
| Migration Impact | 10 min | Migration Lead | Cost implications of recent migration activities |
| Next Month Forecast | 10 min | FinOps Lead | Projected costs for next reporting period |
| Action Items | 15 min | Project Manager | Assignment of action items and responsibilities |

### 5.2 Required Participants
- FinOps Lead (meeting chair)
- Project Director
- Department Representatives
- Technical Leads for each system area
- Finance Representative
- Migration Team Lead

### 5.3 Meeting Outputs
- Approved monthly cost report
- Documented explanation for variances
- Prioritized optimization initiatives
- Assigned action items with owners and due dates
- Approved forecast for next reporting period

## 6. Continuous Improvement Process

### 6.1 Monthly Process Improvement
After each monthly cycle, document:
- Data collection challenges and solutions
- Reporting improvements needed
- Process efficiency opportunities
- Tool enhancement requirements

### 6.2 Quarterly Process Review
Every quarter, conduct a comprehensive review:
- End-to-end process efficiency
- Stakeholder feedback on reports and insights
- Tool and automation improvements
- Documentation updates needed
- Training requirements for team members

### 6.3 Tool and Methodology Evolution
Regularly assess and improve:
- Data collection methods and automation
- Analysis techniques and models
- Visualization methods and dashboards
- Integration with other business processes
- Alignment with organizational objectives

## Appendix A: Monthly Data Collection Checklist

### User Licensing
- [ ] Export Azure DevOps user list with access levels
- [ ] Gather billing data from EA Portal or MCA billing
- [ ] Cross-reference license assignments with billing data
- [ ] Validate department assignments for all users
- [ ] Document license changes from previous month

### System Usage
- [ ] Export pipeline usage metrics from Azure DevOps Analytics
- [ ] Gather billing data for Microsoft-hosted pipelines
- [ ] Collect utilization metrics for self-hosted agents
- [ ] Document artifact and storage usage
- [ ] Record bandwidth consumption for data transfers

### System Hosting
- [ ] Update infrastructure inventory
- [ ] Collect Azure IaaS billing data
- [ ] Gather utilization metrics for all resources
- [ ] Document any infrastructure changes
- [ ] Update migration status for on-premises components

## Appendix B: Data Sources and Access Methods

| Data Source | Access Method | Required Permission | Contact |
|-------------|---------------|---------------------|---------|
| Azure DevOps Admin | https://dev.azure.com/{org}/_settings | Organization Admin | [Admin Team] |
| Cost Management | Azure Portal > Cost Management + Billing | Billing Reader | [Finance Team] |
| EA Portal | https://ea.azure.com | EA Admin | [Finance Team] |
| Azure DevOps Analytics | Azure DevOps > Analytics views | Project Admin | [DevOps Team] |
| Infrastructure Monitoring | [Tool-specific URL] | Monitoring Admin | [Ops Team] |

## Revision History
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-04-15 | FinOps Team | Initial document |
