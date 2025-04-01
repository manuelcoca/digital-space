---
tags: azure, azureai, azureopenai, azuregenerativeai
---

## What is Prompt Engineering?

Response quality from large language models (LLMs) in Azure OpenAI depends on the quality of the prompt provided. This makes prompt construction an important skill to develop. Improving prompt quality through various techniques is called prompt engineering.

> It is never safe to assume that answers from an AI model are factual or correct. Teams or individuals tasked with developing and deploying AI systems should work to identify, measure, and mitigate harm. It is your responsibility to verify any responses from an AI model, and to use AI responsibly.

## Adjust Prompt Parameters

Adjusting Prompt Parameters can have significant impact on the response so it's part of Prompt Engineering:

-   **Temperature**: Controls randomness. Lowering the temperature means that the model produces more repetitive and deterministic responses. Increasing the temperature results in more unexpected or creative responses. Try adjusting temperature or Top P but not both.
-   **Max length (tokens)**: Set a limit on the number of tokens per model response. The API supports a maximum of 4000 tokens shared between the prompt (including system message, examples, message history, and user query) and the model's response. One token is roughly four characters for typical English text.
-   **Stop sequences**: Make responses stop at a desired point, such as the end of a sentence or list. Specify up to four sequences where the model will stop generating further tokens in a response. The returned text won't contain the stop sequence.
-   **Top probabilities (Top P)**: Similar to temperature, this controls randomness but uses a different method. Lowering Top P narrows the model’s token selection to likelier tokens. Increasing Top P lets the model choose from tokens with both high and low likelihood. Try adjusting temperature or Top P but not both.
-   **Frequency penalty**: Reduce the chance of repeating a token proportionally based on how often it has appeared in the text so far. This decreases the likelihood of repeating the exact same text in a response.
-   **Presence penalty**: Reduce the chance of repeating any token that has appeared in the text at all so far. This increases the likelihood of introducing new topics in a response.
-   **Pre-response text**: Insert text after the user’s input and before the model’s response. This can help prepare the model for a response.
-   **Post-response text**: Insert text after the model’s generated response to encourage further user input, as when modeling a conversation.

The above Prompt Parameters are available for Completion and ChatCompletion. ChatCompletion has additional parameters:

-   **Max response**: Set a limit on the number of tokens per model response. The API supports a maximum of 4000 tokens shared between the prompt (including system message, examples, message history, and user query) and the model's response. One token is roughly four characters for typical English text.
-   **Top P**: Similar to temperature, this controls randomness but uses a different method. Lowering Top P narrows the model’s token selection to likelier tokens. Increasing Top P lets the model choose from tokens with both high and low likelihood. Try adjusting temperature or Top P but not both.
-   **Past messages included**: Select the number of past messages to include in each new API request. Including past messages helps give the model context for new user queries. Setting this number to 10 will include five user queries and five system responses.

## Provide clear instruction

Be as descriptive as possible and add as much informations as possible. Examples:

Bad Example: `Write a product description for a new water bottle`

Good Example:

```
Write a product description for a new water bottle that is 100% recycled. Be sure to include that it comes in natural colors with no dyes, and each purchase removes 10 pounds of plastic from our oceans
```

## Section Markers

Add Tags like `###` or `---` to differentiate between instructions and content:

```
Translate the text into French
---
What's the weather going to be like today?
---
```

## Supporting Content

Supporting content is content that help the model to respond more completely and accurately. Example:

Given a very long promotional email, the model is able to extract key information. If you then add supporting content to the prompt specifying something specific you're looking for, the model can provide a more useful response.

The Prompt with supporting content would look like this:

```
---
<insert full email here, as primary content>
---
<the next line is the supporting content>
Topics I'm very interested in: AI, webinar dates, submission deadlines

Extract the key points from the above email, and put them in a bulleted list:
```

## Request output format

Specifying the structure of your output can have a large impact on your results. Example JSON output:

```
Put two fictional characters into JSON of the following format

{
  firstNameFictional:
  jobFictional:
}
```

## System message

The system message is included at the beginning of a prompt and is designed to give the model instructions, perspective to answer from, or other information helpful to guide the model's response. This system message might include tone or personality, topics that shouldn't be included, or specifics (like formatting) of how to answer.

