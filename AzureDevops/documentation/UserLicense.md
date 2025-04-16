# Azure DevOps User Licensing Management Guide

## Overview
This document outlines the comprehensive approach to managing and tracking Azure DevOps user licensing costs as part of the FinOps implementation for the Azure DevOps migration project. It includes information on license types, access methods, cost tracking, and reporting recommendations.

## Azure DevOps License Types

### License Tiers
| License Type | Description | Cost Structure | Best For |
|--------------|-------------|----------------|----------|
| **Stakeholder** | Limited access for stakeholders who need basic functionality | Free | Business users, stakeholders, and occasional contributors |
| **Basic** | Standard access for most team members | Free for first 5 users, paid for 6+ users | Developers, project managers, and regular contributors |
| **Basic + Test Plans** | Includes full test management capabilities | Paid only (30-day free trial available) | Testers and QA professionals |
| **Visual Studio Subscriber** | Automatically detected on sign-in | Included with Visual Studio subscription | Developers with existing Visual Studio subscriptions |
| **GitHub Enterprise** | Automatically detected on sign-in | Included with GitHub Enterprise license | Developers with existing GitHub Enterprise license |

### License Characteristics
- **Stakeholder**: Limited to basic work item tracking and read-only access to most features
- **Basic**: Full access to Azure Boards, Repos, and Pipelines features
- **Basic + Test Plans**: Adds test case management, execution, and reporting features
- **Automatic detection**: VS Subscriptions and GitHub Enterprise licenses are automatically detected

## Accessing License Information

### Method 1: Through Azure DevOps Portal (Direct Access)
1. Navigate to: `https://dev.azure.com/{Your_Organization}`
2. Click on **Organization settings** (gear icon in bottom left)
3. Select **Users** from the left menu
4. View all users with their assigned access levels
5. Optional: Use the **Export users** function to download a CSV

This method provides:
- Complete list of all users in the organization
- Current license level assigned to each user
- Last access timestamp (useful for identifying inactive users)
- Group/team assignments

### Method 2: Through Azure Portal (Billing Access)
1. Log in to Azure Portal: `https://portal.azure.com`
2. Search for "Azure DevOps" and select "Azure DevOps organizations"
3. Select your organization from the list
4. Click "Open organization" to navigate to the Azure DevOps portal
5. Follow steps in Method 1 to access licensing details

### Method 3: Accessing Billing Information in Azure Portal
1. Navigate to Azure Portal: `https://portal.azure.com`
2. Go to **Subscriptions** and select the subscription used for Azure DevOps
3. Select **Cost analysis** from the left menu
4. Filter options:
   - **Service name** = "Azure DevOps" to see only DevOps charges
   - **Group by Meter subcategory** to see specific service costs
   - **Filter or group by tag "_organizationname_"** to separate by organization

## Current Access Status Assessment

### Access Required for FinOps Implementation
For comprehensive license management, the following access is needed:
1. Azure DevOps **Organization Administrator** role
2. Azure Subscription **Billing Reader** role (minimum)
3. Access to all Azure DevOps organizations covered by the FinOps implementation

### Access Troubleshooting
If you can see Azure DevOps costs in Cost Analysis but the Azure DevOps organizations appear empty:

1. **Permission Issue**: You may have billing access but not organization access
   - Request organization access from the current administrator
   - Specify required role: Reader (minimum) or Organization Administrator (preferred)

2. **Microsoft Entra ID Tenant Mismatch**
   - The Azure DevOps organization might be connected to a different Microsoft Entra ID tenant
   - Check your current directory in Azure Portal (profile picture > Switch directory)
   - Request administrator to add your account to the correct tenant

3. **Organization Identification**
   - Use Cost Analysis to identify organization names from "_organizationname_" tag
   - Request access to specific organizations identified in billing data

## License Management Best Practices

### 1. License Inventory Management
- Conduct monthly audit of all licenses
- Document license distribution by team/department
- Track license utilization rates (active vs. assigned)
- Monitor expiration dates for trial licenses

### 2. License Optimization Strategies
- Remove licenses from inactive users (30+ days inactive)
- Downgrade underutilized licenses (e.g., Basic + Test Plans to Basic)
- Implement a shared license pool for occasional users
- Leverage Visual Studio subscriptions where available

### 3. Cost Allocation Methods
- **Direct Attribution**: Assign license costs directly to departments/teams
- **User-Based Allocation**: Distribute costs based on user counts
- **Activity-Based Allocation**: Allocate costs based on usage patterns
- **Project-Based Allocation**: Distribute costs to specific projects

### 4. License Governance
- Establish approval workflow for new license requests
- Document license assignment/removal procedures
- Set up automated notifications for license changes
- Implement regular license utilization reviews

## Reporting and Monitoring

### Essential License Reports
1. **License Inventory Report**
   - Complete inventory of all licenses by type
   - Distribution by department/team
   - Month-over-month change tracking

2. **License Utilization Report**
   - Active vs. inactive license tracking
   - Last access timestamps
   - Utilization percentage by license type

3. **License Cost Analysis**
   - Cost per license type
   - Cost per department/team
   - Cost trend analysis

### Monitoring Approaches
1. **Azure Cost Management**
   - Configure budget alerts for license costs
   - Set up scheduled exports of cost data
   - Create custom cost views for licensing

