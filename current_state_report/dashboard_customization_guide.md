# Cloud Cost Dashboard Customization Guide

**Date:** April 12, 2025

## Executive Summary

This guide provides detailed instructions for optimizing your existing CUDOS (Cloud Usage Dashboard for Optimizing Spend) and CID (Cloud Intelligence Dashboard) implementations. By enhancing these dashboards with custom queries, calculated fields, and visualizations, you can unlock deeper insights into your cloud spending across 185 accounts spanning AWS Landing Zone, ASEA, and Azure environments.

## Current Dashboard Environment

Your organization currently uses:
- CUDOS and CID dashboards for cost reporting
- QuickSight for BI visualization
- AWS Glue and Athena for data processing
- CUR data stored in S3 buckets

Available QuickSight datasets:
- summary_view
- hourly_view
- ec2_running_cost
- s3_view
- compute_savings_plan_eligible_spend

## Dashboard Optimization Opportunities

### 1. Automated Anomaly Detection

#### Current Challenge
- Manual monitoring of cost changes using Cost Explorer
- Reactive approach to identifying cost spikes
- Delayed response to unexpected spending increases

#### Implementation Steps

1. **Create Anomaly Detection Athena Query**

```sql
WITH daily_costs AS (
    SELECT 
        line_item_usage_start_date AS usage_date,
        SUM(line_item_unblended_cost) AS daily_cost
    FROM 
        summary_view
    WHERE 
        line_item_usage_start_date >= DATE_ADD('day', -90, CURRENT_DATE)
    GROUP BY 
        line_item_usage_start_date
),
daily_stats AS (
    SELECT
        AVG(daily_cost) AS avg_cost,
        STDDEV(daily_cost) AS stddev_cost
    FROM
        daily_costs
    WHERE
        usage_date BETWEEN DATE_ADD('day', -90, CURRENT_DATE) AND DATE_ADD('day', -14, CURRENT_DATE)
)
SELECT
    dc.usage_date,
    dc.daily_cost,
    ds.avg_cost,
    ds.stddev_cost,
    CASE
        WHEN dc.daily_cost > (ds.avg_cost + 2 * ds.stddev_cost) THEN 'High Anomaly'
        WHEN dc.daily_cost < (ds.avg_cost - 2 * ds.stddev_cost) THEN 'Low Anomaly'
        ELSE 'Normal'
    END AS anomaly_status,
    (dc.daily_cost - ds.avg_cost) / ds.avg_cost * 100 AS percent_difference
FROM
    daily_costs dc
CROSS JOIN
    daily_stats ds
WHERE
    dc.usage_date >= DATE_ADD('day', -14, CURRENT_DATE)
ORDER BY
    dc.usage_date DESC;
```

2. **Create QuickSight Anomaly Alert Dashboard**
   - Create a new analysis using the query result dataset
   - Add line chart showing daily costs with reference lines for anomaly thresholds
   - Configure calculated field for anomaly visualization:
     ```
     ifelse(
         ${anomaly_status} = 'High Anomaly',
         'red',
         ifelse(
             ${anomaly_status} = 'Low Anomaly',
             'orange',
             'green'
         )
     )
     ```
   - Set up threshold alerts in QuickSight to notify when daily costs exceed thresholds

3. **Configure Email Alerts**
   - Set up QuickSight email reports to be sent daily
   - Configure threshold-based alerts for immediate notification
   - Include direct links to detailed cost analysis views

### 2. Project-Based Cost Attribution (Pre-Tagging Solution)

#### Current Challenge
- No cost allocation tags in place
- Difficult to attribute costs to specific projects or teams

#### Implementation Steps

1. **Create Account-to-Project Mapping Table**
   - Create an S3-hosted CSV file with structure:
     ```
     account_id,project_name,environment,team
     123456789012,project-a,production,team-alpha
     ...
     ```

2. **Configure Athena External Table**

```sql
CREATE EXTERNAL TABLE account_project_mapping (
    account_id STRING,
    project_name STRING,
    environment STRING,
    team STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
LOCATION 's3://your-bucket-name/mappings/'
TBLPROPERTIES ('skip.header.line.count'='1');
```

3. **Create Project Cost View in Athena**

```sql
CREATE OR REPLACE VIEW project_costs AS
SELECT
    m.project_name,
    m.environment,
    m.team,
    line_item_usage_account_id,
    DATE_TRUNC('month', line_item_usage_start_date) AS usage_month,
    SUM(line_item_unblended_cost) AS monthly_cost
FROM
    summary_view s
JOIN
    account_project_mapping m
ON
    s.line_item_usage_account_id = m.account_id
WHERE
    line_item_usage_start_date >= DATE_ADD('month', -6, CURRENT_DATE)
GROUP BY
    m.project_name,
    m.environment,
    m.team,
    line_item_usage_account_id,
    DATE_TRUNC('month', line_item_usage_start_date)
ORDER BY
    m.project_name,
    usage_month;
```

