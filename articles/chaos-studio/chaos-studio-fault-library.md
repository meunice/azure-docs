---
title: Chaos Studio fault and action library
description: Understand the available actions you can use with Chaos Studio including any prerequisites and parameters.
services: chaos-studio
author: johnkemnetz
ms.topic: article
ms.date: 01/05/2022
ms.author: johnkem
ms.service: chaos-studio
ms.custom: ignite-fall-2021
---

# Chaos Studio fault and action library

The following faults are available for use today. Visit the [Fault Providers](./chaos-studio-fault-providers.md) page to understand which resource types are supported.

## Time delay

| Property | Value |
|-|-|
| Fault Provider | N/A |
| Supported OS Types | N/A |
| Description | Adds a time delay before, between, or after other actions. Useful for waiting for the impact of a fault to appear in a service or for waiting for an activity outside of the experiment to complete (for example, waiting for autohealing to occur before injecting another fault). |
| Prerequisites | N/A |
| Urn | urn:provider:Azure-chaosStudio:Microsoft.Azure.Chaos.Delay.Timed |
| duration | The duration of the delay in ISO 8601 format (Example: PT10M) |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [ 
    {
      "type": "delay",
      "name": "urn:provider:Azure-chaosStudio:Microsoft.Azure.Chaos.Delay.Timed",
      "duration": "PT10M"
    }
  ] 
}
```

## CPU pressure

| Property | Value |
|-|-|
| Capability Name | CPUPressure-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows, Linux |
| Description | Add CPU pressure up to the specified value on the VM where this fault is injected for the duration of the fault action. The artificial CPU pressure is removed at the end of the duration or if the experiment is canceled. |
| Prerequisites | **Linux:** Running the fault on a Linux VM requires the **stress-ng** utility to be installed. You can install it using the package manager for your Linux distro, </br> APT Command to install stress-ng: *sudo apt-get update && sudo apt-get -y install unzip && sudo apt-get -y install stress-ng* </br> YUM Command to install stress-ng: *sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm && sudo yum -y install stress-ng*  |
| | **Windows:** None. |
| Urn | urn:csci:microsoft:agent:cpuPressure/1.0 |
| Parameters (key, value)  |
| pressureLevel | An integer between 1 and 99 that indicates how much CPU pressure (%) will be applied to the VM. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON
```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:cpuPressure/1.0",
      "parameters": [
        {
          "key": "pressureLevel",
          "value": "95"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

### Notes
Known issues on Linux:
1. Stress effect may not be terminated correctly if AzureChaosAgent is unexpectedly killed.
2. Linux CPU fault is only tested on Ubuntu 16.04-LTS and Ubuntu 18.04-LTS.

## Physical memory pressure

| Property | Value |
|-|-|
| Capability Name | PhysicalMemoryPressure-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows, Linux |
| Description | Add physical memory pressure up to the specified value on the VM where this fault is injected for the duration of the fault action. The artificial physical memory pressure is removed at the end of the duration or if the experiment is canceled. |
| Prerequisites | **Linux:** Running the fault on a Linux VM requires the **stress-ng** utility to be installed. You can install it using the package manager for your Linux distro, </br> APT Command to install stress-ng: *sudo apt-get update && sudo apt-get -y install unzip && sudo apt-get -y install stress-ng* </br> YUM Command to install stress-ng: *sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm && sudo yum -y install stress-ng*  |
| | **Windows:** None. |
| Urn | urn:csci:microsoft:agent:physicalMemoryPressure/1.0 |
| Parameters (key, value) |  |
| pressureLevel | An integer between 1 and 99 that indicates how much physical memory pressure (%) will be applied to the VM. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:physicalMemoryPressure/1.0",
      "parameters": [
        {
          "key": "pressureLevel",
          "value": "95"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Virtual memory pressure

| Property | Value |
|-|-|
| Capability Name | VirtualMemoryPressure-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows |
| Description | Add virtual memory pressure up to the specified value on the VM where this fault is injected for the duration of the fault action. The artificial virtual memory pressure is removed at the end of the duration or if the experiment is canceled. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:agent:virtualMemoryPressure/1.0 |
| Parameters (key, value) |  |
| pressureLevel | An integer between 1 and 99 that indicates how much physical memory pressure (%) will be applied to the VM. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:virtualMemoryPressure/1.0",
      "parameters": [
        {
          "key": "pressureLevel",
          "value": "95"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Disk I/O pressure (Windows)

| Property | Value |
|-|-|
| Capability Name | DiskIOPressure-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows |
| Description | Uses the [diskspd utility](https://github.com/Microsoft/diskspd/wiki) to add disk pressure to the primary storage of the VM where it is injected for the duration of the fault action. This fault has five different modes of execution. The artificial disk pressure is removed at the end of the duration or if the experiment is canceled. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:agent:diskIOPressure/1.0 |
| Parameters (key, value) |  |
| pressureMode | The preset mode of disk pressure to add to the primary storage of the VM. Must be one of the PressureModes in the table below. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Pressure modes

| PressureMode | Description |
| -- | -- |
| PremiumStorageP10IOPS | numberOfThreads = 1<br/>randomBlockSizeInKB = 64<br/>randomSeed = 10<br/>numberOfIOperThread = 25<br/>sizeOfBlocksInKB = 8<br/>sizeOfWriteBufferInKB = 64<br/>fileSizeInGB = 2<br/>percentOfWriteActions = 50 |
| PremiumStorageP10Throttling |<br/>numberOfThreads = 2<br/>randomBlockSizeInKB = 64<br/>randomSeed = 10<br/>numberOfIOperThread = 25<br/>sizeOfBlocksInKB = 64<br/>sizeOfWriteBufferInKB = 64<br/>fileSizeInGB = 1<br/>percentOfWriteActions = 50 |
| PremiumStorageP50IOPS | numberOfThreads = 32<br/>randomBlockSizeInKB = 64<br/>randomSeed = 10<br/>numberOfIOperThread = 32<br/>sizeOfBlocksInKB = 8<br/>sizeOfWriteBufferInKB = 64<br/>fileSizeInGB = 1<br/>percentOfWriteActions = 50 |
| PremiumStorageP50Throttling | numberOfThreads = 2<br/>randomBlockSizeInKB = 1024<br/>randomSeed = 10<br/>numberOfIOperThread = 2<br/>sizeOfBlocksInKB = 1024<br/>sizeOfWriteBufferInKB = 1024<br/>fileSizeInGB = 20<br/>percentOfWriteActions = 50|
| Default | numberOfThreads = 2<br/>randomBlockSizeInKB = 64<br/>randomSeed = 10<br/>numberOfIOperThread = 2<br/>sizeOfBlocksInKB = 64<br/>sizeOfWriteBufferInKB = 64<br/>fileSizeInGB = 1<br/>percentOfWriteActions = 50 |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:diskIOPressure/1.0",
      "parameters": [
        {
          "key": "pressureMode",
          "value": "PremiumStorageP10IOPS"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Disk I/O pressure (Linux)

| Property | Value |
|-|-|
| Capability Name | LinuxDiskIOPressure-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Linux |
| Description | Uses stress-ng to apply pressure to the disk. One or more worker processes are spawned that perform I/O processes with temporary files. For details on how pressure is applied see https://wiki.ubuntu.com/Kernel/Reference/stress-ng. |
| Prerequisites | Running the fault on a Linux VM requires the **stress-ng** utility to be installed. You can install it using the package manager for your Linux distro, </br> APT Command to install stress-ng: *sudo apt-get update && sudo apt-get -y install unzip && sudo apt-get -y install stress-ng* </br> YUM Command to install stress-ng: *sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm && sudo yum -y install stress-ng* |
| Urn | urn:csci:microsoft:agent:linuxDiskIOPressure/1.0 |
| Parameters (key, value) |  |
| workerCount | Number of worker processes to run. Setting this to 0 will generate as many worker processes as there are number of processors. |
| fileSizePerWorker | Size of the temporary file a worker will perform I/O operations against. Integer plus a unit in bytes (b), kilobytes (k), megabytes (m), or gigabytes (g) (for example, 4m for 4 megabytes, 256g for 256 gigabytes) |
| blockSize | Block size to be used for disk I/O operations, capped at 4 megabytes. Integer plus a unit in bytes (b), kilobytes (k), or megabytes (m) (for example, 512k for 512 kilobytes) |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:linuxDiskIOPressure/1.0",
      "parameters": [
        {
          "key": "workerCount",
          "value": "0"
        },
        {
          "key": "fileSizePerWorker",
          "value": "512m"
        },
        {
          "key": "blockSize",
          "value": "256k"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Arbitrary Stress-ng stress

| Property | Value |
|-|-|
| Capability Name | StressNg-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Linux |
| Description | Run any stress-ng command by passing arguments directly to stress-ng. Useful for when one of the pre-defined faults for stress-ng doesn't meet your needs. |
| Prerequisites | Running the fault on a Linux VM requires the **stress-ng** utility to be installed. You can install it using the package manager for your Linux distro, </br> APT Command to install stress-ng: *sudo apt-get update && sudo apt-get -y install unzip && sudo apt-get -y install stress-ng* </br> YUM Command to install stress-ng: *sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm && sudo yum -y install stress-ng* |
| Urn | urn:csci:microsoft:agent:stressNg/1.0 |
| Parameters (key, value) |  |
| stressNgArguments | One or more arguments to pass to the stress-ng process. For details on possible stress-ng arguments see https://wiki.ubuntu.com/Kernel/Reference/stress-ng |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:stressNg/1.0",
      "parameters": [
        {
          "key": "stressNgArguments",
          "value": "--random 64"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Stop Windows service

| Property | Value |
|-|-|
| Capability Name | StopService-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows |
| Description | Uses the Windows Service Controller APIs to stop a Windows service for the duration of the fault, restarting it at the end of the duration or if the experiment is canceled. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:agent:stopService/1.0 |
| Parameters (key, value) |  |
| serviceName | The name of the Windows service you want to stop. You can run `sc.exe query` in command prompt to explore service names, Windows service friendly names are not supported. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:stopService/1.0",
      "parameters": [
        {
          "key": "serviceName",
          "value": "nvagent"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Time change

| Property | Value |
|-|-|
| Capability Name | TimeChange-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows |
| Description | Changes the system time for the VM where it is injected and resets it at the end of the duration or if the experiment is canceled. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:agent:timeChange/1.0 |
| Parameters (key, value) |  |
| dateTime | A DateTime string in [ISO8601 format](https://www.cryptosys.net/pki/manpki/pki_iso8601datetime.html). If YYYY-MM-DD values are missing, they are defaulted to the current day when the experiment runs. If Thh:mm:ss values are missing, the default value is 12:00:00 AM. If a 2-digit year is provided (YY), it is converted to a 4-digit year (YYYY) based on the current century. If \<Z\> is missing, it is defaulted to the offset of the local timezone. \<Z\> must always include a sign symbol (negative or positive). |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:timeChange/1.0",
      "parameters": [
        {
          "key": "dateTime",
          "value": "2038-01-01T03:14:07"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Kill process

| Property | Value |
|-|-|
| Capability Name | KillProcess-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows, Linux |
| Description | Kills all the running instances of a process that matches the process name sent in the fault parameters. Within the duration set for the fault action, a process is killed repetitively based on the value of the kill interval specified. This fault is a destructive fault where system admin would need to manually recover the process if self-healing is configured for it. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:agent:killProcess/1.0 |
| Parameters (key, value) |  |
| processName | Name of a process running on a VM (without the .exe) |
| killIntervalInMilliseconds | Amount of time the fault will wait in between successive kill attempts in milliseconds. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:killProcess/1.0",
      "parameters": [
        {
          "key": "processName",
          "value": "myapp"
        },
        {
          "key": "killIntervalInMilliseconds",
          "value": "1000"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## DNS failure

| Property | Value |
|-|-|
| Capability Name | DnsFailure-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows |
| Description | Substitutes the response of a DNS lookup request with a specified error code. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:agent:dnsFailure/1.0 |
| Parameters (key, value) |  |
| hosts | Delimited JSON array of host names to fail DNS lookup request for. |
| dnsFailureReturnCode | DNS error code to be returned to the client for the lookup failure (FormErr, ServFail, NXDomain, NotImp, Refused, XDomain, YXRRSet, NXRRSet, NotAuth, NotZone). For more details on DNS return codes, visit [the IANA website](https://www.iana.org/assignments/dns-parameters/dns-parameters.xml#dns-parameters-6) |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:dnsFailure/1.0",
      "parameters": [
        {
          "key": "hosts",
          "value": "[ \"www.bing.com\", \"msdn.microsoft.com\" ]"
        },
        {
          "key": "dnsFailureReturnCode",
          "value": "ServFail"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

### Limitations

* The DNS Failure fault requires Windows 2019 RS5 or newer.
* DNS Cache will be ignored during the duration of the fault for the host names defined in the fault.

## Network latency

| Property | Value |
|-|-|
| Capability Name | NetworkLatency-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows |
| Description | Increases network latency for a specified port range and network block. |
| Prerequisites | Agent must be run as administrator. If the agent is installed as a VM extension, it is run as administrator by default. |
| Urn | urn:csci:microsoft:agent:networkLatency/1.0 |
| Parameters (key, value) |  |
| latencyInMilliseconds | Amount of latency to be applied in milliseconds. |
| destinationFilters | Delimited JSON array of packet filters defining which outbound packets to target for fault injection. Maximum of 3. |
| address | IP address indicating the start of the IP range. |
| subnetMask | Subnet mask for the IP address range. |
| portLow | (Optional) Port number of the start of the port range. |
| portHigh | (Optional) Port number of the end of the port range. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:networkLatency/1.0",
      "parameters": [
        {
          "key": "destinationFilters",
          "value": "[ { \"address\": \"23.45.229.97\", \"subnetMask\": \"255.255.255.224\", \"portLow\": \"5000\", \"portHigh\": \"5200\" } ]"
        },
        {
          "key": "latencyInMilliseconds",
          "value": "100",
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Network disconnect

| Property | Value |
|-|-|
| Capability Name | NetworkDisconnect-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows |
| Description | Blocks outbound network traffic for specified port range and network block. |
| Prerequisites | Agent must be run as administrator. If the agent is installed as a VM extension, it is run as administrator by default. |
| Urn | urn:csci:microsoft:agent:networkDisconnect/1.0 |
| Parameters (key, value) |  |
| destinationFilters | Delimited JSON array of packet filters defining which outbound packets to target for fault injection. Maximum of 3. |
| address | IP address indicating the start of the IP range. |
| subnetMask | Subnet mask for the IP address range. |
| portLow | (Optional) Port number of the start of the port range. |
| portHigh | (Optional) Port number of the end of the port range. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:networkDisconnect/1.0",
      "parameters": [
        {
          "key": "destinationFilters",
          "value": "[ { \"address\": \"23.45.229.97\", \"subnetMask\": \"255.255.255.224\", \"portLow\": \"5000\", \"portHigh\": \"5200\" } ]"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

> [!WARNING]
> The network disconnect fault only affects new connections. Existing **active** connections continue to persist. You can restart the service or process to force connections to break.

## Network disconnect with firewall rule

| Property | Value |
|-|-|
| Capability Name | NetworkDisconnectViaFirewall-1.0 |
| Target type | Microsoft-Agent |
| Supported OS Types | Windows |
| Description | Applies a Windows firewall rule to block outbound traffic for specified port range and network block. |
| Prerequisites | Agent must be run as administrator. If the agent is installed as a VM extension, it is run as administrator by default. |
| Urn | urn:csci:microsoft:agent:networkDisconnectViaFirewall/1.0 |
| Parameters (key, value) |  |
| destinationFilters | Delimited JSON array of packet filters defining which outbound packets to target for fault injection. Maximum of 3. |
| address | IP address indicating the start of the IP range. |
| subnetMask | Subnet mask for the IP address range. |
| portLow | (Optional) Port number of the start of the port range. |
| portHigh | (Optional) Port number of the end of the port range. |
| virtualMachineScaleSetInstances | An array of instance IDs when applying this fault to a virtual machine scale set. Required for virtual machine scale sets. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:agent:networkDisconnectViaFirewall/1.0",
      "parameters": [
        {
          "key": "destinationFilters",
          "value": "[ { \"Address\": \"23.45.229.97\", \"SubnetMask\": \"255.255.255.224\", \"PortLow\": \"5000\", \"PortHigh\": \"5200\" } ]"
        },
        {
          "key": "virtualMachineScaleSetInstances",
          "value": "[0,1,2]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## ARM virtual machine shutdown
| Property | Value |
|-|-|
| Capability Name | Shutdown-1.0 |
| Target type | Microsoft-VirtualMachine |
| Supported OS Types | Windows, Linux |
| Description | Shuts down a VM for the duration of the fault and optionally restarts the VM at the end of the fault duration or if the experiment is canceled. Only Azure Resource Manager VMs are supported. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:virtualMachine:shutdown/1.0 |
| Parameters (key, value) |  |
| abruptShutdown | (Optional) Boolean indicating if the VM should be shut down gracefully or abruptly (destructive). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:virtualMachine:shutdown/1.0",
      "parameters": [
        {
          "key": "abruptShutdown",
          "value": "false"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## ARM virtual machine scale set instance shutdown

| Property | Value |
|-|-|
| Capability Name | Shutdown-1.0 |
| Target type | Microsoft-VirtualMachineScaleSet |
| Supported OS Types | Windows, Linux |
| Description | Shuts down or kill a virtual machine scale set instance for the duration of the fault and optionally restarts the VM at the end of the fault duration or if the experiment is canceled. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:virtualMachineScaleSet:shutdown/1.0 |
| Parameters (key, value) |  |
| abruptShutdown | (Optional) Boolean indicating if the virtual machine scale set instance should be shut down gracefully or abruptly (destructive). |
| instances | A string that is a delimited array of virtual machine scale set instance IDs to which the fault will be applied. |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:virtualMachineScaleSet:shutdown/1.0",
      "parameters": [
        {
          "key": "abruptShutdown",
          "value": "true"
        },
        {
          "key": "instances",
          "value": "[\"1\",\"3\"]"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## Cosmos DB failover

| Property | Value |
|-|-|
| Capability Name | Failover-1.0 |
| Target type | Microsoft-CosmosDB |
| Description | Causes a Cosmos DB account with a single write region to fail over to a specified read region to simulate a [write region outage](../cosmos-db/high-availability.md) |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:cosmosDB:failover/1.0 |
| Parameters (key, value) |  |
| readRegion | The read region that should be promoted to write region during the failover, for example, "East US 2" |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:cosmosDB:failover/1.0",
      "parameters": [
        {
          "key": "readRegion",
          "value": "West US 2"
        }
      ],
      "duration": "PT10M",
      "selectorid": "myResources"
    }
  ]
}
```

## AKS Chaos Mesh network faults

| Property | Value |
|-|-|
| Capability Name | NetworkChaos-2.1 |
| Target type | Microsoft-AzureKubernetesServiceChaosMesh |
| Description | Causes a network fault available through [Chaos Mesh](https://chaos-mesh.org/docs/simulate-network-chaos-on-kubernetes/) to run against your AKS cluster. Useful for recreating AKS incidents resulting from network outages, delays, duplications, loss, and corruption. |
| Prerequisites | The AKS cluster must [have Chaos Mesh deployed](chaos-studio-tutorial-aks-portal.md). |
| Urn | urn:csci:microsoft:azureKubernetesServiceChaosMesh:networkChaos/2.1 |
| Parameters (key, value) |  |
| jsonSpec | A JSON-formatted and, if created via ARM template, REST API, or Azure CLI, JSON-escaped Chaos Mesh spec that uses the [NetworkChaos kind](https://chaos-mesh.org/docs/simulate-network-chaos-on-kubernetes/#create-experiments-using-the-yaml-files). You can use a [YAML-to-JSON converter like this one](https://www.convertjson.com/yaml-to-json.htm) to convert the Chaos Mesh YAML to JSON and minify it and use a [JSON string escape tool like this one](https://www.freeformatter.com/json-escape.html) to escape the JSON spec. Only include the YAML under the "jsonSpec" property (do not include metadata, kind, etc.). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:azureKubernetesServiceChaosMesh:networkChaos/2.1",
      "parameters": [
        {
            "key": "jsonSpec",
            "value": "{\"action\":\"delay\",\"mode\":\"one\",\"selector\":{\"namespaces\":[\"default\"],\"labelSelectors\":{\"app\":\"web-show\"}},\"delay\":{\"latency\":\"10ms\",\"correlation\":\"100\",\"jitter\":\"0ms\"}}"
        }
    ],
      "selectorid": "myResources"
    }
  ]
}
```

## AKS Chaos Mesh pod faults

| Property | Value |
|-|-|
| Capability Name | PodChaos-2.1 |
| Target type | Microsoft-AzureKubernetesServiceChaosMesh |
| Description | Causes a pod fault available through [Chaos Mesh](https://chaos-mesh.org/docs/simulate-pod-chaos-on-kubernetes/) to run against your AKS cluster. Useful for recreating AKS incidents that are a result of pod failures or container issues. |
| Prerequisites | The AKS cluster must [have Chaos Mesh deployed](chaos-studio-tutorial-aks-portal.md). |
| Urn | urn:csci:microsoft:azureKubernetesServiceChaosMesh:podChaos/2.1 |
| Parameters (key, value) |  |
| jsonSpec | A JSON-formatted and, if created via ARM template, REST API, or Azure CLI, JSON-escaped Chaos Mesh spec that uses the [PodChaos kind](https://chaos-mesh.org/docs/simulate-pod-chaos-on-kubernetes/#create-experiments-using-yaml-configuration-files). You can use a [YAML-to-JSON converter like this one](https://www.convertjson.com/yaml-to-json.htm) to convert the Chaos Mesh YAML to JSON and minify it, and use a [JSON string escape tool like this one](https://www.freeformatter.com/json-escape.html) to escape the JSON spec. Only include the YAML under the "jsonSpec" property (do not include metadata, kind, etc.). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:azureKubernetesServiceChaosMesh:podChaos/2.1",
      "parameters": [
        {
            "key": "jsonSpec",
            "value": "{\"action\":\"pod-failure\",\"mode\":\"one\",\"duration\":\"30s\",\"selector\":{\"labelSelectors\":{\"app.kubernetes.io\/component\":\"tikv\"}}}"
        }
    ],
      "selectorid": "myResources"
    }
  ]
}
```

## AKS Chaos Mesh stress faults

| Property | Value |
|-|-|
| Capability Name | StressChaos-2.1 |
| Target type | Microsoft-AzureKubernetesServiceChaosMesh |
| Description | Causes a stress fault available through [Chaos Mesh](https://chaos-mesh.org/docs/simulate-heavy-stress-on-kubernetes/) to run against your AKS cluster. Useful for recreating AKS incidents due to stresses over a collection of pods, for example, due to high CPU or memory consumption. |
| Prerequisites | The AKS cluster must [have Chaos Mesh deployed](chaos-studio-tutorial-aks-portal.md). |
| Urn | urn:csci:microsoft:azureKubernetesServiceChaosMesh:stressChaos/2.1 |
| Parameters (key, value) |  |
| jsonSpec | A JSON-formatted and, if created via ARM template, REST API, or Azure CLI, JSON-escaped Chaos Mesh spec that uses the [StressChaos kind](https://chaos-mesh.org/docs/simulate-heavy-stress-on-kubernetes/#create-experiments-using-the-yaml-file). You can use a [YAML-to-JSON converter like this one](https://www.convertjson.com/yaml-to-json.htm) to convert the Chaos Mesh YAML to JSON and minify it, and use a [JSON string escape tool like this one](https://www.freeformatter.com/json-escape.html) to escape the JSON spec. Only include the YAML under the "jsonSpec" property (do not include metadata, kind, etc.). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:azureKubernetesServiceChaosMesh:stressChaos/2.1",
      "parameters": [
        {
            "key": "jsonSpec",
            "value": "{\"mode\":\"one\",\"selector\":{\"labelSelectors\":{\"app\":\"app1\"}},\"stressors\":{\"memory\":{\"workers\":4,\"size\":\"256MB\"}}}"
        }
    ],
      "selectorid": "myResources"
    }
  ]
}
```

## AKS Chaos Mesh IO faults

| Property | Value |
|-|-|
| Capability Name | IOChaos-2.1 |
| Target type | Microsoft-AzureKubernetesServiceChaosMesh |
| Description | Causes an IO fault available through [Chaos Mesh](https://chaos-mesh.org/docs/simulate-io-chaos-on-kubernetes/) to run against your AKS cluster. Useful for recreating AKS incidents due to IO delays and read/write failures when using IO system calls such as `open`, `read`, and `write`. |
| Prerequisites | The AKS cluster must [have Chaos Mesh deployed](chaos-studio-tutorial-aks-portal.md). |
| Urn | urn:csci:microsoft:azureKubernetesServiceChaosMesh:IOChaos/2.1 |
| Parameters (key, value) |  |
| jsonSpec | A JSON-formatted and, if created via ARM template, REST API, or Azure CLI, JSON-escaped Chaos Mesh spec that uses the [IOChaos kind](https://chaos-mesh.org/docs/simulate-io-chaos-on-kubernetes/#create-experiments-using-the-yaml-files). You can use a [YAML-to-JSON converter like this one](https://www.convertjson.com/yaml-to-json.htm) to convert the Chaos Mesh YAML to JSON and minify it, and use a [JSON string escape tool like this one](https://www.freeformatter.com/json-escape.html) to escape the JSON spec. Only include the YAML under the "jsonSpec" property (do not include metadata, kind, etc.). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:azureKubernetesServiceChaosMesh:IOChaos/2.1",
      "parameters": [
        {
            "key": "jsonSpec",
            "value": "{\"action\":\"latency\",\"mode\":\"one\",\"selector\":{\"labelSelectors\":{\"app\":\"etcd\"}},\"volumePath\":\"\/var\/run\/etcd\",\"path\":\"\/var\/run\/etcd\/**\/*\",\"delay\":\"100ms\",\"percent\":50,\"duration\":\"400s\"}"
        }
    ],
      "selectorid": "myResources"
    }
  ]
}
```

## AKS Chaos Mesh time faults

| Property | Value |
|-|-|
| Capability Name | TimeChaos-2.1 |
| Target type | Microsoft-AzureKubernetesServiceChaosMesh |
| Description | Causes a change in the system clock on your AKS cluster using  [Chaos Mesh](https://chaos-mesh.org/docs/simulate-time-chaos-on-kubernetes/). Useful for recreating AKS incidents that result from distributed systems falling out of sync, missing/incorrect leap year/leap second logic, and more. |
| Prerequisites | The AKS cluster must [have Chaos Mesh deployed](chaos-studio-tutorial-aks-portal.md). |
| Urn | urn:csci:microsoft:azureKubernetesServiceChaosMesh:timeChaos/2.1 |
| Parameters (key, value) |  |
| jsonSpec | A JSON-formatted and, if created via ARM template, REST API, or Azure CLI, JSON-escaped Chaos Mesh spec that uses the [TimeChaos kind](https://chaos-mesh.org/docs/simulate-time-chaos-on-kubernetes/#create-experiments-using-the-yaml-file). You can use a [YAML-to-JSON converter like this one](https://www.convertjson.com/yaml-to-json.htm) to convert the Chaos Mesh YAML to JSON and minify it, and use a [JSON string escape tool like this one](https://www.freeformatter.com/json-escape.html) to escape the JSON spec. Only include the YAML under the "jsonSpec" property (do not include metadata, kind, etc.). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:azureKubernetesServiceChaosMesh:timeChaos/2.1",
      "parameters": [
        {
            "key": "jsonSpec",
            "value": "{\"mode\":\"one\",\"selector\":{\"labelSelectors\":{\"app\":\"app1\"}},\"timeOffset\":\"-10m100ns\"}"
        }
    ],
      "selectorid": "myResources"
    }
  ]
}
```

## AKS Chaos Mesh kernel faults

| Property | Value |
|-|-|
| Capability Name | KernelChaos-2.1 |
| Target type | Microsoft-AzureKubernetesServiceChaosMesh |
| Description | Causes a kernel fault available through [Chaos Mesh](https://chaos-mesh.org/docs/simulate-kernel-chaos-on-kubernetes/) to run against your AKS cluster. Useful for recreating AKS incidents due to Linux kernel-level errors such as a mount failing or memory not being allocated. |
| Prerequisites | The AKS cluster must [have Chaos Mesh deployed](chaos-studio-tutorial-aks-portal.md). |
| Urn | urn:csci:microsoft:azureKubernetesServiceChaosMesh:kernelChaos/2.1 |
| Parameters (key, value) |  |
| jsonSpec | A JSON-formatted and, if created via ARM template, REST API, or Azure CLI, JSON-escaped Chaos Mesh spec that uses the [KernelChaos kind](https://chaos-mesh.org/docs/simulate-kernel-chaos-on-kubernetes/#configuration-file). You can use a [YAML-to-JSON converter like this one](https://www.convertjson.com/yaml-to-json.htm) to convert the Chaos Mesh YAML to JSON and minify it, and use a [JSON string escape tool like this one](https://www.freeformatter.com/json-escape.html) to escape the JSON spec. Only include the YAML under the "jsonSpec" property (do not include metadata, kind, etc.). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:azureKubernetesServiceChaosMesh:kernelChaos/2.1",
      "parameters": [
        {
            "key": "jsonSpec",
            "value": "{\"mode\":\"one\",\"selector\":{\"namespaces\":[\"chaos-mount\"]},\"failKernRequest\":{\"callchain\":[{\"funcname\":\"__x64_sys_mount\"}],\"failtype\":0}}"
        }
    ],
      "selectorid": "myResources"
    }
  ]
}
```

## AKS Chaos Mesh HTTP faults

| Property | Value |
|-|-|
| Capability Name | HTTPChaos-2.1 |
| Target type | Microsoft-AzureKubernetesServiceChaosMesh |
| Description | Causes an HTTP fault available through [Chaos Mesh](https://chaos-mesh.org/docs/simulate-http-chaos-on-kubernetes/) to run against your AKS cluster. Useful for recreating incidents due HTTP request and response processing failures, such as delayed or incorrect responses. |
| Prerequisites | The AKS cluster must [have Chaos Mesh deployed](chaos-studio-tutorial-aks-portal.md). |
| Urn | urn:csci:microsoft:azureKubernetesServiceChaosMesh:httpChaos/2.1 |
| Parameters (key, value) |  |
| jsonSpec | A JSON-formatted and, if created via ARM template, REST API, or Azure CLI, JSON-escaped Chaos Mesh spec that uses the [HTTPChaos kind](https://chaos-mesh.org/docs/simulate-http-chaos-on-kubernetes/#create-experiments). You can use a [YAML-to-JSON converter like this one](https://www.convertjson.com/yaml-to-json.htm) to convert the Chaos Mesh YAML to JSON and minify it, and use a [JSON string escape tool like this one](https://www.freeformatter.com/json-escape.html) to escape the JSON spec. Only include the YAML under the "jsonSpec" property (do not include metadata, kind, etc.). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:azureKubernetesServiceChaosMesh:httpChaos/2.1",
      "parameters": [
        {
            "key": "jsonSpec",
            "value": "{\"mode\":\"all\",\"selector\":{\"labelSelectors\":{\"app\":\"nginx\"}},\"target\":\"Request\",\"port\":80,\"method\":\"GET\",\"path\":\"\/api\",\"abort\":true,\"duration\":\"5m\",\"scheduler\":{\"cron\":\"@every 10m\"}}"
        }
    ],
      "selectorid": "myResources"
    }
  ]
}
```

## AKS Chaos Mesh DNS faults

| Property | Value |
|-|-|
| Capability Name | DNSChaos-2.1 |
| Target type | Microsoft-AzureKubernetesServiceChaosMesh |
| Description | Causes a DNS fault available through [Chaos Mesh](https://chaos-mesh.org/docs/simulate-dns-chaos-on-kubernetes/) to run against your AKS cluster. Useful for recreating incidents due to DNS failures. |
| Prerequisites | The AKS cluster must [have Chaos Mesh deployed](chaos-studio-tutorial-aks-portal.md) and the [DNS service must be installed](https://chaos-mesh.org/docs/simulate-dns-chaos-on-kubernetes/#deploy-chaos-dns-service). |
| Urn | urn:csci:microsoft:azureKubernetesServiceChaosMesh:dnsChaos/2.1 |
| Parameters (key, value) |  |
| jsonSpec | A JSON-formatted and, if created via ARM template, REST API, or Azure CLI, JSON-escaped Chaos Mesh spec that uses the [DNSChaos kind](https://chaos-mesh.org/docs/simulate-dns-chaos-on-kubernetes/#create-experiments-using-the-yaml-file). You can use a [YAML-to-JSON converter like this one](https://www.convertjson.com/yaml-to-json.htm) to convert the Chaos Mesh YAML to JSON and minify it, and use a [JSON string escape tool like this one](https://www.freeformatter.com/json-escape.html) to escape the JSON spec. Only include the YAML under the "jsonSpec" property (do not include metadata, kind, etc.). |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "continuous",
      "name": "urn:csci:microsoft:azureKubernetesServiceChaosMesh:dnsChaos/2.1",
      "parameters": [
        {
            "key": "jsonSpec",
            "value": "{\"action\":\"random\",\"mode\":\"all\",\"patterns\":[\"google.com\",\"chaos-mesh.*\",\"github.?om\"],\"selector\":{\"namespaces\":[\"busybox\"]}}"
        }
    ],
      "selectorid": "myResources"
    }
  ]
}
```

## Network security group (set rules)

| Property | Value |
|-|-|
| Capability Name | SecurityRule-1.0 |
| Target type | Microsoft-NetworkSecurityGroup |
| Description | Enables manipulation or creation of a rule in an existing Azure Network Security Group or set of Azure Network Security Groups (assuming the rule definition is applicable cross security groups). Useful for simulating an outage of a downstream or cross-region dependency/non-dependency, simulating an event that is expected to trigger a logic to force a service failover, simulating an event that is expected to trigger an action from a monitoring or state management service, or as an alternative for blocking, or allowing, network traffic where Chaos Agent cannot be deployed. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:networkSecurityGroup:securityRule/1.0 |
| Parameters (key, value) |  |
| name | A unique name for the security rule that will be created. The fault will fail if another rule already exists on the NSG with the same name. Must begin with a letter or number, end with a letter, number or underscore, and may contain only letters, numbers, underscores, periods, or hyphens. |
| protocol | Protocol for the security rule. Must be Any, TCP, UDP, or ICMP. |
| sourceAddresses | A string representing a json-delimited array of CIDR formatted IP addresses. Can also be a service tag name for an inbound rule, for example, "AppService". Asterisk '*' can also be used to match all source IPs. |
| destinationAddresses | A string representing a json-delimited array of CIDR formatted IP addresses. Can also be a service tag name for an outbound rule, for example, "AppService". Asterisk '*' can also be used to match all destination IPs. |
| action | Security group access type. Must be either Allow or Deny |
| destinationPortRanges | A string representing a json-delimited array of single ports and/or port ranges, such as 80 or 1024-65535. |
| sourcePortRanges | A string representing a json-delimited array single ports and/or port ranges, such as 80 or 1024-65535. |
| priority | A value between 100 and 4096 that's unique for all security rules within the network security group. The fault will fail if another rule already exists on the NSG with the same priority. |
| direction | Direction of the traffic impacted by the security rule. Must be either Inbound or Outbound. |

### Sample JSON

```json
{ 
  "name": "branchOne", 
  "actions": [ 
    { 
      "type": "continuous", 
      "name": "urn:csci:microsoft:networkSecurityGroup:securityRule/1.0", 
      "parameters": [ 
          { 
              "key": "name", 
              "value": "Block_SingleHost_to_Networks" 

          }, 
          { 
              "key": "protocol", 
              "value": "Any" 
          }, 
          { 
              "key": "sourceAddresses", 
              "value": "[\"10.1.1.128/32\"]"
          }, 
          { 
              "key": "destinationAddresses", 
              "value": "[\"10.20.0.0/16\",\"10.30.0.0/16\"]"
          }, 
          { 
              "key": "access", 
              "value": "Deny" 
          }, 
          { 
              "key": "destinationPortRanges", 
              "value": "[\"80-8080\"]"
          }, 
          { 
              "key": "sourcePortRanges", 
              "value": "[\"*\"]"
          }, 
          { 
              "key": "priority", 
              "value": "100" 
          }, 
          { 
              "key": "direction", 
              "value": "Outbound" 
          } 
      ], 
      "duration": "PT10M", 
      "selectorid": "myResources" 
    } 
  ] 
} 
```

### Limitations

* The fault can only be applied to an existing Network Security Group.
* When an NSG rule that is intended to deny traffic is applied existing connections will not be broken until they have been **idle** for 4 minutes. One workaround is to add another branch in the same step that uses a fault that would cause existing connections to break when the NSG fault is applied. For example, killing the process, temporarily stopping the service, or restarting the VM would cause connections to reset.
* Rules are applied at the start of the action. Any external changes to the rule during the duration of the action will cause the experiment to fail.
* Creating or modifying Application Security Group rules is not supported.
* Priority values must be unique on each NSG targeted. Attempting to create a new rule that has the same priority value as another will cause the experiment to fail.

## Azure Cache for Redis reboot

| Property | Value |
|-|-|
| Capability Name | Reboot-1.0 |
| Target type | Microsoft-AzureClusteredCacheForRedis |
| Description | Causes a forced reboot operation to occur on the target to simulate a brief outage. |
| Prerequisites | None. |
| Urn | urn:csci:microsoft:azureClusteredCacheForRedis:reboot/1.0 |
| Fault type | Discrete |
| Parameters (key, value) |  |
| rebootType | The node types where the reboot action is to be performed which can be specified as PrimaryNode, SecondaryNode or AllNodes.  |
| shardId | The ID of the shard to be rebooted.  |

### Sample JSON

```json
{
  "name": "branchOne",
  "actions": [
    {
      "type": "discrete",
      "name": "urn:csci:microsoft:azureClusteredCacheForRedis:reboot/1.0",
      "parameters": [
        {
          "key": "RebootType",
          "value": "AllNodes"
        },
        {
          "key": "ShardId",
          "value": "0"
        }
      ],
      "selectorid": "myResources"
    }
  ]
}
```

### Limitations

* The reboot fault causes a forced reboot to better simulate an outage event, which means there is the potential for data loss to occur.
* The reboot fault is a **discrete** fault type. Unlike continuous faults, it is a one-time action and therefore has no duration.
