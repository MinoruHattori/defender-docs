---
title: Detect and block potentially unwanted applications with Microsoft Defender for Endpoint on Linux
description: Detect and block Potentially Unwanted Applications (PUA) using Microsoft Defender for Endpoint on Linux.
ms.service: defender-endpoint
ms.author: deniseb
author: denisebmsft
ms.reviewer: gopkr
ms.localizationpriority: medium
manager: deniseb
audience: ITPro
ms.collection: 
- m365-security
- tier3
- mde-linux
ms.topic: conceptual
ms.subservice: linux
search.appverid: met150
ms.date: 10/11/2024
---

# Detect and block potentially unwanted applications with Microsoft Defender for Endpoint on Linux

[!INCLUDE [Microsoft Defender XDR rebranding](../includes/microsoft-defender.md)]

**Applies to**:

- Microsoft Defender for Endpoint Server
- [Microsoft Defender for Servers](/azure/defender-for-cloud/integration-defender-for-endpoint)

> Want to experience Defender for Endpoint? [Sign up for a free trial.](https://signup.microsoft.com/create-account/signup?products=7f379fee-c4f9-4278-b0a1-e4c8c2fcdf7e&ru=https://aka.ms/MDEp2OpenTrial?ocid=docs-wdatp-investigateip-abovefoldlink)

The potentially unwanted application (PUA) protection feature in Defender for Endpoint on Linux can detect and block PUA files on endpoints in your network.

These applications are not considered viruses, malware, or other types of threats, but might perform actions on endpoints that adversely affect their performance or use. PUA can also refer to applications that are considered to have poor reputation.

These applications can increase the risk of your network being infected with malware, cause malware infections to be harder to identify, and can waste IT resources in cleaning up the applications.

## How it works

Defender for Endpoint on Linux can detect and report PUA files. When configured in blocking mode, PUA files are moved to the quarantine.

When a PUA is detected on an endpoint, Defender for Endpoint on Linux keeps a record of the infection in the threat history. The history can be visualized from the Microsoft Defender portal or through the `mdatp` command-line tool. The threat name will contain the word "Application".

## Configure PUA protection

PUA protection in Defender for Endpoint on Linux can be configured in one of the following ways:

- **Off**: PUA protection is disabled.
- **Audit**: PUA files are reported in the product logs, but not in Microsoft Defender XDR. No record of the infection is stored in the threat history and no action is taken by the product.
- **Block**: PUA files are reported in the product logs and in Microsoft Defender XDR. A record of the infection is stored in the threat history and action is taken by the product.

> [!WARNING]
> By default, PUA protection is configured in **Audit** mode.

You can configure how PUA files are handled from the command line or from the management console.

### Use the command-line tool to configure PUA protection:

In Terminal, execute the following command to configure PUA protection:

```bash
mdatp threat policy set --type potentially_unwanted_application --action [off|audit|block]
```

### Use the management console to configure PUA protection:

In your enterprise, you can configure PUA protection from a management console, such as Puppet or Ansible, similarly to how other product settings are configured. For more information, see [Threat type settings](linux-preferences.md#threat-type-settings) in [Set preferences for Defender for Endpoint on Linux](linux-preferences.md).

## Related articles

- [Set preferences for Defender for Endpoint on Linux](linux-preferences.md)
[!INCLUDE [Microsoft Defender for Endpoint Tech Community](../includes/defender-mde-techcommunity.md)]
