---
title: What Can We Learn from LLM
updated: 2024-06-06 15:00
category: blog
---

## Self-Supervised Learning

Self-supervised learning has been the key to the success of LLM. It benefits most from seas of Internet data as it uses data as it is without needing annotations and labels. Particularly when the training data is scaled up, the power of transformers is unleashed, according to [scaling law](http://www.incompleteideas.net/IncIdeas/BitterLesson.html). This is the terrian where only self-supervised learning can do, connecting the two beasts of data and weights. (Supervised transformer sounds like a fallacy to me.) 

The major outcome of self-supervised learning is to give powerful and meaningful latent representations for raw tokens, just by learning from the distribution of the massive data. Knowing the distribution of a data set is equivalent to knowing the correlations (joint probability) of any two of the elements in the data set. This is why attention machanism can do a good job in this realm. And when the data collection is large enough, it contains sufficient combinations of tokens for the hungry attention network to learn from it.

There are two self-supervised learning approaches that have proven successful: contrastive learning and generative learning. Contrastive learning needs to construct similar and dissimilar pairs from data samples and is prone to collapse and instablility issues. Therefore, I personally prefer generative learning over constrastive learning. However, constrative learning is commonly used for multimodal alignment such as CLIP, which is straightforward as alignment data are natuarally organized as pairs.

Generative learning is very attractive to me as it is working just like how people think. Predicting a masked word in a sentence as defined by BERT is same as the cloze test I had in high school English courses. And imagining missing parts of a scene is a task our brain is excel at. Therefore, if a network is able to generate meaningful things within or even without context as we do, I would achknowledge it has the intelligence as we have. 

The practices of using pre-text tasks has been learned from the NLP domain to tackle vision self-supervised learning. Among them I like the inpainting task most. The pretraining job of completing an image from a masked input has been applied to single-view images ([MAE](https://arxiv.org/pdf/2111.06377)), multi-view images ([CroCo](https://arxiv.org/pdf/2210.10716)) and videos ([VideoMAE](https://proceedings.neurips.cc/paper_files/paper/2022/file/416f9cb3276121c42eebb86352a4354a-Paper-Conference.pdf)), which enables the mono-vision, 3D vision and temporal vision tasks, respectively.



The latent space produced by the learned encoder is fundamental to the next stages, either for decoding, diffusion or for multi-modality alignment. Oftentimes, the latent space can be learned so well that some downstream tasks can be accomplished by simple nearest-neighbor search, for example nearest-neighbor classification using a small number of labelled data as evidenced by [DINO](https://arxiv.org/pdf/2104.14294).





Dual problem:
Contrastive learning
Generative learning