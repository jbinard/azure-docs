---
title: Generate and download the Permissions analytics report in CloudKnox Permissions Management 
description: How to generate and download the Permissions analytics report in CloudKnox Permissions Management.
services: active-directory
author: Yvonne-deQ
manager: karenh444
ms.service: active-directory
ms.subservice: ciem
ms.workload: identity
ms.topic: how-to
ms.date: 02/23/2022
ms.author: v-ydequadros
---

# Generate and download the Permissions analytics report

> [!IMPORTANT]
> CloudKnox Permissions Management (CloudKnox) is currently in PREVIEW.
> Some information relates to a prerelease product that may be substantially modified before it's released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

This article describes how to generate and download the **Permissions analytics report** in CloudKnox Permissions Management (CloudKnox).

> [!NOTE]
> This topic applies only to Amazon Web Services (AWS) users.

## Generate the Permissions analytics report 

1. In the CloudKnox home page, select the **Reports** tab, and then select the **Systems Reports** subtab.

    The **Systems Reports** subtab displays a list of reports the **Reports** table.
1. Find **Permissions analytics report** in the list, and to download the report, select the down arrow to the right of the report name, or from the ellipses **(...)** menu, select **Download**.

    The following message displays: **Successfully started to generate on-demand report.**

1. For detailed information in the report, select the right arrow next to one of the following categories. Or, select the required category under the **Findings** column.

	- **AWS**
		- Inactive Identities
			- Users
			- Roles
			- Resources
			- Serverless Functions
		- Inactive Groups
		- Super Identities
			- Users
			- Roles
			- Resources
			- Serverless Functions
		- Over-Provisioned Active Identities
			- Users
			- Roles
			- Resources
			- Serverless Functions
		- PCI Distribution
		- Privilege Escalation
			- Users
			- Roles
			- Resources
		- S3 Bucket Encryption
			- Unencrypted Buckets
			- SSE-S3 Buckets
		- S3 Buckets Accessible Externally
		- EC2 S3 Buckets Accessibility
		- Open Security Groups
		- Identities That Can Administer Security Tools
			- Users
			- Roles
			- Resources
			- Serverless Functions
		- Identities That Can Access Secret Information
			- Users
			- Roles
			- Resources
			- Serverless Functions
		- Cross-Account Access
			- External Accounts
			- Roles That Allow All Identities
		- Hygiene: MFA Enforcement
		- Hygiene: IAM Access Key Age
		- Hygiene: Unused IAM Access Keys
		- Exclude From Reports
			- Users
			- Roles
			- Resources
			- Serverless Functions
			- Groups
			- Security Groups
			- S3 Buckets


1. Select a category and view the following columns of information:

	- **User**, **Role**, **Resource**, **Serverless function name**: Displays the name of the identity.
	- **Authorization system**: Displays the authorization system to which the identity belongs.
	- **Domain**: Displays the domain name to which the identity belongs.
	- **Permissions**: Displays the maximum number of permissions that the identity can be granted.
		- **Used**: Displays how many permissions that the identity has used.
		- **Granted**: Displays how many permissions that the identity has been granted.
	- **PCI**: Displays the permission creep index (PCI) score of the identity.
	- **Date last active on**: Displays the date that the identity was last active.
	- **Date created on**: Displays the date when the identity was created.



<!---## Add and remove tags in the Permissions analytics report

1. Select **Tags**.
1. Select one of the categories from the **Permissions analytics report**.
1. Select the identity name to which you want to add a tag. Then, select the checkbox at the top to select all identities.
1. Select **Add tag**.
1. In the **tag** column: 
    - To select from the available options from the list, select **Select a tag**.
    - To search for a tag, enter the tag name.
    - To create a new custom tag, select  **New custom tag**.
    - To create a new tag, enter a name for the tag and select **Create**.
    - To remove a tag, select **Delete**.

1. In the **Value (optional)** box, enter a value, if necessary.
1. Select **Save**.--->
 
## Next steps

- For information on how to view system reports in the **Reports** dashboard, see [View system reports in the Reports dashboard](cloudknox-product-reports.md).
- For a detailed overview of available system reports, see [View a list and description of system reports](cloudknox-all-reports.md).
- For information about how to generate and view a system report, see [Generate and view a system report](cloudknox-report-view-system-report.md).
- For information about how to create, view, and share a system report, see [Create, view, and share a custom report](cloudknox-report-view-system-report.md).
