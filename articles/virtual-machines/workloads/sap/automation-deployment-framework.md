---
title: About SAP deployment automation framework on Azure
description: Overview of the framework and tooling for the SAP deployment automation framework on Azure.
author: kimforss
ms.author: kimforss
ms.reviewer: kimforss
ms.date: 11/17/2021
ms.service: virtual-machines-sap
ms.topic: conceptual
---
# SAP deployment automation framework on Azure

The [SAP deployment automation framework on Azure](https://github.com/Azure/sap-automation) is an open-source orchestration tool for deploying, installing and maintaining SAP environments. You can create infrastructure for SAP landscapes based on SAP HANA and NetWeaver with AnyDB on any of the SAP-supported operating system versions and deploy them into any Azure region. The framework uses [Terraform](https://www.terraform.io/) for infrastructure deployment, and [Ansible](https://www.ansible.com/) for the operating system and application configuration. 

Hashicorp [Terraform](https://www.terraform.io/) is an open-source tool for provisioning and managing cloud infrastructure. 

[Ansible](https://www.ansible.com/) is an open-source platform by Red Hat that automates cloud provisioning, configuration management, and application deployments. Using Ansible, you can automate deployment and configuration of resources in your environment.

The [automation framework](https://github.com/Azure/sap-automation) has two main components:
-	Deployment infrastructure (control plane) 
-	SAP Infrastructure (SAP Workload)

You will use the control plane of the SAP deployment automation framework to deploy the SAP Infrastructure and the SAP application infrastructure. The deployment uses Terraform templates to create the [infrastructure as a service (IaaS)](https://azure.microsoft.com/overview/what-is-iaas) defined infrastructure to host the SAP Applications.

> [!NOTE]
> This automation framework is based on Microsoft best practices and principles for SAP on Azure. Review the [get-started guide for SAP on Azure virtual machines (Azure VMs)](get-started.md) to understand how to use certified virtual machines and storage solutions for stability, reliability, and performance.
> 
> This automation framework also follows the [Microsoft Cloud Adoption Framework for Azure](/azure/cloud-adoption-framework/).

The dependency between the control plane and the application plane is illustrated in the diagram below. In a typical deployment a single control plane is used to manage multiple SAP deployments.

:::image type="content" source="./media/automation-deployment-framework/control-plane-sap-infrastructure.png" alt-text="Diagram showing the SAP deployment automation framework's dependency between the control plane and application plane.":::

The following diagram shows the key components of the control plane and workload zone.

:::image type="content" source="./media/automation-deployment-framework/automation-diagram-full.png" alt-text="Diagram showing the SAP deployment automation framework environment.":::

The application configuration will be performed from the Ansible Controller in the Control plane using a set of pre-defined playbooks. These playbooks will:

- Configure base operating system settings
- Configure SAP-specific operating system settings
- Make the installation media available in the system
- Install the SAP system
- Install the SAP database (SAP HANA, AnyDB)
- Configure high availability (HA) using Pacemaker
- Configure high availability (HA) for your SAP database


## About the control plane

The control plane houses the deployment infrastructure from which other environments will be deployed. Once the control plane is deployed, it rarely needs to be redeployed, if ever.

The control plane provides the following services
-	Terraform Deployment Infrastructure
-	Ansible Controller
-	Persistent storage for the Terraform state files
-	Persistent storage for the Downloaded SAP Software
-	Secure storage for deployment credentials
-	Private DNS zone (optional)

The control plane is typically a regional resource deployed in to the hub subscription in a [hub and spoke architecture](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke). 

The key components of the control plane are:
- Deployment virtual machine 
- Storage account for Terraform state files
- Storage account for SAP installation media
- Azure Key Vault for deployment credentials

### Deployer Virtual Machine

This virtual machine is used to run the orchestration scripts that will deploy the Azure resources using Terraform. It is also the Ansible Controller and is used to execute the Ansible playbooks on all the managed nodes, i.e the virtual machines of an SAP deployment.

## About the SAP Workload

The SAP Workload contains all the Azure infrastructure resources for the SAP Deployments. These resources are deployed from the control plane. 
The SAP Workload has two main components:
-	SAP Workload Zone
-	SAP System

## About the SAP Workload Zone

The workload zone allows for partitioning of the deployments into different environments (Development,
Test, Production)
The SAP Workload Zone provides the following services to the SAP Systems
-	Virtual Networking infrastructure
-	Secure storage for system credentials (Virtual Machines and SAP)
-	Shared Storage (optional)


## About the SAP System

The system deployment consists of the virtual machines that will be running the SAP application, including the web, app and database tiers.

The SAP System provides the following services
-	Virtual machine, storage, and supporting infrastructure to host the SAP applications.

## Glossary

The following terms are important concepts for understanding the automation framework.

### SAP concepts

| Term | Description |
| ---- | ----------- |
| System | An instance of an SAP application that contains the resources the application needs to run. Defined by a unique three-letter identifier, the **SID**.
| Landscape | A collection of systems in different environments within an SAP application. For example, SAP ERP Central Component (ECC), SAP customer relationship management (CRM), and SAP Business Warehouse (BW). |
| Workload zone | Partitions the SAP applications to environments, such as non-production and production environments or development, quality assurance, and production environments. Provides shared resources, such as virtual networks and key vault, to all systems within. |

The following diagram shows the relationships between SAP systems, workload zones (environments), and landscapes. In this example setup, the customer has three SAP landscapes: ECC, CRM, and BW. Each landscape contains three workload zones: production, quality assurance, and development. Each workload zone contains one or more systems.

:::image type="content" source="./media/automation-deployment-framework/sap-terms.png" alt-text="Diagram of SAP configuration with landscapes, workflow zones, and systems.":::

### Deployment components

| Term | Description | Scope |
| ---- | ----------- | ----- |
| Deployer | A virtual machine that can execute Terraform and Ansible commands. Deployed to a virtual network, either new or existing, that is peered to the SAP virtual network. | Region |
| Library | Provides storage for the Terraform state files and SAP installation media. | Region |
| Workload zone | Contains the virtual network into which you deploy the SAP system or systems. Also contains a key vault that holds the credentials for the systems in the environment. | Workload zone |
| System | The deployment unit for the SAP application (SID). Contains virtual machines and supporting infrastructure artifacts, such as load balancers and availability sets. | Workload zone |


## Next steps

> [!div class="nextstepaction"]
> [Get started with the deployment automation framework](automation-get-started.md)
