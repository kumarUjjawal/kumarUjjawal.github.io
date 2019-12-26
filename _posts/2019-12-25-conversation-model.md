---
layout: post
title: "A Neural Conversational Model"
date: 2019-12-25
---

Conversational modeling is an important task in natural language understanding and machine intelligence. The paper presents a simple approach for this task which uses the recently proposed sequence to sequence framework. The model converses by predicting the next sentence given the previous sentence or sentences in a conversation. The strength of the model is that it can be trained end-to-end and thus requires much fewer hand-crafted rules. This straightforward model can generate simple conversations given a large conversational training dataset. 

**Model**

The approach makes use of the sequence-to-sequence (*seq2seq*) framework described in (Sutskever et al., 2014). The model is based on a recurrent neural network which reads the input sequence one token at a time, and predicts the output sequence, also one token at a time. During training, the true output sequence is given to the model, so learning can be done by backpropagation. The model is trained to maximize the cross entropy of the correct sequence given its context. During inference, given that the true output sequence is not observed, we simply feed the predicted output token as input to predict the next output. This is a “greedy” inference approach.

![an image alt text]({{ site.baseurl }}/images/conv1.png "an image title")

Concretely, suppose that we observe a conversation with two turns: the first person utters “ABC”, and second person replies “WXYZ”. We can use a recurrent neural network, and train to map “ABC” to “WXYZ” as shown in Figure 1 above. The hidden state of the model when it receives the end of sequence symbol “eos” can be viewed as the thought vector because it stores the information of the sentence, or thought, “ABC”. 
Applying this technique to conversation modeling is also straightforward: the input sequence can be the concatenation of what has been conversed so far (the context), and the output sequence is the reply. 

**Datasets**

Two datasets were used: a closed-domain IT helpdesk troubleshooting dataset and an open-domain movie transcript dataset. The details of the two datasets are as follows. 

The first dataset which was extracted from a IT helpdesk troubleshooting chat service. In this service, costumers face computer related issues, and a specialist help them by conversing and walking through a solution. Typical interactions (or threads) are 400 words long, and turn taking is clearly signaled. The training set contains 30M tokens, and 3M tokens were used as validation. Some amount of clean up was performed, such as removing common names, numbers, and full URLs. 

The OpenSubtitles dataset consists of movie conversations in XML format. It contains sentences uttered by characters in movies. We applied a simple processing step removing XML tags and obvious non-conversational text (e.g., hyperlinks) from the dataset. The training and validation split has 62M sentences (923M tokens) as training examples, and the validation set has 26M sentences (395M tokens). The split is done in such a way that each sentence in a pair of sentences either appear together in the training set or test set but not both. Given the broad scope of movies, this is an open-domain conversation dataset, contrasting with the technical troubleshooting dataset. 

**Examples**

![an image alt text]({{ site.baseurl }}/images/conv.png "an image title")

**Conclusion**

The paper showed that a simple language model based on the *seq2seq*framework can be used to train a conversational engine. 
The strength of this model lies in its simplicity and generality. We can use this model for machine translation, question/answering, and conversations without major changes in the architecture. Even though the model has obvious limitations, it is surprising to that a purely data driven approach without any rules can produce rather proper answers to many types of questions. However, the model may require substantial modifications to be able to deliver realistic conversations. 


**Disclosure**

Everything is directly from the original paper. This post is meant to be a one place for all the papers that I read and take notes.
Read the original paper [here](https://arxiv.org/pdf/1506.05869.pdf).