2. **Power BI Dashboards**
   - Create license distribution dashboards
   - Set up department-specific views
   - Implement trend visualizations

## Implementation Steps for FinOps

### Phase 1: Discovery and Baseline (Weeks 1-2)
1. Secure necessary access to Azure DevOps organizations
2. Export complete user list with license assignments
3. Document current license distribution by department
4. Establish baseline costs for each license type
5. Identify initial optimization opportunities

### Phase 2: Optimization Implementation (Weeks 3-4)
1. Implement license reclamation process
2. Adjust license assignments based on actual usage
3. Document license policies and procedures
4. Configure automated reporting

### Phase 3: Ongoing Management (Week 5+)
1. Establish monthly license review process
2. Implement forecasting for future license needs
3. Create chargeback/showback reports for departments
4. Continuously monitor for optimization opportunities

## License Costs and Tracking for Temporary vs. Permanent Instances

### Tracking Temporary Instance Licenses
- Add "TemporaryInstance" tag to all temporary instance resources
- Set up separate cost tracking for temporary instance licenses
- Document planned lifecycle and transition plan
- Track migration progress from temporary to permanent instances

### Permanent Instance License Management
- Establish long-term license allocation strategy
- Implement optimization processes for sustainable cost management
- Set up regular review and right-sizing procedures
- Document license lifecycle management approach

## Current Cost Observations and Critical Questions

### Initial Cost Analysis Observations
Based on preliminary review of the Azure Cost Management data:

- **All licenses appear to be Basic tier** with no apparent discount structure applied
- **Meter subcategory shows "Azure Repos and Boards (Basic)"** indicating standard basic licenses
- **Daily costs are increasing steadily** suggesting continuous growth in license assignments
- **No commitment discounts are being applied** despite the increasing volume of licenses

### Understanding License Billing Cycles
Azure DevOps licenses can be billed in different cycles:

- **Monthly Billing**: Most flexible, typically highest per-unit price
- **Quarterly Billing**: 3-month commitment, may offer modest discounts
- **Annual Billing**: 12-month commitment, offers the greatest discounts (typically 5-20%)

The optimal strategy often involves a mixed approach with committed licenses for core team members and flexible monthly licenses for temporary contributors.

### Three Critical Questions for Kickoff Meeting

1. **"What is the current license management process, including who approves new licenses, how we track utilization, and how often licenses are reviewed?"**
   
   This establishes your baseline understanding of the current governance structure, reveals existing processes you can build upon, and identifies immediate gaps that need addressing.

2. **"How are licenses currently distributed between teams/departments, and what is the projected license growth during each phase of the migration project?"**

   This provides critical information for both cost tracking and forecasting, helping establish your baseline while understanding future needs essential for proper budgeting.

3. **"What is the strategy for license allocation between the temporary instance and the enterprise instance, and how will licenses be transitioned when the instances are merged?"**

   This addresses the core challenge in managing costs across both temporary and permanent instances, helping establish proper tagging strategies and plan for consolidation.

### Additional Critical Question Based on Cost Observations

**"I've observed from cost analysis that we're seeing daily increases in Basic license costs with no discount structure applied. What is driving this continuous growth in license usage, what is the projected user count over the next 6-12 months, and have we considered commitment-based discounts to optimize these growing costs?"**

This question acknowledges your preliminary data analysis, identifies a potential cost optimization opportunity, and seeks both explanations for the current trend and projections needed for future planning.

## Question Checklist for Kickoff Meeting

1. Who currently manages Azure DevOps license assignments?
2. Is there an existing license allocation policy?
3. How are departments/teams currently charged for licenses?
4. What is the process for requesting new licenses?
5. Are there any existing license optimization initiatives?
6. What is the planned user growth during the migration?
7. How will licenses be allocated between temporary and permanent instances?
8. What reporting frequency and format is required for license costs?
9. Are there specific license optimization targets?
10. Who needs access to license utilization and cost reports?

## Access Request Template

```
To: [Azure DevOps Administrator]
Subject: Access Request for Azure DevOps FinOps Implementation

As the FinOps specialist assigned to implement cost management for our Azure DevOps migration project, I require the following access:

1. Azure DevOps Organization Access:
   - Organizations: [List identified from Cost Analysis]
   - Access Level: Organization Administrator (or Project Collection Administrator)
   - Purpose: License management, cost optimization, and usage tracking

2. Azure Portal Access:
   - Subscription: [Subscription ID used for Azure DevOps billing]
   - Role: Billing Reader (minimum) or Cost Management Contributor (preferred)
   - Purpose: Access cost data, create reports, and configure cost alerts

This access is essential for implementing proper FinOps practices for our Azure DevOps migration, especially for tracking and optimizing user licensing costs as outlined in our project requirements.

Thank you for your assistance.
```

## Additional Resources

- [Azure DevOps Pricing Documentation](https://azure.microsoft.com/en-us/pricing/details/devops/azure-devops-services/)
- [Manage paid access for users](https://learn.microsoft.com/en-us/azure/devops/organizations/billing/buy-basic-access-add-users)
- [Set up billing for your organization](https://learn.microsoft.com/en-us/azure/devops/organizations/billing/set-up-billing-for-your-organization-vs)
- [Billing FAQs - Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/organizations/billing/billing-faq)

---

*Document created: April 16, 2025*  
*Last updated: April 16, 2025 (Added cost observations and critical kickoff questions)*
