# Keziah's Gate Brief

This is a file that displays Keziah's understanding of the C7 module. No AI was ever used in the creation of this file (not even for grammar checks) to make sure the contents of this file is from Keziah's own words.

## 3-Tier Gate with Examples

### Gate Tier #1: In-Lane Edit

**Explanation:** 
This gate only allows code edits that stay within the specified lane of the coder.

**Example:** 
Coder A who is assigned in making audits makes edits to files for audits -> their changes are approved automatically.

### Gate Tier #2: Constitution / CODEOWNERS Path

**Explanation:**
The constitution files and the CODEOWNERS file are sensitive files that can be dangerous if a bad actor tampers them since these files contain commands.

**Example:**
An edit to the CODEOWNERS files must be reviewed first because if someone just deletes all of the contents in the CODEOWNERS file, they can now automatically merge their own work (even if it is destructive) without having anyone to approve it.

### Gate Tier #3: Secret / Destructive Operation

**Explanation:**
This involves putting secrets (like API keys, webhooks, JWT, etc.) in public / GitHub and / or deleting files and folders / dropping tables.

**Example:**
Someone accidentally puts an API key in a file that is not listed in .gitignore. The reviewer must detect and block this before this gets pushed to GitHub.

This gate won't allow the user to automatically delete files or folders in the repo. Doing so will require someone to review these changes before merging it to the main branch. An example for this is when someone tries to delete the ref_orchestration.md file, this change should be subject to review since deleting this would break the deep audit creation process.

## The Fail-Asymmetry and the Reason Behind It

The fail-asymmetry system is used whenever the auto-reviewer fails. It is basically this; a function (either the one that blocks or the one that auto-merges) must be disabled when it fails.

The blocking part must be disabled when it fails so it won't block anything automatically, and the auto-merging part should be disabled when it fails so nothing is automatically merged unless it is reviewed by another person. This will prevent work from getting unnecessarily blocked or from unsafely being put out in GitHub / production.

## 3 QC Layers and their Places

### Layer 1: Operator Self-Review
This is the first layer because it starts with the one who created / modified the file / folder itself. Before committing this or pushing this to GitHub, the operator must check their output if they have exposed any secrets, broken any files / folders, or touched something they shouldn't have.

### Layer 2: Auto-Reviewer
This is the second layer, the AI. This AI checks changes before they get pushed to GitHub (check for any secret or sensitive files) and before a PR gets merged to main (any change that could break somethin in prod).

### Layer 3: Human Reviewer
This is the last layer which is run by a human. The human catches any errors that could have slipped through the AI's review (wrong information, bad design choices, ineffective copywriting, etc.) and writes down documentation to make sure these errors won't happen again.

## CODEOWNERS Reasoning Note

The CODEOWNERS file is an official file from GitHub that lists users who can review PRs before they get pushed to a certain branch.

### Paths to Gate to a Human

- **Destructive Code:**
If this kind of code gets auto-merged, it could break the website. This also includes changes made to sensitive files such as CLAUDE.md, .gitignore, etc.

- **Leaked Secrets:**
If any secret goes online, there is always a chance someone could have accessed it. There is always risk. Therefore, if there are any secrets or code that looks like a secret key, it should be escalated to a human reviewer before it goes online.

- **Schema Changes:**
Changing the schema could also break the website or how processes run inside the repo.

### Paths to Let Auto-Merge

- **Edits to Non-Sensitive Files:**
These are the kind of edits that can't possibly break a website such as editing the heading and paragraph structure of a page in a website.

- **Audit Creation:**
The audit creation process follows a strict set of templates and codes. Creating an audit would unlikely break a website since it only touches things needed to create an audit.

- **Updates to Audit Structure:**
These kinds of code won't automatically break anything in the website or repo. These updates are also common since there is always something to improve in the audits and regularly blocking these type of edits would just unneccesarily delay processes.