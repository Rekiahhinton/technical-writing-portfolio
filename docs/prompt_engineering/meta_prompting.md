---
title: Meta Prompting for Documentation
---

### Introduction

Meta prompting is a powerful method for extracting precise responses from a language model (LM) by using the model itself to generate a prompt that yields the desired response. By requesting the language model to create a prompt from sample material, a technical writer can "reverse engineer" documentation by using the outputs to create new documentation. This method is also useful for systematically interpreting and parsing material for large repositories within an Agile environment.

In this example, I demonstrate how to reverse engineer documentation with meta prompting. I provide the sample and initial instructions to Claude Sonnet 4.6 to generate a prompt that would yield a similar output to the sample document. I then test the prompt with a different model (Chat GPT-5.3) using new source material. This allows me to assess the effectiveness of the prompt by creating new documentation that achieves the same objectives as the sample, essentially reverse engineering the documentation process. 

### Resources

This example uses the Netlify [README document](prompt_engineering/README_Netlify), which explains how to deploy a static site to Netlify, a hosting provider. 

### Workflow

The first step is to provide context and role to Claude Sonnet 4.6 and attach the sample procedural guide: 

> User: You are a prompt engineer who will create a prompt using some provided content. In this scenario, you will reverse engineer a prompt for a procedural guide, a Netlify README that explains how to deploy a static site to Netlify's platform. Although you will reverse engineer the prompt from specific content, the prompts you develop should be generalized and not specific to the particular procedural guide I will provide. Analyze the sample procedural guide, then wait for further instructions.
>
> Assistant: I've carefully analyzed the Netlify README. Ready for your next instructions — whether that's drafting a generalized prompt, producing variations, or something else.
>
> User: Develop a prompt that would yield content for the section titled, "Create a Netlify site and publish the first build." Be specific about which instructions to provide to help a language model produce a similar step-by-step guide like this one. Include sentence style, structure, and length.
> 
> Assistant:
> 
> **PROMPT**: Write a step-by-step procedural guide section for a technical README that walks a developer through a single, discrete setup task within a larger deployment or configuration workflow.
>
> **Content requirements**:
>
> * The section should cover one focused task from start to finish, beginning at the point of login or access and ending with a confirmation or success state
> * Include 6–8 numbered steps
> * Where relevant, explain why a step is being performed or why a particular tool/account/setting is preferred — but keep explanations brief (1–2 sentences maximum)
> * If a step involves a decision point or a prerequisite that may not yet exist, include a short inline conditional instruction (e.g., "If X doesn't exist yet, do Y first")
> * End the section with a one-sentence description of what will happen automatically going forward once setup is complete
>
> **Style and tone**:
>
> * Use second person ("you") and imperative mood for all action steps (e.g., "Select," "Input," "Navigate")
> * Write in present tense throughout
> * Keep each step to 1–3 sentences; favor brevity over explanation
> * Use inline code formatting (backticks) for all commands, file paths, directory names, configuration values, and UI element labels
> * Write UI navigation paths in the format ```Section > Subsection```
> * Maintain a neutral, professional tone — no enthusiasm, no filler phrases
>
> **Format**:
>
> * Use an H3 heading that names the task as a noun phrase (e.g., "Create a site and publish the first build")
> * Number each step sequentially starting at 1
> * Do not use subheadings within the stepsDo not use callout boxes, bold warnings, or tip blocks — weave any caveats inline within the relevant step
>
> This prompt can be reused for any procedural section by swapping in the specific task, platform, and tooling details while keeping all structural and stylistic rules intact.
>
> 


### Conclusion

