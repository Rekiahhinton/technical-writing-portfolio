---
title: Technical Writer Skills Assessment
---

### Exercise 1: Documenting a Flawed UI

#### Procedure

1. Click the **Source** dropdown menu and select **Users**.
   Note: Selecting **Users** disables the **Legacy Format** option below.
2. Check **PII Obfuscation**. This option excludes personal user data from the exported list.
3. To set the date range for the previous month, click the **Start Date** field to select the first day of the month and click the **End Date** field to select the last day. 
4. Click **Execute** to export the list of non-PII user data from the specified date range.
   
#### Note to the Product Manager

Please consider my suggestions below:

* Usability Problems (2):
  1. Word Choice:
     * "PII Obfuscation" is confusing. The user may not be able to correctly identify that "PII" refers to Personally Identifiable Information. 
  2. Context:
     * The **Include Metadata** option is pre-selected with no explanation. Also, the UI doesn't specify which metadata would be included in the exported list.
     * The **Legacy Format** is disabled with no explanation. The UI also does not explain what "Legacy Format" means or how it applies to the data inside the exported list.
     * The **Save Query** button has no visible function, which may cause users to question if the button worked. The UI also doesn't provide information about where to find the saved query.
* Improvements:
  1. Word Choice:
     * "PII Obfuscation" -> "Include Personal Data." This clearly describes the option and allows the user to select the option to include personal data rather than selecting the option to exclude it. 
  2. More Context:
     * Add a tooltip next to **Include Metadata** to explain which metadata is included and why it is pre-selected.
     * Add a tooltip to explain what **Legacy Format** is and why it is disabled.
     * Add a confirmation message after the user clicks the **Save Query** button and a note to explain that the query is saved in a separate **Saved Queries** tab on another page. 

### Exercise 2: Creating a High-Fidelity Analogy

The Hydra Token is an API token that expires after each use and immediately replaces itself with a new token. Imagine a secret club. In order to interact with the club members, you must have a code. The code is only good for use with a single member. After interacting with each member, the code expires and you receive a new code to use with the next member. If you reuse an old code, access to the club is denied for that session and you must start over from the first member. This Hydra Token functionality provides airtight security to your API calls. It is virtually impossible to steal a token because each token expires immediately after use and attempting to reuse a token invalidates the session.

### Exercise 3: Restructuring Content

#### Section 1: Configuring KitchenSync (Edited)

The KitchenSync tool offers many configuration options, including the option to choose the color of the kitchen to sync. To change the color, open the configuration screen by selecting the **Configure** button on the home screen. Then, select your color by entering the color code into the appropriate field. 

Choose the connection type for the syncing using the **Connection** dropdown menu. To finalize your changes, click the **Finish** button near the bottom of the page. To save your changes, click the **Save** button on the following page.

#### Section 2: Configuring Templates (Edited)

The KitchenSync Migration offers a FOO tool that allows users to reuse templates for FOO Domains that share similar configurations. The templates contain critical policy requirements and KitchenSync environment configurations for each type of KitchenSync object. Check the current FOO Domain Configurations during application migration to create a template that contains the same Artifacts in KitchenSync.

#### Section 3: Thought Process

##### Configuring KitchenSync

I rewrote this section by excluding language that did not contribute to the ideas of the section, thus making the revision more concise. I also centered task-oriented language so the section would be easy to understand and immediately actionable to the user. 

##### Configuring Templates

I rewrote this section by organizing the section around the central idea: the templates. I also removed any language that seemed unnecessary to grasp the ideas of the section while aiming for clear and concise language.

### Exercise 4: Reviewing AI-Assisted Docs-as-Code Content 

#### Session Lockout

Session Lockout prevents unauthorized access by temporarily blocking login attempts after multiple login failures. 

##### Default Behavior

After five consecutive failed login attempts, Session Lockout prevents the user from attempting to log in for **15 minutes**. During the lockout period, the system rejects all login attempts with a specific error message.  

##### Administrator Configuration 

Administrators can configure **attempts** and **duration** for each policy:

* Attempts: The number of failed attempts the system will allow before lockout.
* Duration: The length of time the account remains locked during a lockout.

##### Audit Logging 

All lockout events appear in the security audit log. 

#### PR Review Comment

Thank you for opening this PR. I have made several revisions before approving.

##### Key Changes

1. Added headings to reorganize the content for easy readability.
2. Removed hedging language (ex. "most of the time").
3. Removed vague language ("kind of").
4. Made the content task-oriented for quick use.

##### Suggested Guidelines for Using AI

* AI produces better responses when prompts include specific feature behavior. Always validate against the spec to ensure specific feature behaviors are clearly represented. Ex. five consecutive failed login attempts lock the account for 15 minutes. 
* AI responses need human oversight to produce strong writing contributions. Make sure to review and edit AI output before opening a PR.

#### AI Usage

I utilized AI to help edit for this exercise. Then, I reviewed the final content and checked for accuracy between the output and the spec before submitting. 


