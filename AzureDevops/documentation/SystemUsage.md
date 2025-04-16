# Azure DevOps System Usage (Azure Pipelines) - FinOps Guide

## Overview
This document outlines how to track, monitor, and manage Azure Pipelines usage as part of the FinOps implementation for the Azure DevOps migration project. It includes information on accessing usage data, understanding billing models, monitoring consumption, and optimizing costs.

## Understanding Azure Pipelines Billing Model

### Parallel Jobs Concept
Azure Pipelines uses **parallel jobs** as the primary billing unit:

- **Parallel job**: A job that can run on an agent at a given time
- **Pipeline**: A series of stages, each containing jobs that run on agents
- **Job**: A unit of work that runs on an agent (could be a build or a deployment)

### Types of Parallel Jobs

#### Microsoft-hosted Parallel Jobs
- Microsoft manages and maintains the infrastructure
- Charged based on number of concurrent jobs
- Available with different machine specifications (Windows, Linux, macOS)
- Free tier: 1 job with 60 minutes/month limit for private projects
- Free tier: Up to 10 jobs with 360 minutes/job limit for public projects

#### Self-hosted Parallel Jobs
- You maintain the infrastructure (on-premises or in cloud VMs)
- Charged based on number of concurrent jobs, not number of agents
- Free tier: 1 parallel job for private projects
- Free tier: 1 additional job per Visual Studio Enterprise subscriber
- Free tier: Unlimited jobs for public projects

### Pricing Structure (as of April 2025)
| Type | Free Allowance | Additional Cost |
|------|----------------|-----------------|
| Microsoft-hosted | 1 parallel job (60 min/month for private projects) | $40/month per parallel job |
| Self-hosted | 1 parallel job for private projects | $15/month per parallel job |

*Note: There are no time limits for self-hosted jobs. Microsoft-hosted paid jobs can run for up to 360 minutes (6 hours) each.*

## Accessing Pipeline Usage Information

### Method 1: Azure DevOps Portal - Pool Consumption Report

1. Navigate to Azure DevOps portal: `https://dev.azure.com/{Your_Organization}`
2. Go to **Project Settings** > **Pipelines** > **Agent pools**
3. Select the specific agent pool you want to analyze
4. Click on the **Analytics** tab
5. View the **Pool consumption report** that shows:
   - Running jobs over time
   - Queued jobs over time
   - Parallel job limit line
   - Wait times and bottlenecks

This method provides visual data on:
- Peak usage times
- Queue bottlenecks
- Pipeline efficiency
- Capacity constraints

### Method 2: Azure DevOps Portal - Usage Monitoring

1. Navigate to Azure DevOps portal: `https://dev.azure.com/{Your_Organization}`
2. Go to **Organization settings** (gear icon at bottom left)
3. Select **Usage**
4. In the dropdown, choose **Usage by pipeline**
5. View detailed breakdown of pipeline consumption

This page shows:
- Which pipelines consume the most resources
- Command frequency and throughput units consumed
- Potential throttling or performance issues

### Method 3: Azure DevOps Portal - Parallel Jobs Settings

1. Navigate to Azure DevOps portal: `https://dev.azure.com/{Your_Organization}`
2. Go to **Organization settings** > **Pipelines** > **Parallel jobs**
   (Direct URL: `https://{Your_Organization}/_admin/_buildQueue?_a=resourceLimits`)
3. View:
   - Current parallel job limits for Microsoft-hosted and self-hosted jobs
   - Number of jobs in use
   - Number of jobs purchased

4. Click **View in-progress jobs** to see:
   - Currently running jobs consuming parallel slots
   - Queued jobs waiting for available slots
   - Job durations and pipeline details

### Method 4: Azure Portal - Cost Analysis

1. Navigate to Azure Portal: `https://portal.azure.com`
2. Go to **Subscriptions** and select the subscription used for Azure DevOps
3. Select **Cost analysis** from the left menu
4. Filter options:
   - **Service name** = "Azure DevOps" to see only DevOps charges
   - **Meter subcategory** = "Azure Pipelines" to view pipeline-specific costs
   - **Resource** = Pipeline resources
   - **Group by Resource** to see costs per pipeline resource

5. View **Daily costs** to see consumption patterns over time

### Method 5: Pipeline Analytics and Reports

1. Navigate to Azure DevOps portal: `https://dev.azure.com/{Your_Organization}`
2. Go to a specific pipeline
3. Select the **Analytics** tab
4. View built-in analytics:
   - Pipeline pass rate report
   - Pipeline duration report
   - Test failures report

These reports help identify:
- Long-running pipelines that consume more resources
- Frequently failing pipelines that waste resources
- Performance bottlenecks and optimization opportunities

## Monitoring Pipeline Usage

### Key Metrics to Track

#### 1. Utilization Metrics
- **Parallel job utilization rate**: Percentage of available parallel jobs in use
- **Queue time**: Duration jobs wait before starting execution
- **Pipeline duration**: Time taken to complete pipeline runs
- **Concurrent pipelines**: Number of pipelines running simultaneously

#### 2. Cost Metrics
- **Cost per pipeline run**: Average cost of each pipeline execution
- **Cost per project/team**: Pipeline costs allocated to specific projects
- **Cost trend**: Month-over-month changes in pipeline costs
- **Cost per build minute**: Efficiency metric for pipeline execution

#### 3. Performance Metrics
- **Pipeline success rate**: Percentage of successful pipeline runs
- **Agent idle time**: Time agents spend waiting for jobs
- **Pipeline frequency**: How often pipelines are triggered
- **Job duration distribution**: Identifying exceptionally long-running jobs

