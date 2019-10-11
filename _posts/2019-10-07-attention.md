---
layout: post
title: "Attention Is All You Need"
date: 2019-10-07
---

The paper proposes new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely. Experiments on two machine translation tasks show these models to be superior in quality while being more parallelizable and requiring significantly less time to train. It was found that the Transformer generalizes well to others tasks by applying it successfully to English constituency parsing both with large and limited training. The Transformer can reach a new state of the art in translation quality after being trained for as little as twelve hours on eight P100 GPUs.

**Model Architecture**

The encoder maps an input sequence of symbol representations (x<sub>i</sub>,…x<sub>n</sub>) to a sequence of continuous representations z = (z<sub>1</sub>,…z<sub>n</sub>). Given z, the decoder then generates an output sequence (y<sub>1</sub>,…y<sub>m</sub>) of symbols one element at a time. At each step the model is auto-regressive, consuming the previously generated symbols as additional input when generating a text.
The Transformer follows this overall architecture using stacked self-attention and point-wise, fully connected layers for both the encoder and decoder.

**Encoder Stack**

The encoder is composed of as stack of N = 6 identical layers. Each layer has two sub-layers. The first is a multi-head self-attention mechanism, and the second is a simple, position-wise fully connected feed-forward network. Residual commotion is employed around each of the two sub-layers, followed by layer normalization. That is, the output of each sub-layer is _LayerNorm(x + Sublayer(x))_, where _Sublayer(x)_ is the function implemented by the sub-layer itself. To facilitate these residual connections, all sub-layers in the model, as well as the embeddings layers, produce outputs of dimension d<sub>model</sub> = 512.

**Decoder Stack**

The decoder is also, composed of a stack of N = 6 identical layers. In addition to the two sub-layers in each encoded layer, the decoder inserts a third sub-layer, which performs multi-head attention over the output of the encoder stack. Similar to the encoder, residual connections around each of the sub-layers is also employed, followed by layer normalization. The self-attention sub-layer in the decoder stack is modified to prevent positions from attending to subsequent positions. This masking combined with fact that the output embeddings are offset by one position, ensures that the predictions for position _i_ can depend only on the known outputs at position less that _i_.

**Attention**

An attention function can be described as mapping a query and a set of key-value pairs to an output, where the query, keys, valued, and output are all vectors. The output is computed as weighted sum of the values, where the weight assigned to each values is computed by compatibility function of the query with the corresponding key.

**Scaled Dot-Product Attention**

The attention in this model is called “Scaled Dot-Product Attention”. The input consists of queries and keys of dimension d<sub>k</sub>, and values of dimension d<sub>v</sub>. The dot products of the query with all keys is computed, then each was divided by √d<sub>k</sub>, and softmax function was applied to obtain the weights on the values.
The attention function on a set of queries was computed simultaneously, packed together into a matrix Q. The keys and values are also packed together into matrices K and V. The matrix output is computed as :
**Attention(Q,K,V) = softmax(QK<sup>T</sup> / √d<sub>k</sub>)**

**Muti-Head Attention**

Instead of performing a single attention function with d<sub>model</sub>-dimensional keys, values and queries, it is beneficial to linearly project the queries, keys and values h times with different, learned linear projections to d<sub>k</sub>, d<sub>k</sub> and d<sub>v</sub> dimensions, respectively. On each of these projected versions of queries, keys and values attention function in parallel, yielding d<sub>v</sub>-dimensional output values is performed. These are concatenated and once again projected, resulting in the final values. 
Multi-head attention allows the model to jointly attend to information from different representations subspace at different positions.

**Self Attention**

Self-Attention, sometimes called intra-attention is an attention mechanism relating different positions of a single sequence in order to compute a representation of the sequence. Self-attention has been used successfully in a variety of tasks including reading comprehensions, abstractive summarization, textual entailment and learning task-independent sentence representations. The Transformer is the first transduction model relying entirely on self-attention to compute representations of its input and output without using sequence-aligned RNNs or convolution. Self-attention was chosen because of few reasons. One is the total computational complexity per layer. Another is the amount of computation that can be parallelized, as measure by the minimum number of sequential operations required. The third is the path length between long-range dependencies in the network.

**Position-wise Feed-Forward Networks**

In addition to attention sub-layers, each of the layers in our encoder and decoder contains a fully connected feed-forward network, which is applied to each position separately and identically. This consists of two linear transformations with ReLu activation in between.


**FFN(x) = max(0, xW<sub>1</sub> + b<sub>1</sub>)W<sub>2</sub> + b<sub>2</sub>**

**Embeddings and Softmax**

Similar to other sequence transduction models, learned embeddings to convert the input tokens and output tokens to vectors of dimension d<sub>model</sub> is used. The usual learned linear transformation and softmax function to convert the decoder output to predicted next-token probabilities is used. In the model the same weight matrix between the two embedding layers and the pre-softmax linear transformation is shared. In embedding layers those weights are multiplied by √d<sub>model</sub>.

**Positional Encoding**

To get some information about the relative or absolute position of the tokens in the sequence “positional encoding” is added to the input embeddings at the bottom of the encoder and decoder stacks. The positional encodings have the same dimension d<sub>model</sub> as the embeddings, so that the two can be summed. In this model, sine and cosine functions of different frequencies is used:
**PE(pos,2i) = sin(pos/10000<sup>2i</sup>/d<sub>model</sub>)**

**PE(pos,2i+1) = cod(pos/10000<sup>2i</sup>/d<sub>model</sub>)**

Where pos is the position and i is the dimension.

Read the original paper [here](https://arxiv.org/abs/1706.03762).
