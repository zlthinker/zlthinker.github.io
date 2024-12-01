---
title: What Can We Learn from LLM? Versatility
updated: 2024-07-07 14:00
category: blog
---

* TOC
{:toc}

Recently I have been reading a bunch of multi-modality related papers, which clearly show the trend of network design that we have witnessed from GPT-2 to GPT-4: the network becomes simpler as transformer blocks, more unified and end-to-end with less and less inductive bias human injected. Most importantly, the networks become more versatile, in terms of the flexibility of taking in and giving all forms of inputs and outputs, and performing multiple tasks. 

The number of jobs a single network can support actually grows exponentially instead of linearly as the network is capable of more tasks, since a job is composed of multiple tasks depending on its complexity. Ideally the formula is $$N_{jobs} = 2^{N_{tasks}} - 1$$. The growing versability of current neural networks is why people find hope for AGI. One day when a network can do as many tasks as human, there is no reason not saying AGI has come.

The papers I have read are 
* MovieGen for video generation,
* PaliGemma for vision language reasoning,
* Moshi for chatting,
* OCR 2.0 for OCR tasks, and
* EMMA for self-driving.

Each of the network has unified the I/O formats and similar subtasks in their domains using the techniques that we are going to talk about in the rest of the writing. 


### Tokenize Everything

The first step of getting a versatile network is to make it compatible with different data modalities and formats. Actually the data we human process everyday comes in so many shapes that even ourselves fail to realize it. Recent papers have already been able to handle data like text, image, video, audio, bounding box, segmentation, table, chart, math formula and so on through tokenization. These tokens have become the "language" that transformers can process, understand and learn their patterns and relations.

Each modality of data has its special way of tokenization. For example, MoveiGen controls the FPS of the output video by using the text token of "FPS-16". PaliGemma uses 1024 location tokens for binned normalized image coordinates of bounding boxes, and 128 mask tokens for segmentation. Moshi converts audio segments into tokens through neural audio codec. EMMA still uses plain text tokens even for waypoint coordinates.

Some tokenizers rely on manually designed algorithms such as SentencePiece for texts, while the others like speech, images and videos use learnable tokens. Actually the efficiency of a tokenizer is very important to model performance. As said by [Llama 3 technical report](https://ai.meta.com/blog/meta-llama-3/), using a more efficient tokenizer has boosted the model performance subtantially.

Since almost all the data are structured, positional embedding usually comes with tokenization to encode the positional information of individual tokens. Particularly, for the data with higher dimentions, keeping the structural information lossless is more critical than 1D sequential data.
In MovieGen, the embeddings are learned separately for spatial dimensions H, W and temporal dimension T. In 3D tasks, there are dedicated embedders for encoding the geometries. For example, Plucker corodinates are used to encode the camera rays ([Camera As Rays](https://arxiv.org/pdf/2402.14817)) and camera intrinsics embedding can be learned through linear layers ().

Considering the trends that are going on, it can be foreseen that all the anylitical signals and sensory information in the world can be tokenized on some day and all the tokens can be connected by attention mechanism of trasformer networks. It is like a compiler that translate the information that are understandable (or even not) to we huamns into those understandable by networks. 

### Multi-tasking and end-to-end

The characteristics of these multimodality networks are their being multi-tasking and end-to-end. It is a big advancement in versatility compared with previous methods which can only tackle a single step per network. 

Benefiting from tokenization mentioned above, many of the tasks can be formulated as "predicting the next token". In this way, the networks can perform different tasks in the same forward pass and be trained in the same backward pass. It's analogous to human's brain that different neurons will be activated by different tasks while without changing the architecture. 

First of all, the network can be trained by the combination of datasets from differnt domains that used to be leveraged separately. For example, OCR-2.0 is trained with not just the plain OCR data but also math formulas, molecular formulas, tables, charts, sheet music and geometry shapes.

Second, different objectives can be combined together in training to enable multi-task learning. It is the same thing as a student taking courses of different subjects in a school day. PaliGemma is quite obvious in this as it is trained with tasks including captioning, QA, OCR, detection and segmentation.

On the other hand, an end-to-end network is proved to be a trend as long as training data is sufficient. As being end-to-end, the network is easier to scale up and develop more powerful generalisation capability than a pipeline of several networks. For example, an OCR system used to integrate together networks such as layout analysis, text detection, region extraction and contents recognition, which involves too much inductive bias. As a comparison, OCR-2.0 possesses a end-to-end framework that can generalize across different tasks, and can be further trained as the data grows.



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



