---
title: Text Summarization Prompting
---

## Text Summarization Prompting 

### Introduction

Language models (LMs) excel at text summarization, which is highly useful to technical writers for reducing the time spent reading documents. 

Through structured prompting techniques, I can analyze and summarize documents for consumption with 3x the efficiency of manual summarization. 

### Resources

This example uses the [Support](SUPPORT) documentation from an open-source Node.js CMS application called [Ghost](https://github.com/TryGhost/Ghost/tree/main). 

### Workflow

The first step of the text summarization process is to assign role and provide context. This helps to establish foundational logic and avoid hallucinations. 

>User: You are a highly technical writing assistant, especially adept at analysis and text summarization for software products.
>
>Assistant: Got it! I'm ready to help with technical analysis, text summarization, documentation review, code analysis, or anything else software-related. What are you working on?
>

The second step is to provide the documentation. 

>User: Analyze this document. Then, in no fewer than 250 words, summarize the primary themes and directives of the document. List the resources and links included with a brief explanation of how they relate to the rest of the document.
>
>Assistant: **Analysis & Summary of SUPPORT.md**
>
>This document serves as a support triage and community redirection guide for Ghost, an open-source publishing platform. Its primary purpose is to establish clear boundaries around how users should seek help, and to protect the development team's GitHub workspace from being overwhelmed with non-bug traffic. The document is brief but deliberate in its structure, combining firm directives with friendly, community-oriented language to guide users toward the appropriate channels without alienating them.
>
> **Primary Themes and Directives**
>
>The central theme of the document is **channel management** — ensuring that users seek help in the right places. The most emphatic directive is that users should not open GitHub issues for general support questions, debugging help, or feature ideas. This boundary is reinforced repeatedly and even given its own dedicated section ("Why not GitHub?"), which explains the rationale rather than simply issuing a prohibition. By explaining that GitHub functions as the development team's working environment for tracking bugs and active features, the document frames the restriction as a matter of respecting a professional workspace rather than as a bureaucratic rule.
>
> A secondary theme is **community empowerment**. The document leans heavily on the idea that Ghost is free and open-source software (FLOSS), and that community-driven support is both a practical necessity and a strength. Users are not merely redirected away from GitHub — they are actively invited into a broader ecosystem of knowledge-sharing through the Ghost forum.
>
> A third theme is **tiered support access**. The document quietly distinguishes between free community users and paying Ghost(Pro) customers, noting that the latter have access to direct email support. This establishes a clear value differentiation without being dismissive of free-tier users.
>
> **Resources and Links**
>
> **1. [Ghost Forum](https://forum.ghost.org)**
>
> This is the cornerstone resource of the entire document. It is referenced multiple times and positioned as the correct destination for virtually all non-bug inquiries. The forum hosts a "help" category for debugging and general questions, and an "ideas" category for feature requests. It supports account creation via Google, Twitter, or Facebook, lowering the barrier to entry for new community members.
>
> **2. [Ghost Documentation]( https://ghost.org/docs/)**
>
> Mentioned as an "extensive" resource for self-service answers. This link serves as a first line of defense, implying users should consult documentation before reaching out to the community or support team at all.
>
> **3. Email Support**
>
> Available exclusively to Ghost(Pro) subscribers. This resource is deliberately obscured (written out in plain text rather than as a mailto link), likely to discourage non-Pro users from attempting to use it. It represents the only direct, human-to-team support channel referenced in the document.
>
> **4. GitHub**
>
> While no direct GitHub link is provided, the platform is discussed as the team's internal workspace. Its mention is instructional — users are told what GitHub is for (bug reports, feature tracking) so they understand the narrow scope of appropriate use.
>
> Together, these resources form a layered support ecosystem that scales from self-service documentation, to community forums, to paid direct support — a model common among mature open-source projects.

### Output Accuracy Review

The final step is to conduct an output accuracy review. The review occurs by comparing the summarized analysis to the original support document and checking for accuracy. 

### Result

The summarized analysis is accurate because it draws upon multiple themes from the original document as requested. It also cites each resource the user could access from the document while including context and inferring the intention behind each resource.

The analysis maintains the content quality of the original document, which indicates the text summarization was successful. 

This comparison is necessary to ensure the language model (LM) completes the process without excessive bias that would distort the information of the original document and yield an inaccurate result. 

### Efficiency Gains

Reduced documentation analysis time from 35 minutes to 12 minutes per document using this methodology.

