DST Server Guidelines
=====================

<!-- Don't edit the md-file; edit the Rmd-file and knit-to-md! -->

> **Status**: DRAFT document - Last updated: 2026-02-22

## Table of Contents
1. [Part A: Server Data Export Guidelines](#part-a-server-data-export-guidelines)
2. [Part B: DST Server Behavior Guidelines](#part-b-dst-server-behavior-guidelines)

---

# Part A: Server Data Export Guidelines

## Overview

This section describes how to export analysis results from the DST servers. The guidelines are primarily for researchers working through the lab's access to the DST registers who need to send analysis results from the servers to advance their research projects but do not have direct export rights.

## Contact & Process Management

### Who Handles Exports

All server exports (DK: *hjemsendelser*) are handled by:
- Clara Grønkjær
- Rune Haubo Christensen

### How to Request an Export

**Important**: Always email the data management team, not individual researchers.

- **Email address**: `pck-cribp-datamanagement.region-hovedstadens-psykiatri@regionh.dk`
- **Why**: All relevant people have access to that mailbox, and all correspondences are saved centrally, ensuring continuity regardless of team composition changes

### Export Office Hours

We consider server exports on a scheduled basis to avoid continuous on-call demands.

- **Standard schedule**: [PLACEHOLDER: Insert specific days and times, e.g., "Thursdays at 2 PM"]
- **Special requests**: Contact us to arrange exports outside standard hours

## Export Request Submission Requirements

### File Organization

Your export request must include the full path to a folder containing all files to be exported.

**Path structure**: `Workdata/70XXXX/INIT/hjemsendelser/YYYY-MM-DD`

Where:
- `XXXX` = Project number (typically `8237`)
- `INIT` = Your unique server-wide initials
- `YYYY-MM-DD` = A recent date (typically today's date)

**Important**: 
- Collect all files in a single folder
- Create a new folder with a new (non-future) date for each export request
- **Always use manual copy-paste** (Ctrl+C; Ctrl+V) to move files into `hjemsendelser/YYYY-MM-DD` folders
- Never use automated copy programs for this step

### Email Requirements

Your export request email **must explicitly include**:

1. The full path to your export folder
2. A confirmation statement: *"I have checked all files for microdata and other violations and guarantee that these results can be validly exported."*
3. A description of what the export contains and why it's needed

## Data Safety & Compliance Requirements

### Core Principles

**It is your responsibility** (not ours) as the requester to:
- Demonstrate that the export conforms to this guideline
- Ensure compliance with DST and Sundhedsdatastyrelsen requirements
- Minimize manual review needs (which causes delays)

### Regulatory Compliance

All exports must conform to **both** DST and Sundhedsdatastyrelsen requirements:

| Requirement | DST | Sundhedsdatastyrelsen | Our Policy |
|---|---|---|---|
| Minimum individuals per result | 3 | 5 | **5** |

**Key links** (PLACEHOLDER - insert actual URLs):
- [DST Guidelines](https://www.dst.dk/da/TilSalg/data-til-forskning/regler-og-datasikkerhed/regler-for-hjemtagelse-af-analyseresultater)
- [Sundhedsdatastyrelsen Guidelines](https://sundhedsdatastyrelsen.dk/data-og-registre/forskerservice/forskermaskinen/hjemsendelse-af-analyseresultater)
- [NCRR Guidelines](link-here)

### Microdata Definition & Prevention

**Microdata** = Individual-level data that could identify a person. This is strictly prohibited.

**Common false negatives from check functions**:
- Exposure variable with numeric levels (`0`, `1`, etc.) - these are NOT microdata but will trigger flags
- Solution: Rename levels to letters (`a`, `b`) or descriptive names (`level0`, `level1`)

### File Format Requirements

Exports should contain **only**:
- Rectangular tables/datasets in **Excel** format (`*.xlsx` or `*.xls`)
- **CSV** files (`*.csv`)

**Exception**: In rare cases only, with explicit justification.

**Plots and graphics**:
- Export underlying data tables instead (allow recipients to create graphics locally)
- If plots must be exported: use PDF format (vector graphics, not pixel graphics)
- Avoid exporting multiple versions of the same plot (encourages local post-processing instead)

### Using Check Functions

**Before submitting your export request**, validate your files:

1. **Check function location**: `Workdata/708237/RHBC/001_functions/check_functions.R`
2. **Run the R check functions** on your export tables
3. **Ensure no suspicious counts** are flagged
4. **Eliminate false negatives** (e.g., numeric exposure levels) before submission

We apply the same functions during review. Suspicious results will require modifications and delay the export.

## Special Cases & Best Practices

### Exporting Plots & Graphics

**Problem**: You want to export a plot created on the register.

**Solution**:
1. Export the underlying data (non-microdata) in `.csv` or `.xlsx` format
2. Create graphics outside the DST server (on your local computer)
3. Benefits:
   - Easy to verify that exported data are non-microdata
   - Transparent about what data leave the server
   - Easy to update graphics later without re-exporting

### Small Sample Sizes

- **Official requirement**: Minimum 5 individuals per result
- **Practical consideration**: Results based on <10-20 individuals are rarely meaningful
- **Recommendation**: Use statistical modelling (e.g., smoothing splines) to reduce sensitivity to small sample sizes

### Survival Curves & Non-Parametric Curves

- **Requirement**: Smooth curves before export (e.g., using smoothing splines)
- **Reason**: Each "jump" in an un-smoothed curve may represent a single individual's event
- **Applies to**: Kaplan-Meier curves, ROC curves, calibration curves, any step function

**Best practice**: Export the smoothed data table used to create the curve, not the curve itself.

### Table Data Format

- Export numbers in spreadsheets where they can be checked for microdata violations
- **Avoid**: PDFs (harder to check and verify)
- **Column naming convention**: Use `N` for columns containing individual counts

### Tables vs. Figures

Prioritize exporting tables over figures because:
1. Tables are easier to check for microdata
2. Recipients can regenerate figures with updated formatting without requiring re-export
3. Reduces unnecessary file duplication

## Checklist for Export Requests

Before sending your export request email, verify:

- [ ] All files are in `Workdata/70XXXX/INIT/hjemsendelser/YYYY-MM-DD` folder
- [ ] You have run the R check functions on all tables
- [ ] No false negatives from check functions (or you've corrected them)
- [ ] All results are based on ≥5 individuals
- [ ] Files are in approved formats (`.xlsx`, `.xls`, or `.csv`)
- [ ] Any curves are smoothed (no visible jumps representing individuals)
- [ ] Email contains the full folder path
- [ ] Email contains the compliance statement
- [ ] The recipient of the email is the datamanagement-email and not Clara or Rune

## Common Rejection Reasons & Solutions

| Issue | Reason | Solution |
|---|---|---|
| Small cell counts | Microdata risk | Aggregate data or exclude cells with <5 individuals |
| Unsmoothed curves | Individual identification risk | Apply smoothing splines |
| Multiple plot versions | Unclear what's exported | Export underlying data table instead |
| Numeric exposure levels | False positive flags | Rename to letters or descriptive names |
| PDF with data | Difficult to verify | Export as `.csv` or `.xlsx` |

---

# Part B: DST Server Behavior Guidelines

## Overview

This section covers best practices and required behaviors when working on the DST servers to ensure system stability, prevent data loss, and respect shared resources.

## Connection Management

### Two Ways to Exit the Servers

#### 1. Sign Out
- **Effect**: User interface is reset (like shutting down a computer)
- **On next login**: Programs are closed; you must reopen them
- **Use case**: Preferred for normal work sessions

#### 2. Disconnect
- **Effect**: Like putting your Windows computer to sleep (DK: *Slumre*)
- **On next login**: Programs remain running; continue work immediately
- **⚠️ Warning**: High-risk operation; use sparingly (see point 9 below)

### Memory & Resource Considerations

**Critical**: Disconnecting while data is loaded in memory is one of the biggest mistakes users make.

- You unnecessarily block memory that other users cannot access
- You prevent other researchers from running their analyses
- **Before disconnecting**: Ensure you have released all memory (verify in task manager)

In **RStudio**: Restart the R process to release memory
- Menu: `Session` → `Restart R`
- Keyboard shortcut: `Ctrl+Shift+F10`

### Valid Use of Disconnect

Use `Disconnect` **only** when you have a long-running job:
- Large statistical analyses on big data
- Bootstrap simulations
- Other computationally intensive tasks

**Before initiating the job**:
- Load only relevant data
- Close or restart non-relevant R processes
- Verify memory is not unnecessarily occupied

## Home Folder & File Organization

### Your Personal Workspace

1. **Update README**: Ensure you have an up-to-date entry in `Workdata/708237/README.txt`

2. **Create your home folder**: `Workdata/708237/INIT` where `INIT` = your unique server-wide initials

3. **Work exclusively in your home folder**:
   - Only execute programs in your home folder
   - Only edit files in your home folder
   - Do not modify anything outside your home folder

4. **Why this matters**: 
   - All users technically have unrestricted read/write access to all folders
   - Accidental changes to other users' files can occur easily
   - RStudio creates `.Rhistory` files silently in any directory where you run commands

5. **If you need to review another user's files**:
   - Copy the file(s) or folder to your home folder
   - Work with your local copy
   - If you accidentally modify something, only your copy is affected

### Folder Structure Best Practices

Use a folder structure similar to existing users (e.g., `RHBC`, `CSG`):
- Makes navigation easier for all team members
- Facilitates sharing and learning across projects
- Enables easier mutual support

**Example structure**:
```
Workdata/708237/YOUR_INIT/
├── 001_functions/
├── 002_data/
├── 003_analyses/
├── 004_results/
└── README.md
```

## Resource Management

### Monitoring System Load

**Always** have a task manager window open showing the User pane:

1. Check server load (memory and CPU usage)
2. Ensure you're not consuming excessive resources

### Resource Limits

**Rule of thumb**:
- **Memory**: Never exceed [PLACEHOLDER: insert specific limit, e.g., "X GB"]. Use less memory if the server load is high
- **CPU**: Never exceed 10%. For parallel processing, use a maximum of [PLACEHOLDER: insert specific limit, e.g., 3 nodes]

**Consequences of exceeding limits**:
- All users experience program freezes
- Running jobs terminate with memory allocation errors
- Entire server performance degrades

### Memory Deallocation

**Important**: Do not rely on `gc()` (garbage collection) to free memory manually.

- `gc()` is meant to release memory to the system that R no longer uses for objects
- R continuously runs `gc()` automatically
- Explicitly calling `gc()` rarely provides significant speed improvements
- **Better approach**: Restart your R session when done with memory-intensive operations

## Working with R & RStudio

### Project Initialization

This is the **first step** when starting a new project:

1. Create a folder for your project
2. Initiate an **RStudio project** (generates `<project_name>.Rproj` file)
3. In the future: Double-click the `.Rproj` file to open RStudio in the correct folder

**Benefits**:
- Automatic working directory management
- Reproducible analysis structure
- Easy to share and navigate

### RStudio Configuration

Before working, change these settings via `Tools` → `Global Options`:

#### Workspace Settings
- [ ] **Uncheck**: `Restore .RData into workspace at startup`
- [ ] **Set**: `Save workspace to .RData on exit` → **Never**

#### History Settings
- [ ] **Uncheck**: `Always save history (even when not saving .RData)`

### Important: Weekly RStudio Resets

The DST servers reboot **every night between Sunday and Monday**, clearing all RStudio settings.

- **First login each week**: You must reconfigure these settings again
- **Plan accordingly**: Do this before starting your analyses

### Why These Settings Matter

**`.RData` files are dangerous**:
- Can be extremely large (consuming expensive storage)
- Lead to unreproducible analyses
- One of the most common ways to shoot yourself in the foot
- Make it impossible to identify what produced your results

**`.Rhistory` files**:
- Unnecessarily populate the system
- Can reveal unintended commands if you've worked in shared folders

### Working Directory & Paths

#### Within RStudio Projects
- **Use relative paths** (e.g., `data/myfile.csv`)
- RStudio projects handle the working directory automatically
- **Never use `setwd()`** – it's unnecessary and error-prone

#### Outside RStudio Projects
- **Use absolute paths** (e.g., `/full/path/to/raw/data`)
- Ensures you're always pointing to the correct data

## Parallel Analyses

### Running Analyses in Parallel

[PLACEHOLDER: This section requires detailed guidance on:
- How to set up parallel processing on DST servers
- Best practices for parallel R computations
- Memory and CPU considerations when parallelizing
- Common tools and packages (e.g., `foreach`, `doParallel`, `future`)
- Resource allocation guidelines for parallel jobs
- When parallelization is worth the overhead]

**TODO**: Obtain detailed guidelines from Lars and technical team

## Flow & Process Diagram

[PLACEHOLDER: Add a flowchart/diagram showing:
- Decision tree for export requests
- Timeline from submission to completion
- Server connection decision tree (sign out vs. disconnect)
- Resource management workflow]

**TODO**: Obtain diagram from Lars

---

# Appendix: Tasks & Action Items

## Immediate Actions (Placeholders to Complete)

- [ ] **Office Hours**: Replace "xxxdays" with specific day and time (e.g., "Thursday, 2:00 PM")
- [ ] **Email Address**: Replace `<datamanagement-email>` with actual email
- [ ] **Memory Limit**: Replace "X.X Gb" with specific threshold value
- [ ] **Node limit**: Replace "x nodes" with number of nodes
- [ ] **Regulatory Links**: Add actual URLs for:
  - DST Guidelines
  - Sundhedsdatastyrelsen Guidelines
  - NCRR Guidelines
- [ ] **Parallel Processing**: Complete the "Running Analyses in Parallel" section
- [ ] **Flow Diagram**: Create and add process diagram
- [ ] **Update from On-boarding**: Check on-boarding document for items to reflect here

## Review & Validation Tasks

- [ ] Review current DST guidelines for any missing requirements
- [ ] Review Sundhedsdatastyrelsen guidelines for any missing requirements
- [ ] Review NCRR guidelines for completeness
- [ ] Gather real-world export rejection examples to improve common issues table
- [ ] Get feedback from Clara Grønkjær on export process details
- [ ] Get feedback from Rune Haubo Christensen on server behavior practices
- [ ] Collect data on typical export request turnaround times
- [ ] Document any recent changes in DST or regulatory requirements

## Documentation & Maintenance

- [ ] Set up regular review schedule (e.g., quarterly) for guideline updates
- [ ] Convert this file to include a version control note
- [ ] Create a companion troubleshooting guide for common issues
- [ ] Develop export request template email
- [ ] Create a "frequently asked questions" section based on support inquiries
- [ ] Consider creating separate beginner vs. advanced guides

---

## Document Metadata

- **Current Status**: DRAFT
- **Last Updated**: 2026-02-22
- **Maintained By**: Data Management Team
- **Next Review Date**: [PLACEHOLDER: Set 3-6 months from now]
- **Contributors**: Clara Grønkjær, Rune Haubo Christensen, Team Members
