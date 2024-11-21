---
title: "Turn off Outlook calendar sync in Planner for your organization"
f1.keywords:
- NOCSH
ms.author: jenz
author: jenzamora
manager: jtremper
ms.reviewer: dahopkin
ms.date: 10/30/2024
audience: Admin
ms.topic: article
ms.service: planner
ms.subservice: planner
ms.localizationpriority: high
search.appverid:
- MET150
description: "If you're a global admin and you want to turn off calendar sync in Microsoft Planner, you can use Windows PowerShell"
---

# Turn off Outlook calendar sync in Planner for your organization

> [!NOTE]
> Looking for how to sync your personal Outlook calendar with Planner? [See your Planner calendar in Outlook](https://support.office.com/article/see-your-planner-calendar-in-outlook-5dcccce5-2750-49b5-991b-1837379d96c7).

Calendar sync in Planner allows users to view their scheduled Planner tasks directly in their Outlook calendar. This integration helps users manage their schedules more effectively by consolidating tasks and calendar events in one place. When calendar sync is enabled, users can generate iCalendar links for basic plans and their 'Assigned to me' tasks. These iCalendar links can be used in Outlook and other calendar apps to sync scheduled tasks as events in the calendar, providing a unified view of work commitments and deadlines.

> [!WARNING]
> iCalendar links do not require authentication. Anyone with access to the iCalendar link can view task details for the synced tasks as long as the link is enabled.

If you're a global admin and you want to turn off calendar sync in Microsoft Planner, you can use Windows PowerShell. Planner is automatically turned on for all organizations that have Planner as part of their subscription.

## Prerequisites for making Planner changes in Windows PowerShell

Before you begin, make sure you complete the steps outlined in [Prerequisites for making Planner changes in Windows PowerShell](prerequisites-for-powershell.md) to make Planner changes in Windows PowerShell.

## Turn off or on Outlook calendar sync in Planner using PowerShell

1. Open PowerShell and run the following command to turn off Outlook calendar sync for Planner:

   ```PowerShell
   Set-PlannerConfiguration -AllowCalendarSharing $false
   ```

   To turn Outlook calendar sync back on in Planner:

   ```PowerShell
   Set-PlannerConfiguration -AllowCalendarSharing $true
   ```

   > [!NOTE]
   > You'll need to sign in using your Microsoft Entra credentials.

2. To verify your settings, run:

   ```PowerShell
   Get-PlannerConfiguration
   ```
   - In Planner, go to **Planner** > **My Tasks**. Select the ellipses (...). Outlook calendar sync is enabled if you see the **Add "My Tasks" to Outlook calendar** command, and disabled if you don't.
