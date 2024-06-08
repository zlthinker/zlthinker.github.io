---
title: What Can We Learn from LLM
updated: 2024-06-06 15:00
category: blog
---


Self-supervised learning has been the key to the success of LLM. It benefits most from the massive data from the Internet as it uses data as it is without needing annotations and labels. Particularly when the training data is scaled up, the power of transformers is unleashed, according to [scaling law](http://www.incompleteideas.net/IncIdeas/BitterLesson.html). This is the terrian where only self-supervised learning can do, connecting the two beasts of data and weights. (Supervised transformer sounds like a fallacy to me.) 

The major outcome of self-supervised learning is to give powerful and meaningful latent representations for raw tokens, just by learning from the distribution of the massive data. Knowing the distribution of a data set is equivalent to knowing the correlations (joint probability) of any two of the elements in the data set. This is why attention machanism can do a good job in this realm. And when the data collection is large enough, it contains sufficient combinations of tokens for the hungry attention network to learn from it.


The latent space produced by the learned encoder is fundamental to the next stages, either for decoding, diffusion or for multi-modality alignment. Oftentimes, the latent space can be learned so well that some downstream tasks can be accomplished by simple nearest-neighbor search, for example nearest-neighbor classification using a small number of labelled data as evidenced by [DINO](https://arxiv.org/pdf/2104.14294).



Dual problem:
Contrastive learning
Generative learning