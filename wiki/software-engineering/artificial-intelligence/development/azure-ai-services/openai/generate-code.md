---
tags: azure, azureai, azureopenai, azuregenerativeai
---

One of the capabilities of Azure OpenAI models is to generate code from natural language prompts. Tasks can range from a simple one line command to a full application. The AI models can also edit and update provided code or previous responses to complete the requested task.

## AI models for code generation

Most of the language models available can help with code, with some performing better than others. In the base GPT-3 family, the standard text model (such as `text-davinci-002`) has a good base understanding of code. Codex (such as `code-davinci-002`) has expanded coding capabilities on top of the standard text model.

Recent generations (such as `gpt-35-turbo` and `gpt-4`) can perform and be used for both natural language and code.

## Prompt Examples

Here are few prompt examples to write/create, refactor, convert, explain, test, and document code:

-   `Write a function in [language] to calculate the [mathematical concept].
-   `Create a [language] function to [perform task].`
-   `Write a [language] program that [performs task] using [library or algorithm].`
-   `Write a [language] script that reads from [data source] and outputs to [data destination].
-   `Refactor code`
-   `Convert this code to [language]`
-   `Could you explain what this code is doing?`
-   `Complete the following function`
    -   If comments are provided the output will be more accurate
-   `Write three unit tests for this function`
-   `Can you add comments and documentation to the code`

Prompt examples to fix code and improve performance:

-   `Fix the bugs in this function`
-   `We can improve this function by using a mathematical formula instead`

---

## Resources

Training Resource: https://microsoftlearning.github.io/mslearn-openai/Instructions/Labs/04-code-generation.html
