---
title: Create plans for a virtual machine offer on Azure Marketplace
description: Create plans for a virtual machine offer on Azure Marketplace.
ms.service: marketplace 
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 12/07/2021
---

# Create plans for a virtual machine offer

On the **Plan overview** page (select from the left-nav menu in Partner Center) you can provide a variety of plan options within the same offer. An offer requires at least one plan (formerly called a SKU), which can vary by monetization audience, Azure region, features, or virtual machine (VM) images.

You can create up to 100 plans for each offer and up to 45 of these plans can be private. Learn more about private plans in [Private offers in the Microsoft commercial marketplace](private-offers.md).

After you create your plans, select the **Plan overview** tab to display:

- Plan names
- License models
- Audience (public or private)
- Current publishing status
- Available actions

The actions available on this pane vary depending on the current status of your plan.

- If the plan status is a draft, you can **Delete draft**.
- If the plan status is published live, you can either **Deprecate plan** or **Sync private audience**.

## Create a new plan

Select **+ Create new plan** at the top.

In the **New plan** dialog box, enter a unique **Plan ID** for each plan in this offer. This ID will be visible to customers in the product web address. Use only lowercase letters and numbers, dashes, or underscores, and a maximum of 50 characters.

> [!NOTE]
> The plan ID can't be changed after you select **Create**.

Enter a **Plan name**. Customers see this name when they're deciding which plan to select within your offer. Create a unique name that clearly points out the differences between plans. For example, you might enter **Windows Server** with *Pay-as-you-go*, *BYOL*, *Advanced*, and *Enterprise* plans.

Select **Create**. The **Plan setup** page appears.

## Plan setup

Set the high-level configuration for the type of plan, specify whether it reuses a technical configuration from another plan, and identify the Azure regions where the plan should be available. Your selections here determine which fields are displayed on other panes for the same plan.

### Azure regions

Your plan must be made available in at least one Azure region.

Select **Azure Global** to make your plan available to customers in all Azure Global regions that have commercial marketplace integration. For more information, see [Geographic availability and currency support](marketplace-geo-availability-currencies.md).

Select **Azure Government** to make your plan available in the [Azure Government](../azure-government/documentation-government-welcome.md) region. This region provides controlled access for customers from US federal, state, local, or tribal entities, and for partners who are eligible to serve them. You, as the publisher, are responsible for any compliance controls, security measures, and best practices. Azure Government uses physically isolated datacenters and networks (located in the US only).

