DST Server Behaviour Guidelines
================

- [Overview](#top)
- [Connection Management](#connection-management)
  - [Two Ways to Exit the Servers](#two-ways-to-exit-the-servers)
    - [1. Sign Out](#1-sign-out)
    - [2. Disconnect](#2-disconnect)
  - [Memory & Resource Considerations](#memory--resource-considerations)
  - [Valid Use of Disconnect](#valid-use-of-disconnect)
- [Home Folder & File Organization](#home-folder--file-organization)
  - [Your Personal Workspace](#your-personal-workspace)
  - [Folder Structure Best Practices](#folder-structure-best-practices)
- [Resource Management](#resource-management)
  - [Monitoring System Load](#monitoring-system-load)
  - [Resource Limits](#resource-limits)
  - [Memory Deallocation](#memory-deallocation)
- [Working with R & RStudio](#working-with-r--rstudio)
  - [Project Initialization](#project-initialization)
  - [RStudio Configuration](#rstudio-configuration)
    - [Workspace Settings](#workspace-settings)
    - [History Settings](#history-settings)
  - [Important: Weekly RStudio Resets](#important-weekly-rstudio-resets)
  - [Why These Settings Matter](#why-these-settings-matter)
  - [Working Directory & Paths](#working-directory--paths)
    - [Within RStudio Projects](#within-rstudio-projects)
    - [Outside RStudio Projects](#outside-rstudio-projects)
- [Parallel Analyses](#parallel-analyses)
  - [Running Analyses in Parallel](#running-analyses-in-parallel)
- [Flow & Process Diagram](#flow--process-diagram)

<!-- The .md file is generated from an .Rmd file. Please edit the .Rmd; not the .md !! -->

------------------------------------------------------------------------

> - **Version**: DRAFT document
> - **Authors**: Rune Haubo B. Christensen and Clara S. Grønkjær
> - **Reviewers**: Lars N. Reiter, Andreas M. Appel, and Eva N. S.
>   Wandall <!-- > - **Approved by**: Michael E. Benros (version) -->
> - **Last edits**: 2026-02-27
> - **Responsible**: The data management team (defined below)
> - **Source document**:
>   [DST_server_behaviour_guidelines.Rmd](/server_export/DST_server_behaviour_guidelines.Rmd)

------------------------------------------------------------------------

# Overview

This section covers best practices and required behaviors when working
on the DST servers to ensure system stability, prevent data loss, and
respect shared resources.

[Back to top](#top)

# Connection Management

## Two Ways to Exit the Servers

### 1. Sign Out

- **Effect**: User interface is reset (like shutting down a computer)
- **On next login**: Programs are closed; you must reopen them
- **Use case**: Preferred for normal work sessions

### 2. Disconnect

- **Effect**: Like putting your Windows computer to sleep (DK: *Slumre*)
- **On next login**: Programs remain running; continue work immediately
- **⚠️ Warning**: High-risk operation; use sparingly (see point 9 below)

## Memory & Resource Considerations

**Critical**: Disconnecting while data is loaded in memory is one of the
biggest mistakes users make.

- You unnecessarily block memory that other users cannot access
- You prevent other researchers from running their analyses
- **Before disconnecting**: Ensure you have released all memory (verify
  in task manager)

In **RStudio**: Restart the R process to release memory - Menu:
`Session` → `Restart R` - Keyboard shortcut: `Ctrl+Shift+F10`

## Valid Use of Disconnect

Use `Disconnect` **only** when you have a long-running job: - Large
statistical analyses on big data - Bootstrap simulations - Other
computationally intensive tasks

**Before initiating the job**: - Load only relevant data - Close or
restart non-relevant R processes - Verify memory is not unnecessarily
occupied

[Back to top](#top)

# Home Folder & File Organization

## Your Personal Workspace

1.  **Update README**: Ensure you have an up-to-date entry in
    `Workdata/708237/README.txt`

2.  **Create your home folder**: `Workdata/708237/INIT` where `INIT` =
    your unique server-wide initials

3.  **Work exclusively in your home folder**:

    - Only execute programs in your home folder
    - Only edit files in your home folder
    - Do not modify anything outside your home folder

4.  **Why this matters**:

    - All users technically have unrestricted read/write access to all
      folders
    - Accidental changes to other users’ files can occur easily
    - RStudio creates `.Rhistory` files silently in any directory where
      you run commands

5.  **If you need to review another user’s files**:

    - Copy the file(s) or folder to your home folder
    - Work with your local copy
    - If you accidentally modify something, only your copy is affected

## Folder Structure Best Practices

Use a folder structure similar to existing users (e.g., `RHBC`,
`CSG`): - Makes navigation easier for all team members - Facilitates
sharing and learning across projects - Enables easier mutual support

**Example structure**:

    Workdata/708237/YOUR_INIT/
    ├── 001_functions/
    ├── 002_data/
    ├── 003_analyses/
    ├── 004_results/
    └── README.md

[Back to top](#top)

# Resource Management

## Monitoring System Load

**Always** have a task manager window open showing the User pane:

1.  Check server load (memory and CPU usage)
2.  Ensure you’re not consuming excessive resources

## Resource Limits

**Rule of thumb**: - **Memory**: Never exceed \[PLACEHOLDER: insert
specific limit, e.g., “X GB”\]. Use less memory if the server load is
high - **CPU**: Never exceed 10%. For parallel processing, use a maximum
of \[PLACEHOLDER: insert specific limit, e.g., 3 nodes\]

**Consequences of exceeding limits**: - All users experience program
freezes - Running jobs terminate with memory allocation errors - Entire
server performance degrades

## Memory Deallocation

**Important**: Do not rely on `gc()` (garbage collection) to free memory
manually.

- `gc()` is meant to release memory to the system that R no longer uses
  for objects
- R continuously runs `gc()` automatically
- Explicitly calling `gc()` rarely provides significant speed
  improvements
- **Better approach**: Restart your R session when done with
  memory-intensive operations

[Back to top](#top)

# Working with R & RStudio

## Project Initialization

This is the **first step** when starting a new project:

1.  Create a folder for your project
2.  Initiate an **RStudio project** (generates `<project_name>.Rproj`
    file)
3.  In the future: Double-click the `.Rproj` file to open RStudio in the
    correct folder

**Benefits**: - Automatic working directory management - Reproducible
analysis structure - Easy to share and navigate

## RStudio Configuration

Before working, change these settings via `Tools` → `Global Options`:

### Workspace Settings

- [ ] **Uncheck**: `Restore .RData into workspace at startup`
- [ ] **Set**: `Save workspace to .RData on exit` → **Never**

### History Settings

- [ ] **Uncheck**: `Always save history (even when not saving .RData)`

## Important: Weekly RStudio Resets

The DST servers reboot **every night between Sunday and Monday**,
clearing all RStudio settings.

- **First login each week**: You must reconfigure these settings again
- **Plan accordingly**: Do this before starting your analyses

## Why These Settings Matter

**`.RData` files are dangerous**: - Can be extremely large (consuming
expensive storage) - Lead to unreproducible analyses - One of the most
common ways to shoot yourself in the foot - Make it impossible to
identify what produced your results

**`.Rhistory` files**: - Unnecessarily populate the system - Can reveal
unintended commands if you’ve worked in shared folders

## Working Directory & Paths

### Within RStudio Projects

- **Use relative paths** (e.g., `data/myfile.csv`)
- RStudio projects handle the working directory automatically
- **Never use `setwd()`** – it’s unnecessary and error-prone

### Outside RStudio Projects

- **Use absolute paths** (e.g., `/full/path/to/raw/data`)
- Ensures you’re always pointing to the correct data

[Back to top](#top)

# Parallel Analyses

## Running Analyses in Parallel

\[PLACEHOLDER: This section requires detailed guidance on: - How to set
up parallel processing on DST servers - Best practices for parallel R
computations - Memory and CPU considerations when parallelizing - Common
tools and packages (e.g., `foreach`, `doParallel`, `future`) - Resource
allocation guidelines for parallel jobs - When parallelization is worth
the overhead\]

**TODO**: Obtain detailed guidelines from Lars and technical team

[Back to top](#top)

# Flow & Process Diagram

\[PLACEHOLDER: Add a flowchart/diagram showing: - Decision tree for
export requests - Timeline from submission to completion - Server
connection decision tree (sign out vs. disconnect) - Resource management
workflow\]

**TODO**: Obtain diagram from Lars
