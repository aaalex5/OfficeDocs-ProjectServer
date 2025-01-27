---
title: "Export user data from Microsoft Planner"
f1.keywords:
- NOCSH
ms.author: jenz
author: jenzamora
manager: jtremper
ms.date: 01/27/2025
audience: Admin
ms.topic: article
ms.service: planner
ms.localizationpriority: high
search.appverid:
- MET150
description: "This article describes how a global admin can export data for a specific user from Microsoft Planner.  "
---

# Export user data from Microsoft Planner

> [!IMPORTANT]
>
> This article applies to:
>
> - Basic plans in the Planner app in Teams
> - All plans in other Planner endpoints (including Planner web, Planner mobile, and Planner connectors)
>
> It doesn't apply to To Do lists or premium plans in the Planner app in Teams. [Learn more about the Planner app in Teams](/microsoftteams/manage-planner-app).

This article describes how a global admin can export data for a specific user from Microsoft Planner. The exported data includes data about the user contained in Planner, and also data contained in plans that the user was a part of. The exporting process is done through Windows PowerShell.

> [!NOTE]
> A global admin can export Microsoft Planner user telemetry data through the [Data Log Export Tool](https://go.microsoft.com/fwlink/?linkid=872273) on the [Microsoft Service Trust Portal](https://go.microsoft.com/fwlink/?linkid=872274).

## Prerequisites for making Planner changes in Windows PowerShell

Follow the steps in [Prerequisites for making Planner changes in Windows PowerShell](prerequisites-for-powershell.md) to make Planner changes in Windows PowerShell.

## To export user content from Planner

1. From Windows PowerShell, use the Export-PlannerUserContent cmdlet to export your user's content from Planner.

   ```PowerShell
   Export-PlannerUserContent -UserAadIdOrPrincipalName <user's AADId or UPN> -ExportDirectory <output location>
   ```

    |Parameter|Description|
    |---|---|
    |UserAadIdOrPrincipalName|Use either the Microsoft Entra ID or the UPN of the user for which you want to export content.|
    |ExportDirectory|Location to store your output files. The folder should already exist.|
    |HostName|You only need to use this parameter if you access Planner through a host name other than *task.</span>office.</span>com*. For example, if you access Planner through *tasks.</span>office365.</span>us*, include *-HostName tasks.</span>office365</span>.us* in your command.|
    
    For example, the following will export Adam Barr's user information from Planner using his UPN, and will download the export files to the location C:\PlannerExportAdamBarr.

   ```PowerShell
    Export-PlannerUserContent -UserAadIdOrPrincipalName adambarr@contoso.onmicrosoft.com -ExportDirectory C:\PlannerExportAdamBarr
   ```

2. You'll be prompted to authenticate. Sign-in as yourself (the global admin), not the user you want to export.

3. After the PowerShell cmdlet runs successfully, go to your export location to view your user's exported data files.

## What gets exported and how to read it

After running the PowerShell cmdlet to export your user's data from Planner, you'll receive two types of files in your download location folder:

- A single user file in json format with information about the user.
- One json file for each plan in which the user has a task assigned to them.

## How to read your exported files

You can use the information in this section to help you understand the properties you'll see in both the user and plan json files you received.

## User file

The user file name will be prefixed with "User" and the Microsoft Planner ID of the user. It will have the following properties:

|Property|Description|
|---|---|
|User.Id|Microsoft Planner ID of the user.|
|User.ExternalId|Microsoft Entra ID of the user.|
|User.DisplayName|Display name of the user.|
|User.InternalDisplayName|Microsoft Planner display name of the user.|
|User.UserPrincipalName|User Principal Name (UPN) of the user.|
|User.PrincipalType|Value is always "User."|
|User.SignedInFromPlannerCoreApp|If the user ever signed in from one of the Planner core apps.|
|User.UserDetailsId|Unique identifier of the details object for the user.|
|User.ICalendarPublishEnabled|If True, ICalendar sharing is enabled for the plan. Go to [See your Planner calendar in Outlook](https://support.office.com/article/see-your-planner-calendar-in-outlook-5dcccce5-2750-49b5-991b-1837379d96c7) for more information.|
|User.OptedInNotifications|Notifications for which the user opted in.|
|User.OptedOutNotifications|Notifications for which the user opted out.|
|User.UserPolicies|User specific authorization policies applied to this user.|
|User.FavoritePlans|Bookmark for plans the user has favorited.|
|User.FavoritePlans.Id|Microsoft Planner ID of the plan.|
|User.FavoritePlans.BookmarkName|Name assigned to the bookmark.|
|User.FavoritePlans.OrderHint|Used for sorting order. See [Using order hints in Microsoft Planner](/graph/api/resources/planner-order-hint-format).|
|User.RecentPlans|Plans recently opened by the user.|
|User.RecentPlans.Id|Microsoft Planner ID of the plan.|
|User.RecentPlans.BookmarkName|Name assigned to the bookmark.|
|User.RecentPlans.LastAccess|When the plan was last opened.|
|User.UserData|Custom data from the Planner Web Client.|
|User.UserData.Key|Custom data key.|
|User.UserData.Value|Custom data value.|
|User.AssignedTaskOrdering|Sorting order for tasks assigned to the user.|
|User.AssignedTaskOrdering.PlanId|Microsoft Planner ID of the plan that contains the task.|
|User.AssignedTaskOrdering.Id|Microsoft Planner ID of the task.|
|User.AssignedTaskOrdering.Order|Used for sorting order. See [Using order hints in Microsoft Planner](/graph/api/resources/planner-order-hint-format).|
|User.AssignedTaskOrdering.Title|The title of the task.|

## Plan files

Each plan file name will be prefixed with "Plan" and the Microsoft Planner ID of the plan. Each file will have the following properties:

|Property |Description |
|---|---|
|Plan.Id|Microsoft Planner ID of the plan. |
|Plan.ArchivalInfo|Archival status of the plan.|
|Plan.ArchivalInfo.IsArchived|Is the plan archived.|
|Plan.ArchivalInfo.Justification|Reason why the plan was archived or unarchived.|
|Plan.ArchivalInfo.StatusChangedBy|The user that updated the archival status. See User properties for more detail.|
|Plan.ArchivalInfo.StatusChangedByAppId|Identity of the app that updated the archival status.|
|Plan.ArchivalInfo.StatusChangedDate|When the archival status was updated.|
|Plan.Title|Title of the plan. <br/>*Note*: Plans with the title `RosterPlaceholderPlan_{89F9907E-D21D-4C90-A4B8-7A76CF3E6F70}` indicate that the current file represents a Roster that has been created but doesn't yet have a plan created inside it.|
|Plan.Owner|Owner of the plan (a Group or User entity).|
|Plan.Owner.Id|Microsoft Planner ID of the entity (Group or User). |
|Plan.Owner.ExternalId|Microsoft Entra ID of the entity (Group or User).|
|Plan.Owner.DisplayName|Display name of the owner (Group or User).|
|Plan.Owner.UserPrincipalName|User Principal Name (UPN) if the owner is a user.  |
|Plan.Owner.PrincipalType|The entity type (Group or User).|
|Plan.Container|Container for the plan.|
|Plan.Container.ContainerType|The type of container (Group, Roster). |
|Plan.Container.ExternalId|Microsoft Entra ID of the group.|
|Plan.Container.Description|Display name of the group. |
|Plan.CreatedDate|Date and time the plan was created.|
|Plan.CreatedBy|User that created the plan. See User properties for more detail.|
|Plan.CreatedByAppId|The app that created the plan.|
|Plan.ModifiedDate|Date and time the plan was last updated.|
|Plan.ModifiedBy|Name of the user that last updated the plan. See User properties for more detail.|
|Plan.ModifiedByAppId|Identifier of the app which last modified the plan.|
|Plan.CreationProcessInfo|Details about the process that created the plan.|
|Plan.CreationProcessInfo.Type|Process type that created the plan. For Copy Plan, type will be `CopyPlan`. For Publishing, type will be `PublicTarget`|
|Plan.CreationProcessInfo.SourceId|Identity of the operation that created the plan. For Copy Plan, source identifier will be the source plan identifier. For Publishing, source identifier will be the channel id.|
|Plan.CreationSource|Information about the source which created the plan.|
|Plan.CreationSource.ExternalSource|Contains information for a plan created from an external source.|
|Plan.CreationSource.ExternalSource.ContextScenarioId|An identifier for the scenario associated with this external source.|
|Plan.CreationSource.ExternalSource.ExternalContextId|The identifier of the external entity's containing entity or context.|
|Plan.CreationSource.ExternalSource.ExternalObjectId|The identifier of the entity that an external service associates with this plan.|
|Plan.CreationSource.ExternalSource.ExternalObjectVersion|The external item version of the plan.|
|Plan.CreationSource.ExternalSource.OwnerAppId|The identity of the app where the plan was created.|
|Plan.PlanSharedWithContainers|The containers a plan is shared with.|
|Plan.SharedWithContainers.ContainerType|The container type.|
|Plan.SharedWithContainers.ExternalId|The external identifier.|
|Plan.SharedWithContainers.AccessLevel|The maximum access level to the plan that is granted to members of this container.|
|Plan.PlanDetailsId|Unique identifier of the plan details object. |
|Plan.ICalendarPublishEnabled|If True, ICalendar sharing is enabled for the plan. See [your tasks on a calendar](https://support.office.com/article/use-schedule-view-to-see-tasks-on-a-calendar-8647d2c9-9bc7-466a-b5b3-74b3596fade2) for more information.|
|Plan.CreateTaskCommentWhen|Events that will cause a comment to be created for a task in the plan.|
|Plan.ReferencesToPlan|External systems that link to the plan. For example, embedding a Microsoft Planner plan in Project Online Desktop Client.|
|Plan.ReferencesToPlan.ExternalId|External System's ID for this plan.|
|Plan.ReferencesToPlan.AssociationType|The type of link to the plan, specified by the external app.|
|Plan.ReferencesToPlan.CreatedDate|Date and time the reference object was created.|
|Plan.ReferencesToPlan.CustomLinkText|Text that can be used when displaying the Url.|
|Plan.ReferencesToPlan.DisplayAs|Specifies how the reference data like the Url should be presented in a user experience.|
|Plan.ReferencesToPlan.IsCreationContext|Set to `true` if the reference was set when the Plan was created.|
|Plan.ReferencesToPlan.OwnerAppId|ID of the app that created the reference.|
|Plan.ReferencesToPlan.DisplayNameSegments|Breadcrumbs of the location that describes what references this plan.|
|Plan.ReferencesToPlan.Url|Direct link to the app that references the plan.|
|Plan.CategoryDescriptions|The full set of categories for the plan.|
|Plan.CategoryDescriptions.Index|The index of the category description.|
|Plan.CategoryDescriptions.Description|The label text for the corresponding category description index value.|
|Plan.PlanFollowers|If Plan.Container.ContainerType is Group, then this field is the Users who follow the plan. If the Plan.Container.ContainerType is Roster, then this field is the Users who are members of the Roster. |
|Plan.Background|Details about the plan background.|
|Plan.Background.BaseColor|Hex color code of the base color of the background.|
|Plan.Background.OverlayImage|URL of the background image.|
|Plan.TimelineId|The feature has been deprecated.|
|Plan.TimelineDisplaySettings|The feature has been deprecated.|
|Plan.TimelineLockedWidth|The feature has been deprecated.|
|Plan.Tasks|Tasks objects for the plan.|
|Plan.Tasks.Id|Unique identifier of the task.|
|Plan.Tasks.ArchivalInfo|Information about the archival status of the task.|
|Plan.Tasks.ArchivalInfo.IsArchived|Is the task archived.|
|Plan.Tasks.ArchivalInfo.Justification|Reason why the task was archived or unarchived.|
|Plan.Tasks.ArchivalInfo.StatusChangedBy|The user that updated the archival status. See User properties for more detail.|
|Plan.Tasks.ArchivalInfo.StatusChangedByAppId|Identity of the app that updated the archival status.|
|Plan.Tasks.ArchivalInfo.StatusChangedDate|When the archival status was updated.|
|Plan.Tasks.Id  |Unique identifier of the task.|
|Plan.Tasks.Title|Name of the task.|
|Plan.Tasks.BucketId|The Microsoft Planner ID of the bucket the task is in.|
|Plan.Tasks.BucketName|Name of the bucket.|
|Plan.Tasks.PercentComplete|Completion status of the task, from 0 to 100. |
|Plan.Tasks.Priority|The priority, or urgency, of a task.|
|Plan.Tasks.CreationSource|Information about the source which created the task.|
|Plan.Tasks.CreationSource.Publication|Contains publication info for a task whose creation source is publication.|
|Plan.Tasks.CreationSource.Publication.Id|The identity for a publication.|
|Plan.Tasks.CreationSource.Publication.LastModifiedTime|The date at which the publication was most recently updated.|
|Plan.Tasks.CreationSource.Publication.PublishableTaskId|Identity of the publishable task that is the source of this task.|
|Plan.Tasks.CreationSource.Publication.PublishedToPlanId|The identity of the plan the task belonged to at time of creation.|
|Plan.Tasks.CreationSource.Publication.SourceTeamId|The identity of the team which published this task.|
|Plan.Tasks.CreationSource.Publication.SourceTeamName|The name of the team which published this task.|
|Plan.Tasks.CreationSource.Document|Contains document related sync info for a task.|
|Plan.Tasks.CreationSource.DocumentTaskId|Identity of the task in the document.|
|Plan.Tasks.CreationSource.ExternalSource|Contains information for tasks created from an external source.|
|Plan.Tasks.CreationSource.ExternalSource.ExternalObjectId|The identifier of the entity that an external service associates with a task.|
|Plan.Tasks.CreationSource.ExternalSource.ExternalContextId|The identifier of the external entity's containing entity or context.|
|Plan.Tasks.CreationSource.ExternalSource.ContextScenarioId|An identifier for the scenario associated with this external source.|
|Plan.Tasks.CreationSource.ExternalSource.ExternalObjectVersion|The external item version for the task.|
|Plan.Tasks.CreationSource.ExternalSource.WebUrl|A deep link to the external application that created the task.|
|Plan.Tasks.CreationSource.ExternalSource.DisplayAs|How to display the task external task source.|
|Plan.Tasks.CreationSource.ExternalSource.OwnerAppId|The identity of the app where the task was created.|
|Plan.Tasks.CreationSource.ExternalSource.DisplayNameSegments|A list of segments that Planner will use to show the external source to the user.|
|Plan.Tasks.CreationSource.Recurrence|Recurrence information. If `null`, this Task is not recurring.|
|Plan.Tasks.CreationSource.Recurrence.FromSeriesId|Unique identifier for the recurrence series that this task belongs to.|
|Plan.Tasks.StartDate|Date the task is scheduled to start.|
|Plan.Tasks.DueDate|Date the task is scheduled to complete.|
|Plan.Tasks.ConversationThreadId|Conversation unique identifier from Microsoft Exchange.|
|Plan.Tasks.PreviewType|Preview that is displayed on the task card.|
|Plan.Tasks.OrderHint|Used for sorting order. See [Using order hints in Microsoft Planner](/graph/api/resources/planner-order-hint-format).|
|Plan.Tasks.CreatedBy|User that created the task. See User properties for more detail.|
|Plan.Tasks.CreatedByAppId|The app that created the task.|
|Plan.Tasks.CreatedDate|Date the task was created.|
|Plan.Tasks.CompletedBy|User that completed the task. See User properties for more detail.|
|Plan.Tasks.CompletedByAppId|Identity of the app that completed the task.|
|Plan.Tasks.CompletedDate|Date the task was completed.|
|Plan.Tasks.ModifiedBy|User that last updated the task. See User properties for more detail.|
|Plan.Tasks.ModifiedDate|Date the task was last updated.|
|Plan.Tasks.ModifiedByAppId|The identity of the app that last modified the task.|
|Plan.Tasks.UserContentLastModifiedByAppId|The identity of the app that last modified the task or task details properties.|
|Plan.Tasks.AppliedCategories |The labels selected from the CategoryDescriptions index for the plan.|
|Plan.Tasks.Recurrence|Defines active or inactive recurrence for the task. `null` when recurrence has never been defined for the task.|
|Plan.Tasks.Recurrence.SeriesId|The recurrence series this task belongs to. A GUID-based value that serves as the unique identifier for a series.|
|Plan.Tasks.Recurrence.OccurrenceIndex|The 1-based index of this task within the recurrence series. The first task in a series has the value `1`, the next task in the series has the value `2`, and so on.|
|Plan.Tasks.Recurrence.PreviousInSeriesTaskId|The **Task ID** of the previous task in this series. `null` for the first task in a series since it has no predecessor. Each subsequent task in the series has a value corresponding to its predecessor.|
|Plan.Tasks.Recurrence.NextInSeriesTaskId|The **Task ID** of the next task in this series. This value is assigned at the time the next task in the series is created, and is `null` prior to that time.|
|Plan.Tasks.Recurrence.RecurrenceStartDate|The date and time when this recurrence series began. For the first task in a series (**OccurrenceIndex** = `1`) this value corresponds to **Schedule.Range.StartDate**. For subsequent tasks in the series (**OccurrenceIndex** >= `2`) this value is copied from the previous task and never changes; it preserves the start date of the recurring series.|
|Plan.Tasks.Recurrence.Schedule|The schedule for recurrence. `null` indicates that recurrence has been canceled. Note that if **NextInSeriesTaskId** is assigned then this schedule value will be preserved as a snapshot of what the schedule looked like at the time of completion of this task.|
|Plan.Tasks.Recurrence.Schedule.Pattern|The pattern for recurrence. The pattern, along with the **Schedule.Range**, are used to calculate the **Schedule.NextOccurrenceDate**.|
|Plan.Tasks.Recurrence.Schedule.Pattern.IsDailyCadence|`True` for daily cadence (in which case **DaysOrDates** is empty). `False` otherwise (that is, for weekly, monthly, or yearly cadence).|
|Plan.Tasks.Recurrence.Schedule.Pattern.Interval|The interval applied to the cadence kind. Values greater than 1 mean that a period will be skipped. Examples: for a Daily pattern, an Interval of 2 means tasks will recur every two days (or every other day). For a monthly pattern, an Interval of 3 means tasks will recur every three months (also known as quarterly).|
|Plan.Tasks.Recurrence.Schedule.Pattern.DaysOrDates|Each entry in this collection represents the definition of exactly one day or date. Example: `"FixedYearly,August,15"` means on August 15 of the year. `"FloatingMonthly,Second,Monday"` means on the second Monday of the month. `"Weekly,Wednesday","Weekly,Friday"` means weekly on Wednesdays and Fridays.|
|Plan.Tasks.Recurrence.Schedule.Pattern.FirstDayOfWeek|The first day of the week (typically Sunday); this is only used by weekly patterns, and is `null` for nonweekly patterns.|
|Plan.Tasks.Recurrence.Schedule.Range|Specifies when recurrence starts and ends.|
|Plan.Tasks.Recurrence.Schedule.Range.StartDate|The date from which the **Recurrence.Schedule** should begin. This value may be updated by users when making changes to the **Recurrence.Schedule.Pattern**.|
|Plan.Tasks.Recurrence.Schedule.Range.Kind|Currently the only supported value is `NoEnd`, indicating that the series will not end automatically.|
|Plan.Tasks.Recurrence.Schedule.Range.EndDate|Specifies the date of the last occurrence.|
|Plan.Tasks.Recurrence.Schedule.Range.NumberOfOccurrences|Specifies the total number of occurrences.|
|Plan.Tasks.Recurrence.Schedule.NextOccurrenceDate|The next date for this **Recurrence.Schedule**. When a new task is instantiated to continue the recurrence series, this date is used for the **DueDate** of the new Task.|
|Plan.Tasks.TaskDetailsId |Unique identifier of the details object for the task.|
|Plan.Tasks.Description|Description of the task.|
|Plan.Tasks.DescriptionHtml|Description of the task in HTML format.|
|Plan.Tasks.CompletionRequirements|Requirement set for completing a task.|
|Plan.Tasks.CompletionRequirements.Checklist|The checklist requirements that must be met, before the task can be marked as complete.|
|Plan.Tasks.CompletionRequirements.Checklist.ChecklistIds|The checklist IDs that are mandatory to be completed.|
|Plan.Tasks.CompletionRequirements.Forms|The forms requirements that must be met, before the task can be marked as complete.|
|Plan.Tasks.CompletionRequirements.Forms.RequiredForms|Indicates the forms that are mandatory to be completed.|
|Plan.Tasks.CompletionRequirements.Approval|The approval requirements that must be met, before the task can be marked as complete.|
|Plan.Tasks.CompletionRequirements.Approval.IsApprovalRequired|Indicates if approval is required.|
|Plan.Tasks.Approval|The approval attached to a task.|
|Plan.Tasks.Approval.BasicApproval|The basic approval of a task.|
|Plan.Tasks.Approval.BasicApproval.ApprovalId|Identity of the approval.|
|Plan.Tasks.Approval.BasicApproval.Status|The status of the approval.|
|Plan.Tasks.Approval.BasicApproval.CreatedBy|The user that created this approval. See User properties for more detail.|
|Plan.Tasks.Approval.BasicApproval.CreatedByAppId|The app that created this approval.|
|Plan.Tasks.AssignedToTaskBoardFormatId|Unique identifier for the object that is the task board format.|
|Plan.Tasks.AssignedToTaskBoardFormatUnassignedOrderHint|Used for sorting order. See [Using order hints in Microsoft Planner](/graph/api/resources/planner-order-hint-format).|
|Plan.Tasks.AssignedToTaskBoardFormatOrderHintsByAssignee|Order hint for each of the assignees.|
|Plan.Tasks.AssignedToTaskBoardFormatOrderHintsByAssignee.AssignedTo:|The user that is assigned the task. See User properties for more detail.|
|Plan.Tasks.AssignedToTaskBoardFormatOrderHintsByAssignee.Order|Ordering of the tasks specified by assignee in the Assigned To view.|
|Plan.Tasks.BucketTaskBoardFormatId|Unique identifier for the object that is the bucket task board format.|
|Plan.Tasks.BucketTaskBoardFormatOrderHint|Used for sorting order. See [Using order hints in Microsoft Planner](/graph/api/resources/planner-order-hint-format).|
|Plan.Tasks.ProgressTaskBoardFormatId|Unique identifier for the object when grouped by progress instead of bucket format. |
|Plan.Tasks.ProgressTaskBoardFormatOrderHint|Used for sorting order. See [Using order hints in Microsoft Planner](/graph/api/resources/planner-order-hint-format).|
|Plan.Tasks.TimelineFormatId|The feature has been deprecated.|
|Plan.Tasks.TimelineFormatShowOnTimeline|The feature has been deprecated.|
|Plan.Tasks.TimelineFormatAnchorPosition|The feature has been deprecated.|
|Plan.Tasks.TimelineFormatCalloutHeight|The feature has been deprecated.|
|Plan.Tasks.TimelineFormatColor|The feature has been deprecated.|
|Plan.Tasks.TimelineFormatDrawingStyle|The feature has been deprecated.|
|Plan.Tasks.TimelineFormatLabelOffsetX|The feature has been deprecated.|
|Plan.Tasks.TimelineFormatLabelOffsetY|The feature has been deprecated.|
|Plan.Tasks.TimelineFormatSwimlane|The feature has been deprecated.|
|Plan.Tasks.References|External links.|
|Plan.Tasks.References.Url|URL of the link.|
|Plan.Tasks.References.Alias|Text description of the link.|
|Plan.Tasks.References.Type|The type of file that is being linked to.|
|Plan.Tasks.References.ModifiedBy|The user that last updated the link. See User properties for more detail.|
|Plan.Tasks.References.ModifiedByAppId|The identity of the app that has modified this reference most recently.|
|Plan.Tasks.References.ModifiedDate|Date the link was last updated.|
|Plan.Tasks.References.PreviewPriority|Represents the priority of a reference to be shown as a preview on the task in the UI. Microsoft Planner only shows the highest priority item.|
|Plan.Tasks.References.CreatedBy|The user that created this reference. This can be `null` in some cases of if the data existed before this field existed.|
|Plan.Tasks.References.CreatedByAppId|The app that created this reference. This can be `null` in some cases or if the data existed before this field existed.|
|Plan.Tasks.Assignments|Task assignments.|
|Plan.Tasks.Assignments.AssignedTo|The user that task is assigned to. See User properties for more detail.|
|Plan.Tasks.Assignments.AssignedBy|The user that assigned the task. See User properties for more detail.|
|Plan.Tasks.Assignments.AssignedByAppId|The app that assigned the task to the assignee.|
|Plan.Tasks.Assignments.AssignedDate|The date the task was assigned to the assignee.|
|Plan.Tasks.Assignments.Order|Order of the assignments if the task is assigned to multiple entities.
|Plan.Tasks.Checklist|The checklist for the task.|
|Plan.Tasks.Checklist.Id|Unique identifier of a checklist item.|
|Plan.Tasks.Checklist.Title|Name of the checklist item.|
|Plan.Tasks.Checklist.OrderHint|Used for sorting order. See [Using order hints in Microsoft Planner](/graph/api/resources/planner-order-hint-format).|
|Plan.Tasks.Checklist.IsChecked|If true, the checklist item has been completed.|
|Plan.Tasks.Checklist.ModifiedBy|The user that last updated the checklist. See [User properties](#user-properties-in-the-plansjson-file) for more detail.|
|Plan.Tasks.Checklist.ModifiedByAppId|The app that last modified this checklist item.|
|Plan.Tasks.Checklist.ModifiedDate|Date the checklist was last updated.|
|Plan.Tasks.Checklist.CreatedBy|The user that created the checklist item. See User properties for more detail. Could be `null` if created by the system.|
|Plan.Tasks.Checklist.CreatedByAppId|The app that created the checklist item. This can be `null` in some cases or if the data existed before this field existed.|
|Plan.Tasks.Forms|Task forms.|
|Plan.Tasks.Forms.DisplayName|The display name of the form.|
|Plan.Tasks.Forms.FormResponseId|The identifier of the response.|
|Plan.Tasks.Forms.FormId|The reference of the form.|
|Plan.Tasks.Forms.CreatedBy|The user that created the form. See User properties for more detail. Could be `null` if created by the system or app.|
|Plan.Tasks.Forms.CreatedByAppId|The app that created the form.|
|Plan.Tasks.UserContentLastModifiedBy|The user that last updated the task or task details. See [User properties](#user-properties-in-the-plansjson-file) for more detail.|
|Plan.Tasks.UserContentLastModifiedDate|Date the task or task details was last updated.|
|Plan.Buckets  |Bucket objects for the plan.|
|Plan.Buckets.Id|Unique identifier for the bucket.|
|Plan.Buckets.ArchivalInfo|Information about the archival of the bucket.|
|Plan.Buckets.ArchivalInfo.IsArchived|Is the bucket archived.|
|Plan.Buckets.ArchivalInfo.Justification|Reason why the bucket was archived or unarchived.|
|Plan.Buckets.ArchivalInfo.StatusChangedBy|The user that updated the archival status. See User properties for more detail.|
|Plan.Buckets.ArchivalInfo.StatusChangedByAppId|Identity of the app that updated the archival status.|
|Plan.Buckets.ArchivalInfo.StatusChangedDate|When the archival status was updated.|
|Plan.Buckets.Title|Name of the bucket.|
|Plan.Buckets.OrderHint|Used for sorting order. See [Using order hints in Microsoft Planner](/graph/api/resources/planner-order-hint-format).|
|Plan.Buckets.Createdby|The user that created the bucket. See [User properties](#user-properties-in-the-plansjson-file) for more detail.|
|Plan.Buckets.CreatedByAppId|The app that created the bucket.|
|Plan.Buckets.CreatedDate|Date the bucket was created.|
|Plan.Buckets.ModifiedBy|The user that last updated the bucket. See [User properties](#user-properties-in-the-plansjson-file) for more detail.|
|Plan.Buckets.ModifiedDate|Date the bucket was last updated.|
|Plan.Buckets.ModifiedByAppId|Id of the app which last modified the bucket.|
|Plan.Buckets.CreationSource|Information about the source which created the bucket.|
|Plan.Buckets.CreationSource.ExternalSource|Contains information for buckets created from an external source.|
|Plan.Buckets.CreationSource.ExternalSource.ContextScenarioId|An identifier for the scenario associated with this external source.|
|Plan.Buckets.CreationSource.ExternalSource.ExternalContextId|The identifier of the external entity's containing entity or context.|
|Plan.Buckets.CreationSource.ExternalSource.ExternalObjectId|The identifier of the entity that an external service associates with a bucket.|
|Plan.Buckets.CreationSource.ExternalSource.ExternalObjectVersion|The external item version for the bucket.|
|Plan.Buckets.CreationSource.ExternalSource.OwnerAppId|The identity of the app where the bucket was created.|

### User properties in the Plans.json file

There are many objects in Plans.json data that represent a Microsoft Planner user and will have similar properties. These objects include:

- Plan.CreatedBy
- Plan.ModifiedBy
- Plan.PlanFollowers
- Plan.Tasks.CreatedBy
- Plan.Tasks.CompletedBy
- Plan.Tasks.ModifiedBy
- Plan.Tasks.AssignedToTaskBoardFormatOrderHintsByAssignee.AssignedTo
- Plan.Tasks.References.ModifiedBy
- Plan.Tasks.Assignments.AssignedTo
- Plan.Tasks.Assignments.AssignedBy
- Plan.Tasks.Checklists.ModifiedBy
- Plan.Bucket.Createdby
- Plan.Bucket.Modifiedby

Each of the above will have the following properties:

|Property|Description|
|---|---|
|ID|Microsoft Planner ID of the user.|
|ExternalId|Microsoft Entra ID of the user.|
|DisplayName|Display name of the user.|
|UserPrincipalName|User Principal Name (UPN) of the user.  |
|PrincipalType|The entity type (User or Group).|

## Related articles

[Delete user data in Microsoft Planner](delete-user-data.md)
