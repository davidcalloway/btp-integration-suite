<!-- loio80f3050fb26e42a6b09dfcdb06f8cd50 -->

# Diagnostics

Run diagnostic tasks and collect the necessary information for troubleshooting your Edge Integration Cell.



<a name="loio80f3050fb26e42a6b09dfcdb06f8cd50__section_im3_4gp_hcc"/>

## Prerequisites

A file system is required in order to use *Diagnostics*. This is because the information collected by the diagnostic tasks is stored in the file system. If you didn't enable the file system during the installation of your Edge Integration Cell , you need to reinstall it and choose the *Enable Shared File System*option. For more information, see [Deploy the Edge Integration Cell Solution](deploy-the-edge-integration-cell-solution-ab81b84.md).



<a name="loio80f3050fb26e42a6b09dfcdb06f8cd50__section_nh4_hhp_hcc"/>

## Context

You can access *Diagnostics* through the *Quick Links* card on the Operations Cockpit.

When you experience a problem with your system, SAP Support requires diagnostic data to troubleshoot your issue. You can collect this information by running a diagnostic task.

*Diagnostics* allows you to create new diagnostic tasks and delete previous ones.

> ### Note:  
> You can only run one diagnostic task at a time. Make sure to stop any currently running tasks before starting a new one.

The main table shows a list of previous diagnostic tasks. You can filter the list to quickly locate specific tasks. Use *Adapt Filters* to choose which filters are displayed.

The following information is available for each diagnostic task:


<table>
<tr>
<th valign="top">

Parameter

</th>
<th valign="top">

Information

</th>
</tr>
<tr>
<td valign="top">

*Start Time*

</td>
<td valign="top">

The date when the diagnostics session was initiated.

</td>
</tr>
<tr>
<td valign="top">

*Type*

</td>
<td valign="top">

The type of diagnostic task. It can be any of the following:

-   *Custom Logging*. Temporarily increase the log level for a list of log locations and collect the required debug information for diagnosing your issue.

    To learn more about custom loggers, see [Custom Loggers](custom-loggers-7e52a49.md).

-   *Thread Dump*. Use thread dumps to diagnose unequal resource usage that may cause system slowness. You can configure the number of thread dumps to create for a component and specify the intervals between them.
-   *Heap Dump*. Use heap dumps to diagnose issues related to memory consumption.

    > ### Note:  
    > You can only create a heap dump for one pod per component at a time, as this operation is resource-intensive.

-   *Message Processing Log \(MPL\)*. Diagnose problems related to storage and access to message processing logs. Select one of the following scopes according to your issue:

    -   *Create and collect Message Processing Log*. Select if you experience issues with collecting MPL data from workers, aggregation, and storing in the database.
    -   *Access Message Processing Log*. Select if you experience issues with retrieving MPL from the database.
    -   *Database Logging*. Select if you require extra information on database operations.

    > ### Note:  
    > If you're unsure about the correct scope of your issue, you can choose all of them.

-   *Garbage Collection Logging*. Diagnose memory management issues for specific pods. Unlike other logging tasks, when you create a new *Garbage Collection Logging* task, it gathers all the logs that are already on the file system. Once it's done, the task stops by itself, and you can download the results.



</td>
</tr>
<tr>
<td valign="top">

*Name*

</td>
<td valign="top">

The name of the diagnostic task.

</td>
</tr>
<tr>
<td valign="top">

*Status*

</td>
<td valign="top">

The status of the diagnostic session. It can be either *Completed* or *Running*.

</td>
</tr>
<tr>
<td valign="top">

*Started By* 

</td>
<td valign="top">

The user who ran the diagnostic task.

</td>
</tr>
<tr>
<td valign="top">

*Actions*

</td>
<td valign="top">

Depending on the status of the diagnostic task, you can either *Download* the diagnostic results or *Stop* running the diagnostic session.

</td>
</tr>
</table>

> ### Note:  
> In some *Heap Dump* tasks, the *Name* and *Started By* columns indicate that they are automatically generated. This is because the Worker component is configured to automatically create a heap dump when an `OutOfMemory` situation occurs and the Java VM crashes. These heap dumps are then automatically written into the shared file system.



## Procedure

To create a new diagnostic task, perform the following steps:

1.  Choose *Create Diagnostic Task* and select the required diagnostic type from the dropdown list.
2.  Add the relevant information that SAP Support provided for your diagnostic task and choose *Run*. A new diagnostic session starts in the Edge Integration Cell, and the new task is displayed in the *Diagnostics* table with the status *Running*.
3.  While the diagnostic session is running, reproduce the problem you want to diagnose in parallel. This action writes debugging information to the log files on the file system, allowing *Diagnostics* to collect the relevant data for troubleshooting.
4.  When you're done, choose *Stop* to finish the session. The status changes to *Completed*, and the download button becomes available.

    If you don't stop the session manually, it stops automatically after 10 minutes.

5.  You can now download the results of your diagnostic task. The results save as a zip file on your computer.

    > ### Note:  
    > In the case of heap dumps, you can't download the results directly from the UI due to the large file size. When you choose *Download*, a popup appears with the path to the file's location on the file share. You can download the results file using your preferred Kubernetes tooling. The corresponding command is the following: `kubectl -n edge-icell cp <pod name>:<path> <destination path name>`


> ### Tip:  
> Delete old diagnostic tasks regularly to save space on the file system. You can select one or multiple items from the*Diagnostic Task* list and choose the *Delete* icon.
