---
title: What Can We Learn from LLM? Versatility
updated: 2024-07-07 14:00
category: blog
---

* TOC
{:toc}

Recent advancements in multi-modality neural networks reflect a clear trend in network design, as observed from GPT-2 to GPT-4: networks are becoming increasingly simpler, more unified, and end-to-end, with less human-injected inductive bias. Crucially, these networks are now far more versatile, capable of accepting and producing various forms of input and output, and performing multiple tasks simultaneously.

The versatility of a single network grows exponentially rather than linearly with the number of tasks it can handle. This is because a "job" often comprises multiple tasks of varying complexity. Ideally, the number of jobs a network can perform is expressed as $$N_{jobs} = 2^{N_{tasks}} - 1$$. This exponential growth in capability is fueling optimism about achieving Artificial General Intelligence (AGI). If a network can perform as many tasks as a human, there will be little reason to deny the arrival of AGI.

Here are some groundbreaking papers I’ve reviewed that highlight these trends:
* MovieGen for video generation
* PaliGemma for vision-language reasoning
* Moshi for conversational AI
* OCR 2.0 for OCR tasks
* EMMA for autonomous driving
  
These networks unify input/output formats and streamline similar subtasks within their domains using techniques that I’ll explore below.


### Tokenize Everything

The first step to building a versatile network is ensuring compatibility with various data modalities and formats. Humans process data in numerous forms—text, images, video, audio, and more—often without recognizing its diversity. Similarly, recent models tokenize these data types, converting them into a universal "language" that transformers can process, understand, and learn from.

#### Modality-Specific Tokenization

Each data modality has its unique tokenization approach:
* MovieGen controls output video FPS by using text tokens like "FPS-16."
* PaliGemma employs 1,024 location tokens for normalized image coordinates of bounding boxes and 128 mask tokens for segmentation.
* Moshi encodes audio segments as tokens using a neural audio codec.
* EMMA uses plain text tokens, even for representing waypoint coordinates.
  
Some tokenizers, like SentencePiece for text, are manually designed, while others, such as those for speech, images, and videos, are learnable. Notably, tokenizer efficiency significantly impacts model performance. For instance, the Llama 3 technical report highlighted a substantial performance boost from improved tokenization.

#### Positional Embedding for Structured Data

Since most data has structure, positional embeddings are used to encode the spatial or temporal relationships of tokens. This is particularly critical for higher-dimensional data:

MovieGen learns separate embeddings for spatial dimensions (H, W) and the temporal dimension (T).
3D tasks utilize specialized embeddings, such as Plücker coordinates for encoding camera rays, or learn camera intrinsic embeddings via linear layers.
As tokenization continues to advance, it’s foreseeable that all analytical signals and sensory information could be tokenized and interconnected through transformer attention mechanisms. This process resembles a compiler translating human-understandable information into a format comprehensible by neural networks.

### Multi-Tasking and End-to-End Learning

Modern multi-modality networks stand out for their multi-tasking capabilities and end-to-end frameworks. These characteristics mark a significant leap in versatility compared to earlier single-task methods.

#### Unified Task Formulation

Tokenization allows many tasks to be reformulated as "predicting the next token." This enables networks to handle different tasks in a single forward pass and train them simultaneously in a single backward pass. Analogous to the human brain, different neurons are activated by different tasks without altering the overall architecture.

* Cross-Domain Training: Networks like OCR 2.0 are trained on diverse datasets, combining plain OCR data with math and molecular formulas, tables, charts, sheet music, and geometric shapes.
* Multi-Objective Training: Similar to students taking courses in multiple subjects, networks like PaliGemma combine tasks such as captioning, QA, OCR, detection, and segmentation during training.

#### The End-to-End Advantage

End-to-end architectures demonstrate superior scalability and generalization compared to pipeline approaches. For example:
* Traditional OCR systems rely on a sequence of networks (layout analysis, text detection, region extraction, content recognition), where the weakest link can bottleneck performance.
* OCR 2.0, in contrast, employs an end-to-end framework capable of generalizing across tasks and improving as more data becomes available.

End-to-end systems eliminate intermediate bottlenecks, simplify scaling, and enable seamless integration of additional capabilities, making them a cornerstone of next-generation AI models.



### Data Strategy & Training Recipe


<p align="center">
<img src="/images/LLM-Data/LLM-stages.png" alt="LLM-stages" style="border-radius:15px; width: 800px;"/>
</p>
<p align="center">
<span class="footer"> <i> The training phases of LLM </i></span>
</p>




Data Collection & preprocessing

Gold in gold out


Quality & Accuracy (Noise, outliers, errors, inconsistency & incoherence, biases)
Filtering/curation/cleaning/Profanity or sensitive info or toxicity check

Scale & diversity (De-duplication)
Catogorization (Summarization, Q&A, Codes, Math, Languages, table, charts, numbers, PDF, email)
Sentiment classification
Topic classification

Data augmentation

Unified format, e.g., JSON format

Structuring e.g., pairs for Q&A tasks

Tokenization

Learn patterns and relationships
Improve generalisation to new data samples



