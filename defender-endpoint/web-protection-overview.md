---
title: Web protection
description: Learn about the web protection in Microsoft Defender for Endpoint and how it can protect your organization.
search.appverid: met150
ms.service: defender-endpoint
ms.author: deniseb
author: denisebmsft
ms.reviewer: tdoucette
ms.localizationpriority: medium
ms.date: 12/18/2024
manager: deniseb
audience: ITPro
ms.collection: 
- m365-security
- tier2
- mde-asr
ms.custom: partner-contribution
ms.topic: conceptual
ms.subservice: asr
---

# Web protection

[!INCLUDE [Microsoft Defender XDR rebranding](../includes/microsoft-defender.md)]

**Applies to:**

- [Microsoft Defender for Endpoint Plan 1](microsoft-defender-endpoint.md)
- [Microsoft Defender for Endpoint Plan 2](microsoft-defender-endpoint.md)
- [Microsoft Defender XDR](/defender-xdr)

> Want to experience Microsoft Defender for Endpoint? [Sign up for a free trial.](https://signup.microsoft.com/create-account/signup?products=7f379fee-c4f9-4278-b0a1-e4c8c2fcdf7e&ru=https://aka.ms/MDEp2OpenTrial?ocid=docs-wdatp-main-abovefoldlink&rtc=1)

## About web protection

Web protection in Microsoft Defender for Endpoint is a capability made up of [Web threat protection](web-threat-protection.md), [Web content filtering](web-content-filtering.md), and [Custom indicators](indicators-overview.md). Web protection lets you secure your devices against web threats and helps you regulate unwanted content. You can find Web protection reports in the Microsoft Defender portal by going to **Reports > Web protection**.

:::image type="content" source="media/web-protection.png" alt-text="The web protection cards" lightbox="media/web-protection.png":::

### Web threat protection

The cards that make up web threat protection are **Web threat detections over time** and **Web threat summary**.

Web threat protection includes:

- Comprehensive visibility into web threats affecting your organization.
- Investigation capabilities over web-related threat activity through alerts and comprehensive profiles of URLs and the devices that access these URLs.
- A full set of security features that track general access trends to malicious and unwanted websites.

> [!NOTE]
> For processes other than Microsoft Edge and Internet Explorer, web protection scenarios leverage Network Protection for inspection and enforcement:
> - IP is supported for all three protocols (TCP, HTTP, and HTTPS (TLS)).
> - Only single IP addresses are supported (no CIDR blocks or IP ranges) in custom indicators.
> - Encrypted URLs (full path) can only be blocked on first party browsers (Internet Explorer, Edge).
> - Encrypted URLs (FQDN only) can be blocked in third party browsers (i.e. other than Internet Explorer, Edge).
> - URLs loaded via HTTP connection coalescing, such as content loaded by modern CDNs, are only blocked on Microsoft browsers (Internet Explorer, Microsoft Edge), unless the CDN URL itself is added to the indicator list.
> - Network Protection will block connections on both standard and non-standard ports.
> - Full URL path blocks can be applied for unencrypted URLs.

There might be up to two hours of latency (usually less) between the time the action is taken, and the URL and IP being blocked. For more information, see [Web threat protection](web-threat-protection.md).

### Custom indicators

Custom indicator detections are also summarized in your organizations web threat reports under **Web threat detections over time** and **Web threat summary**.

Custom indicator includes:

- Ability to create IP and URL-based indicators of compromise to protect your organization against threats.
- Investigation capabilities over activities related to your custom IP/URL profiles and the devices that access these URLs.
- The ability to create Allow, Block, and Warn policies for IPs and URLs.

For more information, see [Create indicators for IPs and URLs/domains](indicator-ip-domain.md)

### Web content filtering

Web content filtering includes **Web activity by category**, **Web content filtering summary**, and **Web activity summary**.

Web content filtering includes:

- Users are prevented from accessing websites in blocked categories, whether they're browsing on-premises or away.
- You can conveniently deploy varied policies to various sets of users using the device groups defined in the [Microsoft Defender for Endpoint role-based access control settings](rbac.md).
    > [!NOTE]
    > Device group creation is supported in Defender for Endpoint Plan 1 and Plan 2.
- You can access web reports in the same central location, with visibility over actual blocks and web usage.

For more information, see [Web content filtering](web-content-filtering.md).

## Order of precedence

Web protection is made up of the following components, listed in order of precedence. Each of these components is enforced by the SmartScreen client in Microsoft Edge and by the Network Protection client in all other browsers and processes.

- Custom indicators (IP/URL, Microsoft Defender for Cloud Apps policies)
  - Allow
  - Warn
  - Block

- Web threats (malware, phish)
  - SmartScreen Intel, including Exchange Online Protection (EOP)
  - Escalations

- Web Content Filtering (WCF)

> [!NOTE]
> Microsoft Defender for Cloud Apps currently generates indicators only for blocked URLs.

The order of precedence relates to the order of operations by which a URL or IP is evaluated. For example, if you have a web content filtering policy you can create exclusions through custom IP/URL indicators. Custom Indicators of compromise (IoC) are higher in the order of precedence than WCF blocks.

Similarly, during a conflict between indicators, allows always take precedence over blocks (override logic). That means that an allow indicator takes precedence over any block indicator that is present.

The following table summarizes some common configurations that would present conflicts within the web protection stack. It also identifies the resulting determinations based on the precedence described earlier in this article.

|Custom Indicator policy|Web threat policy|WCF policy|Defender for Cloud Apps policy|Result|
|---|---|---|---|---|
|Allow|Block|Block|Block|Allow (Web protection override)|
|Allow|Allow|Block|Block|Allow (WCF exception)|
|Warn|Block|Block|Block|Warn (override)|

Internal IP addresses aren't supported by custom indicators. For a warn policy when bypassed by the end user, the site is unblocked for 24 hours for that user by default. This time frame can be modified by the Admin and is passed down by the SmartScreen cloud service. The ability to bypass a warning can also be disabled in Microsoft Edge using CSP for web threat blocks (malware/phishing). For more information, see [Microsoft Edge SmartScreen Settings](/DeployEdge/microsoft-edge-policies#smartscreen-settings-policies).

## Protect browsers

In all web protection scenarios, SmartScreen and Network Protection can be used together to ensure protection across both Microsoft and non-Microsoft browsers and processes. SmartScreen is built directly into Microsoft Edge, while Network Protection monitors traffic in non-Microsoft browsers and processes. The following diagram illustrates this concept. This diagram of the two clients working together to provide multiple browser/app coverages is accurate for all features of Web Protection (Indicators, Web Threats, Content Filtering).

> [!NOTE]
> Custom Indicators of Compromise and Web Content Filtering features are currently not supported in Application Guard sessions of Microsoft Edge. These containerized browser sessions can only enforce web threat blocks via the built-in SmartScreen protection. They cannot enforce any enterprise web protection policies.
> :::image type="content" source="/defender/media/web-protection-protect-browsers.png" alt-text="The usage of smartScreen and Network Protection together" lightbox="/defender/media/web-protection-protect-browsers.png":::

## Troubleshoot endpoint blocks

Responses from the SmartScreen cloud are standardized. Tools like Fiddler can be used to inspect the response from the cloud service, which helps determine the source of the block.

When the SmartScreen cloud service responds with an allow, block, or warn response, a response category and server context is relayed back to the client. In Microsoft Edge, the response category is what is used to determine the appropriate block page to show (malicious, phishing, organizational policy).

The following table shows the responses and their correlated features.

|ResponseCategory|Feature responsible for the block|
|---|---|
|CustomPolicy|WCF|
|CustomBlockList|Custom indicators|
|CasbPolicy|Defender for Cloud Apps|
|Malicious|Web threats|
|Phishing|Web threats|

## Advanced hunting for web protection

Kusto queries in advanced hunting can be used to summarize web protection blocks in your organization for up to 30 days. These queries use the information listed above to distinguish between the various sources of blocks and summarize them in a user-friendly manner. For example, the following query lists all WCF blocks originating from Microsoft Edge.

```kusto
DeviceEvents
| where ActionType == "SmartScreenUrlWarning"
| extend ParsedFields=parse_json(AdditionalFields)
| project DeviceName, ActionType, Timestamp, RemoteUrl, InitiatingProcessFileName, Experience=tostring(ParsedFields.Experience)
| where Experience == "CustomPolicy"
```

Similarly, you can use the following query to list all WCF blocks originating from Network Protection (for example, a WCF block in a non-Microsoft browser). The `ActionType` is updated and `Experience` changed to `ResponseCategory`.

```kusto
DeviceEvents
| where ActionType == "ExploitGuardNetworkProtectionBlocked"
| extend ParsedFields=parse_json(AdditionalFields)
| project DeviceName, ActionType, Timestamp, RemoteUrl, InitiatingProcessFileName, ResponseCategory=tostring(ParsedFields.ResponseCategory)
| where ResponseCategory == "CustomPolicy"
```

To list blocks that are due to other features (like Custom Indicators), refer to the table listed earlier in this article. The table outlines each feature and their respective response category. These queries can be modified to search for telemetry related to specific machines in your organization. The ActionType shown in each query shows only those connections that were blocked by a Web Protection feature, and not all network traffic.

## User experience

If a user visits a web page that poses a risk of malware, phishing, or other web threats, Microsoft Edge triggers a block page that resembles the following image:

:::image type="content" source="media/web-protection-indicators-new-block-page.jpg" alt-text="Screenshot showing new block notification for a website." lightbox="media/web-protection-indicators-new-block-page.jpg":::

Beginning with Microsoft Edge 124, the following block page is shown for all Web Content Filtering category blocks.

:::image type="content" source="media/web-protection-new-content-blocked-page.jpg" alt-text="Screenshot showing content blocked." lightbox="media/web-protection-new-content-blocked-page.jpg":::

In any case, no block pages are shown in non-Microsoft browsers, and the user sees a "Secure Connection Failed" page along with a toast notification. Depending on the policy responsible for the block, a user sees a different message in the toast notification. For example, web content filtering displays the message, "This content is blocked."

## Report false positives

To report a false positive for sites that have been deemed dangerous by SmartScreen, use the link that appears on the block page in Microsoft Edge (as shown earlier in this article).

For WCF, you can dispute the category of a domain. Navigate to the **Domains** tab of the WCF reports. You see an ellipsis beside each of the domains. Hover over this ellipsis and select **Dispute Category**. A flyout opens. Set the priority of the incident and provide some other details, such as the suggested category. For more information on how to turn on WCF and how to dispute categories, see [Web content filtering](web-content-filtering.md).

For more information on how to submit false positives/negatives, see [Address false positives/negatives in Microsoft Defender for Endpoint](defender-endpoint-false-positives-negatives.md).

## Related articles

|Article|Description|
|---|---|
|[Web threat protection](web-threat-protection.md) | Prevent access to phishing sites, malware vectors, exploit sites, untrusted or low-reputation sites, and sites that are blocked.|
|[Web content filtering](web-content-filtering.md) | Track and regulate access to websites based on their content categories.|

[!INCLUDE [Microsoft Defender for Endpoint Tech Community](../includes/defender-mde-techcommunity.md)]
