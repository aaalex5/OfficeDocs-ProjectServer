---
title:       "How to turn off the Planner for your organization"
description: "Learn how to turn off Planner for your organization in Microsoft apps that support Planner."
author:      danlucuiPlanner
manager:     bribha
ms.author:   danlucui
ms.service: planner
ms.topic:    article
ms.date:     10/25/2024
---

# Turn off Planner for your organization

> [!IMPORTANT]
> This article applies to:
> - Basic plans in the Planner app in Teams and Web
> - All plans in other Planner endpoints (including Planner mobile, and Planner connectors)
> For
> - premium plans, see [Disable Project for the web or Roadmap from Microsoft 365](/project-for-the-web/remove-roadmap-from-office-365).
> -  To Do lists, see [Manage access to Microsoft Whiteboard for your organization](/microsoft-365/whiteboard/manage-whiteboard-access-organizations).


If you're a global admin and you want to turn off Microsoft Planner for your organization, you can use Windows PowerShell. Planner is automatically turned on for all organizations that  have Planner as part of their subscription.
- [Prerequisites for making Planner changes in Windows PowerShell](#prerequisites-for-making-planner-changes-in-windows-powershell)
- [Turn off Planner for your organization using PowerShell](#turn-off-planner-for-your-organization-using-powershell)

## Prerequisites for making Planner changes in Windows PowerShell

Follow the steps in [Prerequisites for making Planner changes in Windows PowerShell](prerequisites-for-powershell.md) to make Planner changes in Windows PowerShell.

## Turn off Planner for your organization using PowerShell

1. Open PowerShell and run the following command to turn off Outlook calendar sync for Planner:

   ```powershell
   Set-PlannerConfiguration -IsPlannerAllowed $false
   ```
   
   To turn on Planner back:
   
   ```powershell
   Set-PlannerConfiguration -IsPlannerAllowed $true
   ```
   
   > [!NOTE]
   > You'll need to sign in using your Microsoft Entra credentials.

2. To verify your settings, run:

   ```PowerShell
   Get-PlannerConfiguration
   ```
