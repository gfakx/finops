# Azure DevOps System Hosting - FinOps Guide

## Overview
This document outlines the approach to managing and tracking infrastructure costs for Azure DevOps Server on-premises environments as part of the FinOps implementation for the Azure DevOps migration project. It covers the infrastructure components, cost tracking methodologies, optimization opportunities, and governance considerations during the migration process.

## Understanding System Hosting for Azure DevOps

### Infrastructure Components

#### Application Tier
- **Web Front-End Servers**: Hosts the web interfaces and REST APIs
- **Application Servers**: Processes application logic and workflows
- **Search Servers**: Elasticsearch instances for code search functionality
- **Build/Release Agents**: Servers running pipeline jobs

#### Data Tier
- **SQL Server Instances**: Databases for Azure DevOps collections
- **Reporting Servers**: SQL Server Reporting Services (if implemented)
- **Analysis Services**: For analytics and warehouse data

#### Supporting Infrastructure
- **Load Balancers**: For high-availability configurations
- **Proxy Servers**: For geographically distributed teams
- **Backup Infrastructure**: For disaster recovery
- **Monitoring Systems**: For health and performance tracking

### Hardware Requirements Based on Scale

| Deployment Size | Server Configuration | Recommended for |
|-----------------|----------------------|-----------------|
| **Small (< 250 users)** | Single-server: dual-core CPU, 4-8GB RAM, fast storage | Small teams with limited automation |
| **Medium (250-500 users)** | App tier: dual-core CPU, 8GB RAM<br>Data tier: quad-core CPU, 16GB RAM, SSD | Teams with moderate automation needs |
| **Large (500-2000 users)** | App tier: quad-core CPU, 16GB+ RAM<br>Data tier: multiple quad-core CPUs, 16GB+ RAM, high-performance storage | Enterprise deployments with extensive automation |
| **Enterprise (2000+ users)** | Multiple app tiers: quad-core CPU, 16GB+ RAM<br>Clustered data tier: multiple servers, 32GB+ RAM, SAN storage | Large organizations with complex requirements |

*Note: These configurations should be tailored based on specific usage patterns, especially for teams with heavy build/release automation.*

### Software Licensing Components

- **Windows Server**: Operating system licenses for each server
- **SQL Server**: Standard or Enterprise editions (included with paid Azure DevOps Server licenses)
- **Azure DevOps Server**: Per-user or per-core licensing models
- **Visual Studio Subscriptions**: For development teams

## Cost Tracking Methodology

### Infrastructure Inventory Documentation

#### 1. Server Inventory Template
| Server Name | Role | Hardware Specifications | Operating System | SQL Version | Monthly Cost |
|-------------|------|------------------------|------------------|-------------|--------------|
| ADO-APP-01 | Application Tier | 4 cores, 16GB RAM, 500GB SSD | Windows Server 2019 | N/A | $XXX |
| ADO-DB-01 | Data Tier | 8 cores, 32GB RAM, 2TB SSD | Windows Server 2019 | SQL Server 2019 | $XXX |
| ADO-SEARCH-01 | Elasticsearch | 4 cores, 16GB RAM, 1TB SSD | Windows Server 2019 | N/A | $XXX |
| ADO-BUILD-01 | Build Agent | 8 cores, 32GB RAM, 1TB SSD | Windows Server 2019 | N/A | $XXX |

#### 2. Cost Category Breakdown
- **Hardware costs**: Amortized server hardware, storage, and networking equipment
- **Software licensing**: Windows Server, SQL Server, and Azure DevOps Server licenses
- **Datacenter costs**: Power, cooling, rack space, and physical security
- **Support costs**: Vendor support contracts and internal IT support allocation
- **Operational costs**: Backup, monitoring, and administrative overhead

### Cost Allocation Models

#### Direct Attribution
- Assign infrastructure costs directly to the teams or projects using Azure DevOps
- Useful when servers are dedicated to specific teams or business units

#### Consumption-Based
- Allocate costs based on usage metrics (storage consumed, build minutes, etc.)
- More accurate but requires detailed monitoring and metering

#### Equal Distribution
- Divide infrastructure costs equally among all teams or projects
- Simplest approach but doesn't reflect actual usage patterns

### Tracking On-Premises During Migration

#### Inventory and Baseline Documentation
1. **Current Infrastructure Map**: Document all existing components with specifications
2. **Cost Baseline**: Establish current monthly spending on infrastructure
3. **Utilization Baseline**: Document current usage patterns and resource utilization
4. **Depreciation Schedule**: Track remaining value of hardware assets

