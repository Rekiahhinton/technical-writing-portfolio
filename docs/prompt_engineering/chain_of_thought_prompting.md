---
title: Chain-Of-Thought (CoT) Prompting
---

## Chain-Of-Thought (CoT) Prompting: 

### Introduction
Chain-of-Thought (CoT) prompting turns complex problems into workable processes. By prompting the language model (LM) to logically work through each step of the problem, the model can solve problems that require difficult reasoning. Chain-of-Thought prompting breaks big problems into small ones, safeguarding against hallucinations and excessive bias that would distort the logical flow. 

### Resources 
This example borrows the [Markdown Help](sample-documents/Markdown) document from the [Favorite Medium](https://github.com/favoritemedium) Mattermost repository. The document lists helpful tips and examples for learning Markdown.

### Workflow

I begin by establishing role and providing the Markdown document for quick contextual analysis. 

> User:
>
> You are my technical assistant tasked with iteratively reasoning through logical steps to solve complex problems. Analyze the [Markdown.md] file for context and wait for further instructions.
> 
> Assistant: I’ve analyzed the Markdown.md file and captured its structure and capabilities.
>
> The document essentially functions as a Markdown syntax reference guide, outlining formatting primitives and their rendered output examples.
>
> I’m ready for your next instruction.
> 
Then, I begin the first logical step.

> User: What is the intention of the user reading this document?
>
> Assistant: The likely intention of the user reading this document is to learn how to format text using Markdown syntax so their messages or documents render with structured formatting.
> From the content and examples in the file, the reader is being guided to understand how to:
>
> 1. Format Written Content
> 2. Present Technical Content
> 3. Add Rich Elements to Text
> 4. Structure Information
>
> Overall intention: Based on the document's content and structure, the user reading it is most likely trying to learn how to format text using Markdown — either as a beginner reference or a quick-lookup cheat sheet.
>
> The document's tone — with side-by-side examples of syntax and rendered output — strongly suggests it's meant as a __practical, hands-on guide__ rather than a theoretical one. The reader's goal is to __immediately__ apply what they're reading.
>
> 
Next, I continue the logical process by requesting a categorization of the Markdown tasks in the document by complexity. 

>
> User: Categorize the various Markdown tasks by complexity. Group simpler things together first. Then, more complex tasks later.
>
> Assistant: Here's a complexity-based categorization of the Markdown tasks from the document:
>
> - Beginner — Single character or symbol syntax
> - Intermediate — Require paired or structured syntax
> - Advanced — Multi-line or compound structures
> - Expert — Structural/tabular formatting
>

Then, I ask the language model (LM) to determine which categories are complex enoguht to require an introductory preview.

> User: Evaluate the conclusions you drew from the previous prompt. Determine which sections need a "Result" preview to be understood.
>
> Assistant: Great question. Let me critically evaluate each category and determine where a __rendered preview__ is essential for comprehension vs. where the syntax speaks for itself.
>
> - Beginner — Low need for previews
> - Intermediate — Mixed need
> - Advanced — High need for previews
> - Expert — Critical need for previews
>
> The core principle: the greater the gap between raw syntax and rendered output, the more essential a preview becomes. Beginner syntax resembles its output. Expert syntax does not.
>
Finally, I request a cheat sheet for the beginner Markdown user.

>
> User: Create a simplified cheat sheet that a beginner Markdown user could quickly read to understand how to perform the most complex tasks in the document.
>
> Now I'll create a beautifully designed, beginner-friendly cheat sheet as an HTML artifact focused on the most complex Markdown tasks — with clear previews.
>
> Each card follows the same structure: syntax on the left, rendered result on the right, with beginner-friendly tips below.
>

The language model (LM) renders a downloadable file:

[Markdown Cheatsheet](sample-documents/markdown-cheatsheet)

### Conclusion 
Chain-of-Thought (CoT) prompting turns complex logical sequences into small tasks, while keeping the language model (LM) engaged with the most relevant task at hand. This skill is especially useful in technical writing for the ability to break down large documents. 



