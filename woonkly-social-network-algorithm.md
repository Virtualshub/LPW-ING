---
description: 'Natural Language Processing: BERT + GPT2 (Experimental)'
---

# Woonkly Social Network Algorithm

To be objective with the audience, the content and the data, we are developing Natural Language Processing models, which is a branch within Machine Learning, in the field of Artificial Intelligence.

![](.gitbook/assets/image%20%2880%29.png)

These algorithms allow to have a greater compression of the data and to personalize both campaigns to a certain target audience through powerful segmentation, if not also to personalize the user experience at a very high level.  
  
We use Bert and we are experimenting with GPT-2

[BERT \(sistema computacional de comprensión de lenguaje\)Representación de Codificador Bidireccional de Transformadores \(BERT, por sus siglas en inglés\) es una técnica basada en redes neuronales para el pre-entrenamiento del procesamiento del lenguaje natural \(PLN\) desarrollada por Google.&\#8203; BERT fue creado y publicado en 2018 por Jacob Devlin y sus colegas de Google.&\#8203; Google está aprovechando BERT para comprender mejor las búsquedas de los usuarios. &\#8203;es.wikipedia.org](https://es.wikipedia.org/wiki/BERT_%28sistema_computacional_de_comprensi%C3%B3n_de_lenguaje%29)  


In particular, what makes this new model better is that it can **understand passages** within documents in the same way that **BERT** understands words and sentences, allowing the algorithm to understand longer documents.  
  
**Let's first clarify what a passage is on a web page**

According to **Martin Splitt** from Google, a passage is a specific part within a document \(url, post, product page, page\) where the algorithm will try to understand the document in parts and _be able to qualify different parts of a page independently.  
****_  
**The passages** are designed by Google to understand those super long pages, posts, etc. where there is a lot of content and where perhaps, different users are only interested in knowing a part of that content, therefore, they do not have to read \(or search \) all text.

![](.gitbook/assets/image%20%2835%29.png)

  
**How BERT works**

BERT uses Transformer, an attention mechanism that learns the contextual relationships between words \(or subwords\) in a text. In its basic form, Transformer includes two separate mechanisms: an encoder that reads the text input and a decoder that produces a prediction for the task. Since the goal of BERT is to generate a language model, only the encoder mechanism is required. The detailed operation of Transformer is described in a Google [doc](https://arxiv.org/pdf/1706.03762.pdf).  
  
Unlike directional models, which read text input sequentially \(left to right or right to left\), the Transformer encoder reads the entire sequence of words at once. Therefore, it is considered bidirectional, although it would be more accurate to say that it is not directional. This feature allows the model to learn the context of a word based on its entire environment \(left and right of the word\).  
  
The following table is a high-level description of the transformer encoder. The input is a sequence of tokens, which are first embedded in vectors and then processed in the neural network. The output is a sequence of vectors of size H, in which each vector corresponds to an input token with the same index.  
  
When training language models, there is the challenge of defining a prediction target. Many models predict the next word in a sequence \(for example, "Child came home from \_\_\_"\), a directional approach that inherently limits learning from context. To overcome this challenge, BERT uses two training strategies:

  
**Masked LM \(MLM\)**

Before entering sequences of words into BERT, 15% of the words in each sequence are replaced with a \[MASK\] token. The model then tries to predict the original value of the masked words, based on the context provided by the other unmasked words in the sequence. In technical terms, predicting the output words requires:

1. Add a classification layer on top of the encoder output.
2. Multiply the output vectors by the embedding matrix, transforming them into the vocabulary dimension.
3. Calculate the probability of each vocabulary word with softmax.

![](.gitbook/assets/image%20%2857%29.png)

**Next Sentence Prediction \(NSP\)  
  
In the BERT training process, the model receives sentence pairs as input and learns to predict whether the second sentence of the pair is the subsequent sentence in the original** document. During training, 50% of the entries are a pair in which the second sentence is the after sentence in the original document, while in the other 50% a random sentence from the corpus is chosen as the second sentence. The assumption is that the random sentence will be disconnected from the first sentence.  
  
To help the model distinguish between the two sentences in training, the input is processed as follows before entering the model:

1. A token \[CLS\] is inserted at the beginning of the first sentence and a token \[SEP\] at the end of each sentence.
2. A sentence insert indicating either sentence A or sentence B. Sentence inlays are similar in concept to inlays for cards with a vocabulary of 2 added to each tile.
3. A positional embed is added to each token to indicate its position in the sequence. The concept and implementation of positional keying is presented in the Transformer article.

![](.gitbook/assets/image%20%2825%29.png)

To predict whether the second sentence is actually connected to the first, the following ****steps are performed:

1. The entire input sequence goes through the Transformer model.
2. The output of the token \[CLS\] is transformed into a vector with a 2 × 1 shape, using a simple classification layer \(learned matrices of weights and biases\).
3. Is Next Sequence probability calculation with softmax.

When training the BERT model, Masked LM and Next Sentence Prediction are trained together, with the goal of minimizing the combined loss function of the two strategies.  
  
**Conclusion**  
  
BERT is without a doubt a breakthrough in the use of machine learning for natural language processing. The fact that it is accessible and allows quick fine tuning will likely allow for a wide range of practical applications in the future. In this summary, we try to describe the main ideas of the article without drowning in excessive technical detail. For those who want a deeper dive, we recommend reading the full article and the ancillary articles referenced in it. Another useful reference is the [BERT](https://github.com/google-research/bert) [source code](https://github.com/google-research/bert) and models, which cover 103 languages and were generously released as open source by the research team.  


Credits: [https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270](https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270?gi=989dc3fd6c19)  