#### Ongoing Tracking During Migration
1. **Component Status Tracking**: Monitor which components remain active vs. decommissioned
2. **Parallel Costs Monitoring**: Track costs for both on-premises and cloud resources during transition
3. **Migration Progress Metrics**: Percentage of workloads migrated to the cloud
4. **Cost Trend Analysis**: Document cost changes throughout the migration process

## Temporary vs. Permanent Instance Considerations

### Temporary Instance Infrastructure
- **Purpose**: Supporting the migration process until ready to merge with enterprise instance
- **Lifecycle**: Temporary with planned decommissioning date
- **Cost Allocation**: Typically charged to the migration project rather than operations

### Managing Dual Environments
- **Synchronization Costs**: Additional infrastructure or tools needed to keep environments in sync
- **Parallel Operations**: Operational costs of maintaining both environments simultaneously
- **Governance Controls**: Processes to manage configuration changes across both environments
- **Cost Optimization**: Balancing performance needs with cost constraints for temporary infrastructure

### Transition Planning
- **Cutover Timeline**: Schedule for transitioning from temporary to permanent instance
- **Resource Ramp-down**: Plan for decommissioning temporary infrastructure
- **Cost Recovery**: Process for reclaiming value from decommissioned hardware
- **License Transfers**: Process for reassigning software licenses

## Migration Cost Management

### Migration Approaches and Cost Implications

#### Manual Migration
- **Lower infrastructure requirements**: Can be performed gradually with existing resources
- **Higher labor costs**: More time-intensive for the migration team
- **Longer parallel operation period**: Extended period of dual infrastructure costs

#### Data Migration Tool
- **Medium infrastructure needs**: May require additional temporary servers during migration
- **Moderate labor costs**: Semi-automated process with some manual intervention
- **Medium parallel operation period**: Structured approach with defined timeline

#### API-based Migration
- **Higher temporary infrastructure needs**: Custom synchronization and validation servers
- **Higher initial setup costs**: Development of custom migration tools
- **Potentially shorter parallel period**: Can enable more efficient cutover

### Cost Optimization Opportunities

#### During Migration
- **Staggered migration**: Decommission on-premises components as their cloud equivalents are established
- **Resource reclamation**: Repurpose servers from migrated workloads for remaining on-premises needs
- **License optimization**: Transfer licenses where possible to reduce duplicate costs

#### Post-Migration
- **Hardware disposition**: Sell or repurpose decommissioned hardware
- **License reassignment**: Transfer applicable licenses to other on-premises systems
- **Datacenter consolidation**: Reduce datacenter footprint as servers are decommissioned

## Azure IaaS vs. On-Premises Cost Comparison

### Total Cost of Ownership Components

#### On-Premises Costs
- **Capital expenses**: Hardware purchase, datacenter facilities
- **Operational expenses**: Power, cooling, physical space, IT management
- **Lifecycle costs**: Hardware refresh cycles (typically 3-5 years)
- **Scaling costs**: Provisioning for peak capacity needs

#### Azure IaaS Costs
- **Compute costs**: Virtual machine instances based on size and region
- **Storage costs**: Premium or standard storage for VMs and data
- **Networking costs**: VPN, Express Route, data transfer
- **Azure DevOps Server licensing**: Standard licensing fees
- **Reserved instance savings**: Discounts for 1-3 year commitments

### Cost Comparison Methodology
1. **Inventory current infrastructure**: Document specifications and utilization
2. **Map to equivalent Azure services**: Identify appropriate VM sizes and storage types
3. **Calculate on-premises TCO**: Include all direct and indirect costs
4. **Calculate Azure TCO**: Include all cloud service costs
5. **Compare 3-year and 5-year costs**: Account for hardware refresh cycles

### Azure Cost Management Tools
- **Azure Pricing Calculator**: Estimate future cloud costs
- **Azure Cost Management**: Track and analyze actual cloud spending
- **Azure Advisor**: Receive optimization recommendations
- **Azure TCO Calculator**: Compare on-premises vs. cloud costs

## Implementation Approach

### Phase 1: Assessment and Inventory (Weeks 1-2)
1. Document all on-premises infrastructure components
2. Calculate current infrastructure costs
3. Establish utilization baselines
4. Develop cost allocation model
5. Create infrastructure tracking dashboard

