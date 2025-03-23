---
tags: azure, azureai, azureaipersonalizer
---

# Introduction

Azure AI Personalizer helps to improve decision-making for your app in near real-time. It uses _reinforcement learning_ which is a process that enables AI Personalizer to choose the best _action_ for a given _context_, aiming to maximize a _reward_:

-   **Context**: The total information that represents the state of your app, scenario, or users that might be relevant to making a decision.
    -   For instance: Device type (such as laptop, or phone), location, and popular topics of interest of users that visit a site.
-   **Actions**: Sets of items that can be chosen, together with specific attributes that describe each item.
    -   For example: A set of technology review articles and the topics that are discussed in each article.
-   **Reward**: A numerical score ranging between zero and one that indicates whether the decision was bad (zero), or good (one)
    -   For instance: A score of one indicates that a user definitely opened a suggested article, and a zero would mean the user didn't open the article.

# How it works

You use Azure AI Personalizer through the following APIs:

-   Your app calls the **Rank API** whenever it needs to make a decision (each call is referred to as an *event*). In each event, your app sends a request in JSON format, describing a set of actions, a description of each action, and a description the current context. Each event has an *event ID*. Azure AI Personalizer then uses its backend model to decide and return the ID of the best action that maximizes the total average reward.
-   Your app then calls the **Reward API** whenever it can provide feedback to help Azure AI Personalizer to learn whether the action ID it returned was valuable. For instance, if a product was purchased successfully by a user after it was suggested to them. You can base the reward score on your own organization's business metrics and objectives, and generate it using rules inside your app or through an algorithm. You then provide your feedback to the API.

# Create a Personalizer Resource via Azure CLI

```
$ az login

// Create Resource Group
$ az group create \
    --name my-personalizer-resource-group \
    --location westus2

// Create Personalizer
$ az cognitiveservices account create \
    --name my-personalizer-learning-loop \
    --resource-group my-personalizer-resource-group \
    --kind Personalizer \
    --sku F0 \
    --location westus2 \
    --yes

// List keys
$ az cognitiveservices account keys list \
    --name my-personalizer-learning-loop \
    --resource-group my-personalizer-resource-group
```

# Configure Learning Loop

## Configure Reward

To configure your learning loop, you go to your Personalizer resource's Setup page. On the setup page configure rewards first:

![[configure-rewards.png]]

-   **Reward wait time**
    -   Determine how long Azure AI Personalizer should collect reward values for a Rank call from when the first call happens. To decide, you should ask yourself: "How long should Azure AI Personalizer wait for rewards calls?" Any rewards that are received after this time period won't be used for learning, but will be logged.
-   **Default reward**
    -   If no reward call is received by Azure AI Personalizer during the reward wait time period for a Rank call, AI Personalizer will assign this default reward. By default, and in most scenarios, this default reward is zero.
-   **Reward aggregation**
    -   If Azure AI Personalizer received multiple rewards from the same Rank API call, an aggregation method is used. You can choose between sum or earliest. For example, earliest means the earliest score received is used and the rest is discarded. This is useful if you want a unique reward among possibly identical Rank calls.

## Configure Exploration

Choose an appropriate exploration value to determine what percentage of Rank calls should be answered using exploration, which means it tries to find new patterns instead of using the underlying trained model: ![[configure-exploration.png]] If the value is changed, the model will be resetted and retrained with data from last 2 days.

## Configure model update frequency

Configure how often the model should be trained. ![[configure-update-frequency.png]]

## Configure data management

Under data retention, you determine how many days Azure AI Personalizer should keep data logs. Data logs are used for offline evaluations. These types of evaluations enable you to figure out how effective your Azure AI Personalizer is, so that you can optimize it. ![[configure-data-management.png]]

# Configure learning behaviour

There are 2 available learning methods: ![[configure-apprentice-online-behavior.png]]

Through *Apprentice mode*, Azure AI Personalizer can begin its learning process by looking at the choices made by your app's current logic and mimic its decisions. You're able to use metrics from the Azure portal or API to help you understand how well it matches your apps current logic (also referred to as the *baseline policy*).

When your Azure AI Personalizer resource is able to match your app's existing logic around 75-85% of the time, you'll then change its learning behavior from Apprentice mode to *Online mode*. In Online mode, your AI Personalizer will change to return the best actions in the Rank API based on the underlying model and will learn to start to make better decisions than your baseline policy.

## Evaluate Apprentice mode and switch to Online mode

Under **Monitor** page you can view following metrics:

-   **Baseline – average reward**: Average reward of the application’s default business logic (baseline).
-   **Personalizer – average reward**: Average of total reward your AI Personalizer could potentially have reached.
-   **Reward achievement ratio over most recent 1000 events**: A ratio that represents the Baseline and Personalizer reward. ![[evaluation-metrics.png]] Once the Reward achievement ratio has reached around 75-80%, you can change to Online Model.

# Import and export model and learning settings

You can export your Azure AI Personalizer's underlying model and its learning settings (also called *learning policy*) for backup and version control.

To export your model, you go to your AI Personalizer resource's Setup page, select the **Import/export** tab, and select **Export** model: ![[export-model.png]]

Same steps for importing model.
