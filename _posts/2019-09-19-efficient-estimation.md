---
layout: post
title: "Efficient Estimation of Word Representations in Vector Space"
date: 2019-09-19
---

This paper presents two novel model architecture for computing continuous vector representations of words from very large data sets. The quality of these representations is measured in word similarity task, and the results are compared to the previously best performing techniques based on different types of neural networks. 
The main goal of this paper is to introduce techniques that can be used for learning high-quality word vectors from huge data sets with billions of words, and with millions of words in the vocabulary. It also presents a new comprehensive test set for measuring both syntactic and semantic regularities. 

**Model Architecture**

The paper focuses on distributed representations of words learned by neural network. The compare different model architectures it define first the computational complexity of a model as the number of parameters that need to be accessed to fully train the model. Next it will try to maximize the accuracy, while minimizing the computational complexity. For all the models , the training complexity is proportional to 
			**O = E  x T x Q,**

Where E is number of the training epochs, T is the number of the words in the training set and Q is based on each model architecture.
All models are trained using Stochastic Gradient Descent and Backpropagation.

**Feedforward Neural Net Language Model (NNLM)**

The probabilistic feedforward neural network language model consist of input, projection, hidden and output layers. At input layer, N previous words are encoded using 1-of-V coding, where V is the size of the Vocabulary. The input layer is then projected to a projection layer P that has dimensionality N x D, using a shared projection matrix. The hidden layer is used to compute the probability distribution over all the words in the vocabulary, resulting in an output layer with dimensionality V. The resulting computational complexity per each training	example is
			**Q = N x D + N x D x H + H x V**

The model uses hierarchical softmax where the vocabulary is represented as Huffman binary tree. Huffman trees assign short binary codes to frequent words, and this further reduces the number of outputs units that need to be evaluated.  Huffman tree based hierarchical softmax require only about log₂(Unigram_perplexity(V)) outputs. 

**Recurrent Neural Net Language Model (RNNLM)**

Recurrent neural network based language model has been proposed to overcome certain limitations of the feedforward NNLM, such as the need to specify the context length (the order of the model N), and because theoretically RNNs can efficiently represent more complex patterns than the shallow neural networks. The RNN model does not have a projection layer; only input, hidden and output layer. The recurrent matrix that connects hidden layers to itself, using time delayed connections allows the recurrent model to form some kind of short term memory, as information from the past can be represented by the hidden layer state that gets updated based on the current input and the sate of the hidden layer in the previous time step.
The complexity per training example of the RNN model is 			
			**Q = H x H + H x V**

Where the word representation D have the same dimensionality as the hidden layer H.

**Parallel Training Of Neural Network**

To train the model on huge data sets, several models on top of a large-scale distributed framework called DistBelief was used including the feedforward NNLM and the new models proposed in the paper. The framework allows to run multiple replicas of the same model in parallel, and each replica synchronizes its gradient update through a centralized server that keeps all the parameters. For this parallel training mini-batch asynchronous gradient descent with Adagrad was used.

**Continuous Bag-of-Words Model**

The first proposed architecture is similar to feedforward NNLM, where the non-linear hidden layer is removed and the projection layer is shared for all words (not just the projection matrix); thus, all words get projected into the same position (their vectors are averaged). This architecture is called bag-of-words model as the other words in the history does not influence the projection. Furthermore the words from the future were also used.  
The best performance on the task was obtained by building a log-linear classifier with four future and four history words at the input, where the training criterion is to currently classify the current (middle) word. Training complexity is then 			
			**Q = N x D + log₂(V)**

The model is denoted as CBOW, as unlike bag-of-words model, it uses continuous distributed representation of the context.

**Continuous Skip-gram Model**

This architecture is similar to CBOW, but instead of predicting the current word based on the context, it tries to maximize the classification of a word based on another word in the same sentence. It uses each current word as an input to a log-linear classifier with continuous projection layer, and predict word within a certain range before and after the current word. It was found that increasing the range improves the quality of the resulting word vectors, but it also increases the computational complexity.
The training complexity of this architecture is proportional to 
			**Q = C x (D + D x log₂(V))**

Where C is the maximum distance of the words.

**Takeaways**

* It is possible to train high quality word vectors using very simple model architecture
* Because of the  much lower computational complexity, it is possible to compute very accurate high dimensional word vectors from much larger data sets.
* Using DistBelief distributed framework, it should be possible to train the CBOW and Skip-gram models even on corpora with one trillion words, for basically unlimited size of the vocabulary.

***Disclosure*

Most of the things are directly from the paper. This post is meant to be a one place for all the papers that I read and take notes.
Read the original paper [here](https://arxiv.org/pdf/1301.3781.pdf)
