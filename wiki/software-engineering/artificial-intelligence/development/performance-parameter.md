## Overview

#### Metrics

-   **Perplexity:** Measures how well a model predicts text; lower is better.
-   **ROUGE/BLEU:** Evaluates generated text quality by comparing to reference texts.
-   **Accuracy/F1/AUC:** Classification metrics showing correctness of model predictions.
-   **Human evaluation:** Direct human ratings of model outputs for quality and usefulness.
-   **Response time:** How quickly the model generates a response.
-   **Hallucination rate:** Frequency of model generating factually incorrect information.

#### Variables

-   **Model size:** Larger models generally perform better but require more resources.
-   **Context window:** Maximum text length the model can process at once.
-   **Temperature:** Controls randomness in outputs; higher values increase creativity.
-   **Prompt engineering:** Crafting effective instructions to guide model behavior.
-   **Fine-tuning:** Adapting a pre-trained model to specific tasks using labeled data.
-   **Hardware:** Computing resources that affect speed and capabilities.
-   **Inference optimization:** Techniques to make model execution faster and more efficient.

## Context Window

The AI context window determines the number of words or tokens that the model looks at when predicting the next word in a sequence. It helps the model understand the context and make more accurate predictions.

Benefits of large context windows:

-   Faster predictions
-   Accepts large inputs
-   Provides detailed analysis
    -   A context window operates to the left and right of the target token to deeply analyze the data
