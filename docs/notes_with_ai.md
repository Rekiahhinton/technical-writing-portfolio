---
title: Crafting High-Quality, Actionable Notes from Engineering Meetings
---
### Introduction
Technical writers attend many meetings throughout the week. 

It can be challenging to keep track of all of the relevant information a writer would need to craft high-quality work when these meetings pile up; important details can be lost. By utilizing an AI-augmented workflow, I can extract every meaningful detail from an engineering standup to better understand how to serve the documentation needs of the team and create more accurate documentation deliverables.

In this example, I show my process for gathering relevant information from an engineering meeting to create notes to be used for documentation projects.

### Sample Meeting
Find the engineering meeting used in this example on [YouTube](https://www.youtube.com/watch?v=1lzK6EYO800). The sample meeting is a weekly code review workshop.

### Workflow

#### Recording and Transcribing

I begin by recording the meeting using an application like Google Meet. 

Then, I take the recorded meeting and extract a transcript called _Code Review Weekly Workshop Transcript_ which I load into an AI in PDF format. 

I start prompting by establishing role and context before issuing the following commands to the assistant:  

> User: You are an AI assistant tasked with creating a detailed and actionable summary from the provided meeting transcription, indicated with {Code Review Weekly Workshop Transcript}. First, analyze the entire transcript and identify the major themes or topics discussed therein. Generate the primary themes and list them.

> Assistant: Below are the **major themes** identified from the Code Review Weekly Workshop Transcript. These reflect the core discussion threads and recurring concerns raised throughout the session.

> **1. Dependency Management in Code Review**
> 
> * Evaluating whether to introduce new third-party libraries
> * Tradeoffs between building in-house vs. adding a dependency
> * The “left-pad” problem and dependency sprawl
> * Long-term maintenance burden of dependencies
> * Dependency trees and transitive risks

The assistant provides several themes from the transcript. Then, I prompt the assistant to provide a detailed explanation of each theme:

> User: For **Theme 1**, provide detailed notes from the transcript. Include direct quotes and expand on any acronyms. Focus on capturing all the relevant details discussed during the meeting related to this theme, formatted for easy readability with bullet points, numbered lists, and clear hierarchies of information. Avoid truncation or summary but instead provide as much detail as possible, articulated in grammatically correct English.

> Assistant:
> 
> **Theme 1: Dependency Management in Code Review**
> _(Detailed Notes with Direct Quotes and Expanded Context)_
>
> **1. Core Concern: Adding New Dependencies in Merge Requests (MRs)**
> - **MR = Merge Request** (a proposal to merge code changes into the main codebase)
> The discussion begins with a direct concern raised from a **security perspective** about what code reviewers should pay attention to when reviewing changes.
> **Key Quote**
>   - "From the security perspective is there anything that people that are performing code reviews should be aware of?”
>  
> A major point raised:
> - “MRs that introduce new dependencies make people feel different feelings… even in comparison to MRs where a thousand lines of code have changed.”
> 
> **Why?**
> * Adding a dependency is not just adding a file.
> * It introduces:
>   * Unknown runtime code
>   * Transitive dependencies (dependencies of dependencies)
>   * Security, maintenance, and operational risk
>   
> As stated:
> - “A new dependency is like I don’t even know how many lines of runtime code this is changing.”
> This reflects a core concern:
> * A dependency may represent thousands of lines of external code.
> * The reviewer cannot realistically inspect all of it.
> * It may introduce invisible risk beyond the immediate change.

The assistant provides several themes, then a detailed, logical breakdown of each theme with direct quotes and explanations.

Then I instruct the assistant to create a list of action items, decisions, and ownership: 

> User: Now create a list of all action items, decisions, and their corresponding owners or deadlines. If no specific person was assigned, determine the most appropriate person to be responsible.
> 
> Assistant:
>
> **1. Always Prefer the Latest Stable Version of Dependencies**
> **Decision**
> * The team should use the latest stable version of dependencies.
> * Rationale:
>   “We definitely want to use the latest version.”
>   “A new version does not mention a security fix does not mean there is no security fix.”
>   
> Silent patching is common; staying current reduces exposure.
> 
> **Action Items**
> * Verify dependency versions are current when reviewing Merge Requests.
> * Avoid intentionally lagging behind versions unless justified.
>   
> **Owner**
> * **MR Author** – Propose latest version.
> * **Code Reviewer** – Verify version currency.
> * **Security Team** – Provide guidance if versioning risks arise.
>   
> **Deadline**
> * Ongoing — applies to all new dependency additions.

Lastly, I prompt the assistant to create a long, structure summary: 
> User:
>
> Compile all responses into one long summary of the transcript.
>
> Assistant:
>
> Below is a **comprehensive, integrated summary** of the Code Review Weekly Workshop Transcript, combining all major themes, detailed discussion points, action items, decisions, and cultural implications into one cohesive document.
> 
> This summary is intentionally thorough and structured to reflect the depth and nuance of the original discussion.
>
> **Code Review Weekly Workshop**
> 
> **Comprehensive Transcript Summary**
>
> **1. Opening Context**
> The workshop centered on improving code review practices, with a heavy focus on security considerations—particularly around dependency management. What began as a question about what reviewers should watch for quickly evolved into a deep discussion about supply chain risk, architectural defense strategies, vulnerability disclosure processes, and the long-term cost of technical decisions.
>
> A recurring theme throughout the session was:
>
> Code review is not just about correctness — it is about long-term safety, maintainability, and systemic risk.

The assistant provides eight total items, ending with an overarching conclusion: 

> **8. Overarching Philosophy**
>
> The workshop reflects a mature security mindset:
> * Vulnerabilities are inevitable.
> * Dependencies are not free.
> * Transparency builds trust.
> * Defense-in-depth is necessary.
> * Review culture determines long-term system health.
>
> The final takeaway:
>
> Do not assume safety.
> 
> Assume compromise and design for containment.
> 
> Treat dependency introduction as an architectural decision — not a convenience.

The final step is to correct any obvious errors and request a formal document for downloading and sharing. 

[Code Review Weekly Workshop Final Document](docs/Code_Review_Weekly_Workshop_Final_Document)

### Conclusion
Applying this AI-assisted method ensures that I recover all relevant details and accurately capture the major themes of each for reference when creating documentation.