Before you publish to [Azure Government](../azure-government/documentation-government-manage-marketplace-partners.md), test and validate your plan in the environment, because certain endpoints may differ. To set up and test your plan, request a trial account from the [Microsoft Azure Government trial](https://azure.microsoft.com/global-infrastructure/government/request/) page.

> [!NOTE]
> After your plan is published and available in a specific Azure region, you can't remove that region.

### Azure Government certifications

This option is visible only if you selected **Azure Government** as the Azure region in the preceding section.

Azure Government services handle data that's subject to certain government regulations and requirements. For example, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4, and CJIS. To bring awareness to your certifications for these programs, you can provide up to 100 links that describe them. These can be either links to your listing on the program directly or links to descriptions of your compliance with them on your own websites. These links are visible to Azure Government customers only.

Select **Save draft** before continuing to the next tab in the left-nav Plan menu, **Plan listing**.

## Plan listing

Configure the listing details of the plan. This pane displays specific information, which can differ from other plans in the same offer.

### Plan name

This field is automatically filled with the name that you gave your plan when you created it. This name appears on Azure Marketplace as the title of this plan. It is limited to 100 characters.

### Plan summary

Provide a short summary of your plan, not the offer. This summary is limited to 100 characters.

### Plan description

Describe what makes this software plan unique, and describe any differences between plans within your offer. Describe the plan only, not the offer. The plan description can contain up to 2,000 characters.

Select **Save draft** before continuing to the next tab in the left-nav Plan menu, **Pricing and availability**.

## Pricing and availability

On this pane, you configure:

- Markets where this plan is available. Every plan must be available in at least one [market](marketplace-geo-availability-currencies.md).
- The price per hour.
- Whether to make the plan visible to everyone or only to specific customers (a private audience).

### Markets

Every plan must be available in at least one market. Most markets are selected by default. To edit the list, select **Edit markets** and select or clear check boxes for each market location where this plan should (or shouldn't) be available for purchase. Users in selected markets can still deploy the offer to all Azure regions selected in the ["Plan setup"](#plan-setup) section.

Select **Select only Microsoft Tax Remitted** to select only countries/regions in which Microsoft remits sales and use tax on your behalf. Publishing to China is limited to plans that are either *Free* or *Bring-your-own-license* (BYOL).

If you've already set prices for your plan in US dollar (USD) currency and add another market location, the price for the new market is calculated according to current exchange rates. Always review the price for each market before you publish. Review your pricing by selecting **Export prices (xlsx)** after you save your changes.

When you remove a market, customers from that market who are using active deployments will not be able to create new deployments or scale up their existing deployments. Existing deployments are not affected.

Select **Save** to continue.

### Pricing

For the **License model**, select **Usage-based monthly billed plan** to configure pricing for this plan, or **Bring your own license** to let customers use this plan with their existing license. 

For a usage-based monthly billed plan, Microsoft will charge the customer for their hourly usage and they are billed monthly. This is our _Pay-as-you-go_ plan, where customers are only billed for the hours that they've used. When you select this plan, choose one of the following pricing options:

- **Free** – Your VM offer is free.
- **Flat rate (recommended)** – Your VM offer is the same hourly price regardless of the hardware it runs on.
- **Per core** – Your VM offer pricing is based on per CPU core count. You provide the price for one CPU core and we’ll increment the pricing based on the size of the hardware.
- **Per core size** – Your VM offer is priced based on the number of CPU cores on the hardware it's deployed on.
- **Per market and core size** – Assign prices based on the number of CPU cores on the hardware it's deployed on, and also for all markets. Currency conversion is done by you, the publisher. This option is easier if you use the import pricing feature.

For **Per core size** and **Per market and core size**, enter a **Price per core**, and then select **Generate prices**. The tables of price/hour calculations are populated for you. You can then adjust the price per core, if you choose. If using the _Per market and core size_ pricing option, you can additionally customize the price/hour calculation tables for each market that’s selected for this plan.

> [!NOTE]
> To ensure that the prices are right before you publish them, export the pricing spreadsheet and review the prices in each market. Before you export pricing data, first select **Save draft** near the bottom of the page to save pricing changes.

Some things to consider when selecting a pricing option:
- For the first four options, Microsoft does the currency conversion.
- Microsoft suggests using a flat rate pricing for software solutions.
- Prices are fixed, so once the plan is published the prices cannot be adjusted. However, if you would like to reduce prices for your VM offers you can open a [support ticket](support.md).

### Free Trial

You can offer a one-month, three-month, or six-month **Free Trial** to your customers.

### Plan visibility

You can design each plan to be visible to everyone or only to a preselected audience. Assign memberships in this restricted audience by using Azure tenant IDs, subscription IDs, or both.

**Public**: Your plan can be seen by everyone.

**Private**: Make your plan visible only to a preselected audience. After it's published as a private plan, you can update the audience or change it to public. After you make a plan public, it must remain public. It can't be changed back to a private plan.

Assign the audience that will have access to this private plan using *Azure tenant IDs*, *subscription IDs*, or both. Optionally, include a **Description** of each Azure tenant ID or subscription ID that you assign. Add up to 10 subscription IDs and tenant IDs manually or import a CSV spreadsheet if more than 10 IDs are required. For a published offer, select **Sync private audience** for the changes to the private audience to take effect automatically without needing to republish the offer.

> [!NOTE]
> A private or restricted audience is different from the preview audience that you defined on the **Preview** pane. A preview audience can access your offer *before* it's published live to Azure Marketplace. Although the private audience choice applies only to a specific plan, the preview audience can view all private and public plans for validation purposes.

Private offers are not supported with Azure subscriptions established through a reseller of the Cloud Solution Provider program (CSP).

### Hide plan

If your virtual machine is meant to be used only indirectly when it's referenced through another solution template or managed application, select this check box to publish the virtual machine but hide it from customers who might be searching or browsing for it directly.

Any Azure customer can deploy the offer using either PowerShell or CLI.  If you wish to make this offer available to a limited set of customers, then set the plan to **Private**.

Hidden plans do not generate preview links. However, you can test them by [following these steps](azure-vm-create-faq.yml#how-do-i-test-a-hidden-preview-image-).

Select **Save draft** before continuing to the next tab in the left-nav Plan menu, **Technical configuration**.

## Technical configuration

Provide the images and other technical properties associated with this plan.

### Reuse technical configuration

This option lets you use the same technical configuration settings across plans within the same offer and therefore leverage the same set of images. If you enable the _reuse technical configuration_ option, your plan will inherit the same technical configuration settings as the base plan you select.  When you change the base plan, the changes are reflected on the plan reusing the configuration.

Some common reasons for reusing the technical configuration settings from another plan include:

1. The same images are available for both *Pay as you go* and *BYOL*.
2. To reuse the same technical configuration from a public plan for a private plan with a different price. 
3. Your solution behaves differently based on the plan the user chooses to deploy. For example, the software is the same, but features vary by plan.

Leverage [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md) (IMDS) to identify which plan your solution is deployed within to validate license or enabling of appropriate features.

If you later decide to publish different changes between your plans, you can detach them. Detach the plan reusing the technical configuration by deselecting this option with your plan. Once detached, your plan will carry the same technical configuration settings at the place of your last setting and your plans may diverge in configuration. A plan that has been published independently in the past cannot reuse a technical configuration later.

### Operating system

Select the **Windows** or **Linux** operating system family.

Select the Windows **Release** or Linux **Vendor**.

Enter an **OS friendly name** for the operating system. This name is visible to customers.

### Recommended VM sizes

Select the link to choose up to six recommended virtual machine sizes to display on Azure Marketplace.

### Open ports

Add open public or private ports on a deployed virtual machine.

### Properties

Here is a list of properties that can be selected for your VM.

- **Supports backup**: Enable this property if your images support Azure VM backup. Learn more about [Azure VM backup](../backup/backup-azure-vms-introduction.md).

- **Supports accelerated networking**: Enable this property if the VM images for this plan support single root I/O virtualization (SR-IOV) to a VM, enabling low latency and high throughput on the network interface. Learn more about [accelerated networking](https://go.microsoft.com/fwlink/?linkid=2124513).

- **Supports cloud-init configuration**: Enable this property if the images in this plan support cloud-init post deployment scripts. Learn more about [cloud-init configuration](../virtual-machines/linux/using-cloud-init.md).

- **Supports hotpatch**: Windows Server Azure Editions supports Hot Patch. Learn more about [Hot Patch](../automanage/automanage-hotpatch.md).

- **Supports extensions**: Enable this property if the images in this plan support extensions. Extensions are small applications that provide post-deployment configuration and automation on Azure VMs. Learn more about [Azure virtual machine extensions](./azure-vm-certification-faq.yml#vm-extensions).

- **Is a network virtual appliance**: Enable this property if this product is a Network Virtual Appliance. A network virtual appliance is a product that performs one or more network functions, such as a Load Balancer, VPN Gateway, Firewall or Application Gateway. Learn more about [network virtual appliances](https://go.microsoft.com/fwlink/?linkid=2155373).

- **Remote desktop or SSH disabled**: Enable this property if virtual machines deployed with these images don't allow customers to access it using Remote Desktop or SSH. Learn more about [locked VM images](./azure-vm-certification-faq.yml#locked-down-or-ssh-disabled-offer).

- **Requires custom ARM template for deployment**: Enable this property if the images in this plan can only be deployed using a custom ARM template. To learn more see the [Custom templates section of Troubleshoot virtual machine certification](./azure-vm-certification-faq.yml#custom-templates).

### Generations

Generating a virtual machine defines the virtual hardware it uses. Based on your customer’s needs, you can publish a Generation 1 VM, Generation 2 VM, or both.

1. When creating a new offer, select a **Generation type** and enter the requested details:

    :::image type="content" source="./media/create-vm/azure-vm-generations-image-details-1.png" alt-text="A view of the Generation detail section in Partner Center.":::

2. To add another generation to a plan, select **Add generation**...

    :::image type="content" source="./media/create-vm/azure-vm-generations-add.png" alt-text="A view of the 'Add Generation' link.":::

    ...and enter the requested details:

    :::image type="content" source="./media/create-vm/azure-vm-generations-image-details-3.png" alt-text="A view of the generation details window.":::

<!--    The **Generation ID** you choose will be visible to customers in places such as product URLs and ARM templates (if applicable). Use only lowercase, alphanumeric characters, dashes, or underscores; it cannot be modified once published.
-->
3. To update an existing VM that has a Generation 1 already published, edit details on the **Technical configuration** page.

To learn more about the differences between Generation 1 and Generation 2 capabilities, see [Support for generation 2 VMs on Azure](../virtual-machines/generation-2.md).

> [!NOTE]
> A published generation requires at least one image version to remain available for customers. To remove the entire plan (along with all its generations and images), select **Deprecate plan** on the **Plan Overview** page (see first section in this article).

### VM images

Provide a disk version and the shared access signature (SAS) URI for the virtual machine images. Add up to 16 data disks for each VM image. Provide only one new image version per plan in a specified submission. After an image has been published, you can't edit it, but you can delete it. Deleting a version prevents both new and existing users from deploying a new instance of the deleted version.

These two required fields are shown in the prior image above:

- **Disk version**: The version of the image you are providing.
- **OS VHD link**: The image stored in Azure Compute Gallery (formerly know as Shared Image Gallery). Learn how to capture your image in an [Azure Compute Gallery](azure-vm-create-using-approved-base.md#capture-image).

Data disks (select **Add data disk (maximum 16)**) are also VHD shared access signature URIs that are stored in their Azure storage accounts. Add only one image per submission in a plan.

Regardless of which operating system you use, add only the minimum number of data disks that the solution requires. During deployment, customers can't remove disks that are part of an image, but they can always add disks during or after deployment.

> [!NOTE]
> If you provide your images using SAS and have data disks, you also need to provide them as SAS URI. If you are using a shared image, they are captured as part of your image in Azure Compute Gallery. Once your offer is published to Azure Marketplace, you can delete the image from your Azure storage or Azure Compute Gallery.

Select **Save draft**, then select **← Plan overview** at the top left to see the plan you just created.

Once your VM image has published, you can delete the image from your Azure storage.

## Reorder plans (optional)

For VM offers with more than one plan, you can change the order that your plans are shown to customers. The first plan listed will become the default plan that customers will see.

1. On the **Plan overview** page, select the **Edit display rank** button.
1. In the menu that appears, use the hamburger icon to drag your plans to the desired order.

## Next steps

- [Resell through CSPs](azure-vm-resell-csp.md)
