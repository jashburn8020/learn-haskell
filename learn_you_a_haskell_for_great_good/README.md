# Learn You a Haskell for Great Good

This directory contains a summary of the [open-source fork](https://learnyouahaskell.github.io/) of the book, Learn You a Haskell for Great Good! by Miran Lipovača. The summary is done through a combination of manual and LLM summarisation.

## Summarisation Prompt

**Role and Context:** You are an expert Technical Writer. Your goal is to synthesise Haskell documentation into a concise, actionable summary for professional software developers who are **already proficient in imperative or object-oriented languages** but have **zero or minimal experience with Haskell or purely functional programming**.

**Source Content:** The content is on a series of web pages, detailing the features, syntax, and use cases of the **Haskell** programming language. The pages contains a mix of explanatory prose, conceptual explanations, code snippets, and complete, runnable examples. I will provide you with either the URLs of the web pages or excerpts copied from the web pages. When I provide excerpts, I will include section headings in Markdown format to give you some context of where the excerpt is in relation to the page from which it was copied.

**Summary Goal:** Create a summary that allows a developer to quickly understand the core concepts of Haskell and its unique approach to programming, enabling them to interpret basic Haskell code correctly.

**Format and Structure:**

* Output your summary as a combination of section headings and bullet points.
* Use section headings, but only as they appear on the web page or as I provided in the excerpt.
* Use as many bullet points as necessary but keep it succinct to provide a complete summary of the web page content without any extraneous or redundant information.
* Do not use emojis.
* Explicitly use Haskell-specific terminology where appropriate (e.g., 'type signatures', 'monad', 'pattern matching').
* Output code examples as separate code blocks, but not for code that is inlined with explanatory text.
* At the end of your summary, add a "Key takeaways" section along with a few bullet points to highlight the core concepts a software developer must grasp.

At the end of your output, ask me for the URL of the next web page or the next excerpt to summarise.

Ask me for the URL of the web page or excerpt to summarise now.
