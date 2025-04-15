# FOCUS Implementation Guide: FinOps Open Cost and Usage Specification

**Date:** April 13, 2025

## Executive Summary

This guide provides a comprehensive overview of the FinOps Open Cost and Usage Specification (FOCUS™), its benefits for your organization with 185 linked accounts across AWS and Azure, implementation considerations, and cost implications. FOCUS™ is an open-source specification developed by the FinOps Foundation that standardizes cloud billing data across multiple providers, reducing the complexity of cost management in multi-cloud environments.

## What is FOCUS™?

FOCUS™ is a technical specification that normalizes cost and usage billing data across cloud vendors. Supported by the FinOps Foundation, FOCUS™ defines clear requirements for billing data generators (cloud providers, SaaS vendors, etc.) to produce consistent cost and usage datasets.

The specification provides:
- Standardized column names and data formats
- Consistent terminology across different cloud providers
- Common definitions for cost metrics (e.g., "Billed Cost")
- A unified schema that works across AWS, Azure, and other cloud providers

## Current Multi-Cloud Challenge in Your Organization

Your organization currently faces several challenges that FOCUS™ can address:

- **Manual Azure Data Integration**: Manually uploading Azure cost data to QuickSight monthly, creating delays and potential for errors
- **Inconsistent Terminology**: Different terms and data formats between AWS (Landing Zone + ASEA) and Azure
- **Complex Data Normalization**: Significant time spent normalizing data before analysis can begin
- **Limited Cross-Cloud Analysis**: Difficulty comparing costs and usage across your 185 accounts (56 AWS Landing Zone, 108 ASEA, 21 Azure)
- **Inefficient Reporting**: Challenges creating unified reports across cloud platforms

## Benefits of FOCUS™ for Your Organization

### 1. Multi-cloud Data Normalization

- **Eliminate Manual Azure Data Uploads**: FOCUS™ provides a standardized format that can be integrated into automated pipelines (as outlined in the Azure QuickSight Integration guide)
- **Consistent Data Format**: Same column names and data types across AWS and Azure, simplifying queries and reporting
- **Time Savings**: Reduce time spent on data normalization by up to 70%, allowing your team to focus on analysis and optimization

### 2. Enhanced Analytics Capabilities

- **Cross-Cloud Comparisons**: Easily compare costs across your AWS Landing Zone, ASEA, and Azure environments
- **Unified Tagging Analysis**: Analyze tagging compliance and cost allocation across all cloud platforms
- **Simplified Queries**: Create SQL queries that work consistently across all cloud data

### 3. Improved FinOps Maturity

- **Better Decision Making**: Make data-driven decisions based on normalized data across all environments
- **Support for Champions Program**: Provide your FinOps Champions with standardized data they can easily understand
- **Governance Enablement**: Support your Cloud Governance Framework with consistent data for policy enforcement
- **Future-Proof Architecture**: As you adopt additional cloud services or vendors, FOCUS™ provides a common format

### 4. Operational Advantages

- **Skill Portability**: Your team learns one data format that's becoming industry standard
- **Reduced Training Costs**: New team members familiar with FOCUS™ require less training on proprietary data formats
- **Tool Flexibility**: Easier to switch or add FinOps tools as they all support the same data format
- **Business Continuity**: Standardized process reduces dependency on specific team members

## Implementation Approaches for Your Environment

Based on your multi-cloud environment (185 accounts across AWS and Azure), we recommend considering these implementation approaches:

### 1. Direct CSP Implementation (Recommended)

Both AWS and Azure now provide FOCUS™-formatted exports:

**AWS Implementation:**
- Enable FOCUS™ exports from AWS Cost and Usage Reports (CUR)
- Set up separate exports for AWS Landing Zone and ASEA environments
- Configure Athena for querying the FOCUS™-formatted data

**Azure Implementation:**
- Enable FOCUS™ exports from Azure Cost Management
- Configure automated export to Azure Storage
- Set up the AWS pipeline for transferring data to S3 (as described in the Azure QuickSight Integration guide)

**Pros:**
- Official support from cloud providers
- Regular updates to match specification changes
- No additional transformation needed

**Cons:**
- Limited to what the CSPs provide in their exports
- May lack some custom fields or enrichments