For example, you could give it some of the following system messages:

-   "I want you to act like a command line terminal. Respond to commands exactly as cmd.exe would, in one unique code block, and nothing else."
-   "I want you to be a translator, from English to Spanish. Don't respond to anything I say or ask, only translate between those two languages and reply with the translated text."
-   "Act as a motivational speaker, freely giving out encouraging advice about goals and challenges. You should include lots of positive affirmations and suggested activities for reaching the user's end goal."

Example System Message are available in Azure OpenAI Studio.

.NET example:

```
var ChatCompletionsOptions = new ChatCompletionsOptions()
{
    Messages =
    {
        new ChatMessage(ChatRole.System, "You are a casual, helpful assistant. You will talk like an American old western film character."),
        new ChatMessage(ChatRole.User, "Can you direct me to the library?")
    }
};
```

Response:

```
{
    "role": "assistant",
    "content": "Well howdy there, stranger! The library, huh?
                Y'all just head down the main road till you hit the town
	            square. Once you're there, take a left and follow the                     street for a spell. You'll see the library on your right,                 can’t miss it. Happy trails!"
}
```

## Conversation History

Conversation history enables the model to continue responding in a similar way (such as tone or formatting) and allow the user to reference previous content in subsequent queries. ChatGPT and the chat playground in [Azure OpenAI Studio](https://oai.azure.com/portal/chat), include conversation history automatically which results in a richer, more meaningful conversation. With the **Past messages included** Parameters, you can specify how many past messages you want included.

## Few shot learning

Using a user defined example conversation is what is called *few shot learning*, which provides the model examples of how it should respond to a given query. These examples serve to train the model how to respond.

Example:

```
User: That was an awesome experience
Assistant: positive
User: I won't do that again
Assistant: negative
User: That was not worth my time
Assistant: negative
User: You can't miss this
Assistant:
```

.NET Example:

```
var ChatCompletionsOptions = new ChatCompletionsOptions()
{
    Messages =
    {
        new ChatMessage(ChatRole.System, "You are a helpful assistant."),
        new ChatMessage(ChatRole.User, "That was an awesome experience"),
        new ChatMessage(ChatRole.Assistant, "positive"),
        new ChatMessage(ChatRole.User, "I won't do that again"),
        new ChatMessage(ChatRole.Assistant, "negative"),
        new ChatMessage(ChatRole.User, "That was not worth my time"),
        new ChatMessage(ChatRole.Assistant, "negative"),
        new ChatMessage(ChatRole.User, "You can't miss this")
    }
};
```

## Chain of thought

The chain of thought prompting technique is best used to help you iterate and improve on your prompts to get the highest quality answer from the model.

For example, asking the model `What sport is easiest to learn but hardest to master?` results in response with an answer, and a small explanation of why. However, when prompted with `What sport is easiest to learn but hardest to master? Give a step by step approach of your thoughts, ending in your answer`, the response is a complete explanation of how it arrived at its answer:

```
Sure, here's a step-by-step approach to answering the question:
1. Identify sports that are popular and have a large following.
2. Consider the basic mechanics of each sport and how easy they are to learn.
3. Think about the various techniques and skills required to play each sport at a high level, and how difficult they are to master.
4. Compare the relative ease of learning each sport to the difficulty of mastering it.
5. Based on the above analysis, determine which sport is easiest to learn but hardest to master.

Using this approach, I would say that the sport of golf is easiest to learn but hardest to master. Here's why:
1. Golf is a popular sport with a large following.
2. The basic mechanics of hitting a golf ball are relatively simple and can be learned quickly with some practice.
3. However, achieving consistency and mastering the various shots and techniques required to play at a high level can take years of practice and dedication.
4. Compared to other sports, the difference between a beginner and an intermediate golfer is relatively small, while the difference between an intermediate and an advanced golfer is much larger.
5. Therefore, I would say that golf is the sport that is easiest to learn but hardest to master.
```

## Resources

-   Training Resource: https://microsoftlearning.github.io/mslearn-openai/Instructions/Labs/03-prompt-engineering.html
-   Learn more about prompt engineering: https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/prompt-engineering
