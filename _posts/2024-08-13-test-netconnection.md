---
layout: post
title: "Test-NetConnection: checking ports on Windows"
date: 2024-08-13
description: "Test-NetConnection: the Windows stand-in for Nmap, checking whether a host and port answer before you try anything."
categories:
  - TIL
tags:
  - Recon
  - Windows
---

In the absence of Nmap on a Windows PC, Test-NetConnection can serve as an alternative

`Test-NetConnection` is a PowerShell cmdlet in Windows that enables network-related testing and diagnostics. It allows users to check connectivity, test ports, and perform various network-related tasks directly from the PowerShell command line.

**Usage:**

```powershell
Test-NetConnection [-ComputerName] <String> [-Port <Int32>] [-InformationLevel <InformationLevel>]
```

**Key Parameters:**

* `-ComputerName`: Specifies the target host or IP address to test connectivity.
* `-Port`: Specifies the target port number to test. If not provided, a default port is used.
* `-InformationLevel`: Specifies the level of detail in the output.

**Common Scenarios:**

1.  **Basic Connectivity Test:**

    ```powershell
    Test-NetConnection -ComputerName example.com
    ```

    Performs a basic connectivity test to the specified host.
2.  **Port Test:**

    ```powershell
    Test-NetConnection -ComputerName example.com -Port 80
    ```

    Tests connectivity to a specific port on the target host.
3.  **Detailed Output:**

    ```powershell
    Test-NetConnection -ComputerName example.com -Port 443 -InformationLevel Detailed
    ```

    Provides detailed information about the connection, including source and destination IP addresses.

**Examples:**

*   Basic connectivity test to example.com:

    ```powershell
    Test-NetConnection -ComputerName example.com
    ```
*   Test connectivity to port 80 on example.com:

    ```powershell
    Test-NetConnection -ComputerName example.com -Port 80
    ```
*   Detailed output for port 443 on example.com:

    ```powershell
    Test-NetConnection -ComputerName example.com -Port 443 -InformationLevel Detailed
    ```

**Resources:**

* [https://learn.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection?view=windowsserver2022-ps](https://learn.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection?view=windowsserver2022-ps)