### Setting Up Usage Alerts

1. **Budget Alerts in Azure Portal**:
   - Navigate to Azure Portal > Subscriptions > Cost Management
   - Create budget specifically for Azure Pipelines
   - Set alerts at 70%, 90%, and 100% thresholds

2. **Custom Dashboard Widgets**:
   - In Azure DevOps, create a custom dashboard
   - Add pipeline widgets for usage monitoring
   - Set up automated reports with Power BI integration

## Cost Optimization Strategies

### 1. Pipeline Efficiency Optimization
- Implement efficient caching strategies to reduce build times
- Use pipeline templates to standardize and optimize common tasks
- Configure appropriate timeout settings
- Split long-running pipelines into smaller, more efficient jobs

### 2. Agent Pool Optimization
- Right-size Microsoft-hosted agents for specific workloads
- Implement self-hosted agents for high-volume, predictable workloads
- Use spot instances for non-critical pipelines when using self-hosted agents
- Schedule non-urgent builds during off-peak hours

### 3. Parallel Job Management
- Purchase only the number of parallel jobs needed based on usage analysis
- Monitor queuing patterns to identify the optimal number of parallel jobs
- Consider Microsoft-hosted for sporadic needs and self-hosted for consistent usage
- Implement job prioritization to maximize value from available parallel jobs

### 4. License and Tier Optimization
- Evaluate Microsoft-hosted vs. self-hosted cost efficiency for your workloads
- Consider Visual Studio Enterprise subscriptions for teams needing self-hosted jobs
- Analyze public vs. private project settings for open-source components

## Temporary vs. Permanent Instance Tracking

### Tracking Temporary Instance Pipeline Usage
- Tag all pipeline resources with "TemporaryInstance: Yes"
- Create separate agent pools for temporary instance workloads
- Configure dedicated parallel jobs for migration activities
- Track migration-specific pipeline templates separately

### Cost Allocation Model for Pipeline Usage
- **Direct Attribution**: Assign pipeline costs directly to projects based on usage
- **Shared Service Allocation**: Distribute shared pipeline infrastructure costs based on consumption
- **Migration vs. BAU Split**: Separate temporary migration costs from business-as-usual operations

## Implementation Recommendations

### Phase 1: Assessment (Weeks 1-2)
1. Gain access to all required Azure DevOps organizations
2. Generate baseline reports of current pipeline usage
3. Identify peak usage patterns and bottlenecks
4. Document current parallel job configuration and costs

### Phase 2: Optimization (Weeks 3-4)
1. Implement tagging strategy for pipeline resources
2. Configure appropriate monitoring and alerting
3. Optimize parallel job allocation based on usage patterns
4. Create usage dashboards for ongoing monitoring

### Phase 3: Governance (Weeks 5-6)
1. Implement pipeline usage policies and guidelines
2. Create approval workflows for new pipeline resources
3. Set up regular cost review meetings
4. Establish optimization feedback loops

## Key Questions for Kickoff Meeting

1. **"What is our current parallel job configuration for Microsoft-hosted and self-hosted agents, and how was this determined?"**
   
   This establishes your baseline understanding of the current setup and helps identify if it was strategically planned or organically evolved.

2. **"What are our most resource-intensive pipelines, and do we have metrics on their usage patterns and bottlenecks?"**

   This helps identify immediate optimization opportunities and understand which pipelines contribute most to costs.

3. **"How are we currently allocating pipeline costs across projects and teams, and is there a chargeback model in place?"**

   This identifies the financial governance structure and helps determine if costs are properly attributed.

4. **"What is the projected pipeline usage increase during the migration phases, and have we provisioned sufficient capacity?"**

   This addresses capacity planning for the migration project specifically, ensuring sufficient resources.

5. **"Are we currently experiencing queue delays or capacity constraints that impact development velocity?"**

   This identifies current pain points that need immediate addressing as part of the FinOps implementation.

6. **"Have we analyzed the cost-efficiency of our current mix of Microsoft-hosted vs. self-hosted agents?"**

   This questions the optimization of the fundamental cost structure for pipelines.

## Access Request Template

```
To: [Azure DevOps Administrator]
Subject: Access Request for Azure Pipelines Usage Monitoring

As the FinOps specialist implementing cost management for our Azure DevOps migration, I require access to monitor and optimize our Azure Pipelines usage:

1. Azure DevOps Organization Access:
   - Organization: [Organization Name]
   - Access needed: Organization Settings > Pipelines > Parallel Jobs
   - Purpose: Monitor parallel job usage and configuration

2. Azure Portal Access:
   - Subscription: [Subscription ID used for Azure DevOps billing]
   - Role: Cost Management Contributor
   - Purpose: Access pipeline cost data and configure alerts

This access is essential for tracking system usage costs as outlined in our FinOps implementation project requirements.

Thank you for your assistance.
```

## Additional Resources

- [Azure Pipelines Pricing](https://azure.microsoft.com/en-us/pricing/details/devops/azure-devops-services/)
- [Configure and pay for parallel jobs](https://learn.microsoft.com/en-us/azure/devops/pipelines/licensing/concurrent-jobs?view=azure-devops)
- [Pool consumption report](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/pool-consumption-report?view=azure-devops)
- [Monitor usage of Azure DevOps Services](https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/usage-monitoring?view=azure-devops)
- [Pipeline reports](https://learn.microsoft.com/en-us/azure/devops/pipelines/reports/pipelinereport?view=azure-devops)

---

*Document created: April 16, 2025*
