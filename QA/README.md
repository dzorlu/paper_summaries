

Get to the Point
------------------
Generative models exhibit undesirable behavior such as inaccurrately reproduting factuaral details, an inability to deal with OOV, and repeating themselves.

Generation probability for temestep t is calculated from the context vector, decoder state and decoder input. The probability is used
as a soft switch to choose between generting a word from the voacbulary by sampling from Pvocab or copying a word from the input sequence by
sampling from the attention distribution.

Coverage vector keeps track of cumulative attention distributions over all previous decoder timesteps. The vector
respresents the degree of coverage that those words have received so far. This is fed into attention calculatation alongside witha
learnable parameter such that the attention mechanisms current decision is informed by a reminder of its previous decisions.

In addition, the loss is appended with a coverage loss. It is the minimum of attention vector at current time step and cumulative coverage vector. The loss is bounded to 1.

The approach does NOT beat the simple baseline, though. Ugh. This is partly it is trained on news articles which tend to have a lede at the beginiing.

Also, introduction of pointer expectedely reduces the extent of novel appearences of n-grams.

Efficient and Robust Question Answering from Minimal Context over Documents
---------------------------------------------------------------------------
The analysis shows that most questions can be answered within a single sentence.
If the model can accurately predict the `oracle` sentence, the model should be able to achieve comparable performance on overall QA task. The aim
is to create an effective, efficient, and robust QA system which only requires a single or a few sentences to answer a question.

NQ answering model + sentence selector.

Sentence selector consists of encoder and decoder. The encoder is shared and pretrained NQ answering model.
The decoder is a task-specific module which computes the score for the sentence whether the question is answerable or nonanswerable given the sentence.


The scores are thresholded allows the model to select a variable number of sentences for each question.


A deep cascade
--------------
The answer prediction task is split into three joint tasks: document extration, paragraph extraction, and answer span extraction.

bottom up abstractive summarization
------------------------------------
content selection module emits a probability of the token being included in the summarization. The probability is hard-thredholded in the
point-generation network such that tokens with p less than the threshold are masked. Content selection module is a simple bi-LSTM applied
on top of concat of ELMO and CoVe.


Dialogue:
- genericness
- irrelevant response problem
- lack of content
- lack of persona
- repetition

