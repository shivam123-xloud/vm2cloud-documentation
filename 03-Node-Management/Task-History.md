# Task History

---

## Overview

The **Task History** page allows administrators to view tasks and operations performed on the selected VM2Cloud node.

It provides a historical record of administrative operations, including completed and failed tasks. Administrators can use the task history to verify operations, investigate failures, and review activities performed on the node.

---

## When to Use

Use **Task History** to:

- Review completed operations.
- Investigate failed tasks.
- Check when an operation was performed.
- Identify the user who performed an operation.
- Review task output and errors.
- Troubleshoot node-related operations.

---

## Prerequisites

Before viewing Task History, ensure that:

- You are logged in to the VM2Cloud web interface.
- You have permission to view task information.
- The selected node is online or its historical information is available.

---

# View Task History

## Step 1: Open Task History

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Open **Task History**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review Tasks

The Task History page displays operations associated with the selected node.

Depending on the VM2Cloud version, task information may include:

- Start Time
- End Time
- User
- Node
- Task Type
- Status
- Task ID

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# View Task Details

## Step 1: Select a Task

1. Select the required task from the task list.
2. Open the task details or task log.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review the Task Output

Review the task output to determine:

- Whether the operation completed successfully.
- What actions were performed.
- Whether warnings were generated.
- Whether an error occurred.

For failed operations, use the displayed error information to identify the cause.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Task Status

Common task results include:

| Status | Description |
|---|---|
| OK | The operation completed successfully. |
| Error | The operation failed. Review the task output for details. |
| Running | The operation is still in progress. |

---

# Troubleshoot a Failed Task

When a task reports an error:

1. Open the failed task.
2. Review the complete task output.
3. Identify the reported error.
4. Check the configuration of the affected resource.
5. Correct the underlying problem.
6. Retry the operation if appropriate.

Do not repeatedly retry a failed operation without first determining the cause.

---

# Verification

Verify the following:

- Task History opens successfully.
- Tasks for the selected node are displayed.
- Task details can be opened.
- Task output is available.
- Successful and failed operations can be distinguished.

---

# Common Issues

| Issue | Resolution |
|---|---|
| No tasks are displayed | Verify that tasks have been performed on the selected node and that your account has permission to view them. |
| Task failed | Open the task output and review the reported error. |
| Task is still running | Wait for the operation to complete before taking further action. |
| Unable to view task details | Verify your permissions and refresh the interface. |
| Task information appears outdated | Refresh the Task History page. |

---

# Difference from Recent Tasks

**Recent Tasks** and **Task History** serve different purposes.

| Feature | Recent Tasks | Task History |
|---|---|---|
| Location | Dashboard | Node |
| Purpose | Quickly view recent operations | Review historical node operations |
| Scope | Recent environment activity | Selected node |
| Troubleshooting | Basic | Detailed task output |

---

# Best Practices

- Review failed tasks before retrying operations.
- Use task output when troubleshooting.
- Record important task IDs when reporting issues.
- Compare task timestamps with system logs when investigating problems.

---

# Related Documentation

- Recent Tasks
- Node Troubleshooting
- System Troubleshooting
- Node Overview

---

# Summary

The **Task History** page provides a historical view of operations performed on a VM2Cloud node. It allows administrators to review task status and output, verify completed operations, and investigate failed operations.
