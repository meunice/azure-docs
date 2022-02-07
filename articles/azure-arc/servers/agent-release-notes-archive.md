---
title: Archive for What's new with Azure Arc-enabled servers agent
description: The What's new release notes in the Overview section for Azure Arc-enabled servers agent contains six months of activity. Thereafter, the items are removed from the main article and put into this article.
ms.topic: overview
ms.date: 08/27/2021
ms.custom: references_regions
---

# Archive for What's new with Azure Arc-enabled servers agent

The primary [What's new in Azure Arc-enabled servers agent?](agent-release-notes.md) article contains updates for the last six months, while this article contains all the older information.

The Azure Connected Machine agent receives improvements on an ongoing basis. This article provides you with information about:

- Previous releases
- Known issues
- Bug fixes

## Version 1.9 - July 2021

### New features

Added support for the Indonesian language

### Fixed

Fixed a bug that prevented extension management in the West US 3 region

## Version 1.8 - July 2021

### New features

- Improved reliability when installing the Azure Monitor Agent extension on Red Hat and CentOS systems
- Added agent-side enforcement of max resource name length (54 characters)
- Guest Configuration policy improvements:
  - Added support for PowerShell-based Guest Configuration policies on Linux operating systems
  - Added support for multiple assignments of the same Guest Configuration policy on the same server
  - Upgraded PowerShell Core to version 7.1 on Windows operating systems

### Fixed

- The agent will continue running if it is unable to write service start/stop events to the Windows application event log

## Version 1.7 - June 2021

### New features

- Improved reliability during onboarding:
  - Improved retry logic when HIMDS is unavailable
  - Onboarding continues instead of aborting if OS information cannot be obtained
- Improved reliability when installing the Log Analytics agent for Linux extension on Red Hat and CentOS systems

## Version 1.6 - May 2021

### New features

- Added support for SUSE Enterprise Linux 12
- Updated Guest Configuration agent to version 1.26.12.0 to include:
  - Policies are executed in a separate process.
  - Added V2 signature support for extension validation.
  - Minor update to data logging.

## Version 1.5 - April 2021

### New features

- Added support for Red Hat Enterprise Linux 8 and CentOS Linux 8.
- New `-useStderr` parameter to direct error and verbose output to stderr.
- New `-json` parameter to direct output results in JSON format (when used with -useStderr).
- Collect other instance metadata - Manufacturer, model, and cluster resource ID (for Azure Stack HCI nodes).

## Version 1.4 - March 2021

### New features

- Added support for private endpoints, which is currently in limited preview.
- Expanded list of exit codes for azcmagent.
- Agent configuration parameters can now be read from a file with the `--config` parameter.
- Collect new instance metadata to determine if Microsoft SQL Server is installed on the server

### Fixed

Network endpoint checks are now faster.

## Version 1.3 - December 2020

### New features

Added support for Windows Server 2008 R2 SP1.

### Fixed

Resolved issue preventing the Custom Script Extension on Linux from installing successfully.

## Version 1.2 - November 2020

### Fixed

Resolved issue where proxy configuration could be lost after upgrade on RPM-based distributions.

## Version 1.1 - October 2020

### Fixed

- Fixed proxy script to handle alternate GC daemon unit file location.
- GuestConfig agent reliability changes.
- GuestConfig agent support for US Gov Virginia region.
- GuestConfig agent extension report messages to be more verbose if there is a failure.

## Version 1.0 - September 2020

This version is the first generally available release of the Azure Connected Machine Agent.

### Plan for change

- Support for preview agents (all versions older than 1.0) will be removed in a future service update.
- Removed support for fallback endpoint `.azure-automation.net`. If you have a proxy, you need to allow the endpoint `*.his.arc.azure.com`.
- If the Connected Machine agent is installed on a virtual machine hosted in Azure, VM extensions can't be installed or modified from the Arc-enabled servers resource. This is to avoid conflicting extension operations being performed from the virtual machine's **Microsoft.Compute** and **Microsoft.HybridCompute** resource. Use the **Microsoft.Compute** resource for the machine for all extension operations.
- Name of guest configuration process has changed, from *gcd* to *gcad* on Linux, and *gcservice* to *gcarcservice* on Windows.

### New features

- Added `azcmagent logs` option to collect information for support.
- Added `azcmagent license` option to display EULA.
- Added `azcmagent show --json` option to output agent state in easily parseable format.
- Added flag in `azcmagent show` output to indicate if server is on a virtual machine hosted in Azure.
- Added `azcmagent disconnect --force-local-only` option to allow reset of local agent state when Azure service cannot be reached.
- Added `azcmagent connect --cloud` option to support other clouds. In this release, only Azure is supported by service at time of agent release.
- Agent has been localized into Azure-supported languages.

### Fixed

- Improvements to connectivity check.
- Corrected issue with proxy server settings being lost when upgrading agent on Linux.
- Resolved issues when attempting to install agent on server running Windows Server 2012 R2.
- Improvements to extension installation reliability

## Next steps

- Before evaluating or enabling Arc-enabled servers across multiple hybrid machines, review [Connected Machine agent overview](agent-overview.md) to understand requirements, technical details about the agent, and deployment methods.

- Review the [Planning and deployment guide](plan-at-scale-deployment.md) to plan for deploying Azure Arc-enabled servers at any scale and implement centralized management and monitoring.