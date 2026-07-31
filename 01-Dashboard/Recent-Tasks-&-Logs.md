# Recent Tasks & Logs

---

## Overview

The **Recent Tasks** section allows administrators to monitor operations performed within the VM2Cloud environment. Every administrative action, such as creating a virtual machine, uploading an ISO image, or modifying storage, generates a task.

The task log helps administrators monitor the progress of operations, verify whether they completed successfully, and identify any errors that occurred during execution.

---

## When to Use

Use the **Recent Tasks** section to:

* Monitor running tasks.
* Verify completed operations.
* Review failed tasks.
* View task details.
* Troubleshoot administrative operations.

---

## Prerequisites

Before viewing Recent Tasks, ensure that:

* You have logged in to the VM2Cloud web interface.
* You have sufficient permissions to view task information.

---

# View Recent Tasks

## Step 1: Open the Dashboard

1. Log in to the VM2Cloud web interface.
2. Open the Dashboard.

---

### Screenshot 1

```text id="rt01"
[ Place Screenshot Here ]
```

---

## Step 2: Locate the Recent Tasks Panel

The **Recent Tasks** panel is displayed at the bottom of the Dashboard.

It lists the most recent administrative operations performed within the environment.

---

### Screenshot 2

```text id="rt02"
[ Place Screenshot Here ]
```

---

## Step 3: Review Task Information

Each task entry displays information such as:

* Start Time
* End Time
* User
* Node
* Task Description
* Status

Use this information to monitor current and completed operations.

---

### Screenshot 3

```text id="rt03"
[ Place Screenshot Here ]
```

---

## Step 4: View Task Details

1. Select the required task.
2. Review the detailed task information.

Depending on the operation, the details may include:

* Task identifier
* Execution time
* Progress
* Result
* Error messages (if any)

---

### Screenshot 4

```text id="rt04"
[ Place Screenshot Here ]
```

---

## Task Status

The task status indicates the result of an operation.

Common task statuses include:

| Status  | Description                                                         |
| ------- | ------------------------------------------------------------------- |
| Running | The operation is currently in progress.                             |
| OK      | The operation completed successfully.                               |
| Error   | The operation failed. Review the task details for more information. |
| Warning | The operation completed with warnings that may require attention.   |

---

### Screenshot 5

```text id="rt05"
[ Place Screenshot Here ]
```

---

## View Task History

Recent Tasks displays the latest operations performed in the environment.

For older operations, open the **Task History** page from the appropriate resource (such as Datacenter or a Node) to review historical task records.

---

### Screenshot 6

```text id="rt06"
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The Recent Tasks panel is visible.
* New administrative operations appear automatically.
* Task status is displayed correctly.
* Task details can be opened.
* Completed tasks show the expected result.

---

# Common Issues

| Issue                             | Resolution                                                                                                                    |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| No tasks are displayed            | If no administrative operations have been performed recently, the Recent Tasks panel may be empty.                            |
| Task remains in the Running state | Wait for the operation to complete. If it remains active for an extended period, review the related resource and system logs. |
| Task failed                       | Open the task details to review the error message and identify the cause.                                                     |
| Unable to view task details       | Verify that your user account has permission to access task information.                                                      |
| Recent task list is not updating  | Refresh the Dashboard and confirm that the VM2Cloud management services are operational.                                      |

---

# Summary

The Recent Tasks section provides real-time visibility into administrative operations performed in VM2Cloud. By monitoring task status and reviewing task details, administrators can verify successful operations, identify failures, and troubleshoot issues more efficiently.
