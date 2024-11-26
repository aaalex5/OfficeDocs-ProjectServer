---
title: "Gain access to Rosters in Microsoft Planner"
f1.keywords:
- NOCSH
ms.author: jenz
author: jenzamora
manager: jtremper
ms.date: 10/30/2024
ms.reviewer: dahopkin
audience: Admin
ms.topic: article
ms.service: planner
ms.localizationpriority: medium
search.appverid:
- MET150
description: "Learn how to access the roster in Microsoft Planner."
---

# Gain access to Rosters in Microsoft Planner

Rosters are a type of container for Microsoft Planner plans, primarily used for plans associated with task lists in Loop. Access to Roster-contained plans is controlled by the members on the Roster.

In General Data Protection Regulation (GDPR) scenarios where the tenant admin is working on a data subject delete request, they might require access to Roster contained plans. The following PowerShell cmdlets support that access. The tenant admin can use these commands to add and remove a member. This process can be done to give themselves or another person access to a specific roster and the plan data within.

## Prerequisites for making Planner changes in Windows PowerShell

Before you begin, make sure you complete the steps outlined in [Prerequisites for making Planner changes in Windows PowerShell](prerequisites-for-powershell.md) to make Planner changes in Windows PowerShell.

## Manage Roster membership

 > [!Note]
 > If you use Microsoft Planner in a different M365 endpoint, then additionally include `-HostName <hostname Of Microsoft Planner website>` on the following commands. The default is `tasks.office.com`.

1. Run the following command to add a user to a Roster’s membership. The value returned is the added user’s Microsoft Entra user ID.

     ```PowerShell
     Add-PlannerRosterMember -RosterId <Roster Id> -UserAadIdOrPrincipalName "<User’s AAD ID or UPN>"
     ```

2. Run the following command to remove a user from the Roster’s membership. No value is returned, but when this command completes the user is removed.  

     ```PowerShell
     Remove-PlannerRosterMember -RosterId <Roster Id> -UserAadIdOrPrincipalName "<User’s AAD ID or UPN>"
     ```
