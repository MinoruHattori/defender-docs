---
title: What's the difference between junk email and bulk email?
f1.keywords: 
  - NOCSH
ms.author: chrisda
author: chrisda
manager: deniseb
audience: ITPro
ms.topic: conceptual
ms.localizationpriority: medium
search.appverid: 
  - MET150
ms.assetid: 8079f193-1b40-4081-9e5d-d0e50dfbcc59
ms.collection: 
  - m365-security
  - tier2
ms.custom: 
  - seo-marvel-apr2020
description: Admins can learn about the differences between junk email (spam) and bulk email (gray mail) in Exchange Online Protection (EOP).
ms.service: defender-office-365
ms.date: 07/25/2024
appliesto:
  - ✅ <a href="https://learn.microsoft.com/defender-office-365/eop-about" target="_blank">Exchange Online Protection</a>
  - ✅ <a href="https://learn.microsoft.com/defender-office-365/mdo-about#defender-for-office-365-plan-1-vs-plan-2-cheat-sheet" target="_blank">Microsoft Defender for Office 365 Plan 1 and Plan 2</a>
  - ✅ <a href="https://learn.microsoft.com/defender-xdr/microsoft-365-defender" target="_blank">Microsoft Defender XDR</a>
---

# What's the difference between junk email and bulk email in EOP?

In Microsoft 365 organizations with mailboxes in Exchange Online or standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes, customers sometimes ask: "What's the difference between junk email and bulk email?" This article explains the difference and describes the controls that are available in EOP.

