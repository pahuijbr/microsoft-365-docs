---
title: Customize attack surface reduction rules
description: Individually set rules in audit, block, or disabled modes, and add files and folders that should be excluded from attack surface reduction rules
keywords: Attack surface reduction, hips, host intrusion prevention system, protection rules, anti-exploit, antiexploit, exploit, infection prevention, customize, configure, exclude
search.product: eADQiWindows 10XVcnh
ms.prod: m365-security
ms.mktglfcycl: manage
ms.sitesec: library
localization_priority: Normal
audience: ITPro
author: dansimp
ms.author: dansimp
ms.reviewer: 
manager: dansimp
ms.technology: mde
---

# Customize attack surface reduction rules

[!INCLUDE [Microsoft 365 Defender rebranding](../../includes/microsoft-defender.md)]


**Applies to:**
- [Microsoft Defender for Endpoint](https://go.microsoft.com/fwlink/p/?linkid=2154037)
- [Microsoft 365 Defender](https://go.microsoft.com/fwlink/?linkid=2118804)

>Want to experience Defender for Endpoint? [Sign up for a free trial.](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp?ocid=docs-wdatp-assignaccess-abovefoldlink)

> [!IMPORTANT]
> Some information relates to prereleased product which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

[Attack surface reduction rules](enable-attack-surface-reduction.md) help prevent software behaviors that are often abused to compromise your device or network. For example, an attacker might try to run an unsigned script off of a USB drive, or have a macro in an Office document make calls directly to the Win32 API. Attack surface reduction rules can constrain these kinds of risky behaviors and improve your organization's defensive posture.

Learn how to customize attack surface reduction rules by [excluding files and folders](#exclude-files-and-folders) or [adding custom text to the notification](#customize-the-notification) alert that appears on a user's computer.

You can set attack surface reduction rules for devices running any of the following editions and versions of Windows:
- Windows 10 Pro, [version 1709](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1709) or later
- Windows 10 Enterprise, [version 1709](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1709) or later
- Windows Server, [version 1803 (Semi-Annual Channel)](https://docs.microsoft.com/windows-server/get-started/whats-new-in-windows-server-1803) or later
- [Windows Server 2019](https://docs.microsoft.com/windows-server/get-started-19/whats-new-19)
You can use Group Policy, PowerShell, and Mobile Device Management (MDM) configuration service providers (CSP) to configure these settings.

## Exclude files and folders

You can choose to exclude files and folders from being evaluated by attack surface reduction rules. Once excluded, the file won't be blocked from running even if an attack surface reduction rule detects that the file contains malicious behavior.

> [!WARNING]
> This could potentially allow unsafe files to run and infect your devices. Excluding files or folders can severely reduce the protection provided by attack surface reduction rules. Files that would have been blocked by a rule will be allowed to run, and there will be no report or event recorded.

An exclusion applies to all rules that allow exclusions. You can specify an individual file, folder path, or the fully qualified domain name for a resource. However, you cannot limit an exclusion to a specific rule.

An exclusion is applied only when the excluded application or service starts. For example, if you add an exclusion for an update service that is already running, the update service will continue to trigger events until the service is stopped and restarted.

Attack surface reduction supports environment variables and wildcards. For information about using wildcards, see [use wildcards in the file name and folder path or extension exclusion lists](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-antivirus/configure-extension-file-exclusions-microsoft-defender-antivirus#use-wildcards-in-the-file-name-and-folder-path-or-extension-exclusion-lists).
If you are encountering problems with rules detecting files that you believe should not be detected, [use audit mode to test the rule](evaluate-attack-surface-reduction.md).

Rule description | GUID
-|-|-
Block all Office applications from creating child processes | D4F940AB-401B-4EFC-AADC-AD5F3C50688A
Block execution of potentially obfuscated scripts | 5BEB7EFE-FD9A-4556-801D-275E5FFC04CC
Block Win32 API calls from Office macro | 92E97FA1-2EDF-4476-BDD6-9DD0B4DDDC7B
Block Office applications from creating executable content | 3B576869-A4EC-4529-8536-B80A7769E899
Block Office applications from injecting code into other processes | 75668C1F-73B5-4CF0-BB93-3ECF5CB7CC84
Block JavaScript or VBScript from launching downloaded executable content | D3E037E1-3EB8-44C8-A917-57927947596D
Block executable content from email client and webmail | BE9BA2D9-53EA-4CDC-84E5-9B1EEEE46550
Block executable files from running unless they meet a prevalence, age, or trusted list criteria | 01443614-cd74-433a-b99e-2ecdc07bfc25
Use advanced protection against ransomware | c1db55ab-c21a-4637-bb3f-a12568109d35
Block credential stealing from the Windows local security authority subsystem (lsass.exe) | 9e6c4e1f-7d60-472f-ba1a-a39ef669e4b2
Block process creations originating from PSExec and WMI commands | d1e49aac-8f56-4280-b9ba-993a6d77406c
Block untrusted and unsigned processes that run from USB | b2b3f03d-6a65-4f7b-a9c7-1c7ef74a9ba4
Block Office communication applications from creating child processes | 26190899-1602-49e8-8b27-eb1d0a1ce869
Block Adobe Reader from creating child processes | 7674ba52-37eb-4a4f-a9a1-f0f9a1619a2c
Block persistence through WMI event subscription | e6db77e5-3df2-4cf1-b95a-636979351e5b

See the [attack surface reduction](attack-surface-reduction.md) topic for details on each rule.

### Use Group Policy to exclude files and folders

1. On your Group Policy management computer, open the [Group Policy Management Console](https://technet.microsoft.com/library/cc731212.aspx), right-click the Group Policy Object you want to configure and select **Edit**.

2. In the **Group Policy Management Editor**, go to **Computer configuration** and click **Administrative templates**.

3. Expand the tree to **Windows components** > **Microsoft Defender Antivirus** > **Windows Defender Exploit Guard** > **Attack surface reduction**.

4. Double-click the **Exclude files and paths from Attack surface reduction Rules** setting and set the option to **Enabled**. Select **Show** and enter each file or folder in the **Value name** column. Enter **0** in the **Value** column for each item.

> [!WARNING]
> Do not use quotes as they are not supported for either the **Value name** column or the **Value** column.

### Use PowerShell to exclude files and folders

1. Type **powershell** in the Start menu, right-click **Windows PowerShell** and select **Run as administrator**
2. Enter the following cmdlet:

    ```PowerShell
    Add-MpPreference -AttackSurfaceReductionOnlyExclusions "<fully qualified path or resource>"
    ```

Continue to use `Add-MpPreference -AttackSurfaceReductionOnlyExclusions` to add more folders to the list.

> [!IMPORTANT]
> Use `Add-MpPreference` to append or add apps to the list. Using the `Set-MpPreference` cmdlet will overwrite the existing list.

### Use MDM CSPs to exclude files and folders

Use the [./Vendor/MSFT/Policy/Config/Defender/AttackSurfaceReductionOnlyExclusions](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-defender#defender-attacksurfacereductiononlyexclusions) configuration service provider (CSP) to add exclusions.

## Customize the notification

You can customize the notification for when a rule is triggered and blocks an app or file. See the [Windows Security](/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center#customize-notifications-from-the-windows-defender-security-center) article.

## Related topics

* [Reduce attack surfaces with attack surface reduction rules](attack-surface-reduction.md)
* [Enable attack surface reduction rules](enable-attack-surface-reduction.md)
* [Evaluate attack surface reduction rules](evaluate-attack-surface-reduction.md)
* [Attack surface reduction FAQ](attack-surface-reduction.md)
