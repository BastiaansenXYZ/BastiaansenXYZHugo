# Release notes for release $(Build.DefinitionName)
**Release Number**  : $($release.name)
**Release completed** $("{0:dd/MM/yy HH:mm:ss}" -f [datetime]$release.modifiedOn)

**Changes since last successful release to '$stagename'**
**Including releases:**    $(($releases | select-object -ExpandProperty name) -join ", " )

## Builds
@@BUILDLOOP@@
###$($build.definition.name)
# Release notes for build $defname
**Build Number**  : $($build.buildnumber)
**Build started** : $("{0:dd/MM/yy HH:mm:ss}" -f [datetime]$build.startTime)
**Source Branch** : $($build.sourceBranch)
###Associated work items
@@WILOOP@@
* **$($widetail.fields.'System.WorkItemType') $($widetail.id)** [Assigned by: $($widetail.fields.'System.AssignedTo')]     $($widetail.fields.'System.Title')
@@WILOOP@@
### Associated change sets/commits
@@CSLOOP@@
* **ID $($csdetail.changesetid)$($csdetail.commitid)** $($csdetail.comment)
@@CSLOOP@@

----------

@@BUILDLOOP@@