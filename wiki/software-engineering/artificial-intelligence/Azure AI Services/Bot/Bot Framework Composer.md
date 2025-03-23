---
tags: azure, azureai, azureaibotservice
---

The composer is an open-source visual designer for developers building bots. It uses the latest SDK features so you can build complex bots with relative ease.

# Power Virtual Agents vs. Bot Framework Composer

Power Virtual Agents (PVA) is built on the Microsoft Power Platform, and enables users to build a chatbot without requiring any code. This PVA app is tailored for teams made up of both subject matter experts and developers working together.

Some features of the Azure AI Bot Service aren't available in the PVA app, and require using Bot Framework Composer (which can be launched directly from the PVA web app) to integrate those features.

# SDK vs. Composer

Using the Bot Framework Composer presents some advantages when compared to creating a bot with the SDK and coding.

-   Visual design surface in Composer eliminates the need for boilerplate code and makes bot development more accessible.
-   Save time with fewer steps to set up your environment.
-   Visualize Dialogs, allow for building portions of the bot's functionality and how to guide the conversation.
-   Triggers are easily created to invoke a specific dialog, and enabling interruptions is handled by switching a value within a prompt.
-   Composer enables saving of pieces of data to various scopes to remember things between dialogs or sessions.
-   Test your bot directly inside Composer via embedded Web Chat.

You can download and install the Bot Framework Composer from [Install Bot Framework Composer](https://learn.microsoft.com/en-us/composer/install-composer). The tool is under active development, and updates are available frequently.

# Design the user experience

An important consideration for the user experience is how you present the bot and its components to the user. You can implement the following features into a bot:

-   Text - a typical interaction that is lightweight and involves presenting text to the user and having the user respond with text input
-   Buttons - presenting the user with buttons from which to select options. In a pizza order bot, you might decide to use buttons to represent the pizza sizes available. They are a visual way to represent choices to users and add more visual appeal when compared to text
-   Images - using images in the bot interaction adds a graphical appearance to the bot and can enhance the user experience
-   Cards - allow you to present your users with various visual, audio, and/or selectable messages and help to assist conversation flow![[bot-user-interface.png]]

There are some considerations to be aware of when it comes to adding these features. Different channels will render each of these components differently. If a channel doesn't support the feature, the user experience can be degraded due to poor rendering or functional impairments.

# Recommendations to improve performance

-   Whenever possible, ask specific questions that do not require natural language understanding capabilities to parse the response. It will simplify your bot and increase the success with which your bot understands the user
-   Designing a bot to require specific commands from the user can often provide a good user experience while also eliminating the need for natural language understanding capability.
-   If you are designing a bot that will answer questions based on structured or unstructured data from databases, web pages, or documents, consider using technologies like QnA Maker that are designed specifically to address this scenario.
-   When building natural language models, do not assume that users will provide all the required information in their initial query. Design your bot to specifically request the information it requires, guiding the user to provide that information by asking a series of questions, if necessary.

---

# Resources

-   Training Resource: [https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/14-bot-composer.html](https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/14-bot-composer.html)
