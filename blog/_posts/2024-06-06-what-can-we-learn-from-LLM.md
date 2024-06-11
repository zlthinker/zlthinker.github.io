---
title: What Can We Learn from LLM? Self-Supervised Learning
updated: 2024-06-06 15:00
category: blog
---

* TOC
{:toc}

Self-supervised learning (SSL) has been the key to the success of LLM. It benefits most from seas of Internet data as it uses data as it is without needing annotations and labels. Particularly when the training data is scaled up, the power of transformers is unleashed, according to [scaling law](http://www.incompleteideas.net/IncIdeas/BitterLesson.html). This is the terrian where only self-supervised learning can do, connecting the two beasts of data and weights. (Supervised transformer sounds like a fallacy to me.) 

<p align="center">
<img src="/images/SSL-data-network.jpg" alt="ssl-data-network" style="border-radius:15px; width: 350px;"/>
</p>
<p align="center">
<span class="footer"> <i> Self-supervised learning is the best way of combining massive data and huge network so far. </i></span>
</p>

### Representation Learning

The major outcome of self-supervised learning is to give powerful and meaningful latent representations for raw tokens, just by learning from the distribution of the massive data. Knowing the distribution of a data set is equivalent to knowing the correlations (joint probability) of its elements inside. This is why attention machanism can do a good job in this realm (Attention is correlation IMO). And when the data collection is large enough, it contains sufficient combinations of tokens for the hungry transformer network to learn from it.

Learning a good latent space is so important that we often call an encoder/auto-encoder foundation model if it is well trained to enable the downstream tasks such as decoding, diffusion or multi-modality alignment. There are two approaches that have proven successful: contrastive learning and generative learning. Contrastive learning needs to construct similar and dissimilar pairs from data samples. Although being prone to collapse and instablility issues, it is commonly used for multimodal alignment (such as CLIP) and translation tasks, where data naturally form pairs.

### Generative or Contrastive

Personally, generative learning is more attractive to me than constrastive ones as it works in a way just like people thinking. If a network is able to generate meaningful things within or even without context as we do, I would achknowledge it has the intelligence as we have. The learning scheme is usually achieved by pre-text tasks, among which I like inpainting most.

### Inpainting Task

Predicting a masked word in a sentence as defined by [BERT](https://research.google/pubs/bert-pre-training-of-deep-bidirectional-transformers-for-language-understanding/) is same as the cloze test I did in high school English exams. Visually, imagining missing parts of a scene is a task our brain is excel at. The pretraining job of completing an image from a masked input has been applied to single-view images ([MAE](https://arxiv.org/pdf/2111.06377)), multi-view images ([CroCo](https://arxiv.org/pdf/2210.10716)) and videos ([VideoMAE](https://proceedings.neurips.cc/paper_files/paper/2022/file/416f9cb3276121c42eebb86352a4354a-Paper-Conference.pdf)), which enables the mono-vision, 3D vision and temporal vision tasks, respectively. It is still surprising to me that the networks are able to fill in meaningful visual content even more than 80% of the images are missing (e.g., MAE and CroCo).

<p align="center">
<img src="/images/SSL-inpainting.jpg" alt="ssl-inpainting" style="border-radius: 15px; width: 300px;"/>
</p>
<p align="center">
<span class="footer"> <i> Inpainting capability is where intelligence emerges. </i></span>
</p>

### Conclusion

In conclusion, the scale of data will continue to grow, so are the network parameters. Self-supervised learning using pre-text tasks such as inpainting is the only way of making both parties work together. This lesson has already been learned by image and video diffusion model pretraining. But it's just the beginning. Computer vison ecompasses many more tasks beyond diffusion, and the same practices should be transferred to all the other tasks, to achieve optimal success. 