### Phase 2: Migration Planning (Weeks 3-4)
1. Define infrastructure needs for temporary instance
2. Create detailed migration schedule with cost projections
3. Establish cost tracking mechanisms for dual environments
4. Implement tagging strategy for IaaS resources
5. Define governance controls for migration period

### Phase 3: Cost Management During Migration (Weeks 5-10)
1. Monitor costs for both environments
2. Track decommissioning progress
3. Implement optimization recommendations
4. Regular cost review meetings
5. Update forecasts based on actual progress

### Phase 4: Post-Migration Optimization (Weeks 11-12)
1. Verify complete decommissioning of on-premises components
2. Document final cost baseline for cloud environment
3. Implement ongoing cost optimization practices
4. Create long-term cost management plan
5. Document lessons learned for future migrations

## Monitoring and Reporting

### Essential Infrastructure Metrics
- **Server utilization**: CPU, memory, and disk usage
- **SQL Server performance**: Query performance, database size growth
- **Network usage**: Bandwidth consumption, latency metrics
- **Build/release throughput**: Pipeline execution metrics
- **High availability metrics**: Failover occurrences, redundancy status

### Cost Monitoring Approach
- **Monthly infrastructure cost tracking**: Total cost and breakdown by component
- **Comparative analysis**: On-premises vs. cloud cost comparison
- **Migration progress financial metrics**: Cost savings realized through decommissioning
- **Optimization impact measurement**: Quantify savings from optimization initiatives

### Reporting Cadence
- **Weekly**: Migration progress and operational metrics
- **Monthly**: Comprehensive cost analysis and forecasting
- **Quarterly**: TCO reassessment and long-term planning
- **Ad-hoc**: Issue-specific reporting as needed

## On-Premises Tracking Scope Considerations

### Is On-Premises Tracking In Scope?
**Yes, on-premises tracking is in scope for this cloud management project** for several key reasons:

1. **Migration Context**: Since this is a migration project from on-premises to cloud, understanding the current state and costs is essential for measuring migration success.

2. **Temporary/Permanent Instance Strategy**: Managing both temporary and permanent instances requires tracking costs across both on-premises and cloud environments.

3. **Complete Cost Picture**: Effective FinOps requires visibility into all costs, regardless of whether they're cloud or on-premises, to make optimal decisions.

4. **Cost Avoidance Calculation**: Measuring cost savings from migration requires a baseline of on-premises costs.

5. **Decommissioning Tracking**: Monitoring the retirement of on-premises components is a critical part of migration governance.

### On-Premises Tracking Methodology
The FinOps implementation should include:

1. **Baseline Documentation**: Current infrastructure inventory and costs
2. **Phased Tracking**: Track decommissioning progress against the migration plan
3. **Hybrid Cost Analysis**: Combined reporting of on-premises and cloud costs during transition
4. **TCO Comparison**: Regular assessment of actual costs against projected TCO

## Key Questions for Kickoff Meeting

### Critical Questions About System Hosting

1. **"What is our current infrastructure inventory for Azure DevOps Server, including all application tiers, data tiers, and supporting servers, and do we have current utilization metrics for these components?"**

   This question establishes your baseline understanding of the current environment. The answer will help you identify all components that need tracking during migration and establish utilization patterns that will inform cloud resource sizing.

2. **"What is the planned architecture for the temporary instance during migration, how will it interact with both the existing on-premises environment and the future enterprise instance, and what is the expected timeline for decommissioning?"**

   This addresses the core challenge of managing the temporary instance. Understanding the architecture and timeline is crucial for proper cost allocation and planning the decommissioning process.

3. **"What is our current process for tracking and allocating infrastructure costs, and how do we plan to attribute costs during the migration period when we'll be operating both on-premises and cloud resources?"**

   This question focuses on the financial governance aspects of the migration. The answer will help you establish or enhance cost allocation models and ensure proper financial tracking throughout the transition.

## Additional Resources

- [Azure DevOps Server Requirements](https://learn.microsoft.com/en-us/azure/devops/server/requirements?view=azure-devops-2022)
- [Azure Migration Center](https://azure.microsoft.com/en-us/migration/)
- [Azure Total Cost of Ownership (TCO) Calculator](https://azure.microsoft.com/en-us/pricing/tco/calculator/)
- [Azure DevOps Server to Azure DevOps Services Migration Overview](https://learn.microsoft.com/en-us/azure/devops/migrate/migration-overview?view=azure-devops)
- [Microsoft Cost Management Documentation](https://learn.microsoft.com/en-us/azure/cost-management-billing/)

---

*Document created: April 16, 2025*
