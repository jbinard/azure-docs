---
title: Integrate Microsoft Sentinel and Azure Purview  | Microsoft Docs
description: This tutorial describes how to use the Microsoft Sentinel data connector and solution for Azure Purview to enable data sensitivity insights, create rules to monitor when classifications have been detected, and get an overview about data found by Azure Purview, and where sensitive data resides in your organization.
author: batamig
ms.topic: tutorial
ms.date: 02/08/2022
ms.author: bagol
---

# Tutorial: Integrate Microsoft Sentinel and Azure Purview (Public Preview)

> [!IMPORTANT]
>
> The *Azure Purview* solution is in **PREVIEW**. See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for additional legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.
>

[Azure Purview](/azure/purview/) provides organizations with visibility into where sensitive information is stored, helping prioritize at-risk data for protection.

Integrate Azure Purview with Microsoft Sentinel to help narrow down the high volume of incidents and threats surfaced in Microsoft Sentinel, and understand the most critical areas to start.

Start by ingesting your Azure Purview logs into Microsoft Sentinel through a data connector. Then use a Microsoft Sentinel workbook to view data such as assets scanned, classifications found, and labels applied by Azure Purview. Use analytics rules to create alerts for changes within data sensitivity.

Customize the Azure Purview workbook and analytics rules to best suit the needs of your organization, and combine Azure Purview logs with data ingested from other sources to create enriched insights within Microsoft Sentinel.

In this tutorial, you:

> [!div class="checklist"]
>
> * Install the Microsoft Sentinel solution for Azure Purview
> * Enable your Azure Purview data connector
> * Learn about the workbook and analytics rules deployed to your Microsoft Sentinel workspace with the Azure Purview solution

## Prerequisites

Before you start, make sure you have both a [Microsoft Sentinel workspace](quickstart-onboard.md) and [Azure Purview](/azure/purview/create-catalog-portal) onboarded, and that your user has the following roles:

- **An Azure Purview account [Owner](/azure/role-based-access-control/built-in-roles) or [Contributor](/azure/role-based-access-control/built-in-roles) role**, to set up diagnostic settings and configure the data connector.