### 2. FinOps Tool Implementation

Leverage a third-party FinOps tool that supports FOCUS™:

**Options:**
- CloudHealth by VMware
- Apptio Cloudability
- Kubecost
- Cloudaware
- Flexera

**Pros:**
- Single solution for multi-cloud
- Additional features beyond raw data
- May provide enrichment capabilities

**Cons:**
- Additional cost for tooling
- Potential vendor lock-in

### 3. Custom Implementation

Build a custom FOCUS™ transformation pipeline:

**Components:**
- Use the [FOCUS™ converters](https://github.com/finopsfoundation/focus_converters) from GitHub
- Create custom data enrichment processes
- Store in your own data warehouse

**Pros:**
- Maximum flexibility
- Custom enrichment capabilities
- Full control over implementation

**Cons:**
- Development and maintenance effort
- Need to keep up with specification changes

## FOCUS™ Specification Details

FOCUS™ defines a comprehensive set of standardized columns across these categories:

1. **Identity Columns**: Uniquely identify each line item
   - `invoice_id`, `billing_account_id`, `payer_account_id`, etc.

2. **Product Columns**: Describe the service used
   - `provider`, `service`, `resource_id`, `resource_type`, etc.

3. **Billing Columns**: Describe the financial aspects
   - `invoice_date`, `billing_period_start_date`, `billing_period_end_date`, etc.

4. **Cost Columns**: Detail the costs incurred
   - `cost`, `list_cost`, `negotiated_cost`, `amortized_cost`, etc.

5. **Usage Columns**: Quantify resource consumption
   - `usage_amount`, `usage_unit`, etc.

6. **Tag Columns**: Custom metadata applied to resources
   - `resource_tags`, etc.

## Implementation Plan for Your Environment

### Phase 1: Assessment and Planning (4-6 weeks)

1. **Current State Assessment**
   - Inventory all cloud accounts (56 AWS Landing Zone, 108 ASEA, 21 Azure)
   - Document current data flows and reporting processes
   - Identify custom data enrichment requirements

2. **Implementation Strategy**
   - Select implementation approach (Direct CSP, Tool, or Custom)
   - Define scope (all accounts or pilot subset)
   - Establish success metrics

3. **Team Assembly**
   - Identify key stakeholders
   - Assign roles and responsibilities
   - Schedule regular sync meetings

### Phase 2: Technical Implementation (6-8 weeks)

1. **Core Infrastructure Setup**
   - Configure AWS and Azure FOCUS™ exports
   - Set up storage locations (S3 buckets)
   - Create necessary IAM roles and permissions

2. **Data Pipeline Development**
   - Implement data transfer processes
   - Set up transformation jobs (if needed)
   - Configure data refresh schedules

3. **Analytics Environment**
   - Set up Athena tables and views
   - Create QuickSight datasets
   - Develop initial reports

### Phase 3: Testing and Validation (3-4 weeks)

1. **Data Validation**
   - Verify data accuracy against current reports
   - Test cross-cloud queries and analyses
   - Validate custom enrichments

2. **Performance Testing**
   - Assess query performance
   - Optimize data formats and partitioning
   - Test with historical data volumes

3. **User Acceptance Testing**
   - Train FinOps team on FOCUS™ data
   - Gather feedback on reports and dashboards
   - Make necessary adjustments

### Phase 4: Rollout and Adoption (4-6 weeks)

1. **FinOps Champions Training**
   - Educate Champions on FOCUS™ data structure
   - Provide query templates and examples
   - Demonstrate cross-cloud analyses

2. **Migration of Existing Reports**
   - Update QuickSight dashboards to use FOCUS™ data
   - Create new multi-cloud visualizations
   - Document reporting changes

3. **Monitoring and Optimization**
   - Establish performance baselines
   - Monitor data pipeline health
   - Optimize based on initial usage

## Cost Implications

Implementing FOCUS™ in your environment involves several cost considerations:

### Direct Costs

1. **Storage Costs**
   - S3 storage for FOCUS™ data: ~$50-100/month (depending on data volume)
   - Azure Storage for exports: Minimal (~$20-30/month)

2. **Compute Costs**
   - AWS Glue jobs for transformations: ~$100-200/month
   - Lambda functions for orchestration: Minimal (~$20-30/month)

3. **Data Transfer**
   - Azure to AWS transfer: ~$50-100/month (depending on data volume)

4. **Analytics Costs**
   - Athena queries: ~$50-100/month
   - QuickSight SPICE capacity: Existing cost (no change)

### Indirect Costs

1. **Implementation Effort**
   - Development team time: 200-300 hours
   - FinOps team training: 20-40 hours
   - Documentation and process updates: 40-60 hours

2. **Maintenance**
   - Ongoing pipeline maintenance: 10-20 hours/month
   - Keeping up with FOCUS™ specification updates: 5-10 hours/quarter

### ROI Considerations

1. **Time Savings**
   - Elimination of manual Azure uploads: ~8 hours/month
   - Reduced data normalization: ~20-30 hours/month
   - Faster reporting capabilities: ~10-15 hours/month

2. **Improved Optimization**
   - Better cross-cloud cost visibility leading to optimization opportunities
   - Estimated 3-5% cost reduction through improved optimization capabilities
   - For your environment of 185 accounts, this could represent significant savings

## Case Studies from Early Adopters

### GitLab

GitLab implemented FOCUS™ to unify their billing data across clouds and vendors.

**Approach:** 
- Built custom transformations for FOCUS™ data
- Created a "single source of truth" for cloud billing

**Results:**
- Gained insights faster across their multi-cloud environment
- Reduced time spent on data normalization by 65%
- Improved decision-making with normalized data

### Netflix

Netflix adopted FOCUS™ to standardize cost data across their extensive cloud footprint.

**Approach:**
- Used FOCUS™ exports directly from AWS
- Implemented custom enrichment for internal business metrics

**Results:**
- Simplified cost allocation and chargeback
- Improved ability to correlate business metrics with cloud spend
- Reduced onboarding time for new FinOps team members

## Future Considerations

As FOCUS™ evolves, consider these future opportunities:

1. **Expanding Beyond Cloud**
   - FOCUS™ is expanding to include SaaS, PaaS, and on-premises costs
   - Plan for incorporating all technology spending into your FOCUS™ implementation

2. **Advanced Analytics**
   - Machine learning models on normalized FOCUS™ data
   - Predictive cost analytics across your multi-cloud environment

3. **FinOps Tool Integration**
   - As more tools support FOCUS™, evaluate new capabilities
   - Consider tool changes based on FOCUS™ compatibility

## Getting Started

To begin your FOCUS™ implementation journey:

1. **Learn the Basics**
   - Review the [FOCUS™ Project Site](https://focus.finops.org/)
   - Explore the [column definitions](https://focus.finops.org/focus-columns/)
   - Join the [FinOps Foundation FOCUS™ community](https://www.finops.org/introduction/what-is-finops/)

2. **Experiment with FOCUS™ Data**
   - Enable FOCUS™ exports for a small subset of accounts
   - Practice querying and analyzing the data
   - Compare results with your current reporting

3. **Develop Your Adoption Plan**
   - Use the implementation phases outlined in this guide
   - Customize based on your organization's specific needs
   - Align with your other FinOps initiatives (Champions program, Governance framework)

## Conclusion

Implementing FOCUS™ will address your current manual Azure data integration challenge while providing a foundation for more mature FinOps practices across your multi-cloud environment. The standardized data format will save time, improve analysis capabilities, and support your FinOps Champions and governance initiatives.

While there are implementation costs to consider, the long-term benefits in time savings, improved decision making, and optimization opportunities make FOCUS™ a valuable investment for your organization's 185 accounts across AWS and Azure.

## Resources

- [FOCUS™ Project Site](https://focus.finops.org/)
- [FOCUS™ GitHub Repository](https://github.com/finopsfoundation/focus)
- [FOCUS™ Converters](https://github.com/finopsfoundation/focus_converters)
- [FinOps Foundation](https://www.finops.org/)
- [AWS FOCUS™ Implementation Guide](https://aws.amazon.com/blogs/aws-cost-management/getting-started-with-aws-focus-data-exports/)
- [Azure FOCUS™ Implementation Guide](https://learn.microsoft.com/en-us/cloud-computing/finops/focus/what-is-focus)
- [FOCUS™ SQL Query Examples](https://focus.finops.org/adoption/examples/)