4. **Create Project Cost Dashboard in QuickSight**
   - Create a new analysis using the project_costs view
   - Add project hierarchical filters (Project → Environment → Team)
   - Create month-over-month comparison calculated field:
     ```
     ${monthly_cost} - previous(${monthly_cost})
     ```
   - Add trend visualizations and forecasting components

### 3. Resource Optimization Opportunities

#### Current Challenge
- Manual identification of optimization opportunities
- Inconsistent implementation of recommendations

#### Implementation Steps

1. **EC2 Rightsizing Recommendations Query**

```sql
WITH instance_metrics AS (
    SELECT
        line_item_resource_id,
        product_instance_type,
        pricing_term,
        line_item_usage_account_id,
        SUM(line_item_usage_amount) AS usage_hours,
        SUM(line_item_unblended_cost) AS total_cost,
        MAX(pricing_public_on_demand_cost) AS max_on_demand_hourly_cost
    FROM
        ec2_running_cost
    WHERE
        line_item_usage_start_date >= DATE_ADD('day', -30, CURRENT_DATE)
    GROUP BY
        line_item_resource_id,
        product_instance_type,
        pricing_term,
        line_item_usage_account_id
)
SELECT
    line_item_resource_id AS instance_id,
    product_instance_type AS instance_type,
    pricing_term,
    line_item_usage_account_id AS account_id,
    usage_hours,
    total_cost,
    max_on_demand_hourly_cost * usage_hours AS potential_cost,
    CASE
        WHEN pricing_term = 'OnDemand' AND usage_hours >= 24*30*0.7 THEN 'Reserved Instance Candidate'
        WHEN pricing_term = 'OnDemand' AND usage_hours >= 24*30*0.4 THEN 'Savings Plan Candidate'
        ELSE 'Keep as On-Demand'
    END AS recommendation_type,
    CASE
        WHEN pricing_term = 'OnDemand' AND usage_hours >= 24*30*0.7 THEN 
            (max_on_demand_hourly_cost * usage_hours) - (max_on_demand_hourly_cost * usage_hours * 0.6)
        WHEN pricing_term = 'OnDemand' AND usage_hours >= 24*30*0.4 THEN 
            (max_on_demand_hourly_cost * usage_hours) - (max_on_demand_hourly_cost * usage_hours * 0.72)
        ELSE 0
    END AS potential_savings
FROM
    instance_metrics
WHERE
    pricing_term = 'OnDemand'
ORDER BY
    potential_savings DESC;
```

2. **GP2 to GP3 Migration Candidates Query**

```sql
SELECT
    line_item_resource_id AS volume_id,
    line_item_usage_account_id AS account_id,
    product_volume_api_name AS volume_type,
    SUM(line_item_usage_amount) AS allocated_storage_GB_Month,
    SUM(line_item_unblended_cost) AS gp2_monthly_cost,
    -- GP3 pricing calculation (adjust according to your specific pricing)
    SUM(line_item_usage_amount) * 0.08 AS estimated_gp3_cost,
    SUM(line_item_unblended_cost) - (SUM(line_item_usage_amount) * 0.08) AS potential_monthly_savings
FROM
    summary_view
WHERE
    line_item_usage_start_date >= DATE_ADD('month', -1, CURRENT_DATE)
    AND product_volume_api_name = 'gp2'
    AND line_item_line_item_type = 'Usage'
GROUP BY
    line_item_resource_id,
    line_item_usage_account_id,
    product_volume_api_name
HAVING
    SUM(line_item_unblended_cost) > 10 -- Focus on volumes with material cost
ORDER BY
    potential_monthly_savings DESC
LIMIT 100;
```

3. **Create Optimization Dashboard in QuickSight**
   - Create a new analysis combining the recommendation datasets
   - Include calculated fields for total potential savings
   - Create visuals showing savings by recommendation type
   - Add filters for account and resource type
   - Include a "Top 10 Optimization Opportunities" widget

### 4. Multi-Cloud Cost Comparison

#### Current Challenge
- Siloed view of AWS (Landing Zone + ASEA) and Azure costs
- Difficult to compare spending trends across cloud providers

#### Implementation Steps

1. **Configure Azure Cost Export to Storage Account**
   - Set up daily exports of Azure cost data to a storage account
   - Configure an Azure function to transform and copy data to an S3 bucket

2. **Create Athena External Table for Azure Costs**

