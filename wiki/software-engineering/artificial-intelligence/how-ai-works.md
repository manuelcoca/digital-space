# AI Algorithm

Majority of AI algorithm are Neural Networks, but not all. AI includes a variety of techniques and methodologies that extend beyond neural networks. Some notable examples include:

-   **Symbolic AI**: Early AI systems relied heavily on symbolic reasoning, using rules and logic to solve problems. This approach includes expert systems, which apply a set of human-coded rules to make decisions or solve problems.
-   **Search Algorithms**: Algorithms like A\* or Dijkstra’s algorithm, which are used to solve navigation and pathfinding problems by exploring possible paths and choosing the optimal route
-   **Optimization Algorithms**: Techniques like genetic algorithms or simulated annealing, inspired by natural processes, are used for solving complex optimization problems without the use of neural networks.
-   **Statistical Methods**: Before the rise of deep learning, many AI tasks were approached with statistical methods, including Bayesian networks and various forms of regression analysis.

# Neural Networks and Deep Learning

Neural Networks are computational models inspired by the human brain’s structure and function. They consist of layers of nodes, or “neurons,” with each layer capable of performing specific transformations on its inputs. The key concepts include:

-   Layers: Neural networks are structured in layers: an input layer, one or more hidden layers, and an output layer. Each layer contains units (neurons) that perform computations and transmit signals to subsequent layers.
-   Weights and Biases: Connections between neurons have associated weights, and each neuron has a bias. During the learning process, these weights and biases are adjusted to minimize the difference between the actual output and the desired output.
-   Activation Functions: These functions determine whether a neuron should be activated or not, introducing non-linearity into the network, allowing it to learn complex patterns.

## Deep Learning

Deep Learning refers to neural networks with a significant number of layers. These networks have dramatically improved the performance of systems in tasks like image and speech recognition, natural language processing, and many others due to their ability to learn hierarchical representations of data.

Deep Learning (or Neural Networks) are mainly used in AI for tasks involving large amount of data.

# Large Language Models (LLMs)

Most modern LLMs, like GPT (Generative Pretrained Transformer) and BERT (Bidirectional Encoder Representations from Transformers), are based on the transformer architecture, which is a type of neural network. However, not all models or approaches exclusively use neural networks. Some tasks may use hybrid models that incorporate rule-based or statistical methods alongside neural network components to optimize performance or interpretability.

LLM models have revolutionized natural language processing due to their ability to understand, generate, and interact with human language at a high level of proficiency.

## Key components of LLMs

-   Transformers: The core architecture behind many LLMs, including GPT, is the Transformer model, introduced in the paper “Attention is All You Need” in 2017 by Google. Transformers rely on self-attention mechanisms to weigh the significance of different words in a sentence, allowing for more context-aware representations of text.
    -   The Transformer Algorithm is very simple itself. It’s more about the amount of data/tokens used to train the model
-   Pretraining and Fine-tuning: LLMs undergo two main phases of training. During pretraining, they learn from a vast corpus of text data, absorbing general language patterns, grammar, and knowledge. Fine-tuning adjusts the model to perform specific tasks like translation, summarization, or question-answering by training on a smaller, task-specific dataset.

LLMs basically just predicting the next word.

# Machine Learning

Machine Learning is built on top of Deep Learning and tries to predict outcomes based by Model and inferencing (data training).

## Model and inferencing

Many AI systems rely on predictive models that must be *trained* using sample data. The training process analyzes the data and determines relationships between the *features* in the data (the data values that will generally be present in new observations) and the *label* (the value that the model is being trained to predict).

After the model has been trained, you can submit new data that includes known *feature* values and have the model predict the most likely *label*. Using the model to make predictions is referred to as *inferencing*.

Many of the services and frameworks that software engineers can use to build AI-enabled solutions require a development process that involves training a model from existing data before it can be used to inference new values in an application.

## Probability and confidence scores

-   Machine learning models are accurate but not infallible.
-   Predictions are based on probability, not absolute truth.
-   Confidence scores indicate the likelihood of predictions.
-   Developers should use confidence scores to assess predictions.
-   Applying thresholds helps optimize application reliability and reduce risk of marginal probability predictions.

# How AI Algorithms Play Together

Deep learning provides the foundation for neural networks to learn from vast amounts of data. Neural networks, through their design and learning algorithms, enable the construction of sophisticated models like LLMs. LLMs leverage the capabilities of deep neural networks to process and generate language, making sense of the nuances and complexities of human communication.

This intricate relationship between algorithms, neural networks, deep learning, and LLMs illustrates the layered complexity of modern AI systems. Each advancement builds upon the previous, pushing the boundaries of what machines can learn and achieve.
