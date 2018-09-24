---
title: Hybrid ETL with existing on-premises SSIS and Azure Data Factory v2
description: Hybrid ETL with existing on-premises SSIS and Azure Data Factory v2
author: alhieng
ms.date: 9/20/2018
---

# Hybrid ETL with existing on-premises SSIS and Azure Data Factory v2

This example scenario is relevant to organizations that need a cloud-based extract-transform-load (ETL) process and want to incorporate existing SQL Server Integration Services (SSIS) packages into their new cloud data workflow. Organizations often have already invested greatly in developing ETL packages using SSIS for specific tasks. Rewriting these packages can be a daunting task.  In addition, many on-premises SSIS packages may have dependencies on local resources, preventing migration to the cloud. 

Azure Data Factory v2 lets customers take advantage of their existing ETL packages while ending further investment in on-premises ETL development. This example discusses some potential use cases where existing SSIS packages can be leveraged as part of a new cloud data workflow using Azure Data Factory v2.

## Potential use cases

During data warehouse development, data professionals often encounter a variety of data source issues. One common issue is dirty data that prevents loading of data into the data warehouse. This may include data entry errors, multiple sources of reference data, missing values, duplication of records, typos, case differences, and similar problems. Resolving these issues requires a variety of tools and techniques to prepare the data prior to loading into a data warehouse.

Problematic data must be addressed immediately. Otherwise, this data will cause problems when the data is used later in downstream systems, reporting, and data science applications. Inconsistent data can skew your reports and cause bias that leads to unfavorable results. These problems exist in relational databases, delimited files, APIs, NoSQL databases, and data lakes. Dirty data exists across industries, so similar approaches can be used to solve these common problems.

Traditionally, SSIS has been the tool of choice for many SQL Server data professionals. As organizations embrace cloud technologies, many data professionals wonder how to migrate their on-premise SSIS packages into the cloud to leverage their existing work and minimize breaking changes in their current data processing jobs. To meet this need, the Integration Runtime (IR) in Azure Data Factory v2 now supports native execution of SSIS packages in Azure. 

## Architecture

![Architecture overview of a hybrid ETL process using Data Factory][architecture-diagram]

1. Data is sourced from Blob storage into Data Factory.
2. The Data Factory pipeline invokes a stored procedure to execute an SSIS job hosted on-premises via the Data Factory Integrated Runtime.
3. The data cleansing jobs are executed to prepare the data for downstream consumption
4. Once the data cleansing task completes successfully, a copy task is executed to load the clean data into Azure
5. The clean data is then loaded into tables in the SQL Data Warehouse.

### Components

* [Blob storage][docs-blob-storage] is used to store files and as a source for Data Factory to retrieve data.
* [SQL Server Integration Services][docs-ssis] contains the on-premises ETL packages used to execute task-specific workloads.
* [Azure Data Factory][docs-data-factory] is the cloud orchestration engine that takes data from multiple sources and combines, orchestrates, and load the data into a data warehouse.
* [SQL Data Warehouse][docs-sql-data-warehouse] is used to centralize data in the cloud for easy access using standard ANSI SQL queries.

### Alternatives

Data Factory could invoke data cleansing procedures implemented using other technologies, such as a Databricks notebook, Python script, or SSIS instance running in a virtual machine.

## Considerations

When working with hybrid cloud technologies, one of the most critical considerations is to determine where you want your data to live. Assuming you decided that your will use Azure Blob storage to become your data landing area.  Azure feature pack for Integration Services (SSIS) makes it easy for your SSIS packages to interact with cloud data sources.  The feature pack allows for your SSIS packages to push/pull from Azure Storage, Azure Data Lake Store, HDInsight, Azure DW, and much more. For more information on [SSIS Feature pack](https://docs.microsoft.com/en-us/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017).
When you are setting up the SSIS environment, ensure that the SSIS package can reach the SSIS Catalog database.  The SSIS DB stores the package configurations and is a critical component to this architecture. The database contains the package metadata and job logs.  For more info see SSIS Catalog.  For help setting up the Lift and Shift of SQL Integration Services workload.
Launching an SSIS package job can be performed using T-SQL and this feature can be used to launch a job using ADF.  This feature enables many hybrid ETL use cases with ADF being the cloud orchestration engine.   For more info see Run an SSIS package from SSMS with Transact-SQL.

When working with hybrid cloud technologies, one of the most critical considerations is to determine where you want your data to live. Assuming you decided that your will use Azure Blob storage to become your data landing area.  Azure feature pack for Integration Services (SSIS) makes it easy for your SSIS packages to interact with cloud data sources.  The feature pack allows for your SSIS packages to push/pull from Azure Storage, Azure Data Lake Store, HDInsight, Azure DW, and much more. For more information on [SSIS Feature pack](https://docs.microsoft.com/en-us/sql/integration-services/catalog/ssis-catalog?view=sql-server-2017).
When you are setting up the SSIS environment, ensure that the SSIS package can reach the SSIS Catalog database.  The SSIS DB stores the package configurations and is a critical component to this architecture. The database contains the package metadata and job logs.  For more info see SSIS Catalog.  For help setting up the [Lift and Shift of SQL Integration Services workload](https://docs.microsoft.com/en-us/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview?view=sql-server-2017).

Launching an SSIS package job can be performed using T-SQL and this feature can be used to launch a job using ADF.  This feature enables many hybrid ETL use cases with ADF being the cloud orchestration engine.   For more info see [Run an SSIS package from SSMS with Transact-SQL](https://docs.microsoft.com/en-us/sql/integration-services/ssis-quickstart-run-tsql-ssms?view=sql-server-2017).

### Availability, Scalability, and Security

> How do I need to think about managing, maintaining, and monitoring this long term?

> Are there any size considerations around this specific solution?  
> What scale does this work at?  
> At what point do things break or not make sense for this architecture?

> Are there any security considerations (past the typical) that I should know about this?

## Deploy this scenario

> (Optional if it doesn't make sense)
>
> Is there an example deployment that can show me this in action?  What would I need to change to run this in production?

## Pricing

> How much will this cost to run?  
> Are there ways I could save cost?  
> If it scales linearly, than we should break it down by cost/unit.  If it does not, why?  
> What are the components that make up the cost?  
> How does scale effect the cost  
> 
> Link to the pricing calculator with all of the components outlined.  If it makes sense, include a small/medium/large configurations.  Describe what needs to be changed as you move to larger sizes

We have provided three sample cost profiles based on amount of traffic you expect to get:

* [Small][small-pricing]: describe what a small implementation is.
* [Medium][medium-pricing]: describe what a medium implementation is.
* [Large][large-pricing]: describe what a large implementation is.

## Next Steps

> Where should I go next if I want to start building this?  
> Are there any reference architectures that help me build this?

## Related Resources

> Are there any relevant case studies or customers doing something similar?
> Is there any other documentation that might be useful?  

<!-- links -->
[architecture-diagram]: ./media/architecture-diagram-hybrid-etl-with-adf.png
[small-pricing]: https://azure.com/e/
[medium-pricing]: https://azure.com/e/
[large-pricing]: https://azure.com/e/
[availability]: /azure/architecture/checklist/availability
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability

[docs-blob-storage]: /azure/storage/blobs/
[docs-data-factory]: /azure/data-factory/introduction
[docs-resource-groups]: /azure/azure-resource-manager/resource-group-overview
[docs-ssis]: /sql/integration-services/sql-server-integration-services