```sql
CREATE EXTERNAL TABLE azure_costs (
    date_of_usage DATE,
    subscription_id STRING,
    subscription_name STRING,
    resource_group STRING, 
    service_name STRING,
    resource_id STRING,
    resource_type STRING,
    tags MAP<STRING,STRING>,
    cost DOUBLE,
    currency STRING
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
   'separatorChar' = ',',
   'quoteChar' = '\"'
)
LOCATION 's3://your-bucket/azure-costs/'
TBLPROPERTIES ('has_encrypted_data'='false');
```

3. **Create Unified Cloud Cost View**

```sql
CREATE OR REPLACE VIEW multi_cloud_costs AS
-- AWS Costs
SELECT
    'AWS' AS cloud_provider,
    CASE 
        WHEN line_item_usage_account_id IN ('account1', 'account2', ...) THEN 'Landing Zone'
        ELSE 'ASEA'
    END AS account_group,
    line_item_usage_account_id AS account_id,
    DATE_TRUNC('month', line_item_usage_start_date) AS usage_month,
    product_region AS region,
    product_service_code AS service,
    SUM(line_item_unblended_cost) AS cost
FROM
    summary_view
WHERE
    line_item_usage_start_date >= DATE_ADD('month', -6, CURRENT_DATE)
GROUP BY
    line_item_usage_account_id,
    DATE_TRUNC('month', line_item_usage_start_date),
    product_region,
    product_service_code

UNION ALL

-- Azure Costs
SELECT
    'Azure' AS cloud_provider,
    'Azure' AS account_group,
    subscription_id AS account_id,
    DATE_TRUNC('month', date_of_usage) AS usage_month,
    'canada-central' AS region, -- Assuming most resources are in Canada Central
    service_name AS service,
    SUM(cost) AS cost
FROM
    azure_costs
WHERE
    date_of_usage >= DATE_ADD('month', -6, CURRENT_DATE)
GROUP BY
    subscription_id,
    DATE_TRUNC('month', date_of_usage),
    service_name;
```

4. **Create Multi-Cloud Dashboard in QuickSight**
   - Create a new analysis using the multi_cloud_costs view
   - Add trend visualization showing costs by cloud provider over time
   - Create service comparison visuals between AWS and Azure
   - Add calculated fields for month-over-month growth by provider
   - Create executive summary visualizations for total cloud spend distribution

## Implementation Plan

### Week 1: Assessment and Planning
- Review current CUDOS and CID implementations
- Document current QuickSight datasets and their structures
- Identify key stakeholders for dashboard requirements gathering
- Document specific KPIs and metrics needed for monitoring

### Week 2-3: Athena Query Development
- Develop and test anomaly detection queries
- Create project cost attribution view
- Implement optimization recommendation queries
- Set up multi-cloud cost comparison structure

### Week 4-5: QuickSight Dashboard Development
- Design anomaly detection dashboard
- Develop project cost attribution visualizations
- Create optimization opportunity dashboard
- Build multi-cloud comparison dashboard
- Configure calculated fields and parameters for interactive filtering

### Week 6: Alert Configuration and Testing
- Set up email alerts for cost anomalies
- Configure regular dashboard refresh schedules
- Test alerting thresholds with sample data
- Validate dashboard performance

### Week 7-8: Documentation and Training
- Create user guides for interpreting dashboards
- Develop training materials for FinOps champions
- Document data refresh procedures and query maintenance
- Build dashboard maintenance runbook

## Advanced Features to Consider

### 1. Automated Optimization Tracking

Create a dedicated dashboard to track the implementation and impact of cost optimization recommendations:

1. Create an "Optimization Tracker" table in S3/DynamoDB with fields:
   - recommendation_id
   - resource_id
   - recommendation_type
   - estimated_savings
   - implementation_status
   - implementation_date
   - actual_savings
   - team_responsible

2. Develop a QuickSight dashboard showing:
   - Implemented vs. pending recommendations
   - Actual savings vs. estimated savings
   - Top teams by optimization impact
   - Recommendations aging report

### 2. Forecasting Dashboard

Implement advanced forecasting capabilities using QuickSight ML insights:

1. Create forecasted spend by service using ML-powered insights
2. Develop spend forecast by project team
3. Implement "what-if" scenario modeling for major service changes
4. Configure budget tracking with forecast-to-budget variance alerts

### 3. Resource Utilization Dashboard

Combine cost data with CloudWatch metrics to build comprehensive utilization insights:

1. Import CloudWatch metrics for EC2, RDS, and other key services into Athena
2. Create utilization vs. cost efficiency view
3. Identify idle and underutilized resources
4. Develop rightsizing recommendations based on actual utilization patterns

## Conclusion

Optimizing your CUDOS and CID dashboards will provide significant improvements in cloud cost visibility, anomaly detection, and optimization opportunity identification. By implementing the recommendations in this guide, you'll transform your current manual monitoring approach into a proactive, data-driven FinOps practice.

The queries and visualizations outlined here serve as a foundation that can be customized to your organization's specific needs as your FinOps practice matures.
