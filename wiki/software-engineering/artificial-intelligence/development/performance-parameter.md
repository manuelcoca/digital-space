a## Overview

#### Metrics

-   **Perplexity:** Measures how well a model predicts text; lower is better.
-   **ROUGE/BLEU:** Evaluates generated text quality by comparing to reference texts.
-   **Accuracy/F1/AUC:** Classification metrics showing correctness of model predictions.
-   **Human evaluation:** Direct human ratings of model outputs for quality and usefulness.
-   **Response time:** How quickly the model generates a response.
-   **Hallucination rate:** Frequency of model generating factually incorrect information.

##### Quality specifc metricy

-   **Accuracy**: How factually correct the response is
-   **Coherence:** How logically the text flows together (1-5 scale)
-   **Fluency:** Grammar and language quality (1-5 scale)
-   **Groundedness:** Whether claims are verifiable from sources (1-5 scale)
-   **Relevance:** How well the response addresses the query (1-5 scale)

#### Variables

-   **Model size:** Larger models generally perform better but require more resources.
-   **Context window:** Maximum text length the model can process at once.
-   **Max tokens**: Limit the maximum output tokens for the model response.
-   **Temperature:** Controls randomness in outputs; higher values increase creativity.
-   **Top P**: Controls text diversity by selecting the most probable words until a set probability is reached.
-   **Presence Penalty**: Discourages the model from repeating the same words or phrases too frequently by applying a penalty (number) based on their presence in the text.
-   **Frequency penalty**: Discourages the model from generating the same words or phrases too frequently by applying a penalty (number) based on their existing frequency in the text.

##### External variables

-   **Prompt engineering:** Crafting effective instructions to guide model behavior.
-   **Hardware:** Computing resources that affect speed and capabilities.
-   **Inference optimization:** Techniques to make model execution faster and more efficient.

## Context Window

The AI context window determines the number of words or tokens that the model looks at when predicting the next word in a sequence. It helps the model understand the context and make more accurate predictions.

Benefits of large context windows:

-   Faster predictions
-   Accepts large inputs
-   Provides detailed analysis
    -   A context window operates to the left and right of the target token to deeply analyze the data