- **A [Microsoft Sentinel Contributor](../role-based-access-control/built-in-roles.md#microsoft-sentinel-contributor) role**, with write permissions to enable data connector, view the workbook, and create analytic rules.

## Install the Azure Purview solution

The **Azure Purview** solution is a set of bundled content, including a data connector, workbook, and analytics rules configured specifically for Azure Purview data.

> [!TIP]
> Microsoft Sentinel [solutions](sentinel-solutions.md) can help you onboard Microsoft Sentinel security content for a specific data connector using a single process.

**To install the solution**

1. In Microsoft Sentinel, under **Content management**, select **Content hub** and then locate the **Azure Purview** solution.

1. At the bottom right, select **View details**, and then **Create**. Select the subscription, resource group, and workspace where you want to install the solution, and then review the data connector and related security content that will be deployed.

    When you're done, select **Review + Create** to install the solution.

For more information, see [About Microsoft Sentinel content and solutions](sentinel-solutions.md) and [Centrally discover and deploy out-of-the-box content and solutions](sentinel-solutions-deploy.md).


## Start ingesting Azure Purview data in Microsoft Sentinel

Configure diagnostic settings to have Azure Purview data sensitivity logs flow into Microsoft Sentinel, and then run an Azure Purview scan to start ingesting your data.

Diagnostics settings send log events only after a full scan is run, or when a change is detected during an incremental scan. It typically takes about 10-15 minutes for the logs to start appearing in Microsoft Sentinel.

> [!TIP]
> Instructions for enabling your data connector also available in Microsoft Sentinel, on the **Azure Purview** data connector page.
>

**To enable data sensitivity logs to flow into Microsoft Sentinel**:

1. Navigate to your Azure Purview account in the Azure portal and select **Diagnostic settings**.

    :::image type="content" source="media/purview-solution/diagnostics-settings.png" alt-text="Screenshot of an Azure Purview account Diagnostics settings page.":::

1. Select **+ Add diagnostic setting** and configure the new setting to send logs from Azure Purview to Microsoft Sentinel:

    - Enter a meaningful name for your setting.
    - Under **Logs**, select **DataSensitivityLogEvent**.
    - Under **Destination details**, select **Send to Log Analytics workspace**, and select the subscription and workspace details used for Microsoft Sentinel.

1. Select **Save**.

For more information, see [Connect Microsoft Sentinel to Azure, Windows, Microsoft, and Amazon services](connect-azure-windows-microsoft-services.md#diagnostic-settings-based-connections).

**To run an Azure Purview scan and view data in Microsoft Sentinel**:

1. In Azure Purview, run a full scan of your resources. For more information, see [Manage data sources in Azure Purview](/azure/purview/manage-data-sources).

1. After your Azure Purview scans have completed, go back to the Azure Purview data connector in Microsoft Sentinel and confirm that data has been received.

## View recent data discovered by Azure Purview

The Azure Purview solution provides two analytics rule templates out-of-the-box that you can enable, including a generic rule and a customized rule.

- The generic version, *Sensitive Data Discovered in the Last 24 Hours*, monitors for the detection of any classifications found across your data estate during an Azure Purview scan.
- The customized version, *Sensitive Data Discovered in the Last 24 Hours - Customized*, monitors and generates alerts each time the specified classification, such as Social Security Number, has been detected.

Use this procedure to customize the Azure Purview analytics rules' queries to detect assets with specific classification, sensitivity label, source region, and more. Combine the data generated with other data in Microsoft Sentinel to enrich your detections and alerts.

> [!NOTE]
> Microsoft Sentinel analytics rules are KQL queries that trigger alerts when suspicious activity has been detected. Customize and group your rules together to create incidents for your SOC team to investigate.
>

### Modify the Azure Purview analytics rule templates

1. In Microsoft Sentinel, under **Configuration** select **Analytics** > **Active rules**, and search for a rule named **Sensitive Data Discovered in the Last 24 Hours - Customized**.

    By default, analytics rules created by Microsoft Sentinel solutions are set to disabled. Make sure to enable the rule for your workspace before continuing:

    1. Select the rule, and then at the bottom right, select **Edit**.

    1. In the analytics rule wizard, at the bottom of the **General** tab, toggle the **Status** to **Enabled**.

1. On the **Set rule logic** tab, adjust the **Rule query** to query for the data fields and classifications you want to generate alerts for. For more information on what can be included in your query, see:

    - Supported data fields are the columns of the [PurviewDataSensitivityLogs](/azure/azure-monitor/reference/tables/purviewdatasensitivitylogs) table
    - [Supported classifications](/azure/purview/supported-classifications)

    Formatted queries have the following syntax: `| where {data-field} contains {specified-string}`.

    For example:

    ```Kusto
    PurviewDataSensitivityLogs
    | where Classification contains “Social Security Number”
    | where SourceRegion contains “westeurope”
    | where SourceType contains “Amazon”
    | where TimeGenerated > ago (24h)
    ```

1. Under **Query scheduling**, define settings so that the rules show data discovered in the last 24 hours. We also recommend that you set **Event grouping** to group all events into a single alert.

    :::image type="content" source="media/purview-solution/analytics-rule-wizard.png" alt-text="Screenshot of the analytics rule wizard defined to show data detected in the last 24 hours.":::

1. If needed, customize the **Incident settings** and **Automated response**  tabs. For example, in the **Incidents settings** tab, verify that **Create incidents from alerts triggered by this analytics rule** is selected.

1. On the **Review and update** tab, select **Save**.

For more information, see [Create custom analytics rules to detect threats](detect-threats-custom.md).

### View Azure Purview data in Microsoft Sentinel workbooks

In Microsoft Sentinel, under **Threat management**, select **Workbooks** > **My workbooks**, and locate the **Azure Purview** workbook deployed with the **Azure Purview** solution. Open the workbook and customize any parameters as needed.

:::image type="content" source="media/purview-solution/purview-workbook.png" alt-text="Screenshot of the Azure Purview workbook.":::

The Azure Purview workbook displays the following tabs:

- **Overview**: Displays the regions and resources types where the data is located.
- **Classifications**: Displays assets that contain specified classifications, like Credit Card Numbers.
- **Sensitivity labels**: Displays the assets that have confidential labels, and the assets that currently have no labels.

To drill down in the Azure Purview workbook:

- Select a specific data source to jump to that resource in Azure.
- Select an asset path link to show more details, with all the data fields shared in the ingested logs.
- Select a row in the **Data Source**, **Classification**, or **Sensitivity Label** tables to filter the Asset Level data as configured.

### Investigate incidents triggered by Azure Purview events

When investigating incidents triggered by the Azure Purview analytics rules, find detailed information on the assets and classifications found in the incident's **Events**.

For example:

:::image type="content" source="media/purview-solution/purview-incident.png" alt-text="Screenshot of an incident triggered by Purview events.":::

## Next steps

For more information, see:

- [Visualize collected data](get-visibility.md)
- [Create custom analytics rules to detect threats](detect-threats-custom.md)
- [Investigate incidents with Microsoft Sentinel](investigate-cases.md)
- [About Microsoft Sentinel content and solutions](sentinel-solutions.md)
- [Centrally discover and deploy Microsoft Sentinel out-of-the-box content and solutions (Public preview)](sentinel-solutions-deploy.md)
