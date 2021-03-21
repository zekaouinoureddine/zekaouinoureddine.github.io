# BERT : Modern Transformers


## Overview
BERT was one of the first models to show that transformers could reach human-level performance on a variety of language based tasks:
- Question Answering
- Sentiment Classification
- …

## Architecture

![QAbert.png](images\QAbert.png)

BERT consists of a simple stck of transformers blocks, of the type we've described above. This stack is `pre-trained` on large general domain corpus of `800M` words from English books (modern work, from unpublished authors), and `2.5B` words of text from English Wikipedia articles (without markup)

## How it works

### `Pre-training`

`Pre-training` is done through two tasks:

* `Masking`  A certain number of words in the input sequnce are: 
- Masked out, 
- Replaced with random word,
- Kept as is.
The model is then asked to predict, for these words, what the original words were. Note that the model doesn't need to predict the entire denoised sentence, just the modified words. Since the model doesn't knw which words it will be asked about, it learns a representation for every word in the sequence.

* `Newt sequence classification` Two sequnecesof about `256 words` are sampled that either (a) follw each other directly in the corpus, or (b) are both taken from random places. The model must then predict whether a or b is the case.


On the other hand, BERT us WorPiece tokenization, which is somewhere in between word-level and character level sequences. It breaks words like `walking` up into the tokens `walk` and `ing`. This allows the model to make some infreneces based on word structure: two verbs ending in -ing have similar grammatical functions, and two verbs starting with walk-ahve similar semanic function.


### Input
The input is prepended with a special `<cls>` token. The output vector corresponding to this token is used as a sentence representation in sequence classification tasks like the next sentence classification (as opposed to the global average pooling over all vectores that we used in our classification model above).

After pre-training, a single task-specific layer is palced after the body of transformer bloks, which maps the general purpose representation to a task specific output. For calssification tasks, this simply maps the firstoutput token to softmax probabilities over the classes. For more complex tasks, a final sequence-to-sequence layer is designed specifically for the task.


### `Fin-tuning`

The whole model is then retrained to finetune the model for the specific task at hand.


⇒ In an ablation experiment, the authors show that the largest improvement as compared to previous models comes from the bidirectional nature of BERT. That is, previous models like GPT used an autoregressive mask, which allowed attention only over previous tokens. The fact that in BERT all attention is over the whole sequence is the main cause of the improved performance.

### Some details

![details.png](images\details.png)

* **MLM**
In the BERT training process, the model recieves sentences with some random masked words:

1. Adding a classification layer on top of the encoder output.
2. Multiplying the output vectors by the embedding matrix, transforming them into the vocabulary dimension.
3. Claculating the probability of each word in the vocabulary with softmax.

The BERT loss function takes into consideration only the prediction of the masked values and ignores the prediction of the non-masked words. As a consequence, the model converges slower than directionam models, a characteristic which is offset by its increased context awarness.

* **NSP**

In the BERT training process, the model recieves paires of sentences as input and learns to predict if the second in the pair is the subsequent sentence in the original document. 

1. A [CLS] token is inserted at the beginning of the first sente,ce and a [SEP] token is iserted at the end of each sentence.
2. A sentence embedding indicating sentence A or sentence B is added to each token. Sentence embedding are similar in concept to token embeddings with a vocabulary of 2.
3. A positional embedding is added to each token to indicate its position in the sequence . The concept and implementation of positional embedding are presented in the transformer paper.

![hh.png](images\hh.png)


## BERT two Model size

![bertsize.png](images\bertsize.png)

 * `BERT BASE` : Comparable in size to the OpenAI Transformer in order to compare performance. (12 encoder) => (ffnn 768 hidden unites) => (12 attention head)
 * `BERT LARGE` A ridiculiously huge model wich achieved the state of the art results reported in the paper. (24 encoder) => (ffnn 1024 hidden units) => (16 attention head)
 
 => Note that the default implementation of the transformer in the initial paper (6 encoder layers, 512 hidden units, and 8 attention heads)
 
## Model Input and output visually
### Input
![bertinput.png](images\bertinput.png)
### Output
![bertoutpu.png](images\bertoutpu.png)

![bertfinal.png](images\bertfinal.png)

## References
- [TRANSFORMERS FROM SCRATCH](http://peterbloem.nl/blog/transformers)
- [BERT Explained](https://www.lyrn.ai/2018/11/07/explained-bert-state-of-the-art-language-model-for-nlp/)
- 
