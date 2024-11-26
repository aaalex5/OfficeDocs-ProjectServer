---
title: "Block a user from deleting tasks in Microsoft Planner they didn't create"
f1.keywords:
ms.author: jenz
author: jenzamora
manager: jtremper
ms.date: 10/25/2024
audience: Admin
ms.topic: article
ms.service: office-perpetual-itpro
ms.localizationpriority: high
search.appverid:
description: "This article shares information on how admins can block a user from deleting tasks the user didn't create"
---

# Block a user from deleting tasks not created by themselves

> [!IMPORTANT]
>
> This article applies to basic plans only.

Tenant admins can block a specific user from deleting tasks in Microsoft Planner that they didn't create.

## Prerequisites for making Planner changes in Windows PowerShell

Follow the steps in [Prerequisites for making Planner changes in Windows PowerShell](prerequisites-for-powershell.md) to make Planner changes in Windows PowerShell.

## Block a user from deleting tasks they didn't create

1. Use the Set-PlannerUserPolicy cmdlet to block a user from deleting Planner tasks that they didn't create.

   ```PowerShell
    Set-PlannerUserPolicy -UserAadIdOrPrincipalName <user's AADId or UPN> -BlockDeleteTasksNotCreatedBySelf $true
   ```

   |Parameter|Description|
   |:-------------------------|:---|
   |UserAadIdOrPrincipalName|Use either the Microsoft Entra ID or the UPN of the user to modify the setting for.|
   |BlockDeleteTasksNotCreatedBySelf|Whether or not to block the user from deleting tasks not created by themselves.|
   |HostName|You only need to use this parameter if you access Planner through a host name other than `task.</span>office.</span>com`. For example, if you access Planner through `tasks.</span>office365.</span>us`, include `-HostName tasks.</span>office365</span>.us` in your command.|
   |||

    The following cmdlet will block amyg@contoso.onmicrosoft.com from deleting tasks in Planner that they didn't create.

      ```PowerShell
       Set-PlannerUserPolicy -UserAadIdOrPrincipalName amyg@contoso.onmicrosoft.com -BlockDeleteTasksNotCreatedBySelf $true
      ```

2. When you're prompted to authenticate, sign in as yourself (the global admin), not the user you want to unblock.

## Unblock a user from deleting tasks they didn't create

1. Use the Set-PlannerUserPolicy cmdlet to unblock a user from deleting Planner tasks that they didn't create.

   ```PowerShell
     Set-PlannerUserPolicy -UserAadIdOrPrincipalName "<User's AAD ID or UPN>"  -BlockDeleteTasksNotCreatedBySelf $false
   ```

   |Parameter&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Description|
   |:---|:---|
   |UserAadIdOrPrincipalName|Use either the Microsoft Entra ID or the UPN of the user to modify the setting for.|
   |BlockDeleteTasksNotCreatedBySelf|Whether or not to block the user from deleting tasks not created by themselves.|
   |HostName|You only need to use this parameter if you access Planner through a host name other than `task.</span>office.</span>com`. For example, if you access Planner through `tasks.</span>office365.</span>us`, include `-HostName tasks.</span>office365</span>.us` in your command.|
   |||

    For example, the following cmdlet will unblock amyg@contoso.onmicrosoft.com from deleting tasks in Planner that they didn't create.

     ```PowerShell
      Set-PlannerUserPolicy -UserAadIdOrPrincipalName amyg@contoso.onmicrosoft.com -BlockDeleteTasksNotCreatedBySelf $false
     ```

2. When you're prompted to authenticate, sign in as yourself (the global admin), not the user you want to unblock.

## Get a user's current policy

1. Check a user's current policy with the Get-PlannerUserPolicy cmdlet.

    ```PowerShell
      Get-PlannerUserPolicy -UserAadIdOrPrincipalName "<User's AAD ID or UPN>"
    ```

   |Parameter&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Description|
   |------------------------|---|
   |UserAadIdOrPrincipalName|Use either the Microsoft Entra ID or the UPN of the user to view the setting for.|
   |HostName|You only need to use this parameter if you access Planner through a host name other than `task.</span>office.</span>com`. For example, if you access Planner through `tasks.</span>office365.</span>us`, include `-HostName tasks.</span>office365</span>.us` in your command.|
   |||

   For example, the following cmdlet will get the user amyg@contoso.onmicrosoft.com current policy setting

    ```PowerShell
     Get-PlannerUserPolicy -UserAadIdOrPrincipalName amyg@contoso.onmicrosoft.com | fl
     @odata.context                   : https://tasks.office.com/taskApi/tenantAdminSettings/$metadata#UserPolicy/$entity
     id                               : amyg@contoso.onmicrosoft.com
     blockDeleteTasksNotCreatedBySelf : False
   ```

2. When you're prompted to authenticate, sign in as yourself (the global admin).
