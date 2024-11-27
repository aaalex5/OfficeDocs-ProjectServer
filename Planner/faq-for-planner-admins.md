---
# Required metadata
# For more information, see https://review.learn.microsoft.com/en-us/help/platform/learn-editor-add-metadata?branch=main
# For valid values of ms.service, ms.prod, and ms.topic, see https://review.learn.microsoft.com/en-us/help/platform/metadata-taxonomies?branch=main

title:       Frequently asked questions for admins about Microsoft Planner
description: Get answers to frequently asked questions about Microsoft Planner. This article is specific to an admin audience.
author:      ashnapatel01 # GitHub alias
ms.author:   ashnapatel # Microsoft alias
manager: dellerderick
ms.reviewer: namerali
ms.service:  planner
ms.localizationpriority: medium
search.appverid: MET150
f1.keywords: NOCSH
# ms.prod:   # To use ms.prod, uncomment it and delete ms.service
ms.topic:    how-to
ms.date:     11/26/2024
---
# Frequently asked questions for admins about Microsoft Planner

## Can I see who's already using Planner, or see a list of all the Planner sites?

You can see a list of all groups in the Microsoft 365 admin center, in the Groups section, and find out more detailed information about these groups using [Microsoft 365 Reports in the admin center - Microsoft 365 Groups](https://support.office.com/article/office-365-reports-in-the-admin-center--office-365-groups-a27f1a99-3557-4f85-9560-a28e3d822a40). Every group comes with a plan, but a list of plans and active usage of plans are not included in these reports right now.

## Can people outside of my organization get invited to participate in a plan?

Yes. Guest access allows you to invite people who aren't part of your Microsoft 365 organization to participate in a plan. Guest users will have limited functionality, but can perform the following tasks:

- Create and delete tasks and buckets
- Edit task fields
- Attach a file or link to a task, if given additional permission
- Edit the plan name

For more information, see [Guest access in Microsoft Planner](https://support.office.com/article/guest-access-in-microsoft-planner-cc5d7f96-dced-4da4-ab62-08c72d9759c6).

## Can people in my organization use Planner if they don't have an Exchange Online mailbox?

- If you're using Microsoft Planner in a hybrid environment in which your users may have Exchange Online or on-premises mailboxes, note that:
- Planner has full functionality when your user has a product license that includes Exchange Online. Planner users without Exchange Online may have issues with viewing or adding comments to a task.

## How do I change the domain that Planner email notifications come from?

If you're interested in having your notification emails come from a custom email domain, follow the steps described in [Multi-domain support for Microsoft 365 Groups - Admin help](https://support.office.com/article/multidomain-support-for-office-365-groups--admin-help-7cf5655d-e523-4bc3-a93b-3ccebf44a01a).

## How do I make sure all my users can get emails for Planner?

In Planner, users can choose to receive emails when tasks are assigned to them or when tasks are due soon or late (see [Choose whether to have email sent directly to you](https://support.office.com/article/stay-on-top-of-tasks-and-plans-with-email-and-notifications-cce223d6-b0ae-43cf-a080-266e2414a859#bkmk_choosewhethertohaveemailsenttoyou_1_1_1_1_1)). However, email will only be sent to users who have a product license that includes Exchange Online. Users at organizations using on-premises Exchange Server or hybrid configurations may not receive all Planner emails.

## Where is data for the Microsoft Planner app in Microsoft Teams stored?

The Planner app in Teams gives users a way to manage their tasks and plans in one place. The storage location of Planner data depends on the service used to create the tasks, plans, and projects. 

- Tasks in Todo and Outlook are stored in Exchange.
- Plans and their included tasks are stored in Azure.
- Attachments on  tasks in plans and projects are stored in the SharePoint location of the group.
- Projects and their included tasks are stored in Dataverse.

> [!NOTE]
> For details about the support for advanced compliance capabilities such as eDiscovery and Auditing across these different services, refer to the documentation for Microsoft Purview.

## Can I see who's already using Planner, or see a list of all the Planner sites?

You can see a list of all groups in the Microsoft 365 admin center, in the Groups section, and find out more detailed information about these groups using [Microsoft 365 Reports in the admin center - Microsoft 365 Groups](https://support.office.com/article/office-365-reports-in-the-admin-center--office-365-groups-a27f1a99-3557-4f85-9560-a28e3d822a40). Every group comes with a plan, but a list of plans and active usage of plans aren't included in these reports right now.

## How do I turn off Planner for my organization?

When Microsoft Planner is included in your subscription, it's automatically turned on for everyone in your organization. If you want to control which people in your organization have licenses for Planner, for example, if your organization isn't ready to begin using Planner, you can remove or assign Planner licenses by using Office 365 PowerShell.

To control which users have Planner licenses, follow the instructions in [How to use Office 365 PowerShell to manage Microsoft Planner licenses](/office365/troubleshoot/licensing/how-to-use-office-365-powershell-to-manage-microsoft-planner-licenses). When running the scripts in Office 365 PowerShell, the DisabledPlans value for Microsoft Planner is PROJECTWORKMANAGEMENT.

To turn off Planner for your organization, see [How to turn off Planner for your organization](disable-planner.md)

To turn off just the Planner Loop component, see [How to turn off the Planner component for your organization](disable-planner-component.md).

> [!NOTE]
> Removing a user's Planner license only prevents them from navigating to Planner using the Planner tile. Users in your organization without licenses to Planner can still create and modify plans at the direct Planner URL: planner.</span>cloud.</span>microsoft. You can remove users' ability to create plans at planner.cloud.microsoft (see [How do I manage who can create a plan?](#how-do-i-manage-who-can-create-a-plan)), but you can't remove their ability to see and modify existing plans at planner.</span>cloud.</span>microsoft at this time.

## How do I turn off Outlook calendar sync in Planner for my organization?

Outlook calendar sync in Microsoft Planner allows users to view their Planner schedule in Outlook. This feature is turned on automatically in Planner. If you want to turn this off for your organization, follow the steps in [Turn off Outlook calendar sync in Planner for your organization](turn-off-outlook-calendar-sync.md).

## How do I turn off the Planner Loop component for my organization?

The Planner component allows users to view and edit Planner plans as a Loop component in the Loop app and in Microsoft apps that support Loop, for example, in Outlook and Teams. To turn off the component for your organization, follow the steps in [How to turn off the Planner component for your organization](disable-planner-component.md).

See [Use the Planner component in Loop](https://support.microsoft.com/office/use-the-planner-component-in-loop-545e967a-7c69-4e9a-9458-dfabdcf1d752) for details on how the component can be used.

> [!NOTE]
> The Planner component is a new feature that may not yet be available in all Microsoft apps that support Loop.

## How can I apply CA policies to the Planner iOS and Android apps?

To apply CA policies to the Planner iOS and Android apps, please make sure that CA policies are enabled for Exchange or SharePoint within Microsoft Intune in the Azure portal. Enabling CA policy for Planner alone (without policies enabled for Exchange or SharePoint) does not apply the policies for the Planner iOS and Android apps.


