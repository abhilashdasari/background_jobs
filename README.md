# How to move the running jobs into background or add nohup to running jobs

## Introduction

This guide demonstrates how to add `nohup` to a long-running job in Linux, allowing it to continue executing even after you log out. We'll cover two methods: a direct approach and an alternative using the `reptyr` tool.

## Method 1: Direct Approach

### Step 1: Find the Process ID (PID)

First, locate the PID of your running job:

```
ps aux | grep "your_command_name"
```

### Step 2: Disown the Job

Prevent the job from receiving a SIGHUP signal when you log out:

```
disown -h PID
```

Replace `PID` with the actual process ID found in Step 1.

### Step 3: Redirect Output and Apply nohup

Attach `nohup` to the running process and redirect its output:

```
nohup PID > output.log 2>&1 &
```

This command:
- Attaches `nohup` to the process
- Redirects stdout to `output.log`
- Redirects stderr to stdout (2>&1)
- Puts the process in the background (&)

### Step 4: Verify

Confirm that the job is now running with `nohup`:

```
ps -ef | grep PID
```

You should see `nohup` in the command column.

## Method 2: Using reptyr

If the direct method doesn't work, try using the `reptyr` tool.

### Step 1: Install reptyr

For Debian/Ubuntu systems:

```
sudo apt-get install reptyr
```

### Step 2: Suspend the Job

Use `Ctrl+Z` to suspend your running job.

### Step 3: Background the Job

Move the suspended job to the background:

```
bg
```

### Step 4: Disown the Job

Remove the job from the shell's job control:

```
disown %1
```

### Step 5: Reattach with reptyr

Use `reptyr` to reattach to the process:

```
reptyr PID
```

Replace `PID` with your job's process ID.

### Step 6: Suspend Again

Once reattached, press `Ctrl+Z` to suspend the job again.

### Step 7: Apply nohup

Finally, use `nohup` to run the job in the background:

```
nohup bg
```
