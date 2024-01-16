# ReadMe.md

## Overview

As of January 2023, AI Builder consumption can be viewed in the downloadable capacity reports from the Power Platform Admin Center as mentioned here:

[https://learn.microsoft.com/en-us/ai-builder/administer-consumption-report](https://learn.microsoft.com/en-us/ai-builder/administer-consumption-report)

Administrators can also allocate AI Builder Credits in the Power Platform admin center as mentioned here:

[https://learn.microsoft.com/en-us/ai-builder/credit-management#make-credits-available-for-an-environment-allocated-and-unallocated-credits](https://learn.microsoft.com/en-us/ai-builder/credit-management%23make-credits-available-for-an-environment-allocated-and-unallocated-credits)

Unfortunately both of the above experiences are not associated in one UI and there is no way to view allocation vs consumption and determine overages.

The Microsoft Center of Excellence Toolkit (COE Toolkit) enables Power Platform administrators to manage all things Power Platform across the enterprise for a specific tenant. As of January 2024 administrators can view AI Builder inventory in the COE Toolkit but not actual AI Builder consumption. The feature is currently being worked on by the COE Toolkit product team. Until then, the below solution will gather AI Builder consumption across all environments in your tenant and display consumption by AI Builder Model. Additionally, for every Environment that has AI Builder credits allocated, administrators can access a list of Environments with AI Builder overages.

## Installing the Solution

### Center of Excellence Toolkit Version

This solution requires at least the November 2023 version of the COE Toolkit.

### Environment

This solution must be installed in the same environment as your Center of Excellence Toolkit.

### Import Solution

1. Download the solution (AIBuilderConsumptionManagement_x_x_x_managed.zip) from the latest release: https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/releases
2. In the Power Platform maker experience, navigate to the target environment and import the solution.
3. Publish all customizations.
4. Ensure the following Power Automate Flow is turned on: AI Builder Consumption Tenant Update
5. Run the Power Automate Flow and wait for it to finish. This may take anywhere from 10 minutes to one hour.
6. The newly created field "AI Builder Overage" on the Environment table is a roll-up field. Because roll-up fields aggregate nightly you must run the System Job for this field manually if you want to see instant results. To update the System Job, navigate to the System Job area, search for "\*ai" (sans quotes), select the record as below and resume the job from the "More Actions" button as shown below.

![image](https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/assets/89024016/43369ace-7fd8-463c-b120-bb8891967e40)


## Security Roles Required

Please reference the following article for identifying what Security Roles are required to install and operate this solution.

[https://learn.microsoft.com/en-us/power-platform/guidance/coe/setup#what-identity-should-i-install-the-coe-starter-kit-with](https://learn.microsoft.com/en-us/power-platform/guidance/coe/setup%23what-identity-should-i-install-the-coe-starter-kit-with)

## License Requirements

Please reference the following article for identifying what licenses are required to install and operate this solution.

[https://learn.microsoft.com/en-us/power-platform/guidance/coe/setup#what-identity-should-i-install-the-coe-starter-kit-with](https://learn.microsoft.com/en-us/power-platform/guidance/coe/setup%23what-identity-should-i-install-the-coe-starter-kit-with)

## How to Use This Solution

### Configuring the AI Builder Consumption Management solution

Consumption overages can only be calculated by this solution on Environments that have AI Builder credits allocated through the Power Platform Admin View app. Although officially AI Builder Credits are allocated in the Power Platform Admin Center, they must be manually created in the COE Toolkit Dataverse tables as noted here:

1. Navigate to Environments from the Site Map.
2. Click into the respective Environment record.
3. Click on the "Capacity and Add-ons" tab.
4. Add a new Environment Add-on or update the existing one and set the Allocated amount.
5. Note that the Add-on record in question must have the "Add-on Type" field set to "AI" (sans quotes).

![image](https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/assets/89024016/91b9ae50-7bb9-45bf-818d-0dbd9a6f812c)

### Gathering AI Builder Consumption Telemetry

This solution consists of a Power Automate Flow that gathers AI Builder consumption telemetry and stores it in the Dataverse tables that maintain AI Builder Inventory in the COE TK environment. It runs on a nightly basis (midnight) and no user configuration is required.
However, for immediate results, perform the following two steps:
1. Run the Power Automate Flow "AI Builder Consumption Tenant Update" and wait for it to finish.
2. Run the System Job as per step 6 in the Import Solution section above.

The following Dataverse Tables are affected:

For each environment, for each record in "AI Model", a summation is generated from associated records in the "AI Events" table. The "AI Events" table is auto-created by the Power Platform and stores actual credits consumed by model by train/publish/run action per the AI Builder rate card.
In the COE Toolkit environment, the "AIBuilderModels" is updated with the above consumption for ever AI Builder Model. The respective "Environment Addons" table is updated with the total consumption for this Environment for all AI Builder Model consumptions.

Note that all consumption is calculated for the current month from the 1st day of the month. This is per the consumption approach identified in the licensing guide for AI Builder Credits.

### Viewing AI Builder Consumption Overages

In order to view Environments with overages on AI Builder Consumption open the Power Platform Admin View app installed with the COE Toolkit.

Navigate to "Environments" from the Site Map and change the View Selector to the View "Environments AI Builder Overages".

![image](https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/assets/89024016/58834acc-c1b6-4c92-8260-b23c84c2b8a0)


### Viewing AI Builder Consumption By Environment

In order to view AI Builder Consumption for an Environment and for a specific model, click into the Environment record and navigate to "Capacity and Add-ons" tab. Click on "AI" in the Add-Ons sub-grid to open up the record.

![image](https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/assets/89024016/c6ba7fea-86e6-4266-8548-f8f306c4f330)


The fields in "Calculated Actual Consumption" section show actual consumption for ALL AI Builder models since the beginning of the current month. This includes the overage if the Environment has a value in the Allocated field and the Consumption is greater than the Allocated capacity.

The sub-grid labeled "AI Builder Models This Environment" lists the AI Builder models in this Environment and the Credits consumed since the beginning of the current month.

![image](https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/assets/89024016/43d60f9f-6fb1-4fe8-9866-4c7b60e61632)

For additionall analysis, export the list of AI Builder Models for the current environment by clicking on the ellipses, select "Export AiBuilderModels" and then click ont he right-arrow for export options.

![image](https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/assets/89024016/285a3bc2-0ff3-4850-a00b-2a3b7019ca1f)

Chose "Open in Excel Online" for quicker access to the data view.

![image](https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/assets/89024016/79632f64-db42-4b85-a734-d575e2fbdcd6)


### Proactive Notifications of AI Builder Overages

The current version of this solution provides the table and field necessary to identify Environments with AI Builder Overages. For proactive notifications you can create a Power Automate Flow that filters on this field and emails users such as the Environment Owner or the Environment Admin.

![image](https://github.com/v7herman4/COE-TK-AI-Builder-Consumption/assets/89024016/459eea9d-6b02-4375-988a-e8b448664444)

