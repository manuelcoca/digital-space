---
tags: azure, azureai, azureaibotservice
---

Bot solutions on Microsoft Azure are supported by the following technologies:

-   **Azure AI Bot Service**. A cloud service that enables bot delivery through one or more channels, and integration with other services.
-   **Bot Framework Service**. A component of Azure AI Bot Service that provides a REST API for handling bot activities.
-   **Bot Framework SDK**. A set of tools and libraries for end-to-end bot development that abstracts the REST interface, enabling bot development in a range of programming languages. ![[azure-bot-technologies.png]]

# Developing a Bot with the Bot Framework SDK

The SDK is available for multiple programming languages, including Microsoft C# (.NET Core), Python, and JavaScript (Node.js). Full [Bot Framework SDK documentation](https://learn.microsoft.com/en-us/azure/bot-service/bot-activity-handler-concept).

## Bot templates

The easiest way to get started with the Bot Framework SDK is to base your new bot on one the templates it provides:

-   **Empty Bot** - a basic bot skeleton.
-   **Echo Bot** - a simple "hello world" sample in which the bot responds to messages by echoing the message text back to the user.
-   **Core Bot** - a more comprehensive bot that includes common bot functionality, such as integration with the Language Understanding service.

## Testing with the Bot Framework Emulator

-   Bots created with the Bot Framework SDK are intended to run on Azure as cloud services.
-   During development, testing is necessary before deploying the bot.
-   The Bot Framework Emulator is a tool that allows you to test your bot on local or remote web applications.
-   It provides an interactive web chat interface to test the bot, capturing activity events and showing them for monitoring bot behavior as you send messages and review responses.

## Bot application classes and logic

-   Bots use an Adapter class to communicate with the user's channel.
-   Conversations in a bot are made up of activities, representing events like user interactions.
-   Activities happen in the context of a turn, a two-way exchange between user and bot.
-   The Bot Framework Service informs the bot's adapter when activities occur, and the adapter initiates the turn context and calls the bot's Turn Handler method to handle the activity's logic.
-   The logic for processing the activity can be implemented in multiple ways:
    -   **Activity handlers**: For simple stateless interactions Event methods that you can override to handle different kinds of activities.
    -   **Dialogs**: More complex patterns for handling stateful, multi-turn conversations.

# Activity Handlers

For simple bots with short, stateless interactions, you can use Activity Handlers to implement an event-driven conversation model in which the events are triggered by activities such as users joining the conversation or a message being received. When an activity occurs in a channel, the Bot Framework Service calls the bot adapter's **Process Activity** function, passing the activity details. The adapter creates a turn context for the activity and passes it to the bot's *turn handler*, which calls the individual, event-specific activity handler.![[activity-handlers.png]] The **ActivityHandler** base class includes event methods for the many kinds of common activity, including:

-   Message received
-   Members joined the conversation
-   Members left the conversation
-   Message reaction received
-   Bot installed
-   Others...

You can override any activity handlers for which you want to implement custom logic.

# Dialogs

For more complex conversational flows where you need to store *state* between turns to enable a *multi-turn conversation*, you can implement *dialogs*.

There are two common patterns for using dialogs to compose a bot conversation:

## Component dialog

-   A component dialog can include other dialogs and is defined within its dialog set.
-   Typically, the initial dialog in a component dialog is a waterfall dialog, which guides the conversation through a sequence of steps.
-   Each step in a waterfall dialog is often a prompt dialog to gather user input in a sequential manner.
-   The output of each step is passed on to the next step, ensuring that each step is completed before moving on.
-   For instance, a pizza ordering bot might use a waterfall dialog to prompt the user for pizza size, toppings, and payment information in a sequential fashion. ![[component-dialog.png]]

## Adaptive dialogs

-   An adaptive dialog offers a more flexible conversational flow, allowing for interruptions, cancellations, and context switches.
-   It starts with a root dialog that contains a flow of actions, including branches and loops, and triggers.
-   The recognizer, often using the Language Understanding service, detects user intents, which can trigger changes in the conversation flow.
-   For instance, a pizza ordering bot might begin with a welcoming root dialog and, upon recognizing the intent to order a pizza, triggers a dialog to collect pizza order details. The flow can be altered at any point based on user input, allowing actions like canceling the order.![[adaptive-dialog.png]]

# Deploy a bot

```
// Register an Azure app
az add app create

// Create bot application service
az deployment group create

// Deploy bot
az bot prepare-deploy

// Deploy bot as web app
az webapp deployment source config-zip
```

## Register an Azure App

-   To create an application registration, use the "az ad app create" command in the Azure CLI.
-   You need to specify a display name and password for your app identity.
-   This command registers the app and provides registration details, including a unique application ID required for subsequent steps.

## Create a bot application service

-   Your bot needs a Bot Channels Registration resource, an application service, and an application service plan.
-   To create these resources, you can use Azure resource deployment templates provided with the Bot Framework SDK template.
-   Run the "az deployment group create" command, referencing the deployment template, and provide your bot application registration's ID (from the "az ad app create" command output) and the specified password.

## Prepare your bot for deployment

-   Preparing your bot depends on the programming language used.
-   For C# and JavaScript bots, use "az bot prepare-deploy" to configure the bot with package dependencies and build files.
-   For Python bots, create a "requirements.txt" file listing necessary package dependencies for the deployment environment.

# Deploy bot as web app

The final step is to package your bot application files in a zip archive, and use the `az webapp deployment source config-zip` command to deploy the bot code to the Azure resources you created previously.

After deployment has completed, you can test and configure your bot in the Azure portal.

---

# Resources

-   Training Resource: [https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/13-bot-framework.html](https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/13-bot-framework.html)
