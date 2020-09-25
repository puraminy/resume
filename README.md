
# Composing Answer from Multi-spans for Reading Comprehension
## Abstract
This paper presents a novel method to generate answers for non-extraction machine reading comprehension (MRC) tasks whose answers cannot be simply extracted as one span from the given passages. Using a pointer network-style extractive decoder for such type of MRC may result in unsatisfactory performance when the ground-truth answers are given by human annotators or highly re-paraphrased from parts of the passages. On the other hand, using generative decoder cannot well guarantee the resulted answers with well-formed syntax and semantics when encountering long sentences. Therefore, to alleviate the obvious drawbacks of both sides, we propose an answer making-up method from extracted multi-spans that are learned by our model as highly confident $n$-gram candidates in the given passage. That is, the returned answers are composed of discontinuous multi-spans but not just one consecutive span in the given passages anymore....

## Introduction
The task of Machine Reading Comprehension is to generate or extract an answer for a question according to given passages . MRC tasks essentially differ from each other according to the form of their required answers. We have two general categories of MRC tasks, extraction and non-extraction, which are thus respectively solved by extractive and generative decoders in MRC models. So far, the extraction-style MRC tasks along with their datasets and leaderboards have been well-developed . As the answer for this type is shown as a span 
appearing in the given passage, sometimes, such an MRC task is called span-style.

For example, MS MARCO requires the MRC models to generate an answer for the question according to the given redundant passages.

Answers in extraction or span-style MRC tasks are located somewhere of its given passage; therefore, pointer network as a typical extractive decoder for such an answer position prediction has been widely employed.

To alleviate the drawbacks of both types of decoders, in this paper, we propose a novel solution to the non-extraction MRC tasks by making up answers from extracted spans.

All the possible candidate spans will be picked to compose the final answer in their original order in the passage. Experiments on MS MARCO dataset show that the proposed method substantially outperforms two competitive typical one-span and Seq2Seq baseline decoders, and has a better performance on accurately generating long answers.

## Related Work
With the development of deep learning, the research of machine reading comprehension has made great progress. At the same time, various types of reading comprehension tasks require suitable targeted solutions.

SQuAD2 asks models to judge whether the question is answerable according to the given passage, and hotpotQA needs the model to find and reason over multiple supporting documents. Non-extraction style MRC tasks like Dureader and MS MARCO are required to generate an answer according to the given context and question.

The cloze-style MRC like CNN/ Daily Mail (Hermann 
et al. 2015) is to predict a masked word in a given passage.2.

The multi-choice style MRC like RACE requires models to choose a correct answer from a set of answer candidate options. In this paper, we focus on the generative style MRC, which, though it belongs to the type of non-extraction, still keeps strong enough answer clues inside the given passage.

The former adopts contextualized language models , and the latter should be carefully selected according to the answer form of the task type, which is our major focus in this paper.

On the other hand, researchers also seek help from generative models such as Seq2Seq and Pointer-Generator Networks , which help generate answers directly. Tan et al. developed an extraction-then-synthesis framework to synthesize answers from extraction results, which used a pointer network to extract span from a passage, and then adopted a Seq2Seq model to generate answers .

## Method
Recent MRC systems often share a similar option by adopting pre-trained CLMs as the encoder but still differs from the decoder part. Following the general model design, the overview of our model is illustrated in Figure 1. Different from existing pointer network modeling, in which only two boundary words for one span in the passage need to learn and predict, our model is to compose the answer from multiple spans. Thus, we first need to label the answer spans in the text of passage according to the gold answer.

The predicted answer will also be composed by simply concatenating all the predicted spans in their original order in the passage.

Our decoder is hereafter called multi-span decoder to easily distinguish from the pointer network decoder, which only extracts one span as the answer.

We attribute such a negative result to the ignorance of answer spans by this word-level decoding strategy.

Syntactic Spans To limit the n-gram spans for scoring 
with linguistic meaning, we introduce a syntactic constituent parse tree to pick reasonable spans from passages. Using a pre-trained parser may give a constituent parse tree for each input sentence. Then, each subtree in the parse tree can determine a phrase or constituent in the sentence, whose length may vary from 1 to the sentence length.

## Experiment
Ten passages are provided for answering a question, and not every passage contains clues for answering the question. There are also non-answerable questions in the dataset, which means all the given passages do not contain any helpful clue to answer the question. To let the evaluation over our proposed method have a specific focus, we extract an answerable subset of MS MARCO for our main evaluation, which includes all answerable questions and question-related passages. Thus for most experiments in this paper, the passage ranker as another extra factor is not necessary then to avoid additional influential factors. In addition, we split the official development set into two parts to be our development and test sets for evaluating our model.

The official evaluation script provided by MS MARCO is used for our evaluation.

As shown in Table 3, our multi-span decoders in terms of proper span settings perform better than one-span decoder and much better than generative decoder on all the evaluating metrics.

Especially, our syntactic span decoder gives the highest scores in both BLEU and ROUGE-L for answer length 21-25 and nearly the highest scores for all answers, which are longer than 25.

As the complete MS MARCO Q&A task provides several passages for one question, which needs an extra passage selection or ranking module, we simply re-implement the Passage Ranker described in Nishida et al.

The benefit has to come from our multi-span decoder, especially for better handling of long answers according to our analysis above.

## Conclusion
For generative machine reading comprehension, previous work struggled in choosing a one-span extractive decoder or loose Seq2Seq decoder. Therefore in this work, we propose a novel multi-span decoding method to compromise such a dilemma by inspired that even generative answers can be composed of discontinuous multiple spans in the given passages. In detail, our proposed model determines candidate spans in the passage, and then all the candidate spans are connected together with their appearance order in the original passage to compose the final answer. Experiment results show that our model outperforms mainstream onespan/generative decoders on the MS MARCO benchmark. Especially, our models have a better performance on accurately generating long answers.

https://arxiv.org/pdf/2009.06141.pdf
