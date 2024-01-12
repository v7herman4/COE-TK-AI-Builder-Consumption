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

1. Download the solution from the [latest release.](https://github.com/v7herman4/Planner-Tasks-from-Message-Center/releases)
2. In the Power Platform maker experience, navigate to the target environment and import the solution.
3. Publish all customizations.
4. Ensure the following Power Automate Flow is turned on: AI Builder Consumption Tenant Update
5. Run the Power Automate Flow and wait for it to finish.
6. The newly created field "AI Builder Overage" on the Environment table is a roll-up field. Because roll-up fields aggregate nightly you must run the System Job for this field manually if you want to see instant results. To update the System Job, navigate to the System Job area, search for "\*ai" (sans quotes), select the record as below and resume the job from the "More Actions" button as shown below.

![](RackMultipart20240112-1-y86mqa_html_8d760976d94b22ac.png)

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
4. Add a new Environment Add-on or update the existing one and set the Allocated amount. Note that the Add-on record in question must have the "Add-on Type" field set to "AI" (sans quotes).

![](RackMultipart20240112-1-y86mqa_html_df2a6b7ba5f359e1.png)

### Gathering AI Builder Consumption Telemetry

This solution consists of a Power Automate Flow that gathers AI Builder consumption telemetry and stores it in the Dataverse tables that maintain AI Builder Inventory in the COE TK environment. It runs on a nightly basis (midnight) and no user configuration is required.

### Viewing AI Builder Consumption Overages

In order to view Environments with overages on AI Builder Consumption open the Power Platform Admin View app installed with the COE Toolkit.

Navigate to "Environments" from the Site Map and change the View Selector to the View "Environments AI Builder Overages".

![](RackMultipart20240112-1-y86mqa_html_c57ce5275f56bc6a.png)

### Viewing AI Builder Consumption By Environment

In order to view AI Builder Consumption for an Environment and for a specific model, click into the Environment record and navigate to "Capacity and Add-ons" tab. Click on "AI" in the Add-Ons sub-grid to open up the record.

![](RackMultipart20240112-1-y86mqa_html_1af7a3297cac5b8b.png)

The fields in "Calculated Actual Consumption" section show actual consumption for ALL AI Builder models since the beginning of the current month. This includes the overage if the Environment has a value in the Allocated field and the Consumption is greater than the Allocated capacity.

The sub-grid labeled "AI Builder Models This Environment" lists the AI Builder models in this Environment and the Credits consumed since the beginning of the current month.

![](RackMultipart20240112-1-y86mqa_html_e7954fe094f8f9ae.png)

### Proactive Notifications of AI Builder Overages

The current version of this solution provides the table and field necessary to identify Environments with AI Builder Overages. For proactive notifications you can create a Power Automate Flow that filters on this field and emails users such as the Environment Owner or the Environment Admin.

![](RackMultipart20240112-1-y86mqa_html_7bdbbee32809ab4d.png)