- **Junk email** is spam, which is an unsolicited and universally unwanted message (when identified correctly). EOP rejects spam based on the reputation of the source email server. If a message passes source IP inspection, it continues through spam filtering. If the message is classified as **Spam** or **High confidence spam** by spam filtering, what happens to the message depends on the verdict and the anti-spam policy that detected the message.

  For the default actions that are taken on spam and high confidence spam messages in the default anti-spam policy and in the Standard and Strict [preset security policies](preset-security-policies.md), see the **Spam** and **High confidence spam** entries in [EOP anti-spam policy settings](recommended-settings-for-eop-and-office365.md#eop-anti-spam-policy-settings).

  In the default anti-spam policy and in custom anti-spam policies, you can configure the action to take on spam filtering verdicts. For instructions, see [Configure anti-spam policies in EOP](anti-spam-policies-configure.md).

  If you disagree with the spam filtering verdict, you can report messages as spam or good to Microsoft in several ways, as described in [Report messages and files to Microsoft](submissions-report-messages-files-to-microsoft.md).

- **Bulk email** (also known as _gray mail_), is more difficult to classify. Whereas spam is a constant threat, bulk email is often one-time advertisements or marketing messages. Some users want bulk email messages (and in fact, they have deliberately signed up to receive them), while other users consider bulk email to be spam. For example, some users want to receive advertising messages from the Contoso Corporation or invitations to an upcoming conference on cybersecurity, while other users consider these same messages to be spam.

  For more information about how bulk email is identified, see [Bulk complaint level (BCL) in EOP](anti-spam-bulk-complaint-level-bcl-about.md).

## How to manage bulk email

Because of the mixed reaction to bulk email, there isn't universal guidance that applies to every organization.

Anti-spam policies have a default BCL threshold that's used to identify bulk email as spam, and a specific action to take on those bulk messages. For more information, see the following articles:

- [Bulk complaint level (BCL) in EOP](anti-spam-bulk-complaint-level-bcl-about.md)
- [Configure anti-spam policies in EOP](anti-spam-policies-configure.md).
- [EOP anti-spam policy settings](recommended-settings-for-eop-and-office365.md#eop-anti-spam-policy-settings)

Another option that's easy to overlook: if a user complains about receiving bulk email, but the messages are from reputable senders that pass spam filtering in EOP, have the user check for an unsubscribe option in the bulk email message.

## How to tune bulk email

Admins can follow the [recommended bulk threshold values](recommended-settings-for-eop-and-office365.md#anti-spam-anti-malware-and-anti-phishing-protection-in-eop) or choose a bulk threshold value that suits the needs of their organization.

### Tune bulk email in organizations with Exchange Online Protection

> [!TIP]
> The bulk senders insight is currently in Preview, isn't available in all organizations, and is subject to change.

In organizations with EOP, if you select **Edit spam threshold and properties** at the bottom of the **Bulk email threshold & spam properties** section in the details flyout of the default anti-spam policy or a custom anti-spam policy that you select from the **Anti-spam policies** page at <https://security.microsoft.com/antispam>, the **Bulk email threshold** section contains the bulk senders insight: information about the number of messages that were detected as bulk at all BCL levels by all anti-spam policies over the last 60 days.

- By default, the bulk senders insight shows the number of messages that were delivered and identified as bulk at the current BCL threshold of the anti-spam policy.

  :::image type="content" source="media/anti-spam-policy-bulk-senders-insight-bcl-default.png" alt-text="The bulk senders insight in the Bulk email threshold section of an anti-spam policy showing the messages identified as bulk at the current BCL level." lightbox="media/anti-spam-policy-bulk-senders-insight-bcl-default.png":::

- If you decrease the bulk email threshold value, the bulk senders insight changes to show how many fewer messages would be delivered and how many more messages would be identified as bulk. The insight also shows how many bulk message identifications are likely to be false positives (good email identified as bad).

  :::image type="content" source="media/anti-spam-policy-bulk-senders-insight-bcl-lower.png" alt-text="The bulk senders insight in the Bulk email threshold section of an anti-spam policy showing the messages identified as bulk after you decrease the current BCL level." lightbox="media/anti-spam-policy-bulk-senders-insight-bcl-lower.png":::

- If you increase the bulk email threshold value, the bulk senders insight changes to show how many more messages would be delivered and how many fewer messages would be identified as bulk. The insight also shows how many bulk message identifications are likely to be false negatives (bad email delivered).

  :::image type="content" source="media/anti-spam-policy-bulk-senders-insight-bcl-higher.png" alt-text="The bulk senders insight in the Bulk email threshold section of an anti-spam policy showing the messages identified as bulk after you increase the current BCL level." lightbox="media/anti-spam-policy-bulk-senders-insight-bcl-higher.png":::

Selecting **View bulk senders insight** takes you to the main **Bulk sender insights** page. For more information, see [Bulk senders insight in Exchange Online Protection](anti-spam-bulk-senders-insight.md).

### Tune bulk email in organizations with Defender for Office 365 Plan 1 or Plan 2

If you have Defender for Office 365 Plan 1 or Plan 2, you can use the [Threat protection status report](reports-email-security.md#threat-protection-status-report) to identify wanted and unwanted bulk senders:

1. Open the **Threat protection status** report at <https://security.microsoft.com/reports/TPSAggregateReportATP>.

2. Select **View data by Email \> Spam** and **Chart breakdown by Detection Technology**.

3. Select :::image type="icon" source="media/m365-cc-sc-filter-icon.png" border="false"::: **Filter**. In the **Filters** flyout that opens, select only **Bulk** in the **Detection** section.

   Use the **Bulk complaint level** slider to filter the bulk detections by BCL value.

   When you're finished in the **Filters** flyout, select **Apply**.

4. Back on the **Threat protection status** page, select one of the bulk messages from the details table below the chart by clicking anywhere in the row other than the check box next to the first column.

   In the message details flyout that opens, select :::image type="icon" source="media/m365-cc-sc-open-icon.png" border="false"::: **Open email entity** at the top of the flyout to see details about the message in [the Email entity page in Microsoft Defender for Office 365](mdo-email-entity-page.md).

5. After you identify wanted and unwanted bulk senders, adjust the bulk threshold in the default anti-spam policy and in custom anti-spam policies. If some bulk senders don't fit within your bulk threshold, [report the messages to Microsoft for analysis](submissions-admin.md#report-good-email-to-microsoft).

### Tune bulk email in organizations with Defender for Office 365 Plan 2

As of September 2022, Microsoft Defender for Office 365 Plan 2 customers can access BCL from [advanced hunting](/defender-xdr/advanced-hunting-overview). This feature allows admins to look at all bulk senders who sent mail to their organization, their corresponding BCL values, and the amount of email that was received. You can drill down into the bulk senders by using other columns in **EmailEvents** table in the **Email & collaboration** schema. For more information, see [EmailEvents](/defender-xdr/advanced-hunting-emailevents-table).

For example, if Contoso sets their bulk threshold to 7 in anti-spam policies, Contoso recipients receive email from all senders in their Inbox if the BCL value is 6 or less. Admins can run the following query to get a list of all bulk senders in the organization:

```console
EmailEvents
| where BulkComplaintLevel >= 1 and Timestamp > datetime(2022-09-XXT00:00:00Z)
| summarize count() by SenderMailFromAddress, BulkComplaintLevel
```

This query allows admins to identify wanted and unwanted senders. If a bulk sender has a BCL score that's more than the bulk threshold, admins can [report the sender's messages to Microsoft for analysis](submissions-admin.md#report-good-email-to-microsoft). This action also adds the sender as an allow entry in the Tenant Allow/Block List.

Organizations without Defender for Office 365 Plan 2 can try the features in Microsoft Defender XDR for Office 365 Plan 2 for free. Use the 90-day Defender for Office 365 evaluation at <https://security.microsoft.com/atpEvaluation>. Learn about who can sign up and trial terms [here](try-microsoft-defender-for-office-365.md).
